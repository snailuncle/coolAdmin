# cool-admin 前端crud教程

作者: gifted

QQ: 203118908

## 正文

1. 创建一个vue测试页面 

   文件路径: `src\views\test\curd-test.vue`

   代码片段: `cl-crud`

   @load ({ ctx, app }): `ctx 配置参数、事件` `app 处理参数、事件`

2. 进入后台  `http://localhost:9000/` , 在`系统/权限管理/菜单列表`, 添加我们的测试页面

   1. 节点名称: `前端crud测试`
   2. 节点路由: `/crud-test`
   3. 文件路径: `views/test/crud-test.vue`
   4. 保存  

3. 打开`http://localhost:9000/crud-test`, 查看效果

4. 打开饿了么elementUI  `https://element.eleme.cn/#/zh-CN/component/table`

5. 设置几个字段, 此时`src\views\test\crud-test.vue`文件内容如下

   ```vue
   <template>
   	<cl-crud @load="onLoad"></cl-crud>
   </template>
   
   <script>
   export default {
   	data() {
   		return {
   			crud: null
   		};
   	},
   
   	methods: {
   		onLoad({ ctx }) {
   			ctx.set('table', {
   				columns: [
   					{
   						prop: 'name',
   						label: '名字',
   						width: 200,
   						align: 'center'
   					},
   					{
   						prop: 'date',
   						label: '日期',
   						align: 'left'
   					}
   				]
   			}).done();
   		}
   	}
   };
   </script>
   
   <style lang="stylus" scoped></style>
   
   ```

6. 增加序号和多选框, 也就是`index`和`selection`

   ```vue
   			ctx.set('table', {
   				columns: [
   					{
   						type: 'index',
   						width: '60'
   					},
   					{
   						type: 'selection',
   						width: '60'
   					},
   					{
   						prop: 'name',
   						label: '名字',
   						width: 200,
   						align: 'center'
   					},
   					{
   						prop: 'date',
   						label: '日期',
   						align: 'left'
   					}
   				]
   			}).done();
   ```

7. 数据操作, 主要由service控制, 

   添加一些测试数据, 并且加上`app.refresh();`

   数据在`ctx.service page()`中

   ```vue
   <template>
   	<cl-crud @load="onLoad"></cl-crud>
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
   			ctx.service({
   				page() {
   					return Promise.resolve({
   						list: [
   							{
   								id: 1,
   								name: '张三',
   								date: '20200323'
   							},
   							{
   								id: 2,
   								name: '李四',
   								date: '20200324'
   							}
   						],
   						pagination: {
   							page: 1,
   							total: 2,
   							size: 20
   						}
   					});
   				}
   			})
   
   				.set('table', {
   					columns: [
   						{
   							type: 'index',
   							width: '60'
   						},
   						{
   							type: 'selection',
   							width: '60'
   						},
   						{
   							prop: 'name',
   							label: '名字',
   							width: 200,
   							align: 'center'
   						},
   						{
   							prop: 'date',
   							label: '日期',
   							align: 'left'
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

8. 使用服务service: 

   `http://localhost:9000/crud-test` 中会显示系统用户

   

   ```vue
   <template>
   	<cl-crud @load="onLoad"></cl-crud>
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
   			ctx.service(this.$service.system.user)
   
   				.set('table', {
   					columns: [
   						{
   							type: 'index',
   							width: '60'
   						},
   						{
   							type: 'selection',
   							width: '60'
   						},
   						{
   							prop: 'name',
   							label: '名字',
   							width: 200,
   							align: 'center'
   						},
   						{
   							prop: 'createTime',
   							label: '创建时间',
   							align: 'left'
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

9. options, 在table后面加一个op字段

   option默认显示`编辑` `删除` 两个选项

   ```vue
   <template>
   	<cl-crud @load="onLoad"></cl-crud>
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
   			ctx.service(this.$service.system.user)
   
   				.set('table', {
   					columns: [
   						{
   							type: 'index',
   							width: '60'
   						},
   						{
   							type: 'selection',
   							width: '60'
   						},
   						{
   							prop: 'name',
   							label: '名字',
   							width: 200,
   							align: 'center'
   						},
   						{
   							prop: 'createTime',
   							label: '创建时间',
   							align: 'left'
   						}
   					],
   					op: { visible: true }
   				})
   				.done();
   
   			app.refresh();
   		}
   	}
   };
   </script>
   
   <style lang="stylus" scoped></style>
   
   ```

10. 编辑table的option字段, 比如只显示一个编辑选项

    ```
    		op: { visible: true, layout: ['edit'] }
    ```

11. options使用插槽slot, 插槽在`<template>` 中定义

    ```vue
    <template>
    	<cl-crud @load="onLoad">
    		<template #slot-btn>
    			<el-button size="mini" type="text">自定义按钮</el-button>
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
    			ctx.service(this.$service.system.user)
    
    				.set('table', {
    					columns: [
    						{
    							type: 'index',
    							width: '60'
    						},
    						{
    							type: 'selection',
    							width: '60'
    						},
    						{
    							prop: 'name',
    							label: '名字',
    							width: 200,
    							align: 'center'
    						},
    						{
    							prop: 'createTime',
    							label: '创建时间',
    							align: 'left'
    						}
    					],
    					op: { visible: true, layout: ['edit', 'delete', 'slot-btn'] }
    				})
    				.done();
    
    			app.refresh();
    		}
    	}
    };
    </script>
    
    <style lang="stylus" scoped></style>
    
    ```

    

12. 修改 options 的宽度

    ```vue
    					op: {
    						visible: true,
    						layout: ['edit', 'delete', 'slot-btn'],
    						props: { width: '300px' }
    					}
    ```

13. 调换 options 的顺序

    ```vue
    					op: {
    						visible: true,
    						layout: ['delete', 'edit', 'slot-btn'],
    						props: { width: '300px' }
    					}
    ```

14. 令插槽按钮不显示

    ```vue
    <template>
    	<cl-crud @load="onLoad">
    		<template #slot-btn>
    			<el-button size="mini" type="text" v-permission="false">自定义按钮</el-button>
    		</template>
    	</cl-crud>
    </template>
    ```

15. 配置service中的权限

    ```vue
    <template>
    	<cl-crud @load="onLoad">
    		<template #slot-btn>
    			<el-button size="mini" type="text" v-permission="$service.system.user.permission.move"
    				>自定义按钮</el-button
    			>
    		</template>
    	</cl-crud>
    </template>
    ```

16. 顶部按钮修改

    在ctx.service中加了一个set layout, 这个就是配置顶部按钮的

    比如: `'refresh-btn'` `'add-btn'` , 分别对应 刷新 和添加  按钮,

    如果删掉这两个, 那么对应的按钮也就消失了

     `flex1'` 是填充空白, 防止两个按钮太挤

    ```vue
    <template>
    	<cl-crud @load="onLoad">
    		<template #slot-btn>
    			<el-button size="mini" type="text" v-permission="$service.system.user.permission.move"
    				>自定义按钮</el-button
    			>
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
    			ctx.service(this.$service.system.user)
    
    				.set('table', {
    					columns: [
    						{
    							type: 'index',
    							width: '60'
    						},
    						{
    							type: 'selection',
    							width: '60'
    						},
    						{
    							prop: 'name',
    							label: '名字',
    							width: 200,
    							align: 'center'
    						},
    						{
    							prop: 'createTime',
    							label: '创建时间',
    							align: 'left'
    						}
    					],
    					op: {
    						visible: true,
    						layout: ['delete', 'edit', 'slot-btn'],
    						props: { width: '300px' }
    					}
    				})
    				.set('layout', [
    					[
    						'refresh-btn',
    						'add-btn',
    						'multi-delete-btn',
    						'query',
    						'flex1',
    						'search-key',
    						'adv-btn'
    					],
    					['data-table'],
    					['flex1', 'pagination']
    				])
    				.done();
    
    			app.refresh();
    		}
    	}
    };
    </script>
    
    <style lang="stylus" scoped></style>
    
    ```

17. 顶部按钮添加, 同样是使用插槽 `slot-btn`

    要以 `slot-` 开头

    ```vue
    					[
    						'refresh-btn',
    						'add-btn',
    						'multi-delete-btn',
    						'query',
    						'slot-btn',
    						'flex1',
    						'search-key',
    						'adv-btn'
    					],
    ```

18. 动态控制列

    1. 在插槽中添加一个click属性, 值是onTest

    2. 在methods中定义这个onTest

    3. 在data中添加一个canShowName, 用来控制是否显示name字段

    4. 在name这个字段中, 添加一个hidden属性

    5. 打开 `http://localhost:9000/crud-test`

    6. 点击自定义按钮, 名字就会隐藏或者显示

    7. 注意格式 `this.crud.setData('table.columns[prop:name].hidden', this.canShowName);`

       这个表示设置`table/columns/name/hidden` 的属性

    ```vue
    <template>
    	<cl-crud @load="onLoad">
    		<template #slot-btn>
    			<el-button
    				size="mini"
    				type="text"
    				@click="onTest"
    				v-permission="$service.system.user.permission.move"
    				>自定义按钮</el-button
    			>
    		</template>
    	</cl-crud>
    </template>
    
    <script>
    export default {
    	data() {
    		return {
    			crud: null,
    			canShowName: false
    		};
    	},
    
    	methods: {
    		onLoad({ ctx, app }) {
    			this.crud = app;
    			ctx.service(this.$service.system.user)
    
    				.set('table', {
    					columns: [
    						{
    							type: 'index',
    							width: '60'
    						},
    						{
    							type: 'selection',
    							width: '60'
    						},
    						{
    							prop: 'name',
    							label: '名字',
    							width: 200,
    							align: 'center',
    							hidden: this.canShowName
    						},
    						{
    							prop: 'createTime',
    							label: '创建时间',
    							align: 'left'
    						}
    					],
    					op: {
    						visible: true,
    						layout: ['delete', 'edit', 'slot-btn'],
    						props: { width: '300px' }
    					}
    				})
    				.set('layout', [
    					[
    						'refresh-btn',
    						'add-btn',
    						'multi-delete-btn',
    						'query',
    						'slot-btn',
    						'flex1',
    						'search-key',
    						'adv-btn'
    					],
    					['data-table'],
    					['flex1', 'pagination']
    				])
    				.done();
    
    			app.refresh();
    		},
    		onTest() {
    			this.canShowName = !this.canShowName;
    			this.crud.setData('table.columns[prop:name].hidden', this.canShowName);
    		}
    	}
    };
    </script>
    
    <style lang="stylus" scoped></style>
    
    ```

19. 添加表单

    1. 添加了set upsert
    2. span控制宽度比值

```vue
<template>
	<cl-crud @load="onLoad">
		<template #slot-btn>
			<el-button
				size="mini"
				type="text"
				@click="onTest"
				v-permission="$service.system.user.permission.move"
				>自定义按钮</el-button
			>
		</template>
	</cl-crud>
</template>

<script>
export default {
	data() {
		return {
			crud: null,
			canShowName: false
		};
	},

	methods: {
		onLoad({ ctx, app }) {
			this.crud = app;
			ctx.service(this.$service.system.user)

				.set('table', {
					columns: [
						{
							type: 'index',
							width: '60'
						},
						{
							type: 'selection',
							width: '60'
						},
						{
							prop: 'name',
							label: '名字',
							width: 200,
							align: 'center',
							hidden: this.canShowName
						},
						{
							prop: 'createTime',
							label: '创建时间',
							align: 'left'
						}
					],
					op: {
						visible: true,
						layout: ['delete', 'edit', 'slot-btn'],
						props: { width: '300px' }
					}
				})
				.set('upsert', {
					props: {
						width: '700px'
					},
					items: [
						{
							label: '姓名',
							prop: 'name',
							span: 12,
							component: {
								name: 'el-input'
							}
						},
						{
							label: '创建时间',
							prop: 'createTime',
							span: 12,
							component: {
								name: 'el-date-picker'
							}
						}
					]
				})
				.set('layout', [
					[
						'refresh-btn',
						'add-btn',
						'multi-delete-btn',
						'query',
						'slot-btn',
						'flex1',
						'search-key',
						'adv-btn'
					],
					['data-table'],
					['flex1', 'pagination']
				])
				.done();

			app.refresh();
		},
		onTest() {
			this.canShowName = !this.canShowName;
			this.crud.setData('table.columns[prop:name].hidden', this.canShowName);
		}
	}
};
</script>

<style lang="stylus" scoped></style>

```

20. 数据校验

    饿了么element表单校验 `https://element.eleme.cn/#/zh-CN/component/form`

    主要是`rules` 字段

    ```vue
    						{
    							label: '姓名',
    							prop: 'name',
    							span: 12,
    							component: {
    								name: 'el-input'
    							},
    							rules: [
    								{ required: true, message: '请输入活动名称', trigger: 'blur' },
    								{ min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
    							]
    						},
    ```

21. 自定义表单

    1. 在div中新建一个el-dialog弹窗
2. 在data()中添加一个visible属性 值为false
    3. 在onTest中, 将visible设置为true
4. 点击自定义按钮, 就能看见自定义表单了
    
```vue
    <template>
	<div>
    		<cl-crud @load="onLoad">
    			<template #slot-btn>
    				<el-button
					size="mini"
    					type="text"
    					@click="onTest"
    					v-permission="$service.system.user.permission.move"
    					>自定义按钮</el-button
    				>
    			</template>
    		</cl-crud>
    
    		<el-dialog :visible.sync="visible">
    			<el-form>
    				<el-form-item label="请输入您的姓名">
    					<el-input></el-input>
    				</el-form-item>
    			</el-form>
    		</el-dialog>
    	</div>
    </template>
    
    <script>
    export default {
    	data() {
    		return {
    			crud: null,
    			canShowName: false,
    			visible: false
    		};
    	},
    
    	methods: {
    		onLoad({ ctx, app }) {
    			this.crud = app;
    			ctx.service(this.$service.system.user)
    
    				.set('table', {
    					columns: [
    						{
    							type: 'index',
    							width: '60'
    						},
    						{
    							type: 'selection',
    							width: '60'
    						},
    						{
    							prop: 'name',
    							label: '名字',
    							width: 200,
    							align: 'center',
    							hidden: this.canShowName
    						},
    						{
    							prop: 'createTime',
    							label: '创建时间',
    							align: 'left'
    						}
    					],
    					op: {
    						visible: true,
    						layout: ['delete', 'edit', 'slot-btn'],
    						props: { width: '300px' }
    					}
    				})
    				.set('upsert', {
    					props: {
    						width: '700px'
    					},
    					items: [
    						{
    							label: '姓名',
    							prop: 'name',
    							span: 12,
    							component: {
    								name: 'el-input'
    							},
    							rules: [
    								{ required: true, message: '请输入活动名称', trigger: 'blur' },
    								{ min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
    							]
    						},
    
    						{
    							label: '创建时间',
    							prop: 'createTime',
    							span: 12,
    							component: {
    								name: 'el-date-picker'
    							}
    						}
    					]
    				})
    				.set('layout', [
    					[
    						'refresh-btn',
    						'add-btn',
    						'multi-delete-btn',
    						'query',
    						'slot-btn',
    						'flex1',
    						'search-key',
    						'adv-btn'
    					],
    					['data-table'],
    					['flex1', 'pagination']
    				])
    				.done();
    
    			app.refresh();
    		},
    		onTest() {
    			// this.canShowName = !this.canShowName;
    			// this.crud.setData('table.columns[prop:name].hidden', this.canShowName);
    			this.visible = true;
    		}
    	}
    };
    </script>
    
    <style lang="stylus" scoped></style>
    
    ```
    
22. 图片上传

    1. 在 set upsert items 中, 添加一个头像对象

       ```vue
       						{
       							prop: 'avatar',
       							label: '头像',
       							component: {
       								name: 'cl-upload',
       								props: {
       									props: {
       										multiple: true,
       										'multiple-limit': 2
       									}
       								}
       							}
       						}
       ```


    这个步骤的完整代码如下: 

    ```vue
    <template>
    	<div>
    		<cl-crud @load="onLoad">
    			<template #slot-btn>
    				<el-button
    					size="mini"
    					type="text"
    					@click="onTest"
    					v-permission="$service.system.user.permission.move"
    				>自定义按钮</el-button>
    			</template>
    		</cl-crud>
    
    		<el-dialog :visible.sync="visible">
    			<el-form>
    				<el-form-item label="请输入您的姓名">
    					<el-input></el-input>
    				</el-form-item>
    			</el-form>
    		</el-dialog>
    	</div>
    </template>
    
    <script>
    export default {
    	data() {
    		return {
    			crud: null,
    			canShowName: false,
    			visible: false
    		};
    	},
    
    	methods: {
    		onLoad({ ctx, app }) {
    			this.crud = app;
    			ctx.service(this.$service.system.user)
    
    				.set('table', {
    					columns: [
    						{
    							type: 'index',
    							width: '60'
    						},
    						{
    							type: 'selection',
    							width: '60'
    						},
    						{
    							prop: 'name',
    							label: '名字',
    							width: 200,
    							align: 'center',
    							hidden: this.canShowName
    						},
    						{
    							prop: 'createTime',
    							label: '创建时间',
    							align: 'left'
    						}
    					],
    					op: {
    						visible: true,
    						layout: ['delete', 'edit', 'slot-btn'],
    						props: { width: '300px' }
    					}
    				})
    				.set('upsert', {
    					props: {
    						width: '700px'
    					},
    					items: [
    						{
    							label: '姓名',
    							prop: 'name',
    							span: 12,
    							component: {
    								name: 'el-input'
    							},
    							rules: [
    								{ required: true, message: '请输入活动名称', trigger: 'blur' },
    								{ min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
    							]
    						},
    						{
    							label: '创建时间',
    							prop: 'createTime',
    							span: 12,
    							component: {
    								name: 'el-date-picker'
    							}
    						},
    						{
    							prop: 'avatar',
    							label: '头像',
    							component: {
    								name: 'cl-upload',
    								props: {
    									props: {
    										multiple: true,
    										'multiple-limit': 2
    									}
    								}
    							}
    						}
    					]
    				})
    				.set('layout', [
    					[
    						'refresh-btn',
    						'add-btn',
    						'multi-delete-btn',
    						'query',
    						'slot-btn',
    						'flex1',
    						'search-key',
    						'adv-btn'
    					],
    					['data-table'],
    					['flex1', 'pagination']
    				])
    				.done();
    
    			app.refresh();
    		},
    		onTest() {
    			// this.canShowName = !this.canShowName;
    			// this.crud.setData('table.columns[prop:name].hidden', this.canShowName);
    			this.visible = true;
    		}
    	}
    };
    </script>
    
    <style lang="stylus" scoped></style>
    
    ```

23. 高级搜索

    1. 添加set adv
    2. adv和upsert写法一样
    3. 也可以使用插槽
    4. 在cl-curd中, 添加一个插槽slot-cs

    ```vue
    <template>
    	<div>
    		<cl-crud @load="onLoad">
    			<template #slot-btn>
    				<el-button
    					size="mini"
    					type="text"
    					@click="onTest"
    					v-permission="$service.system.user.permission.move"
    				>自定义按钮</el-button>
    			</template>
    			<template #slot-cs>
    				<el-input></el-input>
    			</template>
    		</cl-crud>
    
    		<el-dialog :visible.sync="visible">
    			<el-form>
    				<el-form-item label="姓名">
    					<el-input></el-input>
    				</el-form-item>
    			</el-form>
    		</el-dialog>
    	</div>
    </template>
    
    
    
    <script>
    export default {
    	data() {
    		return {
    			crud: null,
    			canShowName: false,
    			visible: false
    		};
    	},
    
    	methods: {
    		onLoad({ ctx, app }) {
    			this.crud = app;
    			ctx.service(this.$service.system.user)
    
    				.set('table', {
    					columns: [
    						{
    							type: 'index',
    							width: '60'
    						},
    						{
    							type: 'selection',
    							width: '60'
    						},
    						{
    							prop: 'name',
    							label: '名字',
    							width: 200,
    							align: 'center',
    							hidden: this.canShowName
    						},
    						{
    							prop: 'createTime',
    							label: '创建时间',
    							align: 'left'
    						}
    					],
    					op: {
    						visible: true,
    						layout: ['delete', 'edit', 'slot-btn'],
    						props: { width: '300px' }
    					}
    				})
    				.set('upsert', {
    					props: {
    						width: '700px'
    					},
    					items: [
    						{
    							label: '姓名',
    							prop: 'name',
    							span: 12,
    							component: {
    								name: 'el-input'
    							},
    							rules: [
    								{ required: true, message: '请输入活动名称', trigger: 'blur' },
    								{ min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
    							]
    						},
    						{
    							label: '创建时间',
    							prop: 'createTime',
    							span: 12,
    							component: {
    								name: 'el-date-picker'
    							}
    						},
    						{
    							prop: 'avatar',
    							label: '头像',
    							component: {
    								name: 'cl-upload',
    								props: {
    									props: {
    										multiple: true,
    										'multiple-limit': 2
    									}
    								}
    							}
    						}
    					]
    				})
    
    				.set('adv', {
    					items: [
    						{
    							label: '姓名',
    							prop: 'name',
    							span: 12,
    							component: {
    								name: 'el-input'
    							}
    						},
    						{
    							label: '创建时间',
    							prop: 'createTime',
    							span: 12,
    							component: {
    								name: 'el-date-picker'
    							}
    						},
    						{
    							label: '自定义插槽',
    							prop: 'cs',
    							span: 12,
    							component: {
    								name: 'slot-cs'
    							}
    						}
    					]
    				})
    				.set('layout', [
    					[
    						'refresh-btn',
    						'add-btn',
    						'multi-delete-btn',
    						'query',
    						'slot-btn',
    						'flex1',
    						'search-key',
    						'adv-btn'
    					],
    					['data-table'],
    					['flex1', 'pagination']
    				])
    				.done();
    
    			app.refresh();
    		},
    		onTest() {
    			// this.canShowName = !this.canShowName;
    			// this.crud.setData('table.columns[prop:name].hidden', this.canShowName);
    			this.visible = true;
    		}
    	}
    };
    </script>
    
    <style lang="stylus" scoped></style>
    
    ```

24. 设置字典, 也就是按钮的名字, 比如添加改为加加

    文档: `https://docs.cool-admin.com/#/front/crud?id=dict`

    ```vue
    <template>
    	<div>
    		<cl-crud @load="onLoad">
    			<template #slot-btn>
    				<el-button
    					size="mini"
    					type="text"
    					@click="onTest"
    					v-permission="$service.system.user.permission.move"
    				>自定义按钮</el-button>
    			</template>
    			<template #slot-cs>
    				<el-input></el-input>
    			</template>
    		</cl-crud>
    
    		<el-dialog :visible.sync="visible">
    			<el-form>
    				<el-form-item label="姓名">
    					<el-input></el-input>
    				</el-form-item>
    			</el-form>
    		</el-dialog>
    	</div>
    </template>
    
    
    
    <script>
    export default {
    	data() {
    		return {
    			crud: null,
    			canShowName: false,
    			visible: false
    		};
    	},
    
    	methods: {
    		onLoad({ ctx, app }) {
    			this.crud = app;
    			ctx.service(this.$service.system.user)
    				.set('dict', {
    					label: {
    						add: '加加',
    						delete: '减减',
    						update: '编编',
    						refresh: '刷刷',
    						advSearch: '高级搜素'
    					}
    				})
    				.set('table', {
    					columns: [
    						{
    							type: 'index',
    							width: '60'
    						},
    						{
    							type: 'selection',
    							width: '60'
    						},
    						{
    							prop: 'name',
    							label: '名字',
    							width: 200,
    							align: 'center',
    							hidden: this.canShowName
    						},
    						{
    							prop: 'createTime',
    							label: '创建时间',
    							align: 'left'
    						}
    					],
    					op: {
    						visible: true,
    						layout: ['delete', 'edit', 'slot-btn'],
    						props: { width: '300px' }
    					}
    				})
    				.set('upsert', {
    					props: {
    						width: '700px'
    					},
    					items: [
    						{
    							label: '姓名',
    							prop: 'name',
    							span: 12,
    							component: {
    								name: 'el-input'
    							},
    							rules: [
    								{ required: true, message: '请输入活动名称', trigger: 'blur' },
    								{ min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
    							]
    						},
    						{
    							label: '创建时间',
    							prop: 'createTime',
    							span: 12,
    							component: {
    								name: 'el-date-picker'
    							}
    						},
    						{
    							prop: 'avatar',
    							label: '头像',
    							component: {
    								name: 'cl-upload',
    								props: {
    									props: {
    										multiple: true,
    										'multiple-limit': 2
    									}
    								}
    							}
    						}
    					]
    				})
    
    				.set('adv', {
    					items: [
    						{
    							label: '姓名',
    							prop: 'name',
    							span: 12,
    							component: {
    								name: 'el-input'
    							}
    						},
    						{
    							label: '创建时间',
    							prop: 'createTime',
    							span: 12,
    							component: {
    								name: 'el-date-picker'
    							}
    						},
    						{
    							label: '自定义插槽',
    							prop: 'cs',
    							span: 12,
    							component: {
    								name: 'slot-cs'
    							}
    						}
    					]
    				})
    				.set('layout', [
    					[
    						'refresh-btn',
    						'add-btn',
    						'multi-delete-btn',
    						'query',
    						'slot-btn',
    						'flex1',
    						'search-key',
    						'adv-btn'
    					],
    					['data-table'],
    					['flex1', 'pagination']
    				])
    				.done();
    
    			app.refresh();
    		},
    		onTest() {
    			// this.canShowName = !this.canShowName;
    			// this.crud.setData('table.columns[prop:name].hidden', this.canShowName);
    			this.visible = true;
    		}
    	}
    };
    </script>
    
    <style lang="stylus" scoped></style>
    
    ```

25. 刷新事件on,  就是增加一个监听 on refresh

    文档: https://docs.cool-admin.com/#/front/crud?id=load-ctx-app-

    ```vue
    <template>
    	<div>
    		<cl-crud @load="onLoad">
    			<template #slot-btn>
    				<el-button
    					size="mini"
    					type="text"
    					@click="onTest"
    					v-permission="$service.system.user.permission.move"
    				>自定义按钮</el-button>
    			</template>
    			<template #slot-cs>
    				<el-input></el-input>
    			</template>
    		</cl-crud>
    
    		<el-dialog :visible.sync="visible">
    			<el-form>
    				<el-form-item label="姓名">
    					<el-input></el-input>
    				</el-form-item>
    			</el-form>
    		</el-dialog>
    	</div>
    </template>
    
    
    
    <script>
    export default {
    	data() {
    		return {
    			crud: null,
    			canShowName: false,
    			visible: false
    		};
    	},
    
    	methods: {
    		onLoad({ ctx, app }) {
    			this.crud = app;
    			ctx.service(this.$service.system.user)
    				.set('dict', {
    					label: {
    						add: '加加',
    						delete: '减减',
    						update: '编编',
    						refresh: '刷刷',
    						advSearch: '高级搜素'
    					}
    				})
    				.set('table', {
    					columns: [
    						{
    							type: 'index',
    							width: '60'
    						},
    						{
    							type: 'selection',
    							width: '60'
    						},
    						{
    							prop: 'name',
    							label: '名字',
    							width: 200,
    							align: 'center',
    							hidden: this.canShowName
    						},
    						{
    							prop: 'createTime',
    							label: '创建时间',
    							align: 'left'
    						}
    					],
    					op: {
    						visible: true,
    						layout: ['delete', 'edit', 'slot-btn'],
    						props: { width: '300px' }
    					}
    				})
    				.set('upsert', {
    					props: {
    						width: '700px'
    					},
    					items: [
    						{
    							label: '姓名',
    							prop: 'name',
    							span: 12,
    							component: {
    								name: 'el-input'
    							},
    							rules: [
    								{ required: true, message: '请输入活动名称', trigger: 'blur' },
    								{ min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
    							]
    						},
    						{
    							label: '创建时间',
    							prop: 'createTime',
    							span: 12,
    							component: {
    								name: 'el-date-picker'
    							}
    						},
    						{
    							prop: 'avatar',
    							label: '头像',
    							component: {
    								name: 'cl-upload',
    								props: {
    									props: {
    										multiple: true,
    										'multiple-limit': 2
    									}
    								}
    							}
    						}
    					]
    				})
    
    				.set('adv', {
    					items: [
    						{
    							label: '姓名',
    							prop: 'name',
    							span: 12,
    							component: {
    								name: 'el-input'
    							}
    						},
    						{
    							label: '创建时间',
    							prop: 'createTime',
    							span: 12,
    							component: {
    								name: 'el-date-picker'
    							}
    						},
    						{
    							label: '自定义插槽',
    							prop: 'cs',
    							span: 12,
    							component: {
    								name: 'slot-cs'
    							}
    						}
    					]
    				})
    				.set('layout', [
    					[
    						'refresh-btn',
    						'add-btn',
    						'multi-delete-btn',
    						'query',
    						'slot-btn',
    						'flex1',
    						'search-key',
    						'adv-btn'
    					],
    					['data-table'],
    					['flex1', 'pagination']
    				])
    				.on('refresh', async (params, { next }) => {
    					console.log('params =');
    					console.log(params);
    					// 此处 await  then 两种都可以
    					next(params);
    				})
    				.done();
    
    			app.refresh();
    		},
    		onTest() {
    			// this.canShowName = !this.canShowName;
    			// this.crud.setData('table.columns[prop:name].hidden', this.canShowName);
			this.visible = true;
    		}
    	}
    };
    </script>
    
    <style lang="stylus" scoped></style>
    
    ```
    
    # OVER!
    
    
