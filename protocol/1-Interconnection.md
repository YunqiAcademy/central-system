# Central System Interconnection Protocol

## 1. Introduction

Central system interconnection describes multiple central systems interconnection mechanism through blockchain system. With this mechanism, multiple data networks managed by different central systems can build the Datanet .

### 1.1. Purpose

Though interconnection protocol, any data system can be accessed from any interconnected central system even the data system was not identified by the central system that the client sends request to.

### 1.2. Overall Operations

```
request chain ------------------------>
central system ----- blockchain system
<------------------------response chain
```

### 1.3. Operation Types

There are three operation types:

* Central system connection to blockchain system: a central system can connect to and disconnect from the blockchain system.

* Metadata retrieval from blockchain system: central system can retrieve metadata of a central system, metadata of a data system, and metadata of an adapter class.

* Search on blockchain system: a central system may need to search for all the central systems with certain adapter class so that this central system can forward the request to the capable central system; a central system may need to search for all the adapter classes across all central systems so that when a data system connects to central system, the data system can select the corresponding adapter class.

  

## 2. Connecting Central System to Blockchain System

### 2.1. Connection Request

#### 2.1.1. Parameters

* Central system ID（string, required）

* Central system endpoint (string, required)

* Central system description（string, optional）

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
            }
        },
        "body": {
            "type": "object",
            "properties": {
                "id": {
                    "type": "string"
                },
                "endpoint": {
                    "type": "string"
                },
                "desc": {
                    "type": "string"
                }
            },
            "required": ["id", "endpoint"]
        }
    }  
}
```

#### 2.1.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "id": "yunqi",
        "endpoint": "1.2.3.4:9904",
        "desc": "central system deployed at hangzhou yunqi academy of engineering"
    }
}
```

### 2.2. Disconnection Request

#### 2.2.1. Parameters

* Central system ID (string, required)

#### 2.2.2. Schema

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
            }
        },
        "body": {
            "type": "object",
            "properties": {
                "id": {
                    "type": "string"
                }
             },
             "required": ["id"]
         }
    }
}
```

#### 2.2.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "id": "yunqi"
    }
}
```



### 2.3. Syncing Request

#### 2.3.1. Syncing Data System Metadata 

When syncing, a central system will sync **all** its data systems metadata with the blockchain system.

##### 2.3.1.1. Parameters

* Central system ID (string, required)
* Data system metadata (array, required)

##### 2.3.1.2. Schema

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
            }
        },
        "body": {
            "type": "object",
            "properties": {
                "csid": {
                    "type": "string"
                },
                "dataSystems": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "dataAddress": {
                                "type": "string"
                            },
                            "name": {
                                "type": "string"
                            },
                            "endpoint": {
                                "type": "string"
                            },
                            "adapterClass": {
                                "type": "string"
                            },
                            "desc": {
                                "type": "string"
                            },
                            "params": {
                                "type": "object"
                            }
                        },
                        "required": ["dataAddress", "name", "endpoint", "adpaterClass"]
                    }
                }
            }
        }
    }
}
```

##### 2.3.1.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "csid": "yunqi",
        "dataSystems": [
            {
                "dataAddress": "yunqi.BCAE345BCAE345P0",
                "name": "data system 1",
                "endpoint": "1.2.3.4:9979",
                "adapterClass": "odps-adapter"
            },
            {
                "dataAddress": "yunqi.BCAE345BCAE345P1",
                "name": "data system 2",
                "endpoint": "5.6.7.8:3367",
                "adapterClass": "mysql-adapter"
            }
        ]
    }
}
```

#### 2.3.2. Syncing Adapter Classes 

When syncing, a central system will sync **all** its adapter classes with the blockchain system.

##### 2.3.2.1. Parameters

* Central system ID (string, required)

* Adapter classes (array, required)

##### 2.3.2.2. Schema

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
            }
        },
        "body": {
            "type": "object",
            "properties": {
                "csid": {
                    "type": "string"
                },
                "adapterClasses": {
                    "type": "array",
                    "items": {
                        "type": "object",
                        "properties": {
                            "className": {
                                "type": "string"
                            },
                            "desc": {
                                "type": "string"
                            },
                            "protocol": {
                                "type": "object",
                                "properties": {
                                    "name": {
                                        "type": "string"
                                    },
                                    "version": {
                                        "type": "string"
                                    }
                                } 
                            },
                            "params": {
                                "type": "object"
                            }
                        },
                        "required": ["className", "protocol"]
                    }
                }
            }
        }
    }
}
```

##### 2.3.2.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "csid": "yunqi",
        "adapterClasses": [
            {
                "className": "odps-adapter",
                "desc": "ODPS standard adapter",
                "protocol": {
                    "name": "ODPS",
                    "version": "aspara1.0"
                }
            },
            {
                "className": "mysql-adapter",
                "desc": "MySQL standard adapter",
                "protocol": {
                    "name": "MySQL",
                    "version": "5.7"
                }
            }
        ]
    }
}
```

### 2.4. Response

#### 2.4.1. Parameters

* Response code (integer, required)
* Error message (string, optional)

#### 2.4.2. Schema

```json
{
    "type": "object",
    "properties": {
        "code": {
            "type": "integer"
        },
        "message": {
            "type": "string"
        }
    },
    "required": ["code"]
}
```

#### 2.4.3. Example

```json
{
    "code": 200
}
```

```json
{
    "code": 7300,
    "message": "bad request"
}
```



## 3. Retrieve Metadata from Blockchain System

A central system can retrieve metadata from blockchain system given a central system ID or an adapter class name or a data address.

### 3.1. Retrieve Central System Metadata

A central system needs to retrieve metadata of another central system in the cases such as request forwarding (refer to datanet access protocol). 

#### 3.1.1. Request

##### 3.1.1.1. Parameters

* Central system ID (string, required)

##### 3.1.1.2. Schema

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
          }
      },
      "body": {
          "type": "object",
          "properties": {
              "csid": {
                  "type": "string"
              }
          },
          "required": ["csid"]
      }
  }
}
```

##### 3.1.1.3. Example

```json
{
  "header": {
      "version": "datanet 1.0"
  },
  "body": {
      "csid": "yunqi"
  }
}
```

#### 3.1.2. Response

##### 3.1.2.1. Parameters

* Response code  (integer, required)

* Central system meta data (object, required)

##### 3.1.2.2. Schema

```json
{
    "type": "object",
    "properties": {
        "code": {
            "type": "integer"
        },
        "centralsystem": {
            "type": "object",
            "properties": {
                "csid": {
                    "type": "string"
                },
                "endpoint": {
                    "type": "string"
                },
                "desc": {
                    "type": "string"
                },
                "adapterClasses": {
                    "type": "array",
                    "items": {
                        "type": "string"
                    }
                }
            },
            "required": ["csid", "endpoint", "adapterClasses"]
        }
    }
}
```

##### 3.1.2.3. Example

```json
{
  "code": 200,
  "centralsystem": {
      "csid": "yunqi",
      "endpoint": "1.2.3.4:9908",
      "desc": "this central system deployed at qunqi academy of engineering",
      "adapterClasses": ["mysql-5.7", "odps-aspara1.0"]
  }
}
```

### 3.2. Retrieve Adapter Classes

A central system retrieve an adapter classe metadata on the blockchain to guarantee the global unqiueness of an adapter class name during central system connection to blockchain system.

#### 3.2.1. Request

##### 3.2.1.1. Parameters

* Adapter class name (string, required)

##### 3.2.1.2. Schema

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
            }
        },
        "body": {
            "type": "object",
            "properties": {
                "className": {
                    "type": "string"
                }
            },
            "required": ["className"]
        }
    }
}
```

##### 3.2.1.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "className": "odps-adapter"
    }
}
```

#### 3.2.2. Response

##### 3.2.2.1. Parameters

* Response code (integer, required)
* Adapter class (object, required)

##### 3.2.2.2. Schema

```json
{
    "type": "object",
    "properties": {
        "code": {
            "type": "integer"
        },
        "adapterClass": {
            "type": "object",
            "properties": {
                "className": {
                    "type": "string"
                },
                "protocol": {
                    "type": "object",
                    "properties": {
                        "name": {
                            "type": "string"
                        },
                        "version": {
                            "type": "string"
                        }
                    }
                }
                "desc": {
                    "type": "string"
                }
            },
            "required": ["className", "protocol"]
        }
    }
}
```

##### 3.2.2.3. Example

```json
{
    "code": 200,
    "adapterClass": {
        "className": "odps-adapter",
        "protocol": {
            "name": "odps",
            "version": "aspara1.0"
        },
        "desc": "ODPS standard adapter"
    }
}
```

### 3.3. Retrieve Data System Metadata

A central system retrieves metadata of a data system hosted by another central system. Such an operation may happen in the case of request forwarding (refer to datanet access protocol).

#### 3.3.1. Request

##### 3.3.1.1. Parameters

* Data system address (string, required)

##### 3.3.1.2. Schema

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
            }
        },
        "body": {
            "type": "object",
            "properties": {
                "dataAddress": {
                    "type": "string"
                }
            },
            "required": ["dataAddress"]
        }
    }
}
```

##### 3.3.1.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "dataAddress": "yunqi.BCAE345BCAE345P0"
    }
}
```

#### 3.3.2. Response

##### 3.3.2.1. Parameters

* Response code (integer, required)
* Data system metadata (object, required)

##### 3.3.2.2. Schema

```json
{
    "type": "object",
    "properties": {
        "code": {
            "type": "integer"
        },
        "dataSystem": {
            "type": "object",
            "properties": {
                "dataAddress": {
                    "type": "string"
                },
                "name": {
                    "type": "string"
                },
                "endpoint": {
                    "type": "string"
                },
                "adapterClass": {
                    "type": "string"
                }
            }
        }
    }
}
```

##### 3.3.2.3. Example

```json
{
    "code": 200,
    "dataSystem": {
        "dataAddress": "yunqi.BCAE345BCAE345P0",
        "name": "data system 1",
        "endpoint": "1.2.3.4:9979",
        "adapterClass": "odps-adapter"
    }
}
```



## 4. Search for Central System and Adapter Class

A central system searches for specific central systems or adpter classes from blockchain system. Such an operation may happen in the case of request forwarding (refer to datanet access protocol) or in the case of data system connection (refer to datanet growth protocol).

### 4.1. Search Central System by Adapter Class

A central system searches for central systems with an adapter class name. This operation happens when a central system wants to forward client request to a capable central system.

#### 4.1.1. Request

##### 4.1.1.1. Parameters

* Adapter class name (string, required)

##### 4.1.1.2. Schema

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
            }
        },
        "body": {
            "type": "object",
            "properties": {
                "className": {
                    "type": "string"
                }
            },
            "required": ["className"]
        }
    }
}
```

##### 4.1.1.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "className": "odps-adpter"
    }
}
```

#### 4.1.2. Response

##### 4.1.2.1. Parameters

* Response code (integer, required)
* Central systems (array, required)

##### 4.1.2.2. Schema

```json
{
    "type": "object",
    "properties": {
        "code": {
            "type": "integer"
        },
        "centralsystems": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "string"
                    },
                    "endpoint": {
                        "type": "string"
                    }
                }
            } 
        }
    }
}
```

##### 4.1.2.3. Example

```json
{
    "code": 200,
    "centralsystems": [
        {
            "id": "yunqi",
            "endpoint": "1.2.3.4:9908"
        },
        {
            "id": "xuelang-centralsystem",
            "endpoint": "4.3.2.1:9908"
        }
    ]
}
```

### 4.2. Search Adapter Class

A central system searches for adapter classes. This operation happens in case of data system connection (refer to datanet growth protocol). When a data system connects to central system, it needs to select its adapter class so the central system needs to first know all the available adapter classes across all central systems.

#### 4.2.1. Request

##### 4.2.1.1. Parameters

* Protocol name (string, optional)
* Protocol version (string, optional)

##### 4.2.1.2. Schema

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
            }
        },
        "body": {
            "type": "object",
            "properties": {
                "protocolName": {
                    "type": "string"
                },
                "protocolVersion": {
                    "type": "string"
                }
            }
        }
    }
}
```

##### 4.2.1.3. Example

```json
{
    "header": {
        "version": "datanet 1.0"
    },
    "body": {
        "protocolName": "odps",
        "protocolVersion": "aspara 1.0"
    }
}
```

#### 4.2.2. Response

##### 4.2.2.1. Parameters

* Response code (integer, required)
* Adapter classes (array, required)

##### 4.2.2.2. Schema

```json
{
    "type": "object",
    "properties": {
        "code": {
            "type": "integer"
        },
        "adapterClasses": {
            "type": "array",
            "items": {
                "type": "string"
            }
        }
    }
}
```

##### 4.2.2.3. Example

```json
{
    "code": 200,
    "adapterClasses": ["odps-aspara1.0"]
}
```

