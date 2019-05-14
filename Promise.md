# Promise 

因 Nodejs 為事件特性的服務器，又是異步的
但若不停的回調，會造成Callback hell

如需先等func1結束，在做func2，func2結束，在做func3...

```js
  function func1(()=>{
    
  }):
```
