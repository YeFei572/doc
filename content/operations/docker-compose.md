## 1、下载安装包

```sh
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

需要说明下，如果要换版本，那么直接修改`1.24.1`那个版本号即可。可以到`github`上面查询对应的版本号：https://github.com/docker/compose/releases

## 2、授权和创建软链接

```sh
# 授权
sudo chmod +x /usr/local/bin/docker-compose

# 创建软链接
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

# 检查是否安装成功, 弹出版本号和分之号就表示成功
docker-compose --version
```