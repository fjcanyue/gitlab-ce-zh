# gitlab-ce

由于汉化的 GitLab 社区版 Docker Image twang2218/gitlab-ce-zh 不再维护，无法进行版本升级，而直接使用GitLab的官方Docker，会因locale问题无法成功升级，因此出此版本以便大家顺利完成迁移。

# 使用

## 使用 Docker Compose

在原Docker Compose中替换image行即可，如：

```
version: '2'
services:
    gitlab:
      image: 'fjcanyue/gitlab-ce-zh:11.11.8'
      restart: unless-stopped
      hostname: 'gitlab.example.com'
      environment:
        TZ: 'Asia/Shanghai'
        GITLAB_OMNIBUS_CONFIG: |
          external_url 'http://gitlab.example.com'
          gitlab_rails['time_zone'] = 'Asia/Shanghai'
          # 需要配置到 gitlab.rb 中的配置可以在这里配置，每个配置一行，注意缩进。
          # 比如下面的电子邮件的配置：
          # gitlab_rails['smtp_enable'] = true
          # gitlab_rails['smtp_address'] = "smtp.exmail.qq.com"
          # gitlab_rails['smtp_port'] = 465
          # gitlab_rails['smtp_user_name'] = "xxxx@xx.com"
          # gitlab_rails['smtp_password'] = "password"
          # gitlab_rails['smtp_authentication'] = "login"
          # gitlab_rails['smtp_enable_starttls_auto'] = true
          # gitlab_rails['smtp_tls'] = true
          # gitlab_rails['gitlab_email_from'] = 'xxxx@xx.com'
      ports:
        - '80:80'
        - '443:443'
        - '22:22'
      volumes:
        - config:/etc/gitlab
        - data:/var/opt/gitlab
        - logs:/var/log/gitlab
volumes:
    config:
    data:
    logs:
```

## 权限问题

如果数据文件迁移到另外一台机器，启动是可能会遇到没有权限的问题，可使用下面命令解决：

```
docker exec -it <your_gitlab_name> update-permissions
```
