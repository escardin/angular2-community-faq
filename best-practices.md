# *Section still not complete. Work In Progress*

# Table of contents

- [Proper use of EventEmitters](#proper-use-of-eventemitters)
- [ElementRef, nativeElement and Renderer](#elementref-nativeelement-and-renderer)
- [Don't use Subjects](#dont-use-subjects)
- [Don't mix angular2 with 3rd libs](#dont-mix-angular2-with-3rd-libs)
- [Don't use `ngAfterViewChecked` nor `ngAfterContentChecked`](#dont-use-ngafterviewchecked-nor-ngaftercontentchecked)
- [Accessing elements in the view](#accessing-elements-in-the-view)
- [Using @Injectable() in services](#using-injectable-in-services)
- [DynamicComponentLoader and loadAsRoot](#dynamiccomponentloader-and-loadasroot)

## Proper use of EventEmitters

EventEmitter extends from Subject, which give us the ability to subscribe. Should we? It's easy, convenient and super cool!
But we should not! EventEmitter is an angular2 abstraction to communicate between components. Use it only with `@Output()`.
It's not sure that it will continue being an Observable nor that will continue being part of the API at all! 


See more 
- [What is the proper use of an EventEmitter?](http://stackoverflow.com/questions/36076700/what-is-the-proper-use-of-an-eventemitter)




## ElementRef, nativeElement and Renderer

Official documentation states we should not access nativeElement directly, how should we do it then? To access `nativeElement` correctly we must use Renderer since it's WebWorker safe.

Consider the following examples

```js

constructor(renderer: Renderer, elementRef: ElementRef) {}


// Setting an attribute
this.renderer.setElementAttribute(this.elementRef.nativeElement, 'contenteditable', 'true');

// Setting a property
this.renderer.setElementProperty(this.elementRef.nativeElement, 'checked', 'true');

// Setting a style
this.renderer.setElementStyle(this.elementRef.nativeElement, 'background-color', 'red');

// Setting a class
.my-css-class {}
this.renderer.setElementClass(this.elementRef.nativeElement, 'my-css-class', true); // or false to remove

// Invoking a method
this.renderer.invokeElementMethod(this.elementRef.nativeElement, 'click', []); // Click doesn't receive any parameteres
this.renderer.invokeElementMethod(this.elementRef.nativeElement, 'scrollIntoView', [false]); // 'scrollIntoView' accept true or false as parameter

// Listen events in the element
this.renderer.listen(this.elementRef.nativeElement, 'click', () => { /* callback */ });

// Listen to global events
this.renderer.listenGlobal('document', 'click', () => { /* callback */});

```


See more 
- [What are some good alternatives to accessing the DOM using nativeElement in Angular2?](http://stackoverflow.com/questions/36108995/what-are-some-good-alternatives-to-accessing-the-dom-using-nativeelement-in-angu)
- [Dynamically add event listener in Angular 2](http://stackoverflow.com/questions/35080387/dynamically-add-event-listener-in-angular-2)


## Don't use Subjects

- TODO


## Don't mix angular2 with 3rd libs

Most javascript libraries rely on `addEventListener`, `setTimeouts` and others that trigger change detection in angular2. 
Triggering, or listening to events of 3rd party libs can trigger excesively change detection decreasing considerably the perfomance of the application.

On the other hand, these libraries can rely on event handling not patched by angular2, therefore these events will run outside angular2's zone causing change detection NOT to run!

If some cool library hasn't been ported to angular2, be the first to port it! Make yourself famous!


TODO : Improve more ?


See more
- [Using JQuery With Angular 2.0](http://www.syntaxsuccess.com/viewarticle/using-jquery-with-angular-2.0)


## Don't use `ngAfterViewChecked` nor `ngAfterContentChecked`

`ngAfterViewChecked` and `ngAfterContentChecked` are executed everytime change detection runs. 
Doing heavy calculations, adding/removing DOM or similar can end up in a very low performance application.

Using Promises, setTimeouts, setIntervals, among others trigger change detection which executes these two lifecycle hooks. This combination can end in endless loops, memory leaks, etc...

## Accessing elements in the view

Using `document` is discouraged, we cannot rely in it since it's not webworker safe. We [can't access the DOM within a WebWorker!](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)

To access elements in the view safely we can use 

`@ViewChild/@ViewChildren` with local variables or types.

Using local variables

```js
@Component({
	selector : 'my-app',
	template : `
		<div #myDiv>Content inside my div</div>
	`
})
class MyApp {
	@ViewChild('myDiv') myDiv: HTMLElement;

	ngAfterViewInit() {
		// this.myDiv
	}
}

``` 

Using types. Create a `@Directive` and attach it to the element

```js
@Directive({
	selector : 'div'
})
class MyDiv {}

@Component({
	selector : 'my-app',
	template : `
		<div>Content inside my div</div>
	`,
	directives : [MyDiv]
})
class MyApp {
	@ViewChild(MyDiv) myDiv: MyDiv;

	ngAfterViewInit() {
		// this.myDiv
	}
}

``` 


## Using @Injectable() in services

Have you ever seen the error message `Cannot resolve all parameters for MyService(?). Make sure they all have valid type or annotations.`? Well, that's because you missed @Injectable()! Why do we need it really?

>TypeScript generates metadata when the `emitDecoratorMetadata` option is set. However, that doesn’t mean that it generates metadata blindly for each and every class or method of our code. **TypeScript only generates metadata for a class, method, property or method/constructor parameter when a decorator is actually attached to that particular code**. Otherwise, a huge amount of unused metadata code would be generated, which not only affects file size, but it’d also have an impact on our application runtime.

Portion extracted from [Injecting services in services in Angular 2](http://blog.thoughtram.io/angular/2015/09/17/resolve-service-dependencies-in-angular-2.html) by Pascal Precht.

Do we really need @Injectable()? Not really, we could use any annotation, the point is to emit metadata and for that we could use any of them! 

This would work as well

```javascript
class MyService {
	constructor(@Inject(Http) http: Http) {}
}
```

But the best practice is to use @Injectable()

```javascript
@Injectable()
class MyService {
	constructor(http: Http) {}
}
```


Angular2 is promoting [using @Injectable()](https://github.com/angular/angular.io/pull/1012#issuecomment-202326319) as a best practice, so the best option is to follow their recommendations.



## DynamicComponentLoader and loadAsRoot

By @gdi2290, see his [comment](https://github.com/angular/angular/issues/6223#issuecomment-193908465)
>loadAsRoot is used during [bootstrap](https://github.com/angular/angular/blob/2.0.0-beta.8/modules/angular2/src/core/application_ref.ts#L50) where angular is wiring everything up (di, cd etc) and by design it doesn't wire anything up that was rendered on the server (security reasons).

So basically you shouldn't use `loadAsRoot` at least you're rewriting the bootstrap logic, taking in consideration the quote above. Use `loadNextToLocation` or `loadIntoLocation` instead.