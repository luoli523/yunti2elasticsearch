# enable script inline and index

如下场景：

1,创建一个index

```
curl -XPUT localhost:9200/test/type1/1 -d '{
    "counter" : 1,
    "tags" : ["red"]
}'
```

2,update这个index

```
curl -XPOST 'localhost:9200/test/type1/1/_update' -d '{
    "script" : {
        "inline": "ctx._source.counter += count",
        "params" : {
            "count" : 4
        }
    }
}'

curl -XPOST 'localhost:9200/test/type1/1/_update' -d '{
    "script" : {
        "inline": "ctx._source.counter += count",
        "params" : {
            "count" : 4
        }
    }
}'
```

默认情况下，会报如下异常：

```json
{
  "error" : {
    "root_cause" : [ {
      "type" : "remote_transport_exception",
      "reason" : "[Angelica Jones][127.0.0.1:9300][indices:data/write/update[s]]"
    } ],
    "type" : "illegal_argument_exception",
    "reason" : "failed to execute script",
    "caused_by" : {
      "type" : "script_exception",
      "reason" : "scripts of type [inline], operation [update] and lang [groovy] are disabled"
    }
  },
  "status" : 400
}
```

解决方案：

在 ${ES_HOME}/config/elasticsearch.yml中增加如下配置：

    script.inline: on 
    script.indexed: on
