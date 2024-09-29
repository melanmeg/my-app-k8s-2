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
curl -X GET "http://opensearch-cluster-1-masters.opensearch:9200/opensearch_dashboards_sample_data_logs/_count" \
-H 'Content-Type: application/json' \
-d '{
  "query": {
    "term": {
      "response": "200"
    }
  }
}'

curl \
   -u "admin:admin" \
   -H "content-type: application/json" \
   -X GET "$ENDPOINT/test-index-202212"

curl -u "admin:admin" \
   -H "content-type: application/json" \
   -X PUT "http://opensearch-cluster-1.opensearch:9200/test-index-202212/" \
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
```

```bash
curl -X POST "http://192.168.11.100:8088/logs/_doc" -H 'Content-Type: application/json' -d '{
  "timestamp": "2024-09-29T10:00:00Z",
  "level": "INFO",
  "message": "This is a sample log message",
  "service": "my-service",
  "userId": "12345"
}'
```
