# scss-color-var
This package allows you to create color variables that can be related to each other - as well as modified using CSS variables and the `calc()` CSS function.

Simple example

```scss
// src/styles/themes/light.scss
@use `scss-color-var/var.scss`;
@use 'scss-color-var/v.scss' as *;

body {
	var.Colors(
		$text-body: hsl(0, 0%, 40%),
		$primary: hsl(115, 78%, 30%),
		
						// References all HSL values of primary
		$primary-text: (primary),
						//    H        S    L
		$primary-background: (primary, 29%, 97%)
	);

	--padding: 20px;
	padding: v(padding); // -> var(--padding);
	
	color: c(text-body); // -> var(--color-text-body);
	color: var.Color(text-body); // -> var(--color-text-body);
	--hover: #{c(primary)}; // --hover: --color-primary;
}

```

## Usage note
When assigning to variables, use `#{ }` as [SCSS documents here](https://sass-lang.com/documentation/breaking-changes/css-vars).

```scss
--some-color: #{c(primary-text)}; // --some-color: --color-primary-text;
--some-color: c(primary-text); // --some-color: c(primary-texŧ); ❌ Invalid CSS
```

## v.scss
`@use 'scss-color-var/v.scss' as *;`

This file has two shortcuts for referencing variables and color variables:

| Function                 | Description                           | Example Input   | Output                    |
| ------------------------ | ------------------------------------- | --------------- | ------------------------- |
| v($var, $fallback: null) | Shortcut for var(--, $fallback)       | v(margin)       | var(--margin)             |
| c($var, $fallback: null) | Shortcut for var(--color-, $fallback) | c(primary-text) | var(--color-primary-text) |


## util.scss
`@use 'scss-color-var/util.scss'`

This file provides utility functions. You are welcome to contribute more utility functions.

| Function                  | Description                            | Example Input               | Output                                            |
| ------------------------- | -------------------------------------- | --------------------------- | ------------------------------------------------- |
| util.lerp($from, $to, $t) | Lerps a value 0: $from, 1: $to, t: 0-1 | util.lerp(v(a), v(b), .445) | calc(var(--a) * (1.0 - .445) + (var(--b) * .445)) |


## var.scss
`@use 'scss-color-var/var.scss`

Provides the main functions of this package.

```scss
@use "scss-color-var/var";

* {
    // * Assigning color variables
    
    @include var.colors(
        $accent: hsla(216, 83%, 56%, 1);
        $text-body: (accent, accent, 5%),
    )

    // Result

    --color-accent-h: 216;
    --color-accent-s: 83%;
    --color-accent-l: 56%;
    --color-accent-a: 1;
    --color-accent: hsla(
        var(--color-accent-h);
        var(--color-accent-s);
        var(--color-accent-l);
        var(--color-accent-a);
    );

	--color-text-body-h: var(--color-accent-h);
    --color-text-body-s: var(--color-accent-s);
    --color-text-body-l: 5%;
    --color-text-body-a: 1;
    --color-text-body: hsla(
        var(--color-text-body-h);
        var(--color-text-body-s);
        var(--color-text-body-l);
        var(--color-text-body-a);
    );


    // * Here we're referencing another variable using a helper-function.
    
    @include var.colors(
        $accent-contrast: (accent, accent, var.L(accent, '-' 10%), accent)
    );

    // Result
    
    --color-accent-contrast-h: var(--color-accent-h);
    --color-accent-contrast-s: var(--color-accent-s);
    --color-accent-contrast-l: calc(var(--color-accent-l) - 10%);
    --color-accent-contrast-a: var(--color-accent-a);
    --color-accent-contrast: hsla(...);

    // * Getting a color variable
    v(--color-accent);   // -> var(--color-accent);
    var.Color(accent);   // -> var(--color-accent);
    c(accent);           // -> var(--color-accent);
}
```

## var helper functions
When using `+` or `-` in these examples, it will become a `calc()` using the operator and value you've used.

These get the individual HSLA values:
```scss
var.H(accent, '+' 25);         // -> calc(var(--color-accent-h) + 25);
var.S(accent, '-' 10%);        // -> calc(var(--color-accent-s) - 10%);
var.L(accent);                 // -> var(--color-accent-l);
var.A(accent);                 // -> var(--color-accent-a);
```

These replace one individual HSLA value referencing a variable
```scss
// Notice these uses calc()
var.Hue(accent, '+' 25);       	   // -> hsla(calc(var(--color-accent-h) + 25), var(--color-accent-s), var(--color-accent-l), var(--color-accent-a));
var.Saturation(accent, '-' 10%);   // -> hsla(var(--color-accent-h), calc(var(--color-accent-s) - 10%), var(--color-accent-l), var(--color-accent-a));

// Notice these just replace the variable
var.Lightness(accent, 10%);        // -> hsla(var(--color-accent-h), var(--color-accent-s), 10%, var(--color-accent-a));
var.Alpha(accent, 0.5);            // -> hsla(var(--color-accent-h), var(--color-accent-s), var(--color-accent-l), 0.5);
```