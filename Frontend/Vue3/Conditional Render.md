
这里增加了template标签是为了不该表dom的结构，使css不会出错
同时保证了v-if和v-else紧紧贴着使用。

```html
    <p v-if="mode == 1">Showing v-if directive content</p>
    <template v-else-if="mode == 2">
      <p>Bonus content</p>
      <h3 >v-else-if worked</h3>
    </template>
    <p v-else>v-else worked</p>
```


v-show和 conditional render的区别

1. v-show使display： none， 元素在dom里面， conditional render使元素不再dom里面
2. 当页面元素很多的时候， v-show toogle更快。 当元素相对较少时，conditional render加载更快。 
3. v-show不能于v-else连用
4. v-show不能用template标签连用
5. Expansive on load， Cheap on Toggle

