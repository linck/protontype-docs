# Middlewares

## Criando Middlewares

Pra criar uma classe middleware base estender ```ProtonMiddleware```. Um middleware poderá ter um Middleware Function.

Exemplo:

```javascript
import { Middleware, MiddlewareFunctionParams, ProtonMiddleware } from 'protontype';
export class ExampleMiddleware extends ProtonMiddleware {

    @Middleware()
    exampleMiddlewareFunc(params: MiddlewareFunctionParams) {
        cosole.log(params.req);
        console.log(params.res);
        params.next();
    }
}
```

## Middleware Function

Um ***Middleware Function*** é um método dentro de uma classe ```ProtonMiddleware```  anotada com o decorator ```@Middleware``` e tem como parâmetro um objeto do tipo ```MiddlewareFunctionParams```. Este método define o comportamento do middleware.

```javascript
@Middleware()
exampleMiddlewareFunc(params: MiddlewareFunctionParams) {
    cosole.log(params.req);
    console.log(params.res);
    params.next();
}
```

### @Middleware
O decorator ```@Middleware()``` indica qual o método contém o comportamento do middleware.

**Propriedades**

- modelName: Indica qual o Model será injetado como parâmetro no ```MiddlewareFunctionParams```. Opcional

### MiddlewareFunctionParams

Parâmetros de um ***Middleware Function***

**Propriedades**

- **req**: Objeto que contém a requisição http. Corresponde ao objeto de request do [Express](http://expressjs.com/ "").
- **res**: Objeto usado para enviar a resposta http. Corresponde ao objeto de response do [Express](http://expressjs.com/ "").
- **next**: função usada para chamar o próximo middleware da cadeia
- **model**: Instância do model especificado em ***modelName*** de ```@Middleware()```
- **app**: Instância da aplicação Protontype. Por meio dela pode-se acessar as propriedades da aplicação.

## Escopo dos Middlewares
Os middlewares podem atuar em diferentes escopos

### Escopo de Aplicação
Este middleware atuará no escopo da aplicação, ou seja antes de qualquer rota configurada.
Para tornar um middleware global, deve-se adicionar ele no bootstrap da aplicação:

```javascript
new ProtonApplication()
    .addMiddleware(new ExampleMiddleware())
    .addMiddleware(new ExampleMiddleware2())
    .bootstrap();
```

### Escopo de Router
Este middleware atuará para todas as ***Router Functions*** dentro de uma classe ```ExpressRouter```.
Para adicionar middlewares para atuar no escopo do router, este deve ser configurado no decorator ```@RouterClass()```:

```javascript
@RouterClass({
    baseUrl: "/tasks",
    middlewares: [ new ExampleMiddleware(), new ExampleMiddleware2() ]
})
```

### Escopo de Rota (Router Function)
Este middleware atuará somente para aquela rota específica. Para adicionar middlewares ao escopo da rota, este deve ser configurado no decorator ```@Route()```:

```javascript
@Route({
    endpoint: '/list',
    method: Method.GET,
    middlewares: [ new ExampleMiddleware(), new ExampleMiddleware2() ]
})
```