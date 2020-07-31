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
e alterando mathspec para chamada assíncrona , percebemos que o código continua passando  , pois é assíncrono (no meu , diferente do vídeo,  talvez por ser uma versão mais atual, já mostra erros de validação) 
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
Precisamos então de uma maneira de validar valores assíncronos usando mocha 
Acrescente done no argumento da funcao dentro do it . atenção ! moca recomenda nao usar arrow fucntion 

```Javascript
const assert = require('assert');
const Math = require ('../src/math.js');

describe ('Math class',function (){ 
   it('Sum two Numbers',function (done) /// it com done aguarda o done ser invocado 
   {
        const math = new Math();
        this.timeout(3000); /// por isso nao usamos arrow function para 
                            /// poder usar o this deste contexto , se usassemos arrow 
                            ///function o this se referenciaria ao describe 
        math.sum(5,5,(value)=>
        {
            assert.equal(value,10); /// valida se dois valores sao iguais 
            done();
        })
   });  
});

```
Escrevendo testes para funcoes que ainda nao existem 
```Javascript
const assert = require('assert');
const Math = require ('../src/math.js');

describe ('Math class',function (){ 
   it('Sum two Numbers',function (done) /// it com done aguarda o done ser invocado 
   {
        const math = new Math();
        this.timeout(3000); /// por isso nao usamos arrow function para 
                            /// poder usar o this deste contexto 
        math.sum(5,5,(value)=>
        {
            assert.equal(value,10); /// valida se dois valores sao iguais 
            done();
        })
   });  
   it ('Multiply two numbers '); /// teste pendente 
});

```
it.only executa somente este teste 

```Javascript
it.only ('Multiply two numbers',function(){
       const math = new Math();
       assert.equal(math.multiply(5,5),25);
   });
```
it.skip pula o teste 

```Javascript
  it.skip ('Multiply two numbers',function(){
       const math = new Math();
       assert.equal(math.multiply(5,5),25);
   });
```

mocha hooks (before each)  
executa codigo e evita repetições 
outros hooks : before , after e afterEach 
```Javascript

const assert = require('assert');
const Math = require ('../src/math.js');

let value =0; 

describe ('Math class',function (){ 
    ///hook 
    beforeEach(function (){
        value = 0; 
    });


   it('Sum two Numbers',function (done) /// it com done aguarda o done ser invocado 
   {
        const math = new Math();
        this.timeout(3000); /// por isso nao usamos arrow function para 
                            /// poder usar o this deste contexto 
        
        value =5; 

        math.sum(value,5,(value)=>
        {
            assert.equal(value,10); /// valida se dois valores sao iguais 
            done();
        });
   });  
   it ('Multiply two numbers',function(){
       const math = new Math();
       assert.equal(math.multiply(value,5),0);
   });
});
```

### 5.3 Chai 
O moca nao possui uma ferramenta de assert. Ele usa uma ferramenta built in do node. 
O assert nao eh tao legíveis (os its, describes)  na hora de comparar objetos. Para melhroar isso usamos o chai https://www.chaijs.com/api/
npm i --save-dev chai 
que é uma ferramenta de asserts tornando os testes mais descritivos 

```Javascript
const assert = require('assert');
const Math = require ('../src/math.js');
const expect = require ('chai').expect;

let value =0; 

describe ('Math class',function (){ 
    ///hook 
    beforeEach(function (){
        value = 0; 
    });


   it('Sum two Numbers',function (done) /// it com done aguarda o done ser invocado 
   {
        const math = new Math();
        this.timeout(3000);         
        value =5; 

        math.sum(value,5,(value)=>
        {
            expect(value).to.equal(10); /// mais legível 
            done();
        });
   });  
   it ('Multiply two numbers',function(){
       const math = new Math();
       expect(math.multiply(value,5)).to.equal(0); /// mais legível 
   });
});


```
Utilizando para validar objetos 

```Javascript
 it ('Multiply two numbers',function(){
       const math = new Math();
       const obj = {
           name : 'Luiz'
       };

       expect(obj).to.have.property('name'); /// mais legível 
   });
```
Comparando objetos , cuidado ! 

```Javascript
  it.only ('Multiply two numbers',function(){
       const math = new Math();
       const obj = {
           name : 'Luiz'
       };
       const obj2 = {
        name : 'Luiz'
    };
    /// const obj2=obj; /// tornaria a comparação verdadeira 
       expect(obj).to.equal(obj2); /// problemas pois a referencia eh diferente  
       ///expect(obj).to.deep.equal(obj2); /// seriam iguals to deep faz uma comparação dos valores dos objetos 
   });
```
### 5.4 Sinon
útil para testar métodos de objetos. Adicionei o método printSum na classe math como testar ? 

```Javascript
class Math {
   sum(a,b,callback){
      setTimeout(() => {
         callback(a+b);
      },0);
   };

   multiply(a,b){
      return(a*b);      
   };
   printSum(req,res,a,b){
      res.load('index',a+b);
   };

}

module.exports =  Math; 
```
Adicionar uma função espiã que diz se o método foi invocado (spy) na propriedade load do objeto res 

```Javascript
it.only ('Calls req with sum and index values ',function(){
    const req= {};
    const res = {
        load:sinon.spy()
    };
    const math = new Math();
    math.printSum(req,res,5,5);
    expect(res.load.calledOnce).to.be.true;

```
Verificando se o primeiro argumento é index 

```Javascript
   it.only ('Calls req with sum and index values ',function(){
    const req= {};
    const res = {
        load:sinon.spy()
    };
    const math = new Math();
    math.printSum(req,res,5,5);
    expect(res.load.args[0][0]).to.be.equal('index');
 });
```
Verificando se o segundo argumento é soma 
```Javascript
   it.only ('Calls req with sum and index values ',function(){
    const req= {};
    const res = {
        load:sinon.spy()
    };
    const math = new Math();
    math.printSum(req,res,5,5);
    expect(res.load.args[0][1]).to.be.equal(10);
 });
```
Confirma que o método nao foi chamado (stub) 
```Javascript

   it.only ('Calls req with sum and index values ',function(){
    const req= {};
    const res = {
        load : function load(){
            console.log('called');
        }
    };
    sinon.stub(res,'load');
    const math = new Math();
    math.printSum(req,res,5,5);
    expect(res.load.args[0][0]).to.be.equal('index');
 });

```
Passou pela checagem 
```Javascript
 it.only ('Calls req with sum and index values ',function(){
    const req= {};
    const res = {
        load : function load(){
            console.log('called');
        }
    };
    sinon.stub(res,'load').returns('xadsa');
    const math = new Math();
    math.printSum(req,res,5,5);
    expect(res.load.args[0][0]).to.be.equal('index');
 });

```
```Javascript
```











