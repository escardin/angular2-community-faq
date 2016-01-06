# Angular 2 Community FAQ

## Components

### How do I communicate between two sibling components?
There are two common approaches; Use a shared service, or have a common parent pass messages between the two.
  
## Services

### How do I create a service?
A service is just a TypeScript class. If your service has dependencies which need to be injected, you need to annotate it. The `Injectable()` annotation was created for this purpose.
```javascript
export class LogService{}

export class AuthService{}
//This must be marked with an annotation so that Typescript will generate the information required to inject dependencies
@Injectable()
export class TodoService{
  constructor(private logService:LogService,private authService:AuthService){}
}
//Provides a single instance of LogService and TodoService to TodoComponent and all of its children
@Component({
  providers:[LogService, TodoService]
})
export class TodoComponent{
  constructor(private service:TodoService){}
}
//Provide a single instance of AuthService to everything
bootstrap(TodoComponent,[AuthService]);
```
See also: [Injectable Metadata](https://angular.io/docs/ts/latest/api/core/InjectableMetadata-class.html)

### How do I share a service instance between multiple components?
Provide the service in a common parent or the bootstrap. Every time you provide a service you get a new instance.

### How do I communicate between components using a shared service?
See: https://github.com/escardin/angular2-community-faq/blob/master/shared_service.md

## Router

### Any good tutorials for the Router?
Not yet, in the meantime you can look at this: https://www.youtube.com/watch?v=z1NB-HG0ZH4

You can also take a look at this Plunkr, which while complex, does just about everything you would want with the router. http://plnkr.co/edit/Bim8OGO7oddxBaa26WzR?p=preview

### How do I do breadcrumbs with the Router?

See: http://plnkr.co/edit/4cw2fPv3vX36v5Lu9Dnq?p=preview

## Redux with Rx

## RXJS

### Observable.map() (etc...) doesn't work!

See: https://github.com/escardin/angular2-community-faq/blob/master/rxjs_operators.md

## Forms

### Validation

#### How do I use more than one validator on a single control?

See: https://plnkr.co/edit/5yO4HviXD7xIgMQQ8WKs?p=preview

#### How do I make a custom validator?

See: https://plnkr.co/edit/5yO4HviXD7xIgMQQ8WKs?p=preview
