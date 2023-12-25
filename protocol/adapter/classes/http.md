# Class Registration

```json
{
    "header": {
        "version": "Adapter Protocol/1.0"
    },
    "spec": {
        "name": "http-1.1-jsonbody",
        "capabilities": [
            {
                "protocol": "http",
                "version": "1.1"
            }
        ],
        "authors": [
            {
                "name": "mabingtao",
                "email": "devforma@gmail.com"
            }
        ],
        "parameters": {
            "type": "object",
            "properties": {
                "headers": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "key": {"type": "string"},
                            "value": {"type": "string"}
                        },
                        "required": ["key", "value"]
                    }
                },
                "payload": {"type": "object"}
            },
            "required": ["headers", "payload"]
        }
    }
}
```

# Instance Registration

```json
{
    "header": {
        "version": "Adapter Protocol/1.0"
    },
    "spec": {
        "class_id": "http-1.1-xxxxxxx",
        "instance_endpoint": "1.1.1.2:9000",
        "data_transport_protocol": "grpc",
        "authors": [
            {
                "name": "mabingtao",
                "email": "devforma@gmail.com"
            }
        ]
    }
}
```