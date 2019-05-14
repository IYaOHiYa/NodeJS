# Promise 

因 Nodejs 為事件特性的服務器，又是異步的
但若不停的回調，會造成Callback hell

如需先等func1結束，在做func2，func2結束，在做func3...

Ex.異步
```js
  function func1(): void{
    setTimeout(()=>{
      console.log("Im func1");
    }, 3000);
  }
  
  function func2(): void{
    console.log("Im func2");
  }
  
  function main(): void{
    func1();
    func2();
  }
  
  main(); 
  /*
    結果為Im func2先出來
    在來才是Im func1
  */
```

為了讓func1先執行，我們可能會這樣做
```js
  function func1(cb: Function): void{
    setTimeout(()=>{
      console.log("Im func1");
      cb();
    }, 3000);
  }
  
  function func2(cb: Function): void{
    setTimeout(()=>{
      console.log("Im func2");
      cb();
    }, 3000);
  }

  function func3(cb: Function): void{
    setTimeout(()=>{
      console.log("Im func3");
      cb();
    }, 3000);
  }
  
  function main(): void{
    func1(()=>{
      func2(()=>{
        func3(()=>{});
      })
    });
  }
  
  main(); 
  /*
    3秒後 func1
    3秒後 func2
    3秒後 func3
  */
```
於是你的回調就會越來越深...就像愛情一樣，越陷越深

## 回到主題Promise

Promise 是可以對非同步進行處理以及各種操作的東西
通常Promise會包含著三種狀態
- resolve 成功
- reject  失敗
- pending 處理中，結果尚未知曉

馬上來範例
Ex.
```js
  function getSomething(myVal: number): Promise<string>{
    return new Promise((resolve, reject)=>{
      if(myVal > 0){
        resolve("Successed");
      }
      reject("Failed");
    });
  }
  
  function main(): void{
    getSomething(1)
      .then((myVal)=>{
        console.log(myVal); //Successed
      })
      .catch((err)=>{
        console.log(err); //不會顯示，因為myVal > 0
      });
      
    getSomething(-1)
      .then((myVal)=>{
        console.log(myVal); //不會顯示，因為myVal <= 0
      })
      .catch((err)=>{
        console.log(err); //Failed
      });
  }
  
  main();
```

基本上只要是有回傳resolve或reject的狀態下，都可以一直.then下去，then到你不想then為止。
Ex.
```js
  function getSomething(myVal: number): Promise<string>{
    return new Promise((resolve, reject)=>{
      if(myVal > 0){
        resolve("Successed");
      }
      reject("Failed");
    });
  }
  
  function main(): void{
    getSomething(1)
      .then((myVal)=>{
        console.log(myVal);
        return getSomething(2);
      })
      .then((myVal)=>{
        console.log(myVal);
        return getSomething(3);
      })
      .then((myVal)=>{
        console.log(myVal); 
        return getSomething(4);
      })
      .then((myVal)=>{
        console.log(myVal);
        return getSomething(-5);
      })
      .catch((err)=>{
        console.log(err);
      });
  }
  
  main();
```
