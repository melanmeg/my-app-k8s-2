# opensearch

- 保持期間をUIから設定

```bash
# Index Management
# Index policies
# Policy ID: comments_policy
{
    "policy": {
        "description": "Twicasting Comments Policy.",
        "default_state": "hot",
        "states": [
            {
                "name": "hot",
                "actions": [
                    {
                        "replica_count": {
                            "number_of_replicas": 5
                        }
                    }
                ],
                "transitions": [
                    {
                        "state_name": "cold",
                        "conditions": {
                            "min_index_age": "90d"
                        }
                    }
                ]
            },
            {
                "name": "cold",
                "actions": [
                    {
                        "replica_count": {
                            "number_of_replicas": 2
                        }
                    }
                ],
                "transitions": [
                    {
                        "state_name": "delete",
                        "conditions": {
                            "min_index_age": "1095d"
                        }
                    }
                ]
            },
            {
                "name": "delete",
                "actions": [
                    {
                        "delete": {}
                    }
                ],
                "transitions": []
            }
        ]
    }
}
```

- テスト

```bash
curl -k -u admin:admin https://192.168.11.100:8088
curl -k -u admin:admin https://opensearch-cluster-1.opensearch:9200

curl -k -u "admin:admin" \
   -H "content-type: application/json" \
   -X PUT "https://192.168.11.100:8088/test-index/" \
   -d '
{
 "mappings": {
   "properties": {
     "id": {
       "type": "integer"
     },
     "message": {
       "type": "keyword"
     }
   }
 }
}'

curl -k -u "admin:admin" \
   -H "content-type: application/json" \
   -X GET "https://192.168.11.100:8088/test-index"
```

```bash
curl  -k -u admin:admin -X POST "https://192.168.11.100:8088/test-logs/_doc" -H 'Content-Type: application/json' -d '{
  "timestamp": "2024-09-29T10:00:00Z",
  "level": "INFO",
  "message": "This is a sample log message",
  "service": "my-service",
  "userId": "12345"
}'
```
