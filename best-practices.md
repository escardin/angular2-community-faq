# Table of contents

- [Proper use of EventEmitters](#proper-use-of-eventemitters)
- [ElementRef, nativeElement and Renderer](#elementref-nativeelement-and-renderer)
- [Don't use Subjects](#dont-use-subjects)
- [Don't mix angular2 with 3rd libs](#dont-mix-angular2-with-3rd-libs)
- [Don't use `ngAfterViewChecked` nor `ngAfterContentChecked`](#dont-use-ngafterviewchecked-nor-ngaftercontentchecked)
- [Accessing elements in the view](#accessing-elements-in-the-view)
- [Using @Injectable() in services](#using-injectable-in-services)
- ~~[DynamicComponentLoader and loadAsRoot](#dynamiccomponentloader-and-loadasroot)~~ _DynamicComponentLoader has been deprecated, see [5297c9d9](https://github.com/angular/angular/commit/5297c9d9ccc6f8831d1656915e3d78e767022517)_

## Proper use of EventEmitters

While EventEmitter extends from Subject, giving us the ability to subscribe, you should not count on this staying. [Ward Bell has stated](http://www.bennadel.com/blog/3038-eventemitter-is-an-rxjs-observable-stream-in-angular-2-beta-6.htm#comments_47949) that it is highly likely to be removed before release. EventEmitter is intended to ONLY be used with `@Output()`. If you want to subscribe just use a Subject directly.

See more 
- [What is the proper use of an EventEmitter?](http://stackoverflow.com/questions/36076700/what-is-the-proper-use-of-an-eventemitter)

## ElementRef, nativeElement and Renderer

Official documentation recommends against using `nativeElement` directly. The reason for this is because it is not WebWorker safe (It may also not work with Universal, and alternate renderer implementaitons). The preferred approach is to use the `nativeElement` with the Renderer. Of course, if you don't plan on using any of the cool stuff mentioned above, and are only targeting desktop browsers, then you should feel free to do whatever works; sometimes that's the only option.

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

Subjects are a really powerful concept, they're an Observable that can also emit events. This power is easily abused though, and as such, it is better to try and avoid using Subjects whenever possible.

There is a lot of discussion on why using Subjects is a bad idea, and the intent here is merely to be a summary. If you wish to learn more about subjects and why you shouldn't use them, please see the further reading at the end of this section.

The gist of it is that Subjects are uncontrolled inputs to your observable sequence. You could be expecting numbers, and get a string instead. Controlling the input to your sequence is very important. This is why whenever possible you want to use an Observable helper instead of a Subject. Subjects are also hot, meaning that subcribers will only receive new events and not historical ones.

Please also keep in mind, that if you use Observable.create and then hoist the input out of the create call, you have just made a subject.

### What should I use instead?

First and foremost, if you already have an Observable, or something that can easily be observed (Controls, Events, etc..) you should just observe it, and extend the processing chain to meet your needs (making it cold or hot, etc..).

Once you have exhausted all of your options for Obervable.x besides create and Subject, then you must if your entire sequence can be determined at creation. If this is the case, then use Observable.create, otherwise use a Subject.

Further Reading:
- (http://tomstechnicalblog.blogspot.ca/2016/03/rxjava-problem-with-subjects.html)
- (http://davesexton.com/blog/post/To-Use-Subject-Or-Not-To-Use-Subject.aspx)
- (https://medium.com/@benlesh/learning-observable-by-building-observable-d5da57405d87#.12a0rfxtt)
- (https://medium.com/@benlesh/hot-vs-cold-observables-f8094ed53339#.x0q63ntrm)


## Don't mix Angular 2 with third party libraries

Angular 2 works far better with third party libraries than Angular 1 did. In many cases, a library will work just fine with Angular 2. The biggest reasons not to use third party libraries is that they simply will not work optimally with Angular 2.

Most javascript libraries rely on `addEventListener`, `setTimeouts` and others that trigger change detection in Angular 2. Triggering, or listening to events of 3rd party libs can trigger excesive change detection, and considerably decrease the perfomance of your application.

On the other hand, these libraries can rely on event handling not patched by angular2, therefore these events will run outside angular2's zone causing change detection NOT to run!

Additionally, Angular 2 wants templates to be compiled at build time, and not include the compiler in your redistributable (improving speed and load time). This means that while it is possible to use third party libraries in your templates, it is not possible to compile angular templates in your third party libs, as that would require exposing the compiler and bundling it.

With all of that said, libraries that don't manipulate the dom, and don't need to be patched to work with zones can and do work perfectly with Angular 2, without patching or modification.

If some cool library hasn't been ported to angular2, be the first to port it! Make yourself famous!

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

So basically you shouldn't use `loadAsRoot` unless you're rewriting the bootstrap logic, taking into consideration the quote above. Use `loadNextToLocation` or `loadIntoLocation` instead.
