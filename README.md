# mq-deployer consumer部署

## 安装

```bash
npm install -g pm2
npm install -g mq-deployer
```
## 配置mq-deployer.yml和pm2.yml文件

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

## Jenkins包构建完成后部署mq，与consumer对应

```bash
    mq-deployer-send \
    --uri amqp://admin:5IOdXo12V87F5aD4yiIGZd8R000oCuL6@localhost:5672/%2F \
    --exchange text \
    --router com.project-b \
    "{\"packageUrl\": \"${package_url}\"}"
```
