# cool-admin 快速编写通用及自定义API接口
作者: gifted

QQ: 203118908

视频: https://www.bilibili.com/video/bv1VE411c7he

## 正文

1. 在`app\entity\test\test.ts`中填写数据库信息. (代码片段: entity)

2. yarn dev  (一定是先填写数据库信息, 再开启服务, 否则typings\typeorm.d.ts中没有test表信息)

3. 在`app\controller\test.ts`中新建控制器. (代码片段: controller)

4. postman中测试接口. (raw json)  

   ```json
   {
     "name":"小蓝",
     "age":15
   }
   ```

   

## 增删改查

- 增

  ```json
  {
      "name": "小东",
      "age": 18
  }
  ```

  

- 删

  ```json
  {
      "ids":[2]
  }
  ```

  

- 改

  ```json
  {
      "id":2
  }
  ```

  

- 查

  ```
  method: get
  url: http://localhost:7001/test/list
  ```

  

## 关键词查询

```json
http://localhost:7001/test/page?keyWord=二
```

