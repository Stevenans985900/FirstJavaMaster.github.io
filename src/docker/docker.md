# docker安装部署指南

你有两种选择: 在自己的windows系统上安装自己私有的docker, 或者直接使用51远程服务器上已经部署好的docker.

## windows上安装docker

此步骤是可选的, idea可以连接远程docker服务进行操作, 且windows版的docker安装下来占用空间至少6G以上. 各人视情况选择.

1. 前往[https://store.docker.com/editions/community/docker-ce-desktop-windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)下载Docker Community Edition for Windows安装软件(可能需要登录). 或者从`\\192.168.2.10\share`文件夹下拷贝, 文件名为"Docker for Windows Installer.exe".
2. 参考[https://blog.csdn.net/go_d_og/article/details/75675159](https://blog.csdn.net/go_d_og/article/details/75675159)进行安装.完成后系统的托盘会有docker的蓝鲸图标.
3. 右击蓝鲸图标, 点击`settings`, 进入设置.
4. 选中左侧`General`, 勾选`Expose daemon on tcp://localhsot:2375 without TLS`, 用于idea进行连接.
5. 选中左侧`Daemon`, 在`Registry mirrors`框中(用于配置加速镜像代理)中添加`https://nexus.idx365.com:8092`

确认前面的步骤没有错误提示即代表windows上的docker环境已经准备完成.

## 使用远程docker

如果不想安装windows版docker, 我已经在51上开放了docker的远程访问. idea可以连接并控制.

在各自的电脑中, 添加一个环境变量: key为`DOCKER_HOST`, value为`tcp://192.168.2.51:2375`. **然后重启你的IDEA.**

上述步骤可参考[https://blog.csdn.net/lvyuan1234/article/details/69255944](https://blog.csdn.net/lvyuan1234/article/details/69255944). *此网页的内容不只是关于远程docker的配置, 其他的内容比较落后, 不必深究, 下面会有更好的实现方法.*

> 一个环境变量换来6+G的固态硬盘空间非常划算(笑~)

### docker开启远程服务的方法(参考阅读)

编辑docker的`daemon.json`文件: linux系统一般位于`/etc/docker/`文件夹下; windows系统可在设置中选中左侧`Daemon`, 切换显示模式,即可看到json格式的文件内容.

在`daemon.json`文件中, 添加一个字段: `"hosts": ["tcp://0.0.0.0:2375","unix:///var/run/docker.sock"]`. 其中的"0.0.0.0"代表任何ip均可远程访问.

然后执行命令:

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```


## IDEA配置远程docker

教程看这里[IDEA配置远程docker](./idea-docker.md)

## dockerfile-maven-plugin的使用

`dockerfile-maven-plugin`是一个maven插件, 用于项目的自动化docker打包和部署.

其项目地址为[https://github.com/spotify/dockerfile-maven](https://github.com/spotify/dockerfile-maven). 目前已经在index-cloud-plat项目中添加了此插件, 在根级pom.xml文件中.

当我们在idea中双击`package`对项目进行打包时, 此插件会自动根据当前项目的Dockerfile文件对项目打包成镜像, 并部署到51服务器(默认部署到localhost:2375端口的docker服务上, 如果配置了`DOCKER_HOST`环境变量, 则部署到对应服务器上).

当我们在idea中双击`deploy`时, 则会根据pom.xml中的配置, 推送镜像到相应的服务器上, 目前的设置是推送到`nexus.idx365.com:8083`.

## 项目部署

目前采用docker-compose进行项目的部署. docker-compose.yml文件放置于项目的根目录, 但是仅供参考. 各个服务器的的项目运行环境不同, 所以测试环境和生产环境的docker-compose.yml肯定有所不同. **建议在项目根目录的模板上进行调整, 并尽量保持更新**.

服务器上的docker-compose.yml文件一般位于`/data/docker-compose-files`文件夹下. 运行项目只需执行命令`docker-compose up -d`即可. 关于此命令的更多信息可参阅[https://docs.docker.com/compose/](https://docs.docker.com/compose/).