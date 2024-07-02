
# chatgpt-share-server-extend

## 此项目是go语言开发的`chatgpt-share-server`外挂容器`chatgpt-share-server-extend`
## 如何收费？此项目完全免费，安装好之后你提供域名给我过白即可

## 在线体验
- 用户端：[demo.ainx.cc](https://demo.ainx.cc) 【自行注册账号/体验账号:test@gmail.com:test@gmail.com】
- 管理端：[demo.ainx.cc/xyhelper](https://demo.ainx.cc/xyhelper) 

## 日志
- 2024/03/18 修复UserToken过期了还能使用的问题
- 2024/03/21 用户管理模块增加UserToken的模糊查询
- 2024/04/15 增加第三方支付回调事件
- 2024/04/16 增加批量创建userToken功能
- 2024/04/30 增加用户体系全套功能 登录、注册、忘记密码、授权码兑换...

- 2024/06/07 代码重构 改成外挂容器`chatgpt-share-server-extend`、修复已知bug
## 系统截图说明
### 用户界面
- [x] 用户登录
- ![子目录图片](./images/img.png)
- [x] 用户注册
- ![子目录图片](./images/img_1.png)
- [x] 忘记密码
- ![子目录图片](./images/img_2.png)
- [x] 用户主页面【黑色】
- ![子目录图片](./images/img_3.png)
- [x] 用户主页面【白色】
- ![子目录图片](./images/img_4.png)
- [x] 授权码页面
- ![子目录图片](./images/img_5.png)
- [x] 商城购买页面
- ![子目录图片](./images/img_6.png)

### 后台页面
- [x] 客户管理页面
- ![子目录图片](./images/img_7.png)
- [x] 客户新增页面
- ![子目录图片](./images/img_8.png)
- [x] 账号新增页面
- ![子目录图片](./images/img_9.png)
- [x] 授权码新增页面
- ![子目录图片](./images/img_10.png)


## 如何部署

### 1、一键部署

- 安装脚本
```bash
curl -sSfL https://raw.githubusercontent.com/frontend-winter/chatgpt-share-server/master/quick-install.sh | bash
```

- nginx 配置文件 找到你的nginx配置，修改成以下配置。如果你不是nginx 自行配置
```nginx configuration
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE-HOST $remote_addr;
        
        proxy_buffering off;
        proxy_cache_bypass no_cache;
        
        # chatgpt-share-server 的端口
        location / {
            proxy_pass http://127.0.0.1:8300;
        }
        
        # chatgpt-share-server-extend 的端口
        location /exend/ {
            proxy_pass http://127.0.0.1:8301/;
        }
        # chatgpt-share-server-extend 的端口
        location /admin/ {
            proxy_pass http://127.0.0.1:8301/admin/;
        }
        # chatgpt-share-server-extend 的端口
        location /list/ {
            proxy_pass http://127.0.0.1:8301/list/;
        }
        # chatgpt-share-server-extend 的端口
        location /xyhelper/ {
            proxy_pass http://127.0.0.1:8301/xyhelper/;
        }
        # chatgpt-share-server-extend 的端口
        location /u/ {
            proxy_pass http://127.0.0.1:8301/u/;
        }

```


- 备用配置
```nginx configuration
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE-HOST $remote_addr;
        
        proxy_buffering off;
        proxy_cache_bypass no_cache;
        
        # chatgpt-share-server 的端口
        location / {
            proxy_pass http://127.0.0.1:8300;
        }
        
        # chatgpt-share-server-extend 的端口
        location /exend/ {
            proxy_pass http://127.0.0.1:8301/;
        }
        # chatgpt-share-server-extend 的端口
        location /admin/ {
            proxy_pass http://127.0.0.1:8301/admin/;
        }
        
        # chatgpt-share-server-extend 的端口
        location ~ ^/(list|xyhelper|u)(/.*)?$ {
            proxy_pass http://127.0.0.1:8301/$1$2;
        }


```

- 正确的配置看起来的是这样的
- ![子目录图片](./images/img_15.png)

### 2、手动部署

- git clone https://github.com/frontend-winter/chatgpt-share-server.git
- cd chatgpt-share-server
- ./deploy.sh

- 修改你的 nginx配置 如上

### 3、老用户部署：如果你是老系统想直接想用，请继续阅读
1、先备份、先备份、先备份 
- `docker compose down`
- `cp -r ../chatgpt-share-server ../chatgpt-share-server-bak`

2、备份好了？ 接着看
- 找到你的`docker-compose.yml`文件 在 `chatgpt-share-server` 下方再增加一个容器配置
```shell
  chatgpt-share-server-extend:
    image: fewinter/chatgpt-share-server-extend:latest
    restart: always
    ports:
      - 8301:8002
    environment:
      TZ: Asia/Shanghai
      # 接入网关地址
      CHATPROXY: "https://demo.xyhelper.cn"
      # 接入网关的authkey
      AUTHKEY: "xyhelper"
    volumes:
      - ./config.yaml:/app/config.yaml
      - ./data/chatgpt-share-server/:/app/data/
    labels:
      - "com.centurylinklabs.watchtower.scope=fewinter-chatgpt-share-server-extend"
```
- 保存 ./deploy.sh 
- 修改你的 nginx配置 如上

### 4、如果是之前就部署了`fewinter/chatgpt-share-server`的镜像用户要迁移，自行参考`docker-compose.yml`文件修改

### 5、后台增加客户管理页面
- 点击系统管理 - 权限管理 - 菜单管理 - 增加列表【点击新增按钮】
- 填写以下参数，我这里已经填写好了（如果你的文件路径没有client选项，说明nginx配置配置错误了）
- ![子目录图片](./images/img_11.png)
- 新增权限
- ![子目录图片](./images/img_12.png)

### 遇到这种情况的联系我开通一下域名 wx： `frontend-winter`(备注：share过白名单：【你的域名】，不然不会通过)
- ![子目录图片](./images/img_14.png)

### 遇到问题？重启服务
- `docker compose down`
- `./deploy.sh`
### 还是遇到问题？基本上你遇到的百分之90的问题文档都有写，请最少查看文档三遍以上。 
### 还是遇到问题？可以添加微信`frontend-winter`付费安装（备注：安装share，不然不会通过）

### 加微信群
- ![子目录图片](./images/img_13.png)

# 特别说明

本项目仅供学习和研究使用，我们不对任何因使用本项目而导致的任何损失负责。
