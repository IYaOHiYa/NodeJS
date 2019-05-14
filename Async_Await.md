# ES7 Async Await

簡單來說，就是用Async把Promise的物件包起來
在await就可以了

```js
  function getWaitVal(choose: boolean): Promise<string>{
    return new Promise((resolve, reject)=>{
      if(!choose){
        reject("False");
      }
      resolve("U got it");
    });
  }
  
  async function main(){
    try{
      let ans1 = await getWaitVal(true);
      console.log(ans1);

      let ans2 = await getWaitVal(false);
      console.log(ans2);

      let ans3 = await getWaitVal(true);
      console.log(ans3);
    }catch(err){
      console.log(err);
    }
  }

  main();
  
  
  //或是直接執行
(async() => {
  try{
    let ans1 = await getWaitVal(true);
    console.log(ans1);

    let ans2 = await getWaitVal(false);
    console.log(ans2);

    let ans3 = await getWaitVal(true);
    console.log(ans3);
  }catch(err){
    console.log(err);
  }
})();
```
