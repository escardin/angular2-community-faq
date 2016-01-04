#Creating a shared service

A shared service is one way of sharing data between components which are not directly related to each other (i.e. not the immediate parent or child).

live example: http://plnkr.co/edit/cXhr8LwWrISJWpDRLDvL?p=preview

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
