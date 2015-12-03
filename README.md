# Angular 2 Community FAQ

## Components

### How do I communicate between two sibling components?
There are two common approaches; Use a shared service, or have a common parent pass messages between the two.
  
## Services

### How do I create a service?
A service is just a TypeScript class. If your service has dependencies which need to be injected, you need to annotate it. The `Injectable()` annotation was created for this purpose.
```javascript
export class AService{}

export class BService{}
//This must be marked with an annotation so that Typescript will generate the information required to inject dependencies
@Injectable()
export class CService{
constructor(private aService:AService,private bService:BService){}
}
//Provides a single instance of AService and CService to AppComponent and all of its children
@Component({
providers:[AService, CService]
})
export class AppComponent{
constructor(private service:CService){}
}
//Provide a single instance of BService to everything
bootstrap(AppComponent,[BService]);
```
See also: [Injectable Metadata](https://angular.io/docs/ts/latest/api/core/InjectableMetadata-class.html)

### How do I share a service instance between multiple components?
Provide the service in a common parent or the bootstrap. Every time you provide a service you get a new instance.

## Router

### Any good tutorials for the Router?
Not yet, in the meantime you can look at this: https://www.youtube.com/watch?v=z1NB-HG0ZH4

## Redux with Rx
