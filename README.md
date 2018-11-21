# mq-deployer consumer部署

## 安装

```bash
npm install -g pm2
npm install -g mq-deployer
```
## 配置mq和pm2

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
# pm2
apps:
  - name: mq-deployer
    script: mq-deployer-start
    args: ./mq-deployer.yml
    watch: .
    merge_logs: true
    log_date_format: YYYY-MM-DD HH:mm Z


```

## 运行
```bash
pm2 start pm2.yml -- mq-deployer.yml
```

##查看服务是否启动
pm2 list

![a.png](a.png)

