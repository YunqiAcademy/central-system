# Class Registration

```json
{
    "header": {
        "version": "Adapter Protocol/1.0"
    },
    "spec": {
        "name": "odps-apsara-3.16",
        "desc": "create sqljob",
        "capabilities": [
            {
                "protocol": "odps",
                "version": "apsara-3.16"
            }
        ],
        "authors": [
            {
                "name": "mabingtao",
                "email": "devforma@gmail.com"
            }
        ],
        "parameters": {
            "accesskey_id": {"type": "string"},
            "accesskey_secret": {"type": "string"},
            "sql": {"type": "string"}
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
        "class_id": "odps-apsara-3.16-xxxxxxxxxx",
        "instance_endpoint": "1.1.1.1:9000",
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