#Table of contents
- [Adding operators (map is not defined)](#adding-operatorsobservables-map-is-not-defined)
    - [Full RxJS](#full-rxjs)
    - [Individual Operators](#individual-operators)
- [Loading operators individually for RxJS + Angular2 + SystemJS](#loading-operators-individually-for-rxjs--angular2--systemjs)
    - [Configuration for full RxJS](#configuration-for-full-rxjs)
    - [Configuration for individual observables / operators](#configuration-for-individual-observables--operators)
        - [Simpler configuration](#simpler-configuration)
        - [Alternative configuration](#alternative-configuration)
    - [Reference](#reference)
- [What are observables and where can I learn more about them and RxJS?](#what-are-observables-and-where-can-i-learn-more-about-them-and-rxjs)
- [Redux + Rx + Angular2 / ngrx](#redux--rx--angular2--ngrx)
    - [Redux](#redux)
    - [ngrx](#ngrx)

# Adding operators/observables (map is not defined)

Due to the number of operators, the size of RxJS is very large. As such, they have provided a method to trim down the size when bundling where operators are added as needed.
If you are using SystemJS, some further configuration is needed beyond what is discussed in this section. See:[Loading operators individually for RxJS + Angular2 + SystemJS](#loading-operators-individually-for-rxjs--angular2--systemjs)

## Full RxJS
If you want to enable the full functionality of RxJS and don't care about bundle size, you can do:
```typescript
import from 'rxjs/Rx';
import {Observable} from 'rxjs/Observable';

Observable.of(1,2,3);
```
or (from the RxJS docs)
```typescript
import Rx from 'rxjs/Rx';

Rx.Observable.of(1,2,3)
```
## Individual Operators
If you wish to minimize your bundle size you can import individual operators and observables:
```typescript
import {Observable} from 'rxjs/Observable';
// No need to specify the operator, just import and it will autopatch
import 'rxjs/add/operator/map';
import 'rxjs/add/observable/from';

Observable.fromArray([1,2,3]).map(val => val+1).subscribe((d) => console.log(d));
```
It is suggested that operators and observables be added in your bootstrap or similar to avoid isses with operators not existing when you add them in a different class from where the Observable is created. 

Further Reading: https://github.com/ReactiveX/RxJS#installation-and-usage
   
# Loading operators individually for RxJS + Angular2 + SystemJS

This is a simple change, but it has caused a lot of headaches in people. So this explanaition will be super simple and straightfoward.

This guide assumes you have the latest version of Angular2, RxJS 5 and SystemJS.

In your index.html your System.config should look like this

## Configuration for full RxJS

```javascript
System.config({
	map : {
		'rxjs' : 'node_modules/rxjs/Rx.js'
	}
});
```

With this configuration you don't need to import each operator / observable individually ([Full RxJS](#full-rxjs))

## Configuration for individual observables / operators
Note that these are largely equivalent 

### Simpler configuration

```javascript
System.config({
	map: {
	  rxjs: 'node_modules/rxjs'
	},
	packages: {
	  rxjs: { defaultExtension: 'js' }
	}
}
```

### Alternative configuration

```javascript
System.config({
	paths: {
		'rxjs/add/observable/*' : 'node_modules/rxjs/add/observable/*.js',
		'rxjs/add/operator/*' : 'node_modules/rxjs/add/operator/*.js',
		'rxjs/*' : 'node_modules/rxjs/*.js'
	}
});
```
After you're done with this config, you can import them as in [Individual Operators](#individual-operators)

## Reference

- Issue [RxJS #843](https://github.com/ReactiveX/RxJS/pull/843)
- Issue [ng2 #5632](https://github.com/angular/angular/issues/5632)
- Issue [ng2 #5749](https://github.com/angular/angular/issues/5749)
- [plnkr with code example](http://plnkr.co/edit/jzd0SF70GBk1hrSsp4M8)


# What are observables and where can I learn more about them and RxJS?

- You may want to read this introduction: https://gist.github.com/staltz/868e7e9bc2a7b8c1f754
- For visual examples of Rx see: [Rx Marbles](http://rxmarbles.com/)
- [RxJS 4 operator documentation with examples](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators) (applies reasonably to RxJS 5 in Angular 2)
- [Filtering data using observables and form inputs](http://plnkr.co/edit/CTpE1DtaVzk1JU5eQWBu?p=preview)
- [Drag and drop list (similar to JqueryUI sortable list)](http://plnkr.co/edit/LD5FJaI4OOFbKfvhjD4e?p=preview)
- Youtube Talks
  - Rob Wormald
    - [Everything is a Stream](https://www.youtube.com/watch?v=UHI0AzD_WfY)
    - [Angular 2 Data Flow - Jeff Cross, Rob Wormald and Alex Rickabaugh](https://www.youtube.com/watch?v=bVI5gGTEQ_U)
    - [Angular 2 Data Flow - Rob Wormald](https://vimeo.com/144625829)
- Non Free resources:
  - https://egghead.io/series/introduction-to-reactive-programming
  - https://pragprog.com/book/smreactjs/reactive-programming-with-rxjs

# Redux + Rx + Angular2 / ngrx

I suggest you look at [ngrx](https://github.com/ngrx) before going with redux proper, as it does the same sort of job. That is not to say that redux is bad or inappropriate, but if you're not already invested you may find it easier.

## Redux
- [ng2+redux guide](http://www.syntaxsuccess.com/viewarticle/redux-in-angular-2.0) (Thanks @baloodevil)
- It's out of scope for this faq, but a guide for Redux in general and why it is useful: [Egghead.io getting started with redux](https://egghead.io/series/getting-started-with-redux)

## ngrx
- [store](https://github.com/ngrx/store)
- [store example](https://github.com/ngrx/angular2-store-example)
    - [refactoring underway](https://github.com/fxck/angular2-store-example/tree/fxck-refactoring)
- [examples](https://github.com/btroncone/ngrx-examples) (Thanks @btroncone)
- [docs from @btroncone](https://github.com/btroncone/ngrx.github.io)

