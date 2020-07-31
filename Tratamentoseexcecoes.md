##  5 Tratamento e exceções 
### 5.1 como Identificar erros 
Variaveis const nao sofrem hoisting. O JS interrompe a execução caso ocorra um erro.  ```Javascript
console.log(name); 
const name = 'Luiz';
console.log('keep going'); 
```
Podemos usar o try  catch 
```Javascript
try{
    console.log(name); 
    const name = 'Luiz';
    
}catch(err){
    console.log('Error' , err); 
}
console.log('keep going');  /// seria impresso 
```
Utilizando a classe Error para gerar um erro e fornecer uma resposta customizada 

```Javascript
try{
    const name = 'Luiz';
    const myError = new Error ('custom error'); 
    throw myError;
}catch(err){
    console.log('Error' , err); 
}finally{
    console.log('keep going'); 
}

```
Extendendo a classe error 
```Javascript
class CustomError extends Error { 
    constructor({message,data}){
        super(message);
        this.data = data; 
    }
}

try{
    const name = 'Luiz';
    const myError = new CustomError ({
        message: 'custom message error',
        data: {
            type:'Server error' 
        }
    }); 

    throw myError;

}catch(err){
    if(err.data.type ==='Server error')
    {

    }
    else {
        
    }
    console.log('Error' , err); 
    console.log(err.data);
}finally{
    console.log('keep going'); 
}


```

### 5.2 Debugging parte 1 
Podemos utilziar a aba do chrome Networl dentro do modo desenvolvedor (cntrl + shift +i) PAra entender quais requisições estão sendo feitas, os arquivos que estão sendo utilizados ,etc. 
### 5.2 Debugging parte 2 
como identificar erros dentro do próprio código ? 
Pretty print -> remove a mimificação , característica de sites em produção para que consigamos debugar o código . 
Ao encontrar um erro colocar um breakpoint e dar reload. A Execução será interrompida exatamente na linha do erro. 
Pressionando esc um console é aberto e tudo que for digitado será executado exatamente onde está o breakpoint 
Outra opção é colocar no código a palavra debbuger; Isto irá interromper o código como se fosse um breakpoint 

### 5.2 Debugging com console log 

console.warn  amarelo 
console.error  vermelho
console.trace  informações de onde o código foi executado 
console.group agrupa as mensagens de console 
console.endgroup final agrupa as mensagens de console 

tempo de execução de um trecho 
console.time('log time');
setTimeout(() => {
  console.timeEnd('log time'); 
},2000); 

console.table ( formata arrays e objetos) 

console.assert (1===1 , 'Ops' ) ; se for falso mostra ops 
console.log('%c style log) , 'color:blue'; 'font-size:40x') 


Na aba sources , podemos debugar o style e ver como as mudanças ficam em tempo real , m udando o tamanho de botoes , cores , etc 



