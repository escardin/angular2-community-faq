# Loading operators individually for RxJS + Angular2 + SystemJS

This is a simple change, but it has caused a lot of headaches in people. So this explanaition will be super simple and straightfoward.

This guide assumes you have the latest version of Angular2, RxJS 5 and SystemJS.

## Configure SystemJS

In your index.html your System.config should look like this

### Configuration for individual observables / operators 

# Simpler configuration

```
System.config({
	map: {
	  rxjs: 'node_modules/rxjs'
	},
	packages: {
	  rxjs: { defaultExtension: 'js' }
	}
}
```

# Alternative configurations

```
System.config({
	paths: {
		'rxjs/add/observable/*' : 'node_modules/rxjs/add/observable/*.js',
		'rxjs/add/operator/*' : 'node_modules/rxjs/add/operator/*.js',
		'rxjs/*' : 'node_modules/rxjs/*.js'
	}
});
```

After you're done with this config, you can import them 

```typescript
import {Observable} from 'rxjs/Observable';
// No need to specify the operator, just import and it will autopatch
import 'rxjs/add/operator/map';
import 'rxjs/add/observable/from';

Observable.from([1,2,3]).map(val => val+1).subscribe((d) => console.log(d));
```

### Configuration for full RxJS

```
System.config({
	map : {
		'rxjs' : 'node_modules/rxjs/Rx.js'
	}
});
```

With this configuration you don't need to import each operator / observable individually.

```typescript
import {Observable} from 'rxjs';

Observable.from([1,2,3]).map(val => val+1).subscribe((d) => console.log(d));
```


### Reference

- Issue [RxJS #843](https://github.com/ReactiveX/RxJS/pull/843)
- Issue [ng2 #5632](https://github.com/angular/angular/issues/5632)
- Issue [ng2 #5749](https://github.com/angular/angular/issues/5749)
- plnkr with code example http://plnkr.co/edit/jzd0SF70GBk1hrSsp4M8 (not yet working)
