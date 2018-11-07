## 配置文件相关

### 解决跨域问题

在 `nginx.conf` 文件的 `http` 节点中,添加以下字符即可实现跨域请求:

```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Host $server_name;
proxy_set_header X-Forwarded-Proto "https";
```