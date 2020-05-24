第一次使用机场，做个记录  
### 自己搭还是买机场  
听善敬的，买机场。网上也有许多推荐，多看几篇就懂了，推荐的内容大致一样。最初可以用蓝灯的免费流量买，配置完就可以不用蓝灯了。我看的推荐：  
[常见科学上网/翻墙方法汇总:SS机场推荐](https://matters.news/@looklookworld/%E5%B8%B8%E8%A7%81%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91-%E7%BF%BB%E5%A2%99%E6%96%B9%E6%B3%95%E6%B1%87%E6%80%BB-ss%E6%9C%BA%E5%9C%BA%E6%8E%A8%E8%8D%90-ssr%E6%9C%BA%E5%9C%BA%E6%8E%A8%E8%8D%90-v2ray%E6%9C%BA%E5%9C%BA%E6%8E%A8%E8%8D%90-%E5%85%8D%E8%B4%B9%E6%9C%BA%E5%9C%BA-chrome%E6%B5%8F%E8%A7%88%E5%99%A8%E6%8F%92%E4%BB%B6-vpn-%E5%85%8D%E8%B4%B9%E4%B8%8E%E4%BB%98%E8%B4%B9-bafyreicyqnbw2oqhwjmgmh5dkin5z6gjnszz2pjqnxjd55fzejdl67aq6q)

### 购买  
最终选了搬瓦工的机场```Just My Socks```,别忘了加优惠码，第一次买，先买了一个月的试水5.57刀/500G。一个月是用不掉这么多流量的平均每天17G，折腾了一天才用了0.8G     
[Just My Socks 详细图文购买教程](https://bwgjms.com/post/how-to-buy-justmysocks/)  

### ubuntu上使用  
有两种方式，命令行和图形界面，我选了前者因为后面的我装不上，流程如下  
- 通过pip或pip3安装shadowsocks
```pip3 install https://github.com/shadowsocks/shadowsocks/archive/master.zip```  
注意，必须3.0以上的才可以，否则不支持aes-256-gcm加密    
- 配置Shadowsocks  
还是两种方式，直接命令行或者写个配置文件，我是写了配置文件```/etc/shadowsocks/shadowsocks.json```  
**购买**章节链接的指导会告诉你这个文件怎么填  
```
{
    "server":"买的服务器ip",
    "server_port":"买的端口",
    "local_address": "127.0.0.1",
    "local_port":端口可自定义,
    "password":"买的连接密码",
    "timeout":600,
    "method":"买的加密方式"
}
```
### 起飞  
本来想再啰嗦点东西的，为了简洁起见直接起飞。  
1.后台启动```nohup sslocal -c /etc/shadowsocks/shadowsocks.json >/dev/null 2>&1 &```  
2.settings--> NetWork-->NetWorkProxy--> Manual-->Socks Host改为你json中的```local_address```和```local_port```。比如我的是```127.0.0.1 8888```  
目前为止已经能结束了，把浏览器的proxy设置为```遵循系统设置```,打开google验证。如果不行重启再来一遍    

### 优化:设置PAC模式    
如果你怕流量用光，可以设置shadowsocks为PAC模式。**起飞**中的设置是全局代理，浏览器任何网络请求都是走的ss，PAC模式是只把gfwlist中的网站走s代理，其他国内网络可直接访问的不做改动  
```
sudo pip3 install genpac
sudo pip3 install --upgrade genpac
sudo genpac --pac-proxy="SOCKS5 127.0.0.1:8888" -o autoproxy.pac --gfwlist-url="https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt"
```
网上搜的都是上面，我就硬是url无法获取，没办法，打开这个网址复制内容保存本地```gfwlist.txt```，换个参数继续  
```sudo genpac --pac-proxy="SOCKS5 127.0.0.1:8888" -o autoproxy.pac --gfwlist-local gfwlist.txt```  
生成了autoproxy.pac后我放到json同路径，然后再次进settings设置网络代理，只不过这次要设置为Automatic Configration ，URL填```file:///etc/shadowsocks/autoproxy.pac```  
芜湖，再次起飞  


### 波折和感受  
- 提示不支持aes-256-gcm加密  
确保ss版本大于3  
- genpac无法获取url  
搞不懂为哈，直接开链接都行的阿，复制到本地吧，换个参数生成.pac  
- Chrome能上网为啥firefox不行？  
设置firefox的proxy为```Use system proxy settings```  
还有就是别死磕：为啥按步骤来的阿，为啥我不行。走完了流程重启，一般都会有好事发生  

- **感受**  
500G流量我用不完，所以我不用PAC模式；  
firefox国内网络不能看b站直播，用ss代理完美流畅原画；    
网络代理只在settings中设置，浏览器的不要自己设，一律```使用系统设置``` ；   
这个真的快，三四十M/s； 
这是个丐版的流程，遇到问题多百度吧，图形界面我没装，sslocal自带后台参数也用不起来，开机启动也没配。需求是折腾的第一动力       
### 参考  
[Ubuntu 18.04 安装shadowsocks](https://wylu.me/posts/eed37a90/)  
[Ubuntu配置Shadownsocks以及配置pac规则](https://www.mspring.org/2018/11/17/Ubuntu%E9%85%8D%E7%BD%AEShadownsocks%E4%BB%A5%E5%8F%8A%E9%85%8D%E7%BD%AEpac%E8%A7%84%E5%88%99/)  
[ubuntu下, 使用shadowsock和Privoxy帮助你在命令行中, 无障碍进行go get ](https://gist.github.com/alexniver/9a4f1791fe4305b0750a)  
[https://forums.gentoo.org/viewtopic-t-279695-start-0.html](https://forums.gentoo.org/viewtopic-t-279695-start-0.html)  
