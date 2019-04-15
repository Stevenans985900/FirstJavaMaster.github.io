## linux下邮件配置

安装`mailx`和`sendmail`
```
yum -y install mailx sendmail
```

配置发送账号

```
set from=rick2morty@163.com
set smtp=smtp.163.com
set smtp-auth-user=rick2morty@163.com
set smtp-auth-password=xxxx
set smtp-auth=login
```

发送命令
```
echo "邮件内容" | mail -s "邮件标题" 596674740@qq.com
```

