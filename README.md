1.第一步，先cd进我们的home文件夹中，这步其实随意，只是个人习惯而已cd /home


cd /home


2、下载源文件并进行解压安装

wget https://raw.githubusercontent.com/wxfyes/v2rss/main/v2rss.zip && unzip v2rss.zip
或
wget https://www.goyywp.xyz/uploads/v2rss.zip && unzip v2rss.zip

wget https://raw.githubusercontent.com/wxfyes/v2rss/main/v2rss.zip && unzip v2rss.zip
或
wget https://www.goyywp.xyz/uploads/v2rss.zip && unzip v2rss.zip
3、修改servie文件，程序启动目录 路径为/home/v2rss/v2rss.servie 如图修改第7、8行
https://wxf2088.xyz/wp-content/uploads/2020/09/1601456018-b47ee008b968fae.png

4、修改完成后，将service复制进system文件中去
cp /home/v2rss/v2rss.service /usr/lib/systemd/system

5、更新systemctl


systemctl daemon-reload

6、设置开机启动
systemctl enable v2rss

7、启动 v2rss
systemctl start v2rss

#停止v2rss
systemctl stop v2rss
#重启v2rss
systemctl restart v2rss

如果设置开机启动失败，（Job for v2rss.service failed because the control process exited with error code. See “systemctl status v2rss.service” and “journalctl -xe” for details.）那就退而求其次，实现后台运行，只要不重启vps会一直运行

cd /home/v2rss    #进入目录
chmod 755 v2rss   #给予权限
nohup ./v2rss -p 5500 & #开启并后台运行

完毕！这时候就可以用ip:5500去订阅了！但是这样看起来有点像一个半成品，还要输入端口号。下面我教大家如何利用nginx或者caddy进行反向代理，实现域名直接订阅！

vps下nginx其实作者已经说的很清楚了，这里不再复述了！


server {
    listen 80;
    server_name yourdomain.com;
    location / {
        proxy_pass http://127.0.0.1:5500;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
    access_log  your log path;
}



修改yourdomain.com成你的域名

修改your log path 成你的log文件路径

很多小伙伴关心用宝塔如何配置，其实方法很简单，甚至比caddy还要简单！

那么我们在进入宝塔之前，要先提前做好我们的域名解析！弄好后我们进入宝塔面板

首先新建一个站点
修改yourdomain.com成你的域名

修改your log path 成你的log文件路径

很多小伙伴关心用宝塔如何配置，其实方法很简单，甚至比caddy还要简单！

那么我们在进入宝塔之前，要先提前做好我们的域名解析！弄好后我们进入宝塔面板

首先新建一个站点
https://wxf2088.xyz/wp-content/uploads/2020/09/1601456980-a724441b888a8fd.png

https://wxf2088.xyz/wp-content/uploads/2020/09/1601456828-249bbb60c74ef2b.png

好了，至此我么的域名就可以正常的访问了！

caddy的实现方法就更加简单了！点击查看caddy使用方法

