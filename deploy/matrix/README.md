
# 生成私钥和自签名证书（有效期 10 年）
openssl req -x509 -nodes -days 3650 \
  -newkey rsa:2048 \
  -keyout key.pem \
  -out cert.pem \
  -subj "/C=CN/ST=Beijing/L=Beijing/O=Company/CN=192.168.1.100"

<!--
# 参数说明：
# -x509        : 生成自签名证书（无需 CA 签发）
# -nodes       : 私钥不加密（方便自动化部署）
# -days 3650   : 有效期 10 年（内网环境够用）
# -newkey rsa  : 生成 2048 位 RSA 密钥
# -subj        : 证书主题信息，CN 填写服务器 IP 地址
 -->


# 初始化 Synapse 配置
`docker run -it --rm \
  -v $(pwd)/data/synapse:/data \
  -e SYNAPSE_SERVER_NAME=192.168.1.100 \
  -e SYNAPSE_REPORT_STATS=no \
  matrixdotorg/synapse:latest generate`


# 创建管理员账号
## 方式一：命令行直接创建（非交互式）
`docker exec -it matrix-synapse register_new_matrix_user \
  -c /data/homeserver.yaml \
  -u admin \
  -p your_admin_password \
  -a`

## 方式二：交互式创建（会提示输入用户名和密码）
`docker exec -it matrix-synapse register_new_matrix_user \
  -c /data/homeserver.yaml`