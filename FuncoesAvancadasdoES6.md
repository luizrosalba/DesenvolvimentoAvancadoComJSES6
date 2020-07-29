## Funções avançadas do ES6

### 1.1 Arrow function 
O arrow function traz funções anonimas e veio para solucionar o problema do contexto no qual ele é realizado. Antes do ES6, haviam algumas situações onde o contexto (this) poderia ser ambíguo (criar confusão sobre a qual objeto o this está se referindo) . Arrow functions tem o contexto igual ao código que o envolve , o this se referencia as chaves do arrow function . 


Se houver apenas um argumento e uma expressao (que não seja destruction ou rest operator (...) )
pode-se omitir o return se o lado direito for apenas uma expressão
```javascript
var sum = a => a+5;  
console.log(sum(1));
```

parenteses quando há mais de um argumento 
```javascript
var sum = (a,b)=> a+b; 
console.log(sum);
console.log(sum(1,2));
```

 caso haja mais de uma expressão   expressão as chaves são obrigatórias 
```javascript
var sum = (a,b)=> {
  if (a>b) return a+b; 
  else  return a-b;  
}
console.log(sum(1,2));
```

### Hoisting e Arrow function 
Arrow functions nao realizam hoisting , você terá erro se tentar invocar antes de declarar

### Criando objetos 

Criando um objeto via função construtora 
```javascript
function Car(){
  this.foo ='bar'; 
}
console.log(new Car());

Criando um objeto via arrow function 
```javascript
 var createObj = () => ({test:123});
console.log(createObj());
```
Obs: Não é possível criar um objeto usando uma função construtora por arrow function 


### 1.2 Default Function Arguments 
Evita problemas com valores não inicializados

```javascript
function multiply (a=b,b=1)
{
  return a*b; 
}
console.log(multiply(2,3)); //6
console.log(multiply(2)); ///2 
console.log(multiply(undefined,2));///4 
```

```javascript
function multiply (a=1,b=a)
{
  return a*b; 
}
console.log(multiply(2,undefined)); /// b =a resultado 4 
```
 lazy evaluation 
Sempre que o parametro for esquecido uma função será chamada 

```javascript
function randNumber()
{
  return Math.random() *10; 
}

function multiply (a,b = randNumber())
{
  return a*b; 
}
console.log(multiply(2,undefined)); /// resultado aleatório só invocará a função randNumber caso não haja parametro2 
```

### 1.3 Enhanced Object Literals 

um objeto pode ter métodos 
```javascript
var obj = {
  sum:  function (a,b) { return a+ b; } /// poderia ser sum:  function batata(a,b) { return a+ b; } 
}; 
console.log(obj.sum(1,2));
```

Também pode ser declarado como 
```javascript
var obj = {
  sum(a,b) { return a+ b; }
}; 

console.log(obj.sum(1,2)); /// 3 
```


No ES6 Quando a propriedade do objeto é igual a uma variavel , podemos omitir o prop1 : prop1 dentro do obj 
```javascript
var prop1 = 'bla bla'; 
var obj = {
  prop1
}; 
console.log (obj.prop1); /// bla bla 
```

Também funciona com métodos 

```javascript
function metodo1(){
  console.log('bla bla'); 
}
var obj = {
  metodo1
}; 
obj.metodo1(); /// bla bla 
```

No ES6 podemos também usar variaveis e até concatenar o nome da propriedade de um objeto 

```javascript
var propName ='test'; 

var obj = {
  [propName + 'concat']: 'prop value' 
}; 

console.log(obj);
```


```javascript
```

```javascript
```


