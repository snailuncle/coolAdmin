# cool-admin 前后端配合使用

作者: gifted

QQ: 203118908

视频: https://www.bilibili.com/video/BV1N7411w7dj?from=search&seid=11800671574855141738

(备注: 代码中某行高亮使用一对单个反引号)

## 后端



1. 编写表信息, 文件路径`app\entity\app\bannertest.ts`

   文件内容

   ```typescript
   import { Entity, Column } from 'typeorm';
   import { BaseEntity } from 'egg-cool-entity';
   
   /**
   * 数据模型描述
   */
   @Entity({ name: 'app_banner_test' })
   export default class AppBannerTest extends BaseEntity {
     // 标题
     @Column({length:20})
     title: string;
     // 链接
     @Column({length:20, nullable:true})
     link: string;
     // 图片
     @Column({length:20})
     pics: string;
   }
   ```

   

2. 编写控制器, 文件路径`app\controller\admin\app\bannertest.ts`

   文件内容: 

   ```typescript
   import { BaseController } from 'egg-cool-controller';
   import router from 'egg-cool-router';
   
   /**
   * 控制器功能描述
   */
   @router.prefix('/admin/app/bannertest', ['add', 'delete', 'update', 'info', 'list', 'page'])
   export default class AppBannerTestController extends BaseController {
     init() {
       this.setEntity(this.ctx.repo.app.Bannertest);
     }
   }
   ```

   

## 前端

1. 编写测试页面, 文件路径 `app\bannertest\bannertest.vue`, 代码片段`cl-curd`

2. 在测试页面的`onLoad`方法中, 设置服务

   ```typescript
   	methods: {
   		onLoad({ ctx, app }) {
   			this.crud = app;
   
   			`ctx.service(this.$service.app.bannertest)`
   			
   				.set('upsert', {
   					items: [
   						{
   							prop: '',
   							label: '',
   							component: {
   								name: ''
   							}
   						}
   					]
   				})
   				.set('table', {
   					columns: [
   						{
   							label: '',
   							prop: '',
   							align: ''
   						}
   					]
   				})
   				.done();
   	
   			app.refresh();
   		}
   	}
   ```

3. 编写服务, 文件路径: `src\service\app\bannertest.js`

   ```typescript
   import { BaseService, Service, Permission } from '@/cool';
   
   @Service('app/bannertest')
   export default class extends BaseService {}
   
   ```

4. 在`http://localhost:9000/sys/menu`中添加测试菜单, 

   名称: `测试轮播`

   节点路由: `/app/bannertest`

   文件路径: `views/app/bannertest/bannertest.vue`

5. 给该测试菜单添加权限, `app/bannertest`

   增: `add`

   删: `delete`

   改: `update info`

   查: `page info`

6. 查看页面效果`http://localhost:9000/app/bannertest`

7. 开始写组件, 主要是表单类组件: table和form, 

    饿了吗`elementUI`, 网址: `https://element.eleme.cn/#/zh-CN/component/installation`

8. 添加列名后, 查看效果`http://localhost:9000/app/bannertest`

   ```typescript
   				.set('table', {
   					columns: [
   						{
   							label: '图片',
   							prop: 'pics',
   							align: 'center'
   						},
   						{
   							label: '标题',
   							prop: 'title',
   							align: 'center'
   						},
   						{
   							label: '链接',
   							prop: 'link',
   							align: 'center'
   						}
   					]
   				})
   ```

9. 添加按钮的表单组件

   表单中包含三个组件: 图片, 标题, 链接

   ```typescript
   			ctx.service(this.$service.app.bannertest)
   				.set('upsert', {
   					items: [
   						{
   							prop: 'pics',
   							label: '图片',
   							component: {
   								name: 'cl-upload'
   							}
   						},
   						{
   							prop: 'title',
   							label: '标题',
   							component: {
   								name: 'el-input'
   							}
   						},
   						{
   							prop: 'link',
   							label: '链接',
   							component: {
   								name: 'el-input'
   							}
   						}
   					]
   				})
   ```

   

10. 图片列现在是文字, 改为使用插槽, 插槽中是cl自定义组件`<cl-viewer>`, 该组件所在文件: `src\cool\components\viewer\index.vue`

    然后点击刷新按钮,  是***浏览器的刷新按钮***
    
    
    
    ```vue
    <template>
    	<cl-crud @load="onLoad"> 
            <!-- 插槽内容 -->
    		<template #table-column-pics="{scope}">
    			<cl-viewer :urls="[scope.row.pics]" :current="scope.row.pics"></cl-viewer>
    		</template>
    	</cl-crud>
    </template>
    
    <script>
    export default {
    	data() {
    		return {
    			crud: null
    		};
    	},
    
    	methods: {
    		onLoad({ ctx, app }) {
    			this.crud = app;
    	
    			ctx.service(this.$service.app.bannertest)
    				.set('upsert', {
    					items: [
    						{
    							prop: 'pics',
    							label: '图片',
    							component: {
    								name: 'cl-upload'
    							}
    						},
    						{
    							prop: 'title',
    							label: '标题',
    							component: {
    								name: 'el-input'
    							}
    						},
    						{
    							prop: 'link',
    							label: '链接',
    							component: {
    								name: 'el-input'
    							}
    						}
    					]
    				})
    				.set('table', {
    					columns: [
    						{
    							label: '图片',
    							prop: 'pics',
    							align: 'center',
                                // 使用插槽
    							slot: true
    						},
    						{
    							label: '标题',
    							prop: 'title',
    							align: 'center'
    						},
    						{
    							label: '链接',
    							prop: 'link',
    							align: 'center'
    						}
    					]
    				})
    				.done();
    	
    			app.refresh();
    		}
    	}
};
    </script>
    
    <style lang="stylus" scoped></style>
    
    ```
    
11. 图片太大了, 给图片设定高度 height

```vue
    <cl-viewer height="80px" :urls="[scope.row.pics]" :current="scope.row.pics"></cl-viewer>
```

12. 图片缩放, 阿里云文档

     ```
     https://help.aliyun.com/document_detail/44688.html?spm=a2c4g.11174283.6.1367.727d7da2H8ebDp
     ```

13. 图片预览的时候是小图片, 点击后才是大图片

     ```vue
     <cl-viewer
     	height="80px"
     	:urls="[scope.row.pics]"
     	:current="scope.row.pics + '?x-oss-process=image/resize,h_100'"
     ></cl-viewer>
     ```

14. 表单校验, 请关注字段 `rules`

     ```vue
     				.set('upsert', {
     					items: [
     						{
     							prop: 'pics',
     							label: '图片',
     							component: {
     								name: 'cl-upload'
     							},
     							rules: [{ required: true, message: '请上传图片', trigger: 'change' }]
     						},
     						{
     							prop: 'title',
     							label: '标题',
     							component: {
     								name: 'el-input'
     							},
     							rules: [
     								{ required: true, message: '请输入标题', trigger: 'blur' },
     								{
     									min: 3,
     									max: 20,
     									message: '长度在 3 到 20 个字符',
     									trigger: 'blur'
     								}
     							]
     						},
     						{
     							prop: 'link',
     							label: '链接',
     							component: {
     								name: 'el-input'
     							},
     							rules: [
     								{
     									pattern: '[a-zA-Z]+://[^s]*',
     									message: '请输入正确的链接地址',
     									trigger: 'blur'
     								}
     							]
     						}
     					]
     				})
     ```

     
    
15. 设置关键词搜索, 这是个**后端**的内容

    文件路径: `app\controller\admin\app\bannertest.ts`

    ```typescript
    import { BaseController } from 'egg-cool-controller';
    import router from 'egg-cool-router';
    
    /**
    * 控制器功能描述
    */
    @router.prefix('/admin/app/bannertest', ['add', 'delete', 'update', 'info', 'list', 'page'])
    export default class AppBannerTestController extends BaseController {
      init() {
        this.setEntity(this.ctx.repo.app.Bannertest);
        this.setPageOption({
          keyWordLikeFields:['title']
        })
      }
    }
    ```

    
