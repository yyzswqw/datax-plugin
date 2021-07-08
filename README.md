# 历史数据同步DataX插件说明

## 快速介绍

```sahistoryreader```是用于将hive中的数据通过jdbc方式导出的插件，可以使用两种方式之一，第一种是使用row_number()函数方式通过分页查询数据，第二种是使用时间字段条件过滤查询数据。

```sahistorywriter```是用户将数据导入到神策分析的插件。

## **实现原理**

```sahistoryreader```是通过hive的row_number()函数方式实现分页，或者时间条件过滤数据。

```sahistorywriter	```是通过神策分析java SDK将数据生成符合神策分析的数据格式。

## 配置说明

### 使用row_number()函数方式

以下配置仅row_number()方式全部配置参数

```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "name": "sahistoryreader",
                    "parameter": {
                        "column": [
                            "name","age","id","update_date","date_str"
                        ],
                        "pageSize": 15,
                        "password": "",
                        "receivePageSize": 10,
                        "sa": {
                            "hiveUrl": "jdbc:hive2://10.120.232.3:10000/test",
                            "table": "apple"
                        },
                        "useRowNumber": true,
                        "username": "",
                        "where": "age > 18"
                    }
                },
                "writer": {
                    "name": "sahistorywriter",
                    "parameter": {
                        "item": {
                            "itemIdColumn": "id1",
                            "itemType": "couse",
                            "typeIsColumn": false
                        },
                        "saColumn": [
                            {
                                "index":0,
                                "targetColumnName": "name1"
                            },
                            {
                                "index":1,
                                "targetColumnName": "age1",
                                "dataConverters":[
                                    {
                                        "type": "Number2Str"
                                    }
                                ]
                            },
                            {
                                "index":2,
                                "targetColumnName": "id1",
                                "dataConverters":[
                                    {
                                        "type": "BigInt2Date"
                                    }
                                ]
                            },
                            {
                                "index":2,
                                "targetColumnName": "id2",
                                "dataConverters":[
                                    {
                                        "type": "BigInt2Date"
                                    },
                                    {
                                        "type": "Date2Str",
                                        "param": {
                                            "pattern":"yyyy-MM"
                                        }
                                    }
                                ]
                            },
                            {
                                "index":3,
                                "targetColumnName": "update_date1",
                                "dataConverters":[
                                    {
                                        "type": "IfNull2Default",
                                        "param": {
                                            "default": "2021-07-01",
                                            "dataConverters": [
                                                {
                                                    "type": "Str2Date",
                                                    "param": {
                                                        "pattern":"yyyy-MM-dd"
                                                    }
                                                },
                                                {
                                                    "type": "Date2Long"
                                                }
                                            ]
                                        }
                                    }
                                ]
                            },
                            {
                                "index":4,
                                "targetColumnName": "date_str1",
                                "dataConverters":[
                                    {
                                        "type": "IfNull2Default",
                                        "param": {
                                            "default":"20210801",
                                            "dataConverters": [
                                                {
                                                    "type": "IfElse",
                                                    "param": {
                                                        "if":"if(value=='20210801'){return true;}else{return false;}",
                                                        "value": "return a;",
                                                        "else": "return 4321;",
                                                        "sharedPool": "var a = 10;"
                                                    }
                                                }
                                            ]
                                        }
                                    }
                                ]
                            }
                        ],
                        "sdkDataAddress": "/datax/datax/logag/log",
                        "track": {
                            "distinctIdColumn": "id1",
                            "eventName": "testEventName",
                            "isLoginId": true
                        },
                        "type": "item",
                        "user": {
                            "distinctIdColumn": "id1",
                            "isLoginId": true
                        }
                    }
                }
            }
        ],
        "setting": {
            "speed": {
                "channel": "1"
            }
        }
    }
}
```

### 使用时间字段条件过滤方式

以下配置仅时间字段条件过滤方式全部配置参数

```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "name": "sahistoryreader",
                    "parameter": {
                        "column": [
                            "name","age","id","update_date","date_str"
                        ],
                        "datePattern": "yyyy-MM-dd",
                        "endTime": "2021-06-24",
                        "password": "",
                        "sa": {
                            "hiveUrl": "jdbc:hive2://10.120.232.3:10000/test",
                            "table": "apple"
                        },
                        "startTime": "2021-06-14",
                        "taskNum": 5,
                        "timeFieldName": "update_date",
                        "timeInterval": 1000,
                        "useRowNumber": false,
                        "username": ""
                    }
                },
                "writer": {
                    "name": "sahistorywriter",
                    "parameter": {
                        "item": {
                            "itemIdColumn": "id1",
                            "itemType": "couse",
                            "typeIsColumn": false
                        },
                        "saColumn": [
                            {
                                "index":0,
                                "targetColumnName": "name1"
                            },
                            {
                                "index":1,
                                "targetColumnName": "age1",
                                "dataConverters":[
                                    {
                                        "type": "Number2Str"
                                    }
                                ]
                            },
                            {
                                "index":2,
                                "targetColumnName": "id1",
                                "dataConverters":[
                                    {
                                        "type": "BigInt2Date"
                                    }
                                ]
                            },
                            {
                                "index":2,
                                "targetColumnName": "id2",
                                "dataConverters":[
                                    {
                                        "type": "BigInt2Date"
                                    },
                                    {
                                        "type": "Date2Str",
                                        "param": {
                                            "pattern":"yyyy-MM"
                                        }
                                    }
                                ]
                            },
                            {
                                "index":3,
                                "targetColumnName": "update_date1",
                                "dataConverters":[
                                    {
                                        "type": "IfNull2Default",
                                        "param": {
                                            "default": "2021-07-01",
                                            "dataConverters": [
                                                {
                                                    "type": "Str2Date",
                                                    "param": {
                                                        "pattern":"yyyy-MM-dd"
                                                    }
                                                },
                                                {
                                                    "type": "Date2Long"
                                                }
                                            ]
                                        }
                                    }
                                ]
                            },
                            {
                                "index":4,
                                "targetColumnName": "date_str1",
                                "dataConverters":[
                                    {
                                        "type": "IfNull2Default",
                                        "param": {
                                            "default":"20210801",
                                            "dataConverters": [
                                                {
                                                    "type": "IfElse",
                                                    "param": {
                                                        "if":"if(value=='20210801'){return true;}else{return false;}",
                                                        "value": "return a;",
                                                        "else": "return 4321;",
                                                        "sharedPool": "var a = 10;"
                                                    }
                                                }
                                            ]
                                        }
                                    }
                                ]
                            }
                        ],
                        "sdkDataAddress": "/datax/datax/logag/log",
                        "track": {
                            "distinctIdColumn": "id1",
                            "eventName": "testEventName",
                            "isLoginId": true
                        },
                        "type": "item",
                        "user": {
                            "distinctIdColumn": "id1",
                            "isLoginId": true
                        }
                    }
                }
            }
        ],
        "setting": {
            "speed": {
                "channel": "1"
            }
        }
    }
}
```

## **参数说明**

### ``read``

​		`column`：hive表中需要查询的字段名列表

​		`pageSize`：使用row_number()方式时，分页的大小，默认值10000。

​		`password`：连接hive的密码。

​		`receivePageSize`：使用row_number()方式时，每个task负责的页数，默认值5。

​		`sa.hiveUrl`：hive连接的url。

​		`sa.table`：hive要查询的表。

​		`useRowNumber`：是否使用row_number()方式，false或为空时，使用时间字段条件过滤方式。

​		`username`：连接hive的用户名。

​		`where`：使用row_number()方式时的查询条件。

​		`datePattern`：使用时间字段条件过滤方式时，时间格式。

​		`endTime`：使用时间字段条件过滤方式时，条件结束时间（不包含）。

​		`startTime`：使用时间字段条件过滤方式时，条件开始时间。

​		`taskNum`：使用时间字段条件过滤方式时，task数量，默认值为dataX框架提供的值。

​		`timeFieldName`：使用时间字段条件过滤方式时，使用的时间条件字段名。

​		`timeInterval`：使用时间字段条件过滤方式时，时间段通过```taskNum```分片后，如果数据量还是过大时可指定每次查多久的，默认值为查询一天的量，单位毫秒。

### `writer`

​		`sdkDataAddress`：数据存放路径，神策分析将接收到的数据经过转换后将数据存放的路径。

​		`type`：导入神策分析的数据类型，可取值有：track/user/item，分别对应神策的事件/用户/属性。

​		`saColumn`：导入神策分析的属性列表。

​		`saColumn.index`：该列使用读插件字段的下标索引，从0开始。

​		`saColumn.targetColumnName`：导入神策分析的属性名称。

​		`saColumn.dataConverters`：将dataX读出的数据转换为其他类型或者值，所使用的数据转换器。插件支持的数据转换器见下文``内置数据转换器``，支持多个联合使用。

​		`saColumn.dataConverters.type`：使用内置转换器的名称，见下文``内置数据转换器`` ``转换器type``列。

​		`saColumn.dataConverters.param`：使用内置转换器时，转换器必要的参数列表，参数key根据不同转换器不同而不同，见下文``内置数据转换器参数说明``。

​		`track.distinctIdColumn`：type为track时，作为神策distinctId的列，该属性应该在saColumn列表中，并且该属性不能存在空值。

​		`track.eventName`：type为track时，导入神策分析的事件名，当该列值为动态的在```saColumn```中时，可将```saColumn```中的列名改为```eventEventName```（该列不能存在空值），即可达到动态的效果。

​		`track.isLoginId`：type为track时，作为神策distinctId的列是否是登录ID，即用户的唯一标识，布尔值，当该列值为动态的在```saColumn```中时，可将```saColumn```中的列名改为```eventIsLoginId```（该列不能存在空值），即可达到动态的效果。

​		`user.distinctIdColumn`：type为user时，作为神策distinctId的列，该属性应该在saColumn列表中，并且该属性不能存在空值。

​		`user.isLoginId`：type为user时，作为神策distinctId的列是否是登录ID，即用户的唯一标识，布尔值，当该列值为动态的在```saColumn```中时，可将```saColumn```中的列名改为```userIsLoginId```（该列不能存在空值），即可达到动态的效果。

​		`item.itemIdColumn`：type为item时，作为神策itemId的列，该属性应该在saColumn列表中，并且该属性不能存在空值。

​		`item.itemType`：type为item时，作为神策itemType的列，如该配置项的值在saColumn列表中，该属性不能存在空值并且`item.typeIsColumn`配置项应该为`true`，否则将以常量值作为神策itemType。

​		`item.typeIsColumn`：type为item时，`item.itemType`配置项是否在`saColumn`配置项的列表中。

## **类型转换**

###  读插件

|                             java                             |    dataX     |    dataX实际类型     |
| :----------------------------------------------------------: | :----------: | :------------------: |
|                             null                             | StringColumn |   java.lang.String   |
|                       java.lang.String                       | StringColumn |   java.lang.String   |
|                  boolean/java.long.Boolean                   |  BoolColumn  |  java.lang.Boolean   |
| byte/java.long.Byte/short/java.long.Short/int/java.long.Integer/long/java.long.Long |  LongColumn  | java.math.BigInteger |
|        float/java.long.Float/double/java.long.Double         | DoubleColumn |   java.lang.String   |
|                        java.util.Date                        |  DateColumn  |    java.util.Date    |
|                     java.time.LocalDate                      |  DateColumn  |    java.util.Date    |
|                   java.time.LocalDateTime                    |  DateColumn  |    java.util.Date    |
|                        java.sql.Date                         |  DateColumn  |    java.util.Date    |
|                      java.sql.Timestamp                      |  DateColumn  |    java.util.Date    |

### 写插件

|    dataX     |       插件类型       |
| :----------: | :------------------: |
| StringColumn |   java.lang.String   |
|  BoolColumn  |  java.long.Boolean   |
|  LongColumn  | java.math.BigInteger |
| DoubleColumn | java.math.BigDecimal |
|  DateColumn  |    java.util.Date    |
|     null     |         丢弃         |

## 内置数据转换器

|   转换器type   |        转换器全称        |                             功能                             |
| :------------: | :----------------------: | :----------------------------------------------------------: |
|   Long2Date    |    Long2DateConverter    |                  将long转换为java.util.Date                  |
|    Date2Str    |    Date2StrConverter     |            将java.util.Date转换为java.long.String            |
|   Date2Long    |    Date2LongConverter    |                  将java.util.Date转换为long                  |
|   Number2Str   |   Number2StrConverter    |                将数值型转换为java.long.String                |
|    Str2Long    |    Str2LongConverter     |                 将java.long.String转换为long                 |
|    Str2Date    |    Str2DateConverter     |            将java.long.String转换为java.util.Date            |
|  BigInt2Date   | BigInteger2DateConverter |          将java.math.BigInteger转换为java.util.Date          |
|    Str2Int     |     Str2IntConverter     |          将java.long.String转换为java.long.Integer           |
|   Str2Double   |   Str2DoubleConverter    |           将java.long.String转换为java.long.Double           |
| Str2BigDecimal | Str2BigDecimalConverter  |         将java.long.String转换为java.math.BigDecimal         |
| IfNull2Default | IfNull2DefaultConverter  | 将null或者空串的值转换为给定的默认值，支持默认值再转换为其他类型 |
|  NotNull2Null  |  NotNull2NullConverter   |                   将不为null的值转换为null                   |
|     IfElse     |     IfElseConverter      | if表达式条件成立返回特定表达式值，否则返回else表达式值，使用JavaScript引擎解析 |



## 内置数据转换器参数说明

### Long2Date

​	无参数要求

#### 示例

```json
"dataConverters":[
    {
        "type": "BigInt2Date"
    }
]
```

### Date2Str

```pattern```：转换为字符串的时间格式

#### 示例

```json
"dataConverters":[
    {
        "type": "Date2Str",
        "param": {
            "pattern":"yyyy-MM-dd"
        }
    }
]
```



### Date2Long

无参数要求

#### 示例

```json
"dataConverters":[
    {
        "type": "Date2Long"
    }
]
```

### Number2Str

无参数要求

#### 示例

```json
"dataConverters":[
    {
        "type": "Number2Str"
    }
]
```

### Str2Long

无参数要求

#### 示例

```json
"dataConverters":[
    {
        "type": "Str2Long"
    }
]
```

### Str2Date

```pattern```：字符串的时间格式，非必须，内置了时间格式：yyyy-MM-dd、yyyy-MM-dd HH:mm:ss、yyyy-MM-dd HH:mm:ss.SSS、yyyy-MM、yyyyMM、yyyyMMdd、yyyyMMddHHmmss、yyyyMMddHHmmssSSS

```formats```：其他格式的字符串时间格式数组（数据来源的时间格式可能存在多种，可以弥补pattern的不足）

#### 示例

```json
"dataConverters":[
    {
        "type": "Str2Date",
        "param": {
            "pattern":"yyyy-MM-dd",
            "formats":["yyyyMMdd","yyyyMMddHHmmss"]
        }
    }
]
```

### BigInt2Date

无参数要求

#### 示例

```json
"dataConverters":[    {        "type": "BigInt2Date"    }]
```

### Str2Int

无参数要求

#### 示例

```json
"dataConverters":[    {        "type": "Str2Int"    }]
```

### Str2Double

无参数要求

#### 示例

```json
"dataConverters":[    {        "type": "Str2Double"    }]
```

### Str2BigDecimal

无参数要求

#### 示例

```json
"dataConverters":[    {        "type": "Str2BigDecimal"    }]
```

### IfNull2Default

```default```：默认值

```dataConverters```：数据转换器，内置的任意的数据转换器，将```default```参数给定的默认值转换为其他类型

```dataConverters.type```：数据转换器的类型

```dataConverters.param```：数据转换器的参数

#### 示例

如果该列的值为null或者空字符串，那么设置该值为字符串类型的`2021-07-01`，并且将该字符串转换为`yyyy-MM-dd`格式的时间，再将该时间转换为long型的毫秒值。

```json
"dataConverters":[    {        "type": "IfNull2Default",        "param": {            "default": "2021-07-01",            "dataConverters": [                {                    "type": "Str2Date",                    "param": {                        "pattern":"yyyy-MM-dd"                    }                },                {                    "type": "Date2Long"                }            ]        }    }]
```

### NotNull2Null

无参数要求

#### 示例

```json
"dataConverters":[    {        "type": "NotNull2Null"    }]
```

### IfElse

```if```：if条件表达式，使用JavaScript引擎解析，确保返回值为boolean类型，导入神策分析的当前列名，和当前读插件获取到的value值，以及dataConverters下配置的param参数会传递到```if```表达式中,可通过`targetColumnName`、`value`、`param`分别获取对应的值

```value```：if条件表达式成立时，返回转换后的值，使用JavaScript引擎解析，导入神策分析的当前列名，和当前读插件获取到的value值，以及dataConverters下配置的param参数会传递到```value```表达式中,可通过`targetColumnName`、`value`、`param`分别获取对应的值

```else```：if条件表达式不成立时，返回转换后的值，使用JavaScript引擎解析，导入神策分析的当前列名，和当前读插件获取到的value值，以及dataConverters下配置的param参数会传递到```else```表达式中,可通过`targetColumnName`、`value`、`param`分别获取对应的值

```sharedPool```：共享区，使用JavaScript引擎解析，该值定义的变量或常量在```if```、```value```、```else```中都能使用

#### 示例

```json
"dataConverters": [    {        "type": "IfElse",        "param": {            "if":"if(value=='20210801'){return true;}else{return false;}",            "value": "return a;",            "else": "return 4321;",            "sharedPool": "var a = 10;"        }    }]
```



