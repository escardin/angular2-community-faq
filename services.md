#Table of contents
- [How do I create a service?](#how-do-i-create-a-service)
- [How do I share a service instance between multiple components?](#how-do-i-share-a-service-instance-between-multiple-components)
- [How do I communicate between two sibling components?](#how-do-i-communicate-between-two-sibling-components)
- [How do I communicate between components using a shared service?](#how-do-i-communicate-between-components-using-a-shared-service)

# How do I create a service?
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

# How do I share a service instance between multiple components?
Provide the service in a common parent or the bootstrap. Every time you provide a service you get a new instance.

# How do I communicate between two sibling components?
There are two common approaches; Use a shared service, or have a common parent pass messages between the two.

# How do I communicate between components using a shared service?

A shared service is one way of sharing data between components which are not directly related to each other (i.e. not the immediate parent or child).

[Live example](http://plnkr.co/edit/cXhr8LwWrISJWpDRLDvL?p=preview)

```typescript
import {bootstrap} from 'angular2/platform/browser';
import {Component,Injectable} from 'angular2/core';
import {Subject} from 'rxjs/Subject';

/**
 * A shared service for displaying errors
 */
export class ErrorService{
  latestError:Subject=new Subject();
  error(err:any){
    this.latestError.next(err);
  }

}

/*
 * Logs errors to the console.
 * A service which consumes the shared service and provides additional functionality.
 */
@Injectable()
export class ErrorConsoleService{
  constructor(private errorService:ErrorService){
    errorService.latestError.subscribe(err=>console.log(err));
  }
}

/*
 * Displays the latest error from the error service
 * A component which consumes the shared service
 */
@Component({
  selector:'errors',
  template:`<p>{{service.latestError|async}}</p>`
})
export class ErrorComponent{
  constructor(private service:ErrorService){
    
  }
}

/*
 * A form component which when submitted pushes an error to the shared service
 */
@Component({
  selector:'formcmp',
  template:`    
    <form (ngSubmit)="onSubmit()">
      <button type="submit">Submit</button>
    </form>`
})
export class FormComponent{
  constructor(private service:ErrorService){}
  onSubmit() { 
    this.service.error('form submitted');
  }
}

/**
 * Root app component
 */
@Component({
  selector:'my-app',
  directives:[ErrorComponent,FormComponent],
  template:`<errors></errors>
  <formcmp><formcmp>`
})
export class App{
  //If ErrorConsoleService is not injected here, it will never be instantiated and won't work.
  constructor(private errConsole:ErrorConsoleService){}
  
}
bootstrap(App, [ErrorService,ErrorConsoleService])
```
Additional documentation: http://coryrylan.com/blog/angular-2-observable-data-services
