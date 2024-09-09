# chatgpt-share-server-extend

## 此项目是go语言开发的`chatgpt-share-server`外挂容器`chatgpt-share-server-extend`

## 在线体验
- 用户端：[demo.ainx.cc](https://demo.ainx.cc) 【自行注册账号/体验账号:test@gmail.com:test@gmail.com】
- 管理端：[demo.ainx.cc/xyhelper](https://demo.ainx.cc/xyhelper) 

## 系统截图说明
### 用户界面
- [x] 用户主页面【黑色】
- ![子目录图片](./images/img_3.png)
- [x] 用户主页面【白色】
- ![子目录图片](./images/img_4.png)
- [x] 用户登录
- ![子目录图片](./images/img.png)
- [x] 用户注册
- ![子目录图片](./images/img_1.png)
- [x] 忘记密码
- ![子目录图片](./images/img_2.png)
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


## 部署

### 2、部署教程

- 克隆项目：git clone https://github.com/frontend-winter/chatgpt-share-server.git
- 进入到文件夹：cd chatgpt-share-server
- 修改配置文件如下：docker-compose.yml
- ![子目录图片](./images/img_18.png)
- 启动项目：./deploy.sh

- nginx 配置文件 找到你的nginx配置，修改成以下配置。Cabby 
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

- 备用配置（如果上面配置 不生效使用备用配置）
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
- 把这些无用的配置注释
- ![子目录图片](./images/img_16.png)


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
      CHATPROXY: "https://demo.xyhelper.cn" #请替换成你的网关地址
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

### 5、后台增加客户管理页面
- 点击系统管理 - 权限管理 - 菜单管理 - 增加列表【点击新增按钮】
- 填写以下参数，我这里已经填写好了（如果你的文件路径没有client选项，说明nginx配置配置错误了）
- ![子目录图片](./images/img_11.png)
- 新增权限
- ![子目录图片](./images/img_12.png)
- 用户管理修改名称为授权码管理
- ![子目录图片](./images/img_17.png)

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
