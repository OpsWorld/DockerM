DockerM - Docker管理平台
====

##Docker_Web
使用 Flask 编写的Web UI，界面操作通过 Ajax 访问 API 向 RabbitMQ 发送操作信息，Docker_Client 中的 GetQueueInfo2ControlDocker 监听队列，获取信息操作。<br>
目前界面仅有基本信息查看、容器详细 JSON 信息查看、容器基本监控信息、容器开关暂停恢复功能。
###添加主机
先添加主机，然后复制Host ID到Docker_Client下的Config.py文件，Host ID是用来判断主机身份的ID。

<br>
##Docker_Client
一共有三个 Agent ，目前需要手动启动，您可以编写开机启动脚本。
###DockerStateMonitoring.py

    python DockerStateMonitoring.py [start|stop|restart]
    
DockerStateMonitoring.py 一个带有自动发现功能的容器监控 Agent 。<br>
当容器启动的时候，Agent 会启动一个线程监控容器的整个生命周期。<br>
Agent 每隔十秒获取一次信息发往 RabbitMQ ，待Server写入数据库。
###UpdateDockerInfo.py

    python UpdateDockerInfo.py [start|stop|restart]

UpdateDockerInfo.py 是一个定时更新数据库中的容器、网络、镜像、主机信息的 Agent 。<br>
容器信息每十秒更新一次，镜像、网络信息每三分钟更新一次，主机信息当脚本启动时执行一次。<br>
所有信息也是发往 RabbitMQ 待Server写入数据库。
###GetQueueInfo2ControlDocker.py

    python GetQueueInfo2ControlDocker.py [start|stop|restart]

GetQueueInfo2ControlDocker.py 是操作 Docker 的 Agent 。通过监听 RabbitMQ 中根据 Web注册主机时生成的Host ID来命名的队列，获取信息操作 Docker。<br>
目前有主机的启动、关闭、暂停、恢复和删除功能。(容器的基本创建和镜像的拉取正在做)

<br>
##Docker_Server
有一个 Agent ，目前需要手动启动，您可以编写开机启动脚本。在All In One模式下，您可以放在Docker_Client下执行。
###GetQueueInfo2SaveDatabase.py

    python GetQueueInfo2SaveDatabase.py [start|stop|restart]

GetQueueInfo2SaveDatabase.py 是把 RabbitMQ 中的信息写入数据库的 Agent 。<br>
是保证 DockerStateMonitoring.py 、 UpdateDockerInfo.py 两个 Agent 正常运作的基础。

<br>
##下版本规划
添加网页操作日志记录。<br>
网络信息界面的实现。
