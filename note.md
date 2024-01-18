##### 如果是使用basic类型的license，那么如果开启了x-pack的安全功能，传输层的ssl加密功能是一定要开启的。但是，如果使用trial类型的license，那么可以选择只开启安全功能，但是不启用传输层加密。
POST /_license/start_trial?acknowledge=true

##### 设置密码
curl -XPOST elastic:elastic "http://localhost:9200/_security/user/dev_elastic" \
-H 'Content-Type: application/json' \
-d '{
"password": "dev_elastic",
"roles": ["superuser"]
}'

##### 申请crt文件
openssl x509 -req -in kibana-server.csr -signkey kibana-server.key -out kibana-server.crt

##### 创建拼音索引
PUT /index_01
```json
{
  "settings": {
    "analysis": {
      "analyzer": {
        "ik_smart_pinyin": {
          "type": "custom",
          "tokenizer": "ik_smart",
          "filter": [
            "my_pinyin",
            "word_delimiter"
          ]
        },
        "ik_max_word_pinyin": {
          "type": "custom",
          "tokenizer": "ik_max_word",
          "filter": [
            "my_pinyin",
            "word_delimiter"
          ]
        }
      },
      "filter": {
        "my_pinyin": {
          "type": "pinyin",
          "keep_first_letter": true,
          "keep_separate_first_letter": true,
          "keep_full_pinyin": true,
          "keep_original": true,
          "limit_first_letter_length": 16,
          "lowercase": true
        }
      }
    }
  }
}
```

POST /index_01/_mapping
```json
{
  "properties": {
    "title": {
      "type": "text",
      "analyzer": "ik_max_word_pinyin",
      "search_analyzer": "ik_smart_pinyin"
    },
    "desc": {
      "type": "text",
      "analyzer": "ik_max_word_pinyin",
      "search_analyzer": "ik_smart_pinyin"
    },
    "img": {
      "type": "text"
    },
    "price": {
      "type": "text"
    }
  }
}
```
POST /index_01/_doc
```json
{
  "title":"女式包",
  "price":"22",
  "desc":"包包，女人的最爱"
}
```