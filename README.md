<!--

@license Apache-2.0

Copyright (c) 2024 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# saxpy

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Multiply a vector `x` by a constant `alpha` and add the result to `y`.



<section class="usage">

## Usage

```javascript
import saxpy from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-saxpy-wasm@esm/index.mjs';
```

You can also import the following named exports from the package:

```javascript
import { Module } from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-saxpy-wasm@esm/index.mjs';
```

#### saxpy.main( N, alpha, x, strideX, y, strideY )

Multiplies a vector `x` by a constant `alpha` and adds the result to `y`.

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@esm/index.mjs';

var x = new Float32Array( [ 1.0, 2.0, 3.0, 4.0, 5.0 ] );
var y = new Float32Array( [ 1.0, 1.0, 1.0, 1.0, 1.0 ] );

saxpy.main( x.length, 5.0, x, 1, y, 1 );
// y => <Float32Array>[ 6.0, 11.0, 16.0, 21.0, 26.0 ]
```

The function has the following parameters:

-   **N**: number of indexed elements.
-   **alpha**: scalar constant.
-   **x**: input [`Float32Array`][@stdlib/array/float32].
-   **strideX**: index increment for `x`.
-   **y**: input [`Float32Array`][@stdlib/array/float32].
-   **strideY**: index increment for `y`.

The `N` and stride parameters determine which elements in the strided arrays are accessed at runtime. For example, to multiply every other value in `x` by `alpha` and add the result to the first `N` elements of `y` in reverse order,

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@esm/index.mjs';

var x = new Float32Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );
var y = new Float32Array( [ 1.0, 1.0, 1.0, 1.0, 1.0, 1.0 ] );

var alpha = 5.0;

saxpy.main( 3, alpha, x, 2, y, -1 );
// y => <Float32Array>[ 26.0, 16.0, 6.0, 1.0, 1.0, 1.0 ]
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

<!-- eslint-disable stdlib/capitalized-comments -->

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@esm/index.mjs';

// Initial arrays...
var x0 = new Float32Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );
var y0 = new Float32Array( [ 7.0, 8.0, 9.0, 10.0, 11.0, 12.0 ] );

// Create offset views...
var x1 = new Float32Array( x0.buffer, x0.BYTES_PER_ELEMENT*1 ); // start at 2nd element
var y1 = new Float32Array( y0.buffer, y0.BYTES_PER_ELEMENT*3 ); // start at 4th element

saxpy.main( 3, 5.0, x1, -2, y1, 1 );
// y0 => <Float32Array>[ 7.0, 8.0, 9.0, 40.0, 31.0, 22.0 ]
```

#### saxpy.ndarray( N, alpha, x, strideX, offsetX, y, strideY, offsetY )

Multiplies a vector `x` by a constant `alpha` and adds the result to `y` using alternative indexing semantics.

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@esm/index.mjs';

var x = new Float32Array( [ 1.0, 2.0, 3.0, 4.0, 5.0 ] );
var y = new Float32Array( [ 1.0, 1.0, 1.0, 1.0, 1.0 ] );
var alpha = 5.0;

saxpy.ndarray( x.length, alpha, x, 1, 0, y, 1, 0 );
// y => <Float32Array>[ 6.0, 11.0, 16.0, 21.0, 26.0 ]
```

The function has the following additional parameters:

-   **offsetX**: starting index for `x`.
-   **offsetY**: starting index for `y`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying buffer, the offset parameters support indexing semantics based on starting indices. For example, to multiply every other value in `x` by a constant `alpha` starting from the second value and add to the last `N` elements in `y` where `x[i] -> y[n]`, `x[i+2] -> y[n-1]`,...,

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@esm/index.mjs';

var x = new Float32Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );
var y = new Float32Array( [ 7.0, 8.0, 9.0, 10.0, 11.0, 12.0 ] );

var alpha = 5.0;

saxpy.ndarray( 3, alpha, x, 2, 1, y, -1, y.length-1 );
// y => <Float32Array>[ 7.0, 8.0, 9.0, 40.0, 31.0, 22.0 ]
```

* * *

### Module

#### saxpy.Module( memory )

Returns a new WebAssembly [module wrapper][@stdlib/wasm/module-wrapper] instance which uses the provided WebAssembly [memory][@stdlib/wasm/memory] instance as its underlying memory.

<!-- eslint-disable node/no-sync -->

```javascript
import Memory from 'https://cdn.jsdelivr.net/gh/stdlib-js/wasm-memory@esm/index.mjs';

// Create a new memory instance with an initial size of 10 pages (640KiB) and a maximum size of 100 pages (6.4MiB):
var mem = new Memory({
    'initial': 10,
    'maximum': 100
});

// Create a BLAS routine:
var mod = new saxpy.Module( mem );
// returns <Module>

// Initialize the routine:
mod.initializeSync();
```

#### saxpy.Module.prototype.main( N, α, xp, sx, yp, sy )

Multiplies a vector `x` by a constant and adds the result to `y`.

<!-- eslint-disable node/no-sync -->

```javascript
import Memory from 'https://cdn.jsdelivr.net/gh/stdlib-js/wasm-memory@esm/index.mjs';
import oneTo from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-one-to@esm/index.mjs';
import ones from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-ones@esm/index.mjs';
import zeros from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-zeros@esm/index.mjs';
import bytesPerElement from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-base-bytes-per-element@esm/index.mjs';

// Create a new memory instance with an initial size of 10 pages (640KiB) and a maximum size of 100 pages (6.4MiB):
var mem = new Memory({
    'initial': 10,
    'maximum': 100
});

// Create a BLAS routine:
var mod = new saxpy.Module( mem );
// returns <Module>

// Initialize the routine:
mod.initializeSync();

// Define a vector data type:
var dtype = 'float32';

// Specify a vector length:
var N = 5;

// Define pointers (i.e., byte offsets) for storing two vectors:
var xptr = 0;
var yptr = N * bytesPerElement( dtype );

// Write vector values to module memory:
mod.write( xptr, oneTo( N, dtype ) );
mod.write( yptr, ones( N, dtype ) );

// Perform computation:
mod.main( N, 5.0, xptr, 1, yptr, 1 );

// Read out the results:
var view = zeros( N, dtype );
mod.read( yptr, view );

console.log( view );
// => <Float32Array>[ 6.0, 11.0, 16.0, 21.0, 26.0 ]
```

The function has the following parameters:

-   **N**: number of indexed elements.
-   **α**: scalar constant.
-   **xp**: input [`Float32Array`][@stdlib/array/float32] pointer (i.e., byte offset).
-   **sx**: index increment for `x`.
-   **yp**: input [`Float32Array`][@stdlib/array/float32] pointer (i.e., byte offset).
-   **sy**: index increment for `y`.

#### saxpy.Module.prototype.ndarray( N, α, xp, sx, ox, yp, sy, oy )

Multiplies a vector `x` by a constant and adds the result to `y` using alternative indexing semantics.

<!-- eslint-disable node/no-sync -->

```javascript
import Memory from 'https://cdn.jsdelivr.net/gh/stdlib-js/wasm-memory@esm/index.mjs';
import oneTo from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-one-to@esm/index.mjs';
import ones from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-ones@esm/index.mjs';
import zeros from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-zeros@esm/index.mjs';
import bytesPerElement from 'https://cdn.jsdelivr.net/gh/stdlib-js/ndarray-base-bytes-per-element@esm/index.mjs';

// Create a new memory instance with an initial size of 10 pages (640KiB) and a maximum size of 100 pages (6.4MiB):
var mem = new Memory({
    'initial': 10,
    'maximum': 100
});

// Create a BLAS routine:
var mod = new saxpy.Module( mem );
// returns <Module>

// Initialize the routine:
mod.initializeSync();

// Define a vector data type:
var dtype = 'float32';

// Specify a vector length:
var N = 5;

// Define pointers (i.e., byte offsets) for storing two vectors:
var xptr = 0;
var yptr = N * bytesPerElement( dtype );

// Write vector values to module memory:
mod.write( xptr, oneTo( N, dtype ) );
mod.write( yptr, ones( N, dtype ) );

// Perform computation:
mod.ndarray( N, 5.0, xptr, 1, 0, yptr, 1, 0 );

// Read out the results:
var view = zeros( N, dtype );
mod.read( yptr, view );

console.log( view );
// => <Float32Array>[ 6.0, 11.0, 16.0, 21.0, 26.0 ]
```

The function has the following additional parameters:

-   **ox**: starting index for `x`.
-   **oy**: starting index for `y`.

</section>

<!-- /.usage -->

<section class="notes">

* * *

## Notes

-   If `N <= 0` or `alpha == 0`, `y` is left unchanged.
-   This package implements routines using WebAssembly. When provided arrays which are not allocated on a `saxpy` module memory instance, data must be explicitly copied to module memory prior to computation. Data movement may entail a performance cost, and, thus, if you are using arrays external to module memory, you should prefer using [`@stdlib/blas-base/saxpy`][@stdlib/blas/base/saxpy]. However, if working with arrays which are allocated and explicitly managed on module memory, you can achieve better performance when compared to the pure JavaScript implementations found in [`@stdlib/blas/base/saxpy`][@stdlib/blas/base/saxpy]. Beware that such performance gains may come at the cost of additional complexity when having to perform manual memory management. Choosing between implementations depends heavily on the particular needs and constraints of your application, with no one choice universally better than the other.
-   `saxpy()` corresponds to the [BLAS][blas] level 1 function [`saxpy`][saxpy].

</section>

<!-- /.notes -->

<section class="examples">

* * *

## Examples

<!-- eslint no-undef: "error" -->

```html
<!DOCTYPE html>
<html lang="en">
<body>
<script type="module">

import discreteUniform from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-array-discrete-uniform@esm/index.mjs';
import saxpy from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-saxpy-wasm@esm/index.mjs';

var opts = {
    'dtype': 'float32'
};
var x = discreteUniform( 10, 0, 100, opts );
console.log( x );

var y = discreteUniform( x.length, 0, 10, opts );
console.log( y );

saxpy.ndarray( x.length, 5.0, x, 1, 0, y, -1, y.length-1 );
console.log( y );

</script>
</body>
</html>
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2024. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/blas-base-saxpy-wasm.svg
[npm-url]: https://npmjs.org/package/@stdlib/blas-base-saxpy-wasm

[test-image]: https://github.com/stdlib-js/blas-base-saxpy-wasm/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/blas-base-saxpy-wasm/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/blas-base-saxpy-wasm/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/blas-base-saxpy-wasm?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/blas-base-saxpy-wasm.svg
[dependencies-url]: https://david-dm.org/stdlib-js/blas-base-saxpy-wasm/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/blas-base-saxpy-wasm/tree/deno
[deno-readme]: https://github.com/stdlib-js/blas-base-saxpy-wasm/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/blas-base-saxpy-wasm/tree/umd
[umd-readme]: https://github.com/stdlib-js/blas-base-saxpy-wasm/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/blas-base-saxpy-wasm/tree/esm
[esm-readme]: https://github.com/stdlib-js/blas-base-saxpy-wasm/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/blas-base-saxpy-wasm/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/blas-base-saxpy-wasm/main/LICENSE

[blas]: http://www.netlib.org/blas

[saxpy]: http://www.netlib.org/lapack/explore-html/de/da4/group__double__blas__level1.html

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@stdlib/array/float32]: https://github.com/stdlib-js/array-float32/tree/esm

[@stdlib/wasm/memory]: https://github.com/stdlib-js/wasm-memory/tree/esm

[@stdlib/wasm/module-wrapper]: https://github.com/stdlib-js/wasm-module-wrapper/tree/esm

[@stdlib/blas/base/saxpy]: https://github.com/stdlib-js/blas-base-saxpy/tree/esm

</section>

<!-- /.links -->
