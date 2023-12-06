# scss-color-var
Manage and access colour variables in SCSS.

### Quick start

1. import files

```scss
@use `scss-color-var/cvar.scss`;

// Utility
// v(variable) becomes var(--variable)
// c(variable) becomes var(--color-variable)
@use 'scss-color-var/v.scss' as *;
```

2. define your colors

```scss

:root {
    cvar.colors(
        // a. Defining colors using  rgba, hsla  etc.
        $text-body: hsl(0, 0%, 40%),
		$primary: hsl(115, 78%, 30%),

        // b. Creating relationships to other colors: (H, S, L, A)
        $primary-background: (primary, primary, 90%),
        $primary-text: (primary, primary, text-body),

        // c. Use utility methods
        $primary-text-faded: cvar.A(primary-text, .66),
        // same as
        // $primary-text-faded: (primary-text, primary-text, primary-text, .5)

        $primary-text-very-faded: cvar.A(primary-text, '-'.2) // relative; becomes 0.46
    )

}

```

3. use them

```scss
// Libraries like Svelte can preprend certain imports @ compontent-based styling
@use 'scss-color-var/v.scss' as *;
@use `scss-color-var/cvar.scss`;

buttom.primary {
    background-color: c(primary-background);
    color: c(primary-text);

    border: 1px solid cvar.l(background-color, +'.1') // adds .1 to lightness

    --the-alpha: #{cvar.getAlpha(primary-text)};
}

```

<br>

That's it!

> [!NOTE]  
> When assigning to variables, use `#{ }` as [SCSS documents here](https://sass-lang.com/documentation/breaking-changes/css-vars).  
> Ex. `--hover-text: #{c(primary-hover)};`

<br>
<br>

### v
`@use 'scss-color-var/v.scss' as *;`

This file has two shortcuts for referencing variables and color variables:

| Function                 | Description                           | Example Input   | Output                    |
| ------------------------ | ------------------------------------- | --------------- | ------------------------- |
| v($var, $fallback: null) | Shortcut for var(--, $fallback)       | v(margin, 5px)  | var(--margin, 5px)        |
| c($var, $fallback: null) | Shortcut for var(--color-, $fallback) | c(primary-text) | var(--color-primary-text) |



<br>
<br>



### util
`@use 'scss-color-var/util.scss'`

This file provides utility functions. You are welcome to contribute more utility functions.

| Function                  | Description                            | Example Input               | Output                                            |
| ------------------------- | -------------------------------------- | --------------------------- | ------------------------------------------------- |
| util.lerp($from, $to, $t) | Lerps a value 0: $from, 1: $to, t: 0-1 | util.lerp(v(a), v(b), .445) | calc(var(--a) * (1.0 - .445) + (var(--b) * .445)) |


<br>
<br>


### cvar
`@use 'scss-color-var/cvar.scss`

Provides the main functions of this package.

<br>

#### cvar.colors(...$colors)
Defines color variables, prefixed with 'color'

```scss

cvar.colors(
    $black: hsla(0, 0%, 0%, 1),
    $shadeofgrey: hsla(black, black, 50%, black)
)

// becomes

--color-black-h: 0;
--color-black-s: 0%;
--color-black-l: 0%;
--color-black-a: 1;
--color-black: hsla(
    var(--color-black-h),
    var(--color-black-s),
    var(--color-black-l),
    var(--color-black-a)
);

--color-shadeofgrey-h: var(--color-black-h);
--color-shadeofgrey-s: var(--color-black-s);
--color-shadeofgrey-l: 50%;
--color-shadeofgrey-a: var(--color-black-a);
--color-shadeofgrey: hsla(
    var(--color-shadeofgrey-h),
    var(--color-shadeofgrey-s),
    var(--color-shadeofgrey-l),
    var(--color-shadeofgrey-a)
);

```

<br>

#### Replaced values
`cvar.h($variable, $value)`
`cvar.s($variable, $value)`
`cvar.l($variable, $value)`
`cvar.a($variable, $value)`

All returns a color where the respective color parameter has been replaced with the $value.

```scss
cvar.a(primary, .5);

// becomes

hsla(
    var(--color-primary-h),
    var(--color-primary-s),
    var(--color-primary-l),
    .5
)
```

Relative colors

```scss
cvar.h(primary, '+' 45deg);

// becomes

hsla(
    calc(var(--color-primary-h) + 45deg),
    var(--color-primary-s),
    var(--color-primary-l),
    var(--color-primary-a)
)
```

#### Get variables
`cvar.getH($variable)`
`cvar.getS($variable)`
`cvar.getL($variable)`
`cvar.getA($variable)`

All returns the variable representing that colors color-parameter.

`cvar.getH(primary)` returns `var(--color-primary-h)`, etc.

<br>
<br>
<br>
<br>