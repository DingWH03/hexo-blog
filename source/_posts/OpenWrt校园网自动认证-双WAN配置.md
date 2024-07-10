---
title: OpenWrt简易校园网自动认证+双WAN配置
date: 2024-07-04 16:14:17
categories: OpenWrt
tags: [OpenWrt, 网络配置, 自动认证]
author: DingWH03
---
# OpenWrt校园网自动认证+双WAN配置

最近换了新宿舍，旧的tp-link路由器搬东西的时候坏了，于是去闲鱼上50元淘了个刷好了openwrt的极路由4，路由器是有点老，但是好在人家便宜，刷上openwrt玩法也多，经过在网上查阅资料，成功配置好了双wan接入，配置好负载均衡后可叠加网速（其实相对于校园网来说电信宽带50M带宽实属有点捉襟见肘，但是人家有公网IPV4啊），配合HFUT校园网自动认证脚本可实现路由器端自动登录。

## 线路连接
原生WAN口连接电信宽带，拨号上网，LAN1口配置成WAN2，直接接入校园网，或者接入已经接入校园网的并配置成关闭DHCP的路由器
> 路由器关闭DHCP可视为AP模式，相当于交换机的功能，每个连接的用户可视为一个单独的用户，每个人都需要进行认证，每个人独立带宽。否则所有连接到路由器的人用同一个校园网账号上网。

## 宽带拨号上网
登录openwrt，选择**网络->接口->WAN**，找到WAN点击编辑，将协议修改为PPPOE，并在下面设置好宽带账号密码即可。
{% asset_img 1pppoe.png pppoe-wan %}
> 若账号密码正确但始终无法拨号成功，检查线路连接正常后，拨打10000号人工客服解绑一下端口吧，换设备容易出现这种情况。
> 配置完成后记得点击**保存并应用**，这样才会生效。

## 设置LAN1为WAN2
首先在刚才的**网络->接口**页面，选择设备标签页，将LAN1先取消配置，然后从网桥设备中移除。
{% asset_img 3.png 从网桥中移除lan1 %}
然后选择添加设备，再将LAN1添加为**网络设备**，配置项保持默认即可保持默认即可。
> 也许不用删除LAN1再添加，谁知道呢，反正我删除再添加没遇到问题。
再次来到**接口**页面，点击左下角添加新接口，名称为WAN2，协议选择DHCP客户端（自动获取ip地址），设备选择LAN1，然后为WAN2配置防火墙区域，放在外网WAN一组即可。
{% asset_img 4.png 配置防火墙区域 %}

## 为WAN与WAN2设置网关跃点
跃点数决定了两个WAN的优先级，数字越小优先级越高，要配置下面的负载均衡一定要为WAN和WAN2设置成不同的跃点数，在**网络->接口->WAN->编辑->高级设置->使用网关跃点进行设置**，如下图。
{% asset_img 5.png 配置网关跃点 %}

## 配置负载均衡
负载均衡的存在是为了让网络同时使用两条WAN接口，将WAN与WAN2配置成balanced可以实现网速叠加。
### 配置接口
在**网络->负载均衡->接口**标签页中，若已有其他接口添加可以选择删除，然后为WAN、WAN2添加进接口中，只需要添加跟踪主机或ip地址即可，我选择的是www.baidu.com，用于判断网络连接是否正常。
{% asset_img 6.png MWAN接口 %}
然后为WAN2校园网接口添加**刷新连接跟踪表**，作用是后面需要实现的校园网自动登录。
{% asset_img 7.png 刷新连接跟踪表 %}
配置完成后如下图，显示出跃点数才算成功。
{% asset_img 8.png 接口配置成功 %}
### 配置成员
为两个接口配置成员即可，跃点数决定负载均衡优先级，一般相同即可，权重可配置成两条接入WAN带宽之比。
{% asset_img 9.png 成员配置 %}
### 配置策略
这里建议添加两个策略，一个是balanced均衡，另一个是wan2_only，及仅校园网。balanced选择两个成员，wan2_only选择wan2即可。
> 在某些情况下，某些银行app以及部分使用https协议的应用，在双ip负载均衡条件下可能会报ip切换速度过快的警告，当然切换过快，毕竟一个应用数据通过两条线与服务器连接。
{% asset_img 10.png 策略配置 %}
### 配置规则
为了配合前面我们添加的策略，我们添加两条规则，分别是Default_role和https，像下图这样配置即可。
{% asset_img 11.png 规则配置 %}
点击保存并应用后，至此算是配置成功了，但是当前还是不能访问网络，因为校园网登录还没实现。

## 校园网自动登录
这里我提供合肥工业大学合肥校区校园网自动登录脚本，其他学校的可以优先查找Github仓库，若找不到，技术强的大佬可以通过抓包的方式自行摸索。自动登录脚本参考自[合肥工业大学校园网自动登陆指南](https://github.com/HowardZorn/HFUT-DrCOM)，我为了做成自动化脚本稍微进行了一些改动。
    
    #!/bin/bash

    username="2022xxxxxx"
    password="xxxxxx"


    echo 'Checking IP address and network connectivity...'

    # Get the IP address of the current machine
    ip_address=$(ip -4 addr show lan1 | awk '/inet / {print $2}')

    # Function to test network connectivity
    check_network() {
        local url="http://detectportal.firefox.com/success.txt"
        local expected="success"

        # Use curl to fetch the content of the URL and store it in a variable
        local content=$(curl --ipv4 --silent --max-time 5 --interface lan1 "$url")
        local result=$?

        if [ $result -eq 0 ] && [ "$content" = "$expected" ]; then
    #         echo "Network connectivity test successful."
            return 0  # Success
        else
    #         echo "Failed to establish network connectivity or content does not match."
            return 1  # Failure
        fi
    }


    # Check if the IP address starts with "170" and if the network test times out
    if [[ $ip_address == 172.* ]]; then
        echo 'IP address starts with 172.'
        if ! check_network; then
            echo 'Network test failed. Executing login script...'

            # visit 172.16.200.11/12/13 to obtain the session giver URL.
            # http://210.45.240.245/switch.php?xxxxx will give you the correct session id.
            url=$(curl 172.16.200.13 --silent --interface lan1 | sed -n "s/.*'\(.*\)'.*/\1/p")

            # get PHP session
            curl $url --cookie ./cookies --cookie-jar ./cookies --output /dev/null --silent --interface lan1

            # login, phpsessid is a critical parameter.
            curl http://210.45.240.245/post.php --cookie ./cookies --cookie-jar ./cookies --data-urlencode 'username='$username --data-urlencode 'password='$password --data-urlencode '0MKKey=%B5%C7+%C2%BC' --silent --interface lan1

            # test
            curl --ipv4 --silent --interface lan1 http://detectportal.firefox.com/success.txt

            echo 'Login script executed.'
        else
            echo 'Network test succeeded. No need to execute login script.'
        fi
    else
        echo 'IP address does not start with 172. Skipping login script.'
    fi
将该脚本保存至/root/auto-login.sh，修改成正确的账号密码，授予可执行权限`chmod +x ./auto-login.sh`。
再次打开openwrt管理网页，找到**网络->负载均衡->通知**，将下面两行代码粘贴进文件最后两行，当WAN2状态改变时会自动执行这个脚本，登录到校园网。
    bash /root/auto-login.sh > /root/auto-login.log
    date > /root/flag
> 还记得配置负载均衡的配置接口部分的**刷新连接跟踪表**吗，那玩意就是用来触发这个脚本的。

## 结束
到这里OpenWrt简易校园网自动认证+双WAN配置基本上算是结束了，本人经验有限，也是在慢慢摸索中摸索出来的，如有错误欢迎指正，如愿意跟我交流或是有疑问可发邮件<dingwh2023@126.com>联系我。
