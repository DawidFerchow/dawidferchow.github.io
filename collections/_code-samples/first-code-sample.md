---
layout: post
title: Test
desc: should work because yes
---

# Language Test

Mauris viverra dictum ultricies. Vestibulum quis ipsum euismod, facilisis metus sed, varius ipsum. Donec scelerisque lacus libero, eu dignissim sem venenatis at. Etiam id nisl ut lorem gravida euismod.

## Javascript
```javascript
const path = require('path');
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  mode: 'development',
  devtool: 'inline-source-map',
  devServer: {
    writeToDisk: true,
    contentBase: path.join(__dirname, 'dist'),
    publicPath: path.join(__dirname, 'dist'),
    compress: true,
    port: 8000,
  },
});
```

## Sass
```scss
$backgrounds: elliptical, bridge, stretch, jetski, beach, hula, pier, yoga, town, market, runner, bike;
$breakpoint-sm: 750px;

@each $background in $backgrounds {
    str-slice($background, 11);
	.#{$background} {
		background-image: url('/wp-content/uploads/2022/08/#{$background}.jpg');

		@media screen and (max-width: $breakpoint-sm) {
			background-image: url('/wp-content/uploads/2022/08/#{$background}-mobile.jpg');
		}
	}
}

$custom: asd(111px);
```
## PHP
```php
use Random\Engine\Xoshiro256StarStar;
use Random\Randomizer;

$blueprintRng = new Xoshiro256StarStar(
    hash('sha256', "Example seed that is converted to a 256 Bit string via SHA-256", true)
);

$fibers = [];
for ($i = 0; $i < 8; $i++) {
    $fiberRng = clone $blueprintRng;
    // Xoshiro256**'s 'jump()' method moves the blueprint ahead 2**128 steps, as if calling
    // 'generate()' 2**128 times, giving the Fiber 2**128 unique values without needing to reseed.
    $blueprintRng->jump();

    $fibers[] = new Fiber(function () use ($fiberRng, $i): void {
        $randomizer = new Randomizer($fiberRng);

        echo "{$i}: " . $randomizer->getInt(0, 100), PHP_EOL;
    });
}

// The randomizer will use a CSPRNG by default.
$randomizer = new Randomizer();

// Even though the fibers execute in a random order, they will print the same value
// each time, because each has its own unique instance of the RNG.
$fibers = $randomizer->shuffleArray($fibers);
foreach ($fibers as $fiber) {
    $fiber->start();
}
```