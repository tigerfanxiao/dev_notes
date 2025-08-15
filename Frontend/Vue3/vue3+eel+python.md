
https://itnext.io/an-awesome-python-gui-vue3-vite-and-eel-ce95fa17878e



OptionAPI 写法


```vue
<script>
export default {
  data(){
    return {
      greeting: ""  // 初始化为空字符串
    }
  },
  methods: {
    update_greeting(_greeting) {
      this.greeting = _greeting
    },

    click(){
      eel.get_greeting("fanxiao")(this.update_greeting)  // 在index.html中引入
      eel.hello_world()
      console.log("clicked")
    }
  },
}
</script>
```
CompositionAPI
```vue
<script setup>
import { ref } from 'vue';

const greeting = ref("")

const update_greeting = (_greeting) => {
	greeting.value = _greeting
}

const click = () => {
	eel.get_greeting("user")(update_greeting)
	eel.hello_world()
	console.log("clicked")
}

</script>
```



eel + ws 读取文件方法

https://stackoverflow.com/questions/59143267/python3-js-how-do-i-handle-local-file-uploads-with-eel

