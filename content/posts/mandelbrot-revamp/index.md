---
title: Mandelbrot Parallelized
date: 2022-03-06 00:00:00+0000
description: Complete revamp of the mandelbrot parallelization codebase
image: cover.png
slug: mandelbrot-revamp
categories:
    - Mandelbrot
    - Math
tags:
    - C++
weight: 1
---

As I had mentioned in my past blog about the [*Mandelbrot Research
Repo*](mandelbrot.md) "I've refactored this piece of code like 
6140275936420759697 times...", so I guess that makes it "6140275936420759697"
plus one.

This time around I finalized the implementation for a `Canvas` class that
collects all the pixels you want and writes to a file you give it.
By making it a class it is now much easier to work on the underlying byte buffer
without having to get your hands dirty.

```cpp
template <size_t width, size_t height> class Canvas;
```
The only downside to this implementation is the fact that this class will get
allocated on the stack so for any render of 1.5k+ pixels in both dimensions ends
up overflowing most computer's stacks nowadays.
The way to get around this is by declaring the canvas object as a `static`
duration object so that it gets stored into storage and not the actual stack.
```cpp
	static auto canvas = Canvas<2000, 2000>();
```

One cool thing about this implementation is the way you can use lambdas to
modify the pixels isnide the byte buffer.
For example, with the new `construct(auto op)` method you can simply pass a
function pointer (though realistically, you would only pass a lambda) and the
`construct` method applies that function to each pixel in the canvas.
```cpp
	auto mandelbrot = [](std::complex<long double> coordinate) {
		const auto max_iteration = 20000ul;
		auto x = 0.0L;
		auto y = 0.0L;
		auto x_squared = 0.0L;
		auto y_squared = 0.0L;
		auto iteration = 0u;

		while (std::islessequal((x_squared + y_squared), 4) &&
		       (iteration < max_iteration)) {
			y = fma(2 * x, y, coordinate.imag());
			x = x_squared - y_squared + coordinate.real();
			x_squared = x * x;
			y_squared = y * y;
			iteration++;
		}
		return Color(iteration);
	};

	canvas.construct(mandelbrot);
```

This can then be parallelized using [OpenMP](https://openmp.org/specifications/)
with a range-based for-loop.
```cpp
	auto construct(auto op) -> void
	{
#pragma omp parallel for
		for (size_t idx = 0; idx < width * height; idx++)
			pixels[idx] = op(coordinate(idx));
	}
```

One final fix that's pending is being able to do histogram coloring.
This can (probably) be easily implemented by just changing the `return
Color(iteration)` part at the end of that lambda.
But I haven't found a convenient way of doing that yet.
Furthermore, it would be good to support different scaled images.
The rendered image for this implementation breaks if `width != height`.

# Links:
 - [mandelbrot gamma implementation](https://github.com/Sunglas/mandelbrot/tree/main/c%2B%2B/mandelbrot-gamma)
