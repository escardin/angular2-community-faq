#Table of Contents

- [When will Angular 2 be released?](#when-will-angular-2-be-released)
- [Seriously though, can I get an estimate?](#seriously-though-can-i-get-an-estimate)
- [Is it ready for me?](#is-it-ready-for-me)
- [Beta](#beta)
- [Stability/Bugginess](#stabilitybugginess)
    - [Known Regressions](#known-regressions)
- [API Stability](#api-stability)
- [Documentation](#documentation)
- [Ecosystem](#ecosystem)
- [Community/Blogs/Support/Etc...](#communityblogssupportetc)
- [Do I have to use Typescript?](#do-i-have-to-use-typescript)
- [Do I have to use a build system? What build system should I use?](#do-i-have-to-use-a-build-system-what-build-system-should-i-use)
- [Router](#router)
- [Internationalization](#internationalization)
- [Animation](#animation)
- [Twitter Bootstrap/Material Design](#twitter-bootstrapmaterial-design)

# When will Angular 2 be released?

When it's ready.

# Seriously though, can I get an estimate?

Any estimate given will be wrong. A better question to ask might be 'is it ready for me?'

# Is it ready for me?

That depends. Some Google teams are already using Angular 2 in production. Let's break it down.

# Beta

The Angular 2 team has stated that what they have released is ready, and they hope to minimize any further breaking changes.
Read the release announcement: http://angularjs.blogspot.ca/2015/12/angular-2-beta.html

# Stability/Bugginess

Angular 2 is very well tested~~, and does not exhibit a lot of bugs~~. Their git hub page shows compatibility for all the browsers they support, and they do benchmarks to test that they have not slowed things down. If you were to stick with a given release of Angular 2 in beta, you could go to production with it successfully. That said, there are some known regressions.

## Known Regressions

Here are a number of known issues/regressions that the community has found affecting whether you could go to production with a given version of Angular 2

- Minification/mangling busted [#6380](https://github.com/angular/angular/issues/6380)
- Attribute and class bindings throw an error when declared inside ngForm elements in Beta.1 [6374](https://github.com/angular/angular/issues/6374)
- ngModel broken on select elements [#6573](https://github.com/angular/angular/issues/6573)
- Problems with zone.js in beta10/11 [#7660](https://github.com/angular/angular/issues/7660)
- beta.6: TypeError: viewFactory_<name>0 is not a function [#7037](https://github.com/angular/angular/issues/7037)
- beta.2: "Attempt to use a dehydrated detector" in routes, triggered by eventEmitter [#6786](https://github.com/angular/angular/issues/6786)

# API Stability

With the release of beta, the Angular 2 team is hoping not to make breaking API changes. Some are likely to happen, but hopefully it should not require sweeping changes to your app. The only known proposed api breakages at this time are the router, and the animation API, as the current animation api is said to be temporary.

# Documentation

The documentation is largely incomplete, and some examples are out of date. The good news is that the really critical stuff is reasonably documented, and Angular 2 is substantially simpler to learn than Angular 1.

# Ecosystem

The ecosystem is still just getting started. There will likely be a fair bit of work to catch up with Angular 1. That said, Angular 2 is largely free of the 'Angularisms' that made having Angular specific plugins a necessity. In many cases you can just use a vanilla js library with a thin component or directive wrapper you write yourself.

# Community/Blogs/Support/Etc...

The Angular 2 community is thriving, and a lot of information and tutorials can be found on the internet. The major problem is a lot of content covers the earlier alpha versions, and may not be directly applicable to the beta. The concepts are consistent from alpha to beta, and in most cases the only changes are small syntax changes.

If you need support, the best place to look is the Angular 2 gitter: https://gitter.im/angular/angular. There are lots of people there who can help you out.

# Do I have to use Typescript?

Technically, no. You can use whatever you like. It is strongly recommended that you use TypeScript though.

Documentation appears to be centered on TypeScript first, and the vast majority of examples are in it as well. Most users of Angular 2 have chosen to use Typescript, and that is what they will be familiar with.

The angular team is also developing a plugin for TypeScript that will add Intellisense type hinting for Angular 2 specific constructs, for any editor that supports TypeScript integration.

# Do I have to use a build system? What build system should I use?

It is STRONGLY RECOMMENDED that you use a build system such as Webpack or SystemJs. 

Using a build system is not required or critical to the functioning of your Angular 2 app, however it is the long term plan, and to get all of the benefits of Angular 2 you will need a compile step. There are lots of bootstrap templates floating around github. Start your project with one of those and you won't have too much trouble.

- Webpack Basic Starter: https://github.com/ocombe/ng2-webpack
- Webpack Advanced Starter: https://github.com/AngularClass/angular2-webpack-starter
- SystemJs Basic Starter: https://github.com/pkozlowski-opensource/ng2-play 
- SystemJs Advanced Starter: https://github.com/mgechev/angular2-seed

# Router

The Angular 2 router has some serious gaps at this time. Most of them have been worked around in some fashion in the Router section of this FAQ. It is stable, though there is a Proposed API change.

# Internationalization

As stated in the beta announcement, Internationalization is not yet complete. If you need this and are not willing to write your own internationalization module and port to the released one when it is done, Angular 2 may not be ready for you yet.

# Animation

Angular 2's animation module is very rudimentary and the release version is not yet ready. You will have to write your own animations, or wait.

# Twitter Bootstrap/Material Design

Angular specific versions of these libraries are not yet available, though several users are using them successfully with some workarounds/caveats.
