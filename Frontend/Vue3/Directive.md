
# directive
|directive|Usage|Comments|
|---|---|---|
|[[v-cloak]]|等到完全vue render之后再显示页面|需要配置css|
|`v-bind:href="url_var"`|修改标签的属性值|缩写`:href="url_var"`|
|`:value="default_value"`|输入默认值后不更改||
|v-model|修改标签内的内容|
|v-html|输出raw html||
|`v-on:click="method"`|添加事件|放在属性里, 缩写`@click="age++"`|
|`v-if="mode==1"`|条件渲染||
|`v-else-if`|必须紧紧贴着`v-if`使用|对于标签类型没有限制|
|`v-else`|必须紧紧贴着`v-if`使用|如果改变结构，可能会造成css错误|

