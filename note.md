##### 如果是使用basic类型的license，那么如果开启了x-pack的安全功能，传输层的ssl加密功能是一定要开启的。但是，如果使用trial类型的license，那么可以选择只开启安全功能，但是不启用传输层加密。
POST /_license/start_trial?acknowledge=true

##### 设置密码
curl -XPOST elastic:elastic "http://localhost:9200/_security/user/dev_elastic" \
-H 'Content-Type: application/json' \
-d '{
"password": "dev_elastic",
"roles": ["superuser"]
}'

# 申请crt文件
openssl x509 -req -in kibana-server.csr -signkey kibana-server.key -out kibana-server.crt