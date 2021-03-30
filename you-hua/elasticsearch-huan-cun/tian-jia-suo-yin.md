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











