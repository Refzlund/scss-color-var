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

        // b. Creating relationships to other colors: H, S, L, A
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

### v.scss
`@use 'scss-color-var/v.scss' as *;`

This file has two shortcuts for referencing variables and color variables:

| Function                 | Description                           | Example Input   | Output                    |
| ------------------------ | ------------------------------------- | --------------- | ------------------------- |
| v($var, $fallback: null) | Shortcut for var(--, $fallback)       | v(margin)       | var(--margin)             |
| c($var, $fallback: null) | Shortcut for var(--color-, $fallback) | c(primary-text) | var(--color-primary-text) |



<br>
<br>



### util.scss
`@use 'scss-color-var/util.scss'`

This file provides utility functions. You are welcome to contribute more utility functions.

| Function                  | Description                            | Example Input               | Output                                            |
| ------------------------- | -------------------------------------- | --------------------------- | ------------------------------------------------- |
| util.lerp($from, $to, $t) | Lerps a value 0: $from, 1: $to, t: 0-1 | util.lerp(v(a), v(b), .445) | calc(var(--a) * (1.0 - .445) + (var(--b) * .445)) |


<br>
<br>


### var.scss
`@use 'scss-color-var/cvar.scss`

Provides the main functions of this package.

| Function                  | Description                                                 | Example                            | Output |
| ------------------------- | ----------------------------------------------------------- | ---------------------------------- | ------ |
| cvar.colors(...$colors)   | Defines color variables, prefixed with 'color'              | cvar.colors($black: hsla(0,0,0,1)) |        |
| cvar.h($variable, $value) | Returns a color where hue has been replaced with the $value | cvar.h(primary, '+' 60deg)         | 
| cvar.s($variable, $value) |
| cvar.l($variable, $value) |
| cvar.a($variable, $value) |
| cvar.getH($var)           | Returns the hue variable                                    | cvar.getH(primary) | --color-primary-h |
| cvar.getS($var)           | Returns the saturation variable                             | cvar.getS(primary) | --color-primary-s |
| cvar.getL($var)           | Returns the lightness variable                              | cvar.getL(primary) | --color-primary-l |
| cvar.getA($var)           | Returns the alpha variable                                  | cvar.getA(primary) | --color-primary-a |

<br>
<br>
<br>
<br>