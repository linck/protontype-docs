
<div align="center">
  <h1><strong>ProtonType</strong></h1>
  <a href="https://protontype.github.io/">
    <img src="https://avatars1.githubusercontent.com/u/34164645?s=200&v=4">
  </a>
  <br>
</div>

Um simples web framework feito em TypeScript.

O ProtonType tem como objetivo tornar simples e agradável o desenvolvimento de APIs REST.

## Instalação
```bash
npm install protontype --save
```
 
## Models
Usa [TypeORM](http://typeorm.io/#/) por padrão para manipulação de banco de dados. Mas pode ser usado qualquer framework.

```typescript
@Entity()
export class TasksModel {
    @PrimaryGeneratedColumn()
    id: number;
    @Column({ nullable: true })
    title: string;
    @Column()
    done: boolean;
    @Column()
    userId: number;
}
```
## Middlewares
Suporta implementação de middlewares

```typescript
export class TasksMiddleware extends BaseMiddleware {
    @Middleware()
    printTaskTitle(params: MiddlewareFunctionParams) {
        cosole.log(params.req.body.title);
        params.next();
    }
}
```

## Router
Rotas básicas de CRUD já implementadas nos ```CrudRouter```

```typescript
 @RouterClass({
    baseUrl: "/tasks",
    model: TasksModel,
    middlewares: [new TasksMiddleware()]
})
export class TasksRouter extends CrudRouter {
    /*
    GET / - Lista todos registros
    POST / - Cria um registro
    GET /:id - Consulta um registro
    PUT /:id - Atualiza um registro
    DELETE /:id - Remove um registro
    */

    //Novas rotas customizadas ....
}
```

Ou pode implementar rotas customizadas
```typescript
 @RouterClass({
    baseUrl: "/tasks",
    model: TasksModel,
    middlewares: [new TasksMiddleware()]
})
export class TasksRouter extends BaseRouter {
    @Route({
        endpoint: '/test/msg',
        method: Method.GET,
        middlewares: [new OtherMiddleware()]
    })
    routeTest(params: RouterFunctionParams) {
        console.log("Hello!");
    }
}
```

## Acessando o banco de dados
```typescript
let tasksRepository = TypeORMDB.getBD().getRepository(TasksModel);
let tasks = await tasksRepository.find();
``` 

## Iniciando a aplicação

```typescript
new ProtonApplication()
    .addRouterAs(TasksRouter)
    .addMiddlewareAs(SomeoneGlobalMiddleware)
    .start();
```

## Exemplos
- [Exemplo básico](https://github.com/protontype/protontype-sample)

- [Exemplo com o módulo do Sequelize](https://github.com/protontype/protontype-sequelize-sample)

## Versão de desenvolvimento
```bash
npm install protontype@dev --save
```

<div>
  <br>
	<a href="https://travis-ci.org/protontype/protontype">
		<img src="https://travis-ci.org/protontype/protontype.svg?branch=develop">
	</a>
	<a href="https://www.npmjs.com/package/protontype">
		<img src="https://badge.fury.io/js/protontype.svg">
	</a>
  <br>
</div>
