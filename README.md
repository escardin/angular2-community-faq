# Angular 2 Community FAQ

## [When will Angular 2 be released?](angular2readiness.md#when-will-angular-2-be-released)

## [Is Angular 2 Ready for me?](angular2readiness.md#is-it-ready-for-me)


## Components

### How do I communicate between two sibling components?
There are two common approaches; Use a shared service, or have a common parent pass messages between the two.

[Shared service example](services.md#how-do-i-communicate-between-components-using-a-shared-service)

## Router

### Any good tutorials for the Router?
Not yet, in the meantime you can look at this: https://www.youtube.com/watch?v=z1NB-HG0ZH4

You can also take a look at this Plunkr, which while complex, does just about everything you would want with the router. http://plnkr.co/edit/Bim8OGO7oddxBaa26WzR?p=preview

### How do I do breadcrumbs with the Router?

See: http://plnkr.co/edit/4cw2fPv3vX36v5Lu9Dnq?p=preview

### How do I do make the Router components and directives available for every component?

See: http://plnkr.co/edit/FmMBasgv1rC1Qs6sJTMA?p=preview

## [Redux with Rx and Angular 2](https://github.com/ngrx/store)

## RXJS

### [Observable.map() (etc...) doesn't work!](rxjs_operators.md)

### What are observables and where can I learn more about them and Rx?

- You may want to read this introduction: https://gist.github.com/staltz/868e7e9bc2a7b8c1f754
- For visual examples of Rx see: [Rx Marbles](http://rxmarbles.com/)
- [RxJS 4 operator documentation with examples](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators) (applies reasonably to RxJS 5 in Angular 2)
- [Filtering data using observables and form inputs](http://plnkr.co/edit/CTpE1DtaVzk1JU5eQWBu?p=preview)
- [Drag and drop list (similar to JqueryUI sortable list)](http://plnkr.co/edit/LD5FJaI4OOFbKfvhjD4e?p=preview)
- Non Free resources:
  - https://egghead.io/series/introduction-to-reactive-programming
  - https://pragprog.com/book/smreactjs/reactive-programming-with-rxjs


## Forms

### Validation

#### How do I use more than one validator on a single control?

See: https://plnkr.co/edit/5yO4HviXD7xIgMQQ8WKs?p=preview

#### How do I make a custom validator?

See: https://plnkr.co/edit/5yO4HviXD7xIgMQQ8WKs?p=preview
