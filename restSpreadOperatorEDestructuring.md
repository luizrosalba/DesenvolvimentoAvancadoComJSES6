## Aplicando Conceitos Rest, Spread , Operator e Destructuring 
### 2.1 Conheça Rest e Spread Operator 
Arguments -> variavel reservada dentro de uma função para armazenar os argumentos da função 
Antes do ES6 : 
```Javascript
function soma(a,b){
  var value=0; 
  for (var i=0; i<arguments.length ; i++ )
  {
    value+= arguments[i];
  }
  return value; 
}
console.log(soma(1,2,3,4,5)); /// 15 
``` 
Depois do ES6 com o rest operator ... (tres pontos dentro da lista de argumentos)  
(pega todos os argumentos de uma função e transforma em um array) 
```Javascript
function soma(...args){
  return args.reduce((acc,value)=> acc+value,0); 
}
console.log(soma(1,2,3,4,5)); //// 15 
```
Obs : arguments tem como prototype um objeto enquanto o operador rest retorna um array 
Obs2 : arguments é inexistente em arrow functions 

Mesmo exemplo com arrow function 

```Javascript
const sum = (...rest) => {
 console.log( rest.reduce((acc,value)=> acc+value,0));
} ;
(sum(1,2,3,4,5)); /// 15 
```
Apply pega o retorno do método multiply e aplica no metodo soma 

```Javascript

const multiply  = (...args) => {
 console.log( args.reduce((acc,value)=> acc*value,1));
} ;

const sum = (...rest) => {
 multiply.apply(undefined,rest);
} ;
(sum(5,5,5,2,3));
```

Para evitar o problema dos contextos novamente , utiliza-se o spread operator ele pega todos os itens de um array e transforma em argumentos para uma segunda função (parecido com o apply) 

```Javascript

const multiply  = (...args) => {
 console.log( args.reduce((acc,value)=> acc*value,1));
} ;

const sum = (...rest) => {
 multiply(...rest);
} ;
(sum(5,5,5,2,3)); ///750

```
O spread operator pode ser usado em strings, arrays, e objetos literais e iteraveis 

```Javascript

const str = 'Digital Input'; 
const arr = [1,2,3,4]; 
function logArgs(a,b,c,) {
  console.log(a,b,c);
}
logArgs(...str); /// D i g 
logArgs(...arr); /// 1 2 3 cada item vira um argumento da funcao log

```
Concatenando com o spread operator 
```Javascript
const arr = [1,2,3,4]; 
const arr2 = [...arr,5,6,7]; 
console.log(arr2); /// 1 , 2 , 3 , 4, 5 , 6 ,7 
```
Pode ser usado em objetos 

```Javascript

const obj = {
  test:123
};

const obj2 = { 
 ...obj,
  test:'hello' ,
   
}
console.log(obj2); 
/// {  test:"hello"  } 
/// obs; a ordem faz diferença  nesse caso obj vem antes de hello por isso a saida eh hello 

```

Erro , não pode ser feito pois spread só pode ser usado com objetos iteraveis 

```Javascript
const obj = {
  test:123
};
const arr = [...obj];  /// erro 
```
Shallow clone (clone raso ): Utilizar um spread operator para clonar um objeto inicial não permite que alterar o objeto clonado altere o objeto  inicial 

```Javascript
const obj = {
  test:123
};
const obj2 = obj;
obj2.test = 456 ; 

console.log(obj); /// 456 alterou o objeto inical 
```

```Javascript
const obj = {
  test:123
};
const obj2 = {...obj};
obj2.test = 456 ; 

console.log(obj);/// 123  nao alterou o objeto inicial
```

## 2.2 Destructuring em React.js (imutável) 
utilizado para destruir arrays com uma notação enxuta 

```Javascript
var arr = ['apple','banana','orange',['Tomato']];

var apple=arr[0];
var banana=arr[1];
var orange=arr[2];
var tomato=arr[3][0];
/// com destruction 
var [apple2,banana2,orange2,[tomato2]] =   [
  'apple',
  'banana',
  'orange',
  ['tomato']]; /// notacao é mais enxuta 


```

O mesmo pode ser feito com objetos  
```Javascript
var obj  = {
  name:'Celso'
}
 /// sem destruction name = obj.name;  
var {name} = obj /// destroi o objeto , procura a propriedade name e pega seu valor 
console.log(name); 

```
Destruindo e logo em seguida atribuindo a outra variável 
```Javascript
var obj  = {
  name:'Celso'
}
var {name:name2} = obj
console.log(name2); 
```

Também pode ser feito com nestes props 
```Javascript
var obj  = {
  name:'Celso',
  props:{
    age:26
  }
}

//var age = obj.prop.age;  antigo 
var {props:{age}} = obj;  /// com destruction 
console.log(age); 

```
Posso acessar diretamente uma propriedade de um objeto e customizar seu nome 

```Javascript
var arr = [ {name: 'apple' , type : 'fruit'}] ; 

var [{name:fruitname,type}]=arr; 
console.log(fruitname,type); 

```
Demonstrando a destrução como argumento com default values de uma função 
```Javascript
function sum([a,b]=[0,0]){
  return a + b; 
} 
console.log(sum([5,5]));  ///10 
console.log(sum([5]));  /// NAn ?  
```



