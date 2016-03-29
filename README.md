# Angular 2 Community FAQ

#Table of Contents

## [General Angular 2 Questions](angular2readiness.md)
- [When will Angular 2 be released?](angular2readiness.md#when-will-angular-2-be-released)
- [Seriously though, can I get an estimate?](angular2readiness.md#seriously-though-can-i-get-an-estimate)
- [Is it ready for me?](angular2readiness.md#is-it-ready-for-me)
- [Beta](angular2readiness.md#beta)
- [Stability/Bugginess](angular2readiness.md#stabilitybugginess)
    - [Known Regressions](angular2readiness.md#known-regressions)
- [API Stability](angular2readiness.md#api-stability)
- [Documentation](angular2readiness.md#documentation)
- [Ecosystem](angular2readiness.md#ecosystem)
- [Community/Blogs/Support/Etc...](angular2readiness.md#communityblogssupportetc)
- [Do I have to use Typescript?](angular2readiness.md#do-i-have-to-use-typescript)
- [Do I have to use a build system? What build system should I use?](angular2readiness.md#do-i-have-to-use-a-build-system-what-build-system-should-i-use)
    - [Router](angular2readiness.md#router)
    - [Internationalization](angular2readiness.md#internationalization)
    - [Animation](angular2readiness.md#animation)
    - [Twitter Bootstrap/Material Design](angular2readiness.md#twitter-bootstrapmaterial-design)

##Setting up Angular 2 with...
   - SystemJS
   - Webpack
   - JSPM
   - Rollup
   - ES5/ES6 (no ts)
   - Others (are there any others?)

##[RxJS](rxjs.md)
- [Adding operators (map is not defined)](rxjs.md#adding-operatorsobservables-map-is-not-defined)
    - [Full RxJS](rxjs.md#full-rxjs)
    - [Individual Operators](rxjs.md#individual-operators)
- [Loading operators individually for RxJS + Angular2 + SystemJS](rxjs.md#loading-operators-individually-for-rxjs--angular2--systemjs)
    - [Configuration for full RxJS](rxjs.md#configuration-for-full-rxjs)
    - [Configuration for individual observables / operators](rxjs.md#configuration-for-individual-observables--operators)
        - [Simpler configuration](rxjs.md#simpler-configuration)
        - [Alternative configuration](rxjs.md#alternative-configuration)
    - [Reference](rxjs.md#reference)
- [What are observables and where can I learn more about them and RxJS?](rxjs.md#what-are-observables-and-where-can-i-learn-more-about-them-and-rxjs)
- [Redux + Rx + Angular2 / ngrx](rxjs.md#redux--rx--angular2--ngrx)
    - [Redux](rxjs.md#redux)
    - [ngrx](rxjs.md#ngrx)

##[Router](router.md)
- [How do I protect routes?](router.md#protecting-routes)
- [How do I resolve data before a route activates?](router.md#resolving-route-data)
- [How do I handle invalid URLs?](router.md#handling-invalid-urls)
- [How do I add classes to elements that contain an active router link?](router.md#adding-classes-to-elements-containing-active-router-links)
- [How do I add optional parameters for a route?](router.md#optional-parameters)
- [How do I make a breadcrumb component?](router.md#breadcrumb-component)
- [How do I make router components and directives available for every component?](router.md#global-router-directives)
- [How do I use regular expressions for a route?](router.md#router-regex-serializer)
- [How do I dynamically configure routes](router.md#router-dynamic-config)

##[Forms](#forms-1)  
- [Validation](#validation)
    - [Multiple Validators](#how-do-i-use-more-than-one-validator-on-a-single-control)
    - [Custom validation](#how-do-i-make-a-custom-validator)
    - [Async Validators](#how-do-i-make-an-async-validator)
    - [Dependent Validators](#how-do-i-validate-a-control-based-on-the-value-in-another-control)
- [Complex Forms](#complex-forms)
    - [Dependent Inputs](#how-do-i-have-a-select-box-that-is-dependent-on-another-select-box)
    - [Form elements in sub components](#how-do-i-have-form-elements-in-sub-components)

##Templates
- [How do I make n elements in an *ngFor without a model?](https://plnkr.co/edit/FTPFoylc8s8pEiVRMoB9?p=preview)

##[Services and Components](services.md)
- [How do I create a service?](services.md#how-do-i-create-a-service)
- [How do I share a service instance between multiple components?](services.md#how-do-i-share-a-service-instance-between-multiple-components)
- [How do I communicate between two sibling components?](services.md#how-do-i-communicate-between-two-sibling-components)
- [How do I communicate between components using a shared service?](services.md#how-do-i-communicate-between-components-using-a-shared-service)

##Testing
- [How do I test an error path with Http and MockBackend?](testing-http-services.md)

##[Making fun things](cool_stuff.md)
- Drag and Drop
- Other fun things...


# Router

## Any good tutorials for the Router?
Not yet, in the meantime you can look at this: https://www.youtube.com/watch?v=z1NB-HG0ZH4

You can also take a look at this Plunkr, which while complex, does just about everything you would want with the router. http://plnkr.co/edit/Bim8OGO7oddxBaa26WzR?p=preview

## [How do I do breadcrumbs with the Router?](http://plnkr.co/edit/4cw2fPv3vX36v5Lu9Dnq?p=preview)

## [How do I make the Router components and directives available for every component?](http://plnkr.co/edit/FmMBasgv1rC1Qs6sJTMA?p=preview)

# Forms

## Validation

### [How do I use more than one validator on a single control?](https://plnkr.co/edit/5yO4HviXD7xIgMQQ8WKs?p=preview)

### [How do I make a custom validator?](https://plnkr.co/edit/5yO4HviXD7xIgMQQ8WKs?p=preview)

### [How do I make an async validator?](http://plnkr.co/edit/vlzDapiOgVWLNqltEbGb?p=preview)

### [How do I validate a control based on the value in another control?] (http://plnkr.co/edit/NqQhBPJJo1PzHfisvh9J?p=preview)

## Complex Forms

### [How do I have a select box that is dependent on another select box](http://plnkr.co/edit/VTCKxH82XVy6XswaiKbg?p=preview)

AKA: How do I make a state/province dropdown that changes contents when the country changes?

### [How do I have form elements in sub components](https://plnkr.co/edit/awfs0HGLkJOd6qdY8rhF?p=preview)
