---
layout: post
title: pymongo修改mongodb数据key的名字
---

官方api:<http://docs.mongodb.org/manual/reference/operator/update/rename/>

如果更新rename所有数据的key, 可以使用下面的代码, 比如 将a重命名为b

    def rename_key():
        for item in collection.find():
            collection.update({'_id': item['_id']}, {'$rename': {'a': 'b'}})

