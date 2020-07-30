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

## Generators 


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


```Javascript
```

```Javascript
```

```Javascript
```

```Javascript
```





