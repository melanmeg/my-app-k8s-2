# opensearch

- 保持期間をUIから設定



```bash
# Policy ID: comments_policy
{
    "policy": {
        "description": "twicasting comments policy.",
        "default_state": "comments_hot_state",
        "states": [
            {
                "name": "comments_hot_state",
                "actions": [
                    {
                        "replica_count": {
                            "number_of_replicas": 5
                        }
                    }
                ],
                "transitions": [
                    {
                        "state_name": "comments_cold_state",
                        "conditions": {
                            "min_index_age": "90d"
                        }
                    }
                ]
            },
            {
                "name": "comments_cold_state",
                "actions": [
                    {
                        "replica_count": {
                            "number_of_replicas": 2
                        }
                    }
                ],
                "transitions": [
                    {
                        "state_name": "delete_state",
                        "conditions": {
                            "min_index_age": "1095d"
                        }
                    }
                ]
            },
            {
                "name": "delete_state",
                "actions": [
                    {
                        "delete": {}
                    }
                ],
                "transitions": []
            }
        ],
        "ism_template": {
            "index_patterns": [
                "comments-index*"
            ]
        }
    }
}
```
