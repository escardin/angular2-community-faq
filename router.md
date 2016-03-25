# Table Of Contents
- [How do I protect routes?](#protecting-routes)
- [How do I resolve data before a route activates?](#resolving-route-data)
- [How do I handle invalid URLs?](#handling-invalid-urls)
- [How do I add classes to elements that contain an active router link?](#adding-classes-to-elements-containing-active-router-links)
- [How do I add optional parameters for a route?](#optional-parameters)
- [How do I make a breadcrumb component?](#breadcrumb-component)
- [How do I make router components and directives available for every component?](#global-router-directives)
- [How do I use regular expressions for a route?](#router-regex-serializer)
- [How do I dynamically configure routes](#router-dynamic-config)

## Protecting routes

Routes are protected using the `CanActivate` decorator. Dependency Injection is not available within the CanActivate function without a workaround.

http://plnkr.co/edit/SF8gsYN1SvmUbkosHjqQ?p=preview

## Resolving route data

Resolving route data has not been implemented yet. As a workaround you can use the Dependency Injection workaround used with `CanActivate`, a custom router outlet, and a service to add providers for the route.

http://plnkr.co/edit/SCNA0P?p=preview

## Optional parameters

Any parameter that is not defined in your route `path` is considered optional and will be added to the URL as a query string parameter for top level routes or a matrix parameter for child routes.

```js
@RouteConfig([
  { path: '/users/:id', component: Users, name: 'Users' }
])
class AppComponent{}  
```

```html
  <a [routerLink]="['/Users', {id: 'John', name: 'Smith'}]">John Smith</a>
```

## Handling Invalid URLs

This feature has not been implemented yet, but you can use a route with a wildcard parameter to redirect another route

```js
@RouteConfig([
  { path: '/404', component: NotFound, name: 'NotFound' },
  { path: '/**', redirectTo: ['/NotFound'] }
])
class AppComponent{}
```

## Adding classes to elements containing active router links

This can be accomplished with a custom directive that finds the first `routerLink` and if it becomes active, it will add the class the element.

```js
import {isPresent, isBlank} from 'angular2/src/facade/lang';
import {Directive, Input, Query, QueryList, Renderer, ElementRef} from 'angular2/core';
import {Router, RouterLink} from 'angular2/router';

@Directive({ selector: '[routerActive]' })
export class RouterActive {
  @Input('routerActive') activeClass;

  constructor(
    @Query(RouterLink) public routerLinks:QueryList<RouterLink>,
    public element: ElementRef,
    public router: Router,
    public renderer: Renderer
  ) {}

  ngOnInit() {
    this.router.root.subscribe((_) => {
      this.checkActive();
    });    
  }

  ngAfterViewInit() {
    if (!this.activeClass) {
      this.activeClass = 'router-link-active';
    }

    this.checkActive();
  }

  checkActive() {
    let active = this.routerLinks.first.isRouteActive;
    this.renderer.setElementClass(this.element.nativeElement, this.activeClass, active);
  }
}
```

```js
import {Component} from 'angular2/core';
import {RouteConfig} from 'angular2/router';
import {RouterActive} from './RouterActive';

@Component({
  selector: 'my-app',
  template: `
  <nav>
    <ol>
      <li linkActive="active">
        <a [routerLink]="['Home']">Home</a>
      </li>
      <li linkActive="active">
        <a [routerLink]="['Away']">Away</a>
      </li>
    </ol>
  </nav>
  <router-outlet></router-outlet>`,
  directives: [ROUTER_DIRECTIVES, RouterActive]
})
@RouteConfig([
  { path: '/', component: Home, name: 'Home' },
  { path: '/about', component: About, name: 'About' }
])
export class AppComponent {}
```

## Breadcrumb component

http://plnkr.co/edit/4cw2fPv3vX36v5Lu9Dnq?p=preview

## Global router directives

Add the router components and directives to the platform directives. This will allow you to use the `routerLink` and `router-outlet` directives without adding ROUTER_DIRECTIVES to each component.

```js
import {bootstrap} from 'angular2/platform/browser';
import {App} from './app';
import {provide, PLATFORM_DIRECTIVES} from 'angular2/core';

bootstrap(App, [
  provide(PLATFORM_DIRECTIVES, {useValue: [ROUTER_DIRECTIVES], multi: true})
]);
```

## Router regex serializer

To use regular expressions for route matching, you provide a regular expression and a serializer that will return the generated url.

```js
@Component({
  selector: 'regex-route',
  template: `Regex Route`
})
class RegexRoute{}

@RouteConfig([
  { name : 'RegexRoute',
    regex: '^/test/(.+)/(.+)$', // matches /test/:parama/:paramb
    serializer: (params) => new GeneratedUrl(`/${params.a}/${params.b}`, {}),
    component: RegexRoute
  }
])
export class App {}
```

```html
  <a [routerLink]="['RegexRoute', {a: 'param1', b: 'param2'}]">Regex Route</a>
```

## Router dynamic config

To generate a route config dynamically, inject the router into your main app component and configure the routes .

```js
import {Router} from 'angular2/router';

@Component({...})
export class AppComponent {
  constructor(router: Router) {
    let routes = [];

    if (someCondition) {
      routes = [
        { path: '/', component: Home, name: 'Home' },
        { path: '/about', component: About, name: 'About' }
      ]);
    } else {
      routes = [
        { path: '/home', component: Home, name: 'Home', useAsDefault: true },
        { path: '/away', component: Away, name: 'Away' }
      ]);
    }

    router.config(routes);
  }
}
```
