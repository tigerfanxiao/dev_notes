
```javascript
vm.component('hello', {
  template: `<h1>{{ message }}</h1>`,
  data() {
    return {
      message: 'Hello World!'
    }
  }
})
```

```html
 <div id="app">
    <hello></hello>
  </div>
```