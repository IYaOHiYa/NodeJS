# Co

[![npm require][npm-co-version-img]][npm-co-url]

用於讓異步的代碼 "同步化" 的唷
Co的建立是基於兩個ES6的玩意兒上
- Promise
- Generator

由於Co是別人編寫的庫，所以需要用npm安裝一下

1. npm install co --save
2. npm install @typs/co --save //安裝typescript庫，才有提示

## function co
將多個Promise的物件包起來，最後的結果在用then取得

Ex.
```js
function getXXX(pass: boolean): Promise<string>{
    return new Promise((resolve, reject)=>{
        if(!pass){
            reject("Fail to pass");
        }
        resolve("You may pass");
    });
}

co(function* (){
    let str1 = yield getXXX(true);
    let str2 = yield getXXX(false);
    let str3 = yield getXXX(true);
    let str4 = yield getXXX(true);
    return 87878;
})
.then((val)=>{
    console.log(`Finish ${val}`);
})
.catch((err)=>{
    console.log(err);
});
```

## function co.wrap
將多個Promise的物件寫在wrap裡，就可以像寫同步程式那樣撰寫

Ex.
```js
function getXXX(pass: boolean): Promise<string>{
    return new Promise((resolve, reject)=>{
        if(!pass){
            reject("Fail to pass");
        }
        resolve("You may pass");
    });
}

(co.wrap(function* (){
    try{
        let str1 = yield getXXX(true);
        let str2 = yield getXXX(true);
        let str3 = yield getXXX(true);
        let str4 = yield getXXX(true);

        console.log(str1);
        console.log(str2);
        console.log(str3);
        console.log(str4);
    }catch(err){
        console.log(err);
    }
}))();
```


[npm-co-version-img]: https://img.shields.io/npm/v/co.svg?style=flat-square
[npm-co-url]: https://www.npmjs.com/package/co
