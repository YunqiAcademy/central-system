# Datanet Access Protocol



## 1. Introduction

Datanet access protocol is designed for a client to get data from a data system. We separate the protocol into metadata access and data access because we could getting metadata does not need to go through the data system whereas getting data has to send the request to data system.



## 2. Metadata Access

Client can get metadata of a data system for any data address on the Datanet.



### 2.1. Overall Operations

```
request chain -------------------->
client ------------- central system
<--------------------response chain
```



### 2.2. Request from Client to Central System

If the requested data system is hosted by this central system, the central system will retrieve the metadata in its local database directly. If not, this central system will send a request to blockchain system to retrieve the metadata for this data address (refer to Interconnection Protocol for details).

##### 2.2.1. Parameters

- **dataAddress:** the data address of the data system (string, required)

##### 2.2.2. Schema

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



##### 2.2.3. Example

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



### 2.3. Response from Central System to Client

#### 2.3.1. Parameters

- **code**: response code (integer, required)
- **message**: error message (string, optional)
- **data**
  - **name:** the name of the data system (string, required)  
  - **adapterClass:** adapter class (string, required)   
  - **endpoint:** the network address of the data system (string, required)  
  - **desc:** the description of the data system (string, optional)  
  - **paramsDef:**  the business parameters definition of the data system (array of object, optional)  


#### 2.3.2. Schema

```json
{
    "type": "object",
    "properties": {
        "code": {"type": "integer"},
        "message": {"type": "string"},
      	"data": {
          "type": "object",
          "properties": {
              "name": {"type": "string"},
              "adapterClass": {"type": "string"},
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
            "required": ["name", "adapterClass", "endpoint"]
        }
    },
    "required": ["code"]
}
```

#### 2.3.3. Example

```json
{
    "code": 200,
    "message": "",
    "data": {
      "name": "odps in citybrain",
      "adapterClass": "odps-apsara-3.16",
      "endpoint": "http://1.2.3.4/api",
      "desc": "a big data process system",
      "paramsDef": []
    }
}
```



## 3. Data Access

A client can get the data by sending a request to a central system with a data address on the Datanet.

### 3.1. Overall Operation

```
request chain ---------------------------------->
client ------- central system ------- data system
<----------------------------------response chain
```

#### 3.1.1. Request Forwarding

The central system can forward the client request to another central system. Such cases could happen when central system does not have the capabilities for the requested central system, or this central system is not the host of the requested data system, or for the purpose of workload balance.

To avoid long forwarding chain, central system does **not** allow more than one forwarding to complete the client request. For example, if central system A forwards the client request to central system B, central system B cannot further forward this request to another central system.

When forwarding the client request to another central system, the parameter **allowForwarding** is required and needs to be set as False (see definition in Section 3.3.). 



### 3.3. Request from Client to Central System

#### 3.3.1. Parameters

- **dataAddress:** the data address of the data system (string, required)
- **allowForwarding**: whether we allow the central system to forward the request to another central system (boolean, optional)
- **params:** user-defined parameters for the request data system (array of object, optional)

#### 3.3.2. Schema

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
                "allowForwarding": {"type": "boolean"},
                "params": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "name": {"type": "string"},
                            "value": {"type": "object"}
                        }
                    }
                },
               "required": ["dataAddress", "allowForwarding"]
            }
        }
    },
    "required": ["header", "body"]
}
```

#### 3.3.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "dataAddress": "yunqi.15A5C76E1E021000",
        "params": [
            {
                "name": "ak",
                "value": "saqwadd"
            },
            {
                "name": "sk",
                "value": "acadada"
            },
            {
                "name": "sql",
                "value": "select * from employee limit 2"
            }
        }
    }  
}
```


### 3.5. Response from Central System to Client 

#### 3.5.1. Parameters

- **code:** status code of the request (integer, required)
- **message:** error message of the request if failed (string, optional)  
- **data:** result data of the request (object, optional)

#### 3.5.2. Schema

```json
{
    "type": "object",
    "properties": {
        "code": {"type": "integer"},
        "message": {"type": "string"},
      	"data": {"type": "object"}
    },
    "required": ["code"]
}
```

#### 3.5.3. Example

```json
{
    "code": 200,
    "data": [
          ["name","age"],
          ["alex",36],
          ["sarah",28]
     ]
}

```
