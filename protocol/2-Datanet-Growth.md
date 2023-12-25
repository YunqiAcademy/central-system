# Datanet Growth Protocol


## 1. Introduction

### 1.1. Purpose

The Datanet Growth Protocol is designed for connection, modification, and disconnection of a data system on Datanet. This protocol provides a unique data address for a data system and maintains the metadata of a data system, so that a client can easily request for data using a globally-consensuses data address and through the corresponding adapter instance.



### 1.2. Overall Operation

The Datanet Growth Protocol follows the following operation chain:

```
request chain ---------------->  
data system ---- central system
<--------------- response chain
```



### 1.3. Operation Types

There are three operation types:

* Data system connection: A data system can connect itself with a central system by registering its metadata to the central system. This data system will then receive a unique data address on the Datanet.
* Data system modification: A data system can update its metadata on its connected central system.
* Data system disconnection: A data system can delete its metadata from its connected central system. After deletion, the data system is no longer connected to this central system.



## 2. Data System Connection

### 2.1. Request from Data System to Central System

When a data system joins the Datanet, it makes a connection request to central system.

The data system should choose an adapter class that matches the data system protocol. The adapter class must be chosen from the adapter class list maintained by the blockchain system. As long as there is one central system that has the adapter class, the blockchain system should have it in the class list. The central system that this data system tries to connect to does not have to have this adapter class, though it is recommended to have it for a quicker data access without forwarding the request to another capable central system (refer to Datanet Access Protocol).

#### 2.1.1. Parameters

- **name:** the name of a data system (string, required)
- **endpoint:** the network address of the data system (string, required)
- **adapterClassName:** the name of adapter class (string, required)
- **desc:** the description of the data system (string, optional)
- **paramsDef:**  the user-defined parameters (array of object, optional)

#### 2.1.2. Schema

```json
{
    "type": "object",
    "properties": {
        "header": {
            "type": "object",
            "properties": {
                "version": {
                    "const": "datanet 1.0"
                }
            },
            "required": ["version"]
        },
        "body": {
            "type": "object",
            "properties": {
                "name": {"type": "string"},
                "adapterClassName": {"type": "string"},
                "endpoint": {"type": "string"},
                "desc": {"type": "string"},
                "paramsDef": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "name": {"type": "string"},
                            "type": {"type": "string"}
                        }
                    }
                }
            },
            "required": ["name", "adapterClassName", "endpoint"]
        }
    },
    "required": ["header", "body"]
}
```

#### 2.1.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "name": "odps-citybrain",
        "desc": "a big data processing system",
        "adapterClassName": "odps-apsara-3.16",
        "endpoint": "http://172.25.205.111/api"
    }
}
```



### 2.2. Response from Central System to Data System

#### 2.2.1. Parameters

- **code**: status of the request (integer, required)  
- **message**: error message of the request if failed (string, optional)  
- **dataAddress**: the data address of the data system (string, required) 

#### 2.2.2. Schema
```json
{
    "type": "object",
    "properties": {
        "code": {"type": "integer"},
        "message": {"type": "string"},
        "dataAddress": {"type": "string"}
    },
    "required": ["code", "dataAddress"]
}
```

#### 2.2.3. Example
```json
{
    "code": 200,
    "dataAddress": "yunqi.15A5C76E1E021000"
}
```



## 3. Data System Modification

### 3.1. Request from Data System to Central System

Data system can modify its metadata on central system.

#### 3.1.1. Parameters

- **dataAddress:** the data address of the data system (string, required)  
- **name:** the name of the data system (string, optional)  
- **endpoint:** the network address of the data system (string, optional)  
- **adapterClassName:** the name of adapter class (string, optional)   
- **desc:** the description of the data system (string, optional)  
- **paramsDef:**  user-defined parameters (array of object, optional) 

#### 3.1.2. Schema

```json
{
    "type": "object",
    "properties": {
        "header": {
            "type": "object",
            "properties": {
                "version": {
                    "const": "datanet 1.0"
                }
            },
            "required": ["version"]
        },
        "body": {
            "type": "object",
            "properties": {
                "dataAddress": {"type": "string"},
                "name": {"type": "string"},
                "desc": {"type": "string"},
                "adapterClassName": {"type": "string"},
                "endpoint": {"type": "string"},
                "paramsDef": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "name": {"type": "string"},
                            "type": {"type": "string"}
                        }
                    }
                }
            },
            "required": ["dataAddress"]
        }
    },
    "required": ["header", "body"]
}
```

#### 3.1.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "dataAddress": "yunqi.15A5C76E1E021000",
        "desc": "a big data processing system"
    }
}
```



### 3.2. Response from Central System to Data System

#### 3.2.1. Parameters

- **code**: status code of the request (integer, required)  
- **message**: error message of the request if failed (string, optional)  

#### 3.2.2. Schema
```json
{
    "type": "object",
    "properties": {
        "code": {"type": "integer"},
        "message": {"type": "string"}
    },
    "required": ["code"]
}
```

#### 3.2.3. Example

```json
{
    "code": 200
}
```



## 4. Data System Disconnection

A data system can disconnect itself from the central system. After disconnection, the data address is no longer valid on the Datanet.

### 4.1. Request from Data System to Central System


#### 4.1.1. Parameters

- **dataAddress:** the data address of the data system (string, required)  

#### 4.1.2. Schema

```json
{
    "type": "object",
    "properties": {
        "header": {
            "type": "object",
            "properties": {
                "version": {
                    "const": "datanet 1.0"
                }
            },
            "required": ["version"]
        },
        "body": {
            "type": "object",
            "properties": {
                "dataAddress": {"type": "string"}
            },
            "required": ["dataAddress"]
        }
    },
    "required": ["header", "body"]
}
```

#### 4.1.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "dataAddress": "yunqi.15A5C76E1E021000"
    }
}
```


### 4.2. Response from Central System to Data System

#### 4.2.1. Parameters

- **code** status code of the request (integer, required)  
- **message** error message of the request if failed (string, optional)  

#### 4.2.2. Schema
```json
{
    "type": "object",
    "properties": {
        "code": {"type": "integer"},
        "message": {"type": "string"}
    },
    "required": ["code"]
}
```

#### 4.2.3. Example

```json
{
    "code": 200
}
```

