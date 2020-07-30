## 4.1 Callbacks e Promises 

### Callbacks e Promises 

Callback hell Fica dificil saber quem está tratando os dados de quem 
```Javascript
function doSomething(callback){
  setTimeout(function(){
    callback('First data'); 
  },1000);
}

function doOtherThing (callback){
  setTimeout(function(){
    callback ('Second data'); 
  },1000);
}

function doAll()
{
    try
    {
        doSomething(function(data)
        {
              var processedData = data.split(''); 
              try
              {
                doOtherThing(function(data2)
                {
                  var processedData2 = data2.split('');
                  try{
                     setTimeout(function()
                      {
                        console.log(processedData,processedData2);
                      },1000);
                  }catch(err)
                  { ///handle error 

                  }
                });
             }
             catch(err)
            {
              ///handle error 
            }

      });
  } catch (err)
  {
      /// handle error 
  }
}

doAll();
```
Uma Promisse pode ter 3 estados : Pending (em execução ), Fulfilled (terminou de executar)  e Reject (caso ocorra algum erro) . Uma grande vantagem da  promise é que já faz o tratamento do erro conforme abaixo 
////1000 = 1ms 
```Javascript  
////Promisses 
const doSomethingPromise = new Promise((resolve,reject)=>{
 setTimeout(function(){
   // did something 
  resolve('First data'); 
  },1000); 
});

const doOtherThingPromise = new Promise((resolve,reject)=>{
  setTimeout(function(){
      // did something 
    resolve ('Second data'); 
  },1000);
});
doSomethingPromise.then(data=>console.log(data));

```
Tratamento com throw new Error : 
```Javascript
////Promisses 
const doSomethingPromise = new Promise((resolve,reject)=>{
  throw new Error ('something went wrong');
  setTimeout(function(){
   // did something 
  resolve('First data'); 
  },1000); 
});

const doOtherThingPromise = new Promise((resolve,reject)=>{
  setTimeout(function(){
      // did something 
    resolve ('Second data'); 
  },1000);
});

doSomethingPromise
.then(data=>console.log(data))
.catch(error=>console.log(error));

```
Executando uma depois a outra promise 
```Javascript
////Promisses 
const doSomethingPromise = new Promise((resolve,reject)=>{
  setTimeout(function(){
   // did something 
  resolve('First data'); 
  },1000); 
});

const doOtherThingPromise = new Promise((resolve,reject)=>{
  setTimeout(function(){
      // did something 
    resolve ('Second data'); 
  },1000);
});

doSomethingPromise
.then(data=>{
  console.log(data);  
  return doOtherThingPromise;
})
.then(data2=>console.log(data2))
.catch();

```
Usando função para garantir o tempo de execução e incluindo um tratamento de erro global para ambos as promisses encadeadas 

```Javascript
////Promisses 
////Promisses 
const doSomethingPromise = ()=> new Promise((resolve,reject)=>{
  setTimeout(function(){
   // did something 
  resolve('First data'); 
  },1000); 
});

const doOtherThingPromise = () => new Promise((resolve,reject)=>{
  setTimeout(function(){
      // did something 
    resolve ('Second data'); 
  },1000);
});

doSomethingPromise()
.then(data=>{
  console.log(data);  
  return doOtherThingPromise();
})
.then(data2=>console.log(data2))
.catch(error => console.log('Ops',error)); /// tratamento global de erro 

```
Callback hell resolvido por promise 
```Javascript
////Promisses 
const doSomethingPromise = ()=> 
new Promise((resolve,reject)=>{
   
  setTimeout(function(){
   // did something 
  resolve('First data'); 
  },1000); 
});

const doOtherThingPromise = () => 
  new Promise((resolve,reject)=>{
     setTimeout(function(){
      // did something 
      resolve ('Second data'); 
  },1000);
});

doSomethingPromise()
.then(data=>{
  console.log(data.split(''));  
  return doOtherThingPromise();
})
.then(data2=>console.log(data2.split('')))
.catch(error => console.log('Ops',error)); /// tratamento global de erro 

```
Executando em paralelo ( o then só é executado após o fim da promise) 
```Javascript
////Promisses 
const doSomethingPromise = ()=> 
new Promise((resolve,reject)=>{
   
  setTimeout(function(){
   // did something 
  resolve('First data'); 
  },1000); 
});

const doOtherThingPromise = () => 
  new Promise((resolve,reject)=>{
     setTimeout(function(){
      // did something 
      resolve ('Second data'); 
  },1000);
});

Promise.all([doSomethingPromise(),doOtherThingPromise()]).then(data => {
  console.log(data[0].split(''));
  console.log(data[1].split(''));
});

```
No Promisse all caso haja interrupção de uma , cancela-se o funcionamento das duas funções 
```Javascript
////Promisses 
const doSomethingPromise = ()=> 
new Promise((resolve,reject)=>{
   
  setTimeout(function(){
   // did something 
  resolve('First data'); 
  },1000); 
});

const doOtherThingPromise = () => 
  new Promise((resolve,reject)=>{
    throw new Error("ops");
     setTimeout(function(){
      // did something 
      resolve ('Second data'); 
  },1000);
});

Promise.all([doSomethingPromise(),doOtherThingPromise()]).then(data => {
  console.log(data[0].split(''));
  console.log(data[1].split(''));
}).catch(err=>{
  console.log(err);
});

```
Retornando a que resolver primeiro 
```Javascript
////Promisses 
const doSomethingPromise = ()=> 
new Promise((resolve,reject)=>{
   
  setTimeout(function(){
   // did something 
  resolve('First data'); 
  },1500); 
});

const doOtherThingPromise = () => 
  new Promise((resolve,reject)=>{
     setTimeout(function(){
      // did something 
      resolve ('Second data'); 
  },1000);
});

Promise.race([doSomethingPromise(),doOtherThingPromise()]).then(data => {
  console.log(data);
});
```

## 4.2 Fetch Async await Eventemmiter

Fetch é uma nova API com o mesmo intuito da antiga xmlhttpRequest , fazer requisições, mas utiliza promises 
Fetch retorna uma promise . O exemplo mostra uma requisição  
```Javascript
fetch('/data.json').then(responseStream=>{
   responseStream.json().then(data=>{
       console.log(data);
   });
  })

```
Caso haja um erro de rede será pego no catch 
```Javascript
fetch('http:localhost:8081/data.json') /// erro de rede nao existe esta porta 
.then(responseStream=> responseStream.json())
.then(data=>{
       console.log(data);
   }).catch(err=> {
        console.log("Erro de rede",err);
   });
  

```
Outro exemplo , agora com erro de nome de arquivo inexistente 
```Javascript
fetch('http:localhost:8080/datassss.json') /// erro de fim inesperado de input json
.then(responseStream=> responseStream.json())
.then(data=>{
       console.log(data);
   }).catch(err=> {
        console.log("Erro de rede",err);
   });

```
Tratando o erro para nao tentar parsear para json 
```Javascript
fetch('http:localhost:8080/data222.json') 
.then(responseStream => {
    console.log(responseStream)
    if(responseStream.status===200){
        return responseStream.json();
    }else {
        throw new Error('Request error');
    }
})
.then(data=>{
       console.log(data);
   }).catch(err=> {
        console.log("Erro de rede",err);
   });
```
por default o segundo parametro do fetch eh um get. mas podemos forçar um post 
```Javascript
fetch('http:localhost:8080/data.json',{
    method:'post',
    body:JSON.stringfy
}) /// erro de fim inesperado de input json
.then(responseStream => {
    console.log(responseStream)
    if(responseStream.status===200){
        return responseStream.json();
    }else {
        throw new Error('Request error');
    }
})
.then(data=>{
       console.log(data);
   }).catch(err=> {
        console.log("Erro de rede",err);
   });
```
Async 
```Javascript
```

```Javascript
```

```Javascript
```

```Javascript
```

```Javascript
```

```Javascript
```
