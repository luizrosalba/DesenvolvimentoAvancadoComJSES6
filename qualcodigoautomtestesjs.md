##  Conceitos aplicados a qualidade de código e automação de testes em JS 

### 5.1 Testes TDD e BDD 

####  Testes automatizados : 
unitários (menor parte do programa, componentes ,métodos, classes, funções , etc), 
integrados (testam a integração entre os componentes , como elas se comunicam etc. ) 
e funcionais (ou end-to-end , blackbox) (testa a integração do seu sistema com outros sistemas) 

piramide de testes : unitários (base) -> integrados -> funcionais (topo)

#### Testes automatizados  ou manuais : 
usabilidade , aceitação do usuário, protótipos, funcionais ex: alpha beta , carga , segurança , etc.... 

#### Test Driven Developoment (TDDD) Desenvolvimento orientado a testes 

Cria os testes antes de escrever o código é um dos pulares do extreme programming e consiste em testar e refatorar em pequenos cilcos onde vc escreve o teste antes do código , faz o mesma passar e refatota o código
Escrita do teste -> escrita do código -> refatoração 

Vantagens : 
Feedback rápido 
maior segurança 
codigo mais limpo 
produtividade 


#### Behavior Driven Development (BDD) Desenvolvimento orientado a comportamento
Técnica de desenvolviemnto ágil que visa integrar regras de negócio a linguagens de programação 
Descreve o comportamento dos componentes de maneira que reflita o negócio em si. 
Escreve o teste antes do codigo com o comportamento esperado do codigo 

Pilares 
testes 
documentação 
exemplo 

vantagens 
compartilhamento de conhecimento 
documentação dinâmica 
visão do todo desenvolvedor e negocio falam a mesma linguagem 



### 5.2 Mocha, Chai e Sinon 

Mocha : Ferramenta para executar testes ( test-runner) . Descritivo segue os padroes do BDD. Pode ser utilizando em node ou no browser . 
No terminal do Vs : 

npm init -y 
npm i --save-dev mocha 

alterar o package.json para executar o mocha 
```Javascript
{
  "name": "wwwroot",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "mocha"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "mocha": "^8.1.0"
  }
}
```

criar o diretorio test ( usado pelo mocha) 
criar o arquivo math.spec.js dentro do diretorio (esse é o padrao TDD) 
```Javascript
const assert = require('assert');
const Math = require ('../src/math.js');

describe ('Math class',function (){ 
   it('Sum two Numbers',function () /// it descreve o comportamento esperado 
   {
        const math = new Math();
        assert.equal(math.sum(5,5),10); /// valida se dois valores sao iguais 
   });  
});

```
Criei o diretorio src com o arquivo math.js dentro 
```Javascript
class Math {
   sum = function sum(){ };
}

module.exports =Math; 
```
rodei o npm run test 
erro -> undefined ==10 o teste executado deveria retornar 10 e  retorna undefined. 

Acertando a classe math para fazer a conta corretamente 
```Javascript
class Math {
   sum = function sum(a,b){return a+b };
}

module.exports =  Math;  /// o mocha nao retorna mais o erro 
```
Limpando a classe (TDD) também chamada de refatorar 
```Javascript
class Math {
   sum(a,b){return a+b };
}

module.exports =  Math; 
```
Tornando math assíncrona 
```Javascript
class Math {
   sum(a,b,callback){
      setTimeout(() => {
         callback(a+b);
      },0);
   };
}
module.exports =  Math; 
```
e alterando mathspec para chamada assíncrona 
```Javascript
const assert = require('assert');
const Math = require ('../src/math.js');

describe ('Math class',function (){ 
   it('Sum two Numbers',function () /// it descreve o comportamento esperado 
   {
        const math = new Math();
        math.sum(5,5,(value)=>
        {
            assert.equal(value,10); /// valida se dois valores sao iguais 
        })
          
   });  
});


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












