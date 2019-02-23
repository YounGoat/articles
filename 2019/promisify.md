#	PoC: Design Elegant Asyschronous Functions In Node.js

On designing asynchronous functions, I used to make them *return a promise* OR *accept a callback function*. I call this __PoC__.

I use package __[undertake][0]__ to create a PoC asynchronous function:
```javascript
const undertake = require('undertake');

const add = undertake.async(function*(a, b) {
	// A promisified function.
	let fa = function(a) {
		return Promise.resolve(a);
	};

	//  A traditional asynchronous function.
	let fb = function(b, callback) {
		// Here `null` means no exception happens.
		callback(null, b);
	};

	let m = yield fa(a);
	let n = yield fb(b);
	let ret = m + n;
	return ret;
});

// If no callback offered, a promise will be returned.
add(1, 2)
	.then(data => {
		// ...
	}).catch(ex => {
		// ...
	});

// If callback found, it will be run.
add(1, 2, (ex, data) => {
	if (ex) {
		// ...
	}
	else {
		// ...
	}
});
```

Charged upon notorious [Callback Hell][5], tranditional asynchrous functions accepting arguments list ended with a callback function are not welcome. Now in 2019, __Promise__ and __async / await__ become popular. Since Node.js version 8.0.0, offical method [util.promisify(original)][1] is released to help transforming a traditional asynchronous function to a promisified one.

##	References

*	Sebastian Lindström, 
	[Getting to know asynchronous JavaScript: Callbacks, Promises and Async/Await][2], 
	2017.06.03
*	Job Vranish, 
	[Never Mix Promises and Callbacks in NodeJS][3], 
	2017.04.06
*	maxogden, [Callback Hell, A guide to writing asynchronous JavaScript programs][4], updated at 2016.02.05

[0]: https://www.npmjs.com/package/undertake
[1]: https://nodejs.org/dist/latest/docs/api/util.html#util_util_promisify_original
[2]: https://medium.com/codebuddies/getting-to-know-asynchronous-javascript-callbacks-promises-and-async-await-17e0673281ee
[3]: https://spin.atomicobject.com/2017/04/06/nodejs-promises-callbacks/
[4]: http://callbackhell.com/
[5]: https://en.wiktionary.org/wiki/callback_hell

[版权声明](../LICENSE/zh_cn.md) | [LICENSE](../LICENSE/en_us.md)