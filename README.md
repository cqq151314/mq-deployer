# mq-deployer部署

## 在本地启动一个用于测试的 rabbitmq 服务

使用 [docker-compose](https://github.com/docker/compose) 启动 rabbitmq 服务

```bash
cd rabbitmq
docker-compose up
```

等待 rabbitmq 服务运行后，访问 <http://localhost:15672> 登录 rabbitmq 管理界面。帐号、密码可以从 `rabbitmq/docker-compose.yml` 中获得。

在 rabbitmq 管理界面中可以查看 Connections, Channels, Exchanges, Queues 等信息。

## mq-deployer

### 安装 & 使用

1. 安装

	```bash
	npm install -g mq-deployer
	```

2. 运行服务，监听 rabbitmq 服务端的消息

	```bash
	## 运行 config.sample.yml 中定义的服务队列，用于监听 rabbitmq 服务端的消息
	mq-deployer start config.sample.yml
	```

3. 发送消息

	```bash
	## 向 rabbitmq 发送一个部署包消息并等待服务队列完成部署
	## 消息内容为 JSON 字符串 { "packageUrl": ".../xxx.tar.gz" }
	## 目前仅支持处理 tag.gz 包
	mq-deployer send \
	  --uri amqp://admin:5IOdXo12V87F5aD4yiIGZd8R000oCuL6@localhost:5672/%2F \
	  --exchange test \
	  --router com.project-a \
	  "{\"packageUrl\": \".../xxx.tar.gz\"}"
	```
## 配置mq-deployer consumer

1. 配置mq-deployer.yml

	```bash
	# mq-deployer consumer config
    uri: amqp://admin:5IOdXo12V87F5aD4yiIGZd8R000oCuL6@localhost:5672/%2F
    exchange: test
    tasks:
    - name: project-a
        destination: /tmp/mq-deployer
        router: com.project-a

    - name: project-b
        destination: /tmp/mq-deployer
        router: com.project-b
	```
2、配置pm2.yml

```bash
# pm2.yml
apps:
  - name: mq-deployer
    script: mq-deployer-start
    args: ./mq-deployer.yml
    watch: .
    merge_logs: true
    log_date_format: YYYY-MM-DD HH:mm Z


```

## 运行mq-deployer.yml,启动mq服务
```bash
pm2 start pm2.yml -- mq-deployer.yml
```

## 查看服务是否启动
pm2 list

![a.png](a.png)

[参考资料](https://github.com/arzyu/mq-deployer)