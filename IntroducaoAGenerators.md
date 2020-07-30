## 3.1 Symbols e Iterators 

 Symbols são maneiras de gerar um identificador único. Symbol nao pode ser invocado usando new. 
```Javascript
const uniqueId = Symbol (); 
console.log (uniqueId); 
```
Mesmo com o mesmo valor , symbols são identificadores unicos e retornam falso 

```Javascript
const uniqueId = Symbol ("Oi"); 
const uniqueId2 = Symbol ("Oi"); 
console.log (uniqueId===uniqueId2); 
```
Pode ser utilizado para gerar propriedades privadas. Não é listado se imprimimos (log) o objeto somente quando fazemos Object.getOwnPropertySymbols(); 

Propriedades do symbols : Iterator , to Primitive 

```Javascript
const arr = [1,2,3,4]; 
const it = arr[Symbol.iterator]();
console.log (it.next()); 
```
Iterando só os valores 
```Javascript
const arr = [1,2,3,4]; 
const it = arr[Symbol.iterator]();
while (true){
  let {value,done} = it.next(); 
  console.log (value); 
  if (done)   break; 
}
```
Transformando um objeto não iteravel em iteravel, possivelmente igonrando valores , etc ... 
```Javascript
//// well known symbols 


const arr = [1,2,3,4]; 
const str = 'Digital Inovation One'; 

const obj = {
  values:[1,2,3,4],
  [Symbol.iterator](){
    let i = 0; 
      return {
        next:() => {
          i++;
          return {
            value: this.values[i-1],
            done: i>this.values.length
          };
        }
      };
  }
};

const it = obj[Symbol.iterator]()

console.log(it.next());

```

## 3.2 Generators 
Generators são funções com pausa e despausa de valores atraves da interface de iteração criada por um iterator 

```Javascript
//// well known symbols 
const arr = [1,2,3,4]; 
const str = 'Digital Inovation One'; 

const obj = {
  values:[1,2,3,4],
  [Symbol.iterator](){
    let i = 0; 
      return {
        next:() => {
          i++;
          return {
            value: this.values[i-1],
            done: i>this.values.length
          };
        }
      };
  }
};

//// generator 
function* hello (){
  console.log('Hello');
  yield 1; /// pausa a execação da função e executa em partes 
  console.log('From');
  yield 2; /// pausa a execação da função e executa em partes 
  console.log('Function!');
}
const it = hello();
console.log(it.next()); 
console.log(it.next());
console.log(it.next());
```
Pode receber valores de fora da função 
```Javascript
//// generator 
function* hello (){
  console.log('Hello');
  yield 1; /// pausa a execação da função e executa em partes 
  console.log('From');
  const value = yield 2; /// pausa a execação da função e executa em partes 
  console.log(value);
}
const it = hello();
console.log(it.next()); 
console.log(it.next());
console.log(it.next('Outside!'));
```

Executa cada step do while apos invocar o next 
```Javascript
//// generator 
function* naturalNumbers(){
  let number = 0 ; 
  while(true){
    yield number; 
    number++;
  }
}
const it = naturalNumbers();
console.log(it.next());
console.log(it.next());
console.log(it.next());

```
Iterando um generator com pausa , reproduzindo o exemplo anterior de maneira mais simples 
Generator aqui foi utilizado como uma forma de construir iteradores 

```Javascript
const arr = [1,2,3,4]; 
const str = 'Digital Inovation One'; 

const obj = {
  values:[1,2,3,4],
  *[Symbol.iterator](){
    for (var i = 0 ; i < this.values.length;i++){
      yield this.values[i];
    }
  }
};

for (let value of obj) { /// for of é para objetos iteraveis 
  console.log(value)
}
```





