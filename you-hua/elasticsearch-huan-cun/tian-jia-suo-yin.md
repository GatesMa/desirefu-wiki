# 添加索引

为队伍和学生账号数据添加缓存在elasticsearch的索引结构：

#### 学生账号：

```javascript
PUT /desirefu
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 2
  },
  "mapping": {
    "normalaccount": {
      "properties": {
        "accountId": {
          "type": "long"
        },
        "accountType": {
          "type": "integer"
        },
        "collegeId": {
          "type": "integer"
        },
        "collegeName": {
          "type": "keyword"
        },
        "departmentId": {
          "type": "integer"
        },
        "departmentName": {
          "type": "keyword"
        },
        "major": {
          "type": "keyword"
        },
        "stuId": {
          "type": "keyword"
        },
        "realName": {
          "type": "keyword"
        },
        "createdTime": {
          "type": "keyword"
        }
      }
    }
  }
}
```

**队伍索引：**

```javascript
POST /desirefu/organize/_mapping
{
  "properties": {
    "organizeId": {
      "type": "long"
    },
    "competitionId": {
      "type": "long"
    },
    "competition": {
      "properties": {
        "competitionId": {
          "type": "long"
        },
        "accountId": {
          "type": "long"
        },
        "accountType": {
          "type": "integer"
        },
        "type": {
          "type": "integer"
        },
        "title": {
          "type": "keyword"
        },
        "founder": {
          "type": "keyword"
        },
        "content": {
          "type": "text"
        },
        "pv": {
          "type": "integer"
        },
        "status": {
          "type": "integer"
        },
        "beginTime": {
          "type": "keyword"
        },
        "endTime": {
          "type": "keyword"
        },
        "createdIme": {
          "type": "keyword"
        },
        "overviewImg": {
          "type": "keyword"
        },
        "overviewText": {
          "type": "text"
        }
      }
    },
    "srcAccountId": {
      "type": "long"
    },
    "nickName": {
      "type": "keyword"
    },
    "createdIme": {
      "type": "keyword"
    },
    "memberNum": {
      "type": "integer"
    }
  }
}

```

SpringBoot 链接elasticsearch报错：Caused by: java.lang.NoClassDefFoundError: org/apache/logging/log4j/Level，原因是elasticsearch的包需要使用log4j，需要引入下面的jar包：

```javascript
compile group: 'org.apache.logging.log4j', name: 'log4j-api', version: '2.7'
```







