# scss-color-var

### `@use 'scss-color-var/v.scss' as *`
This is a variable shortcut to access `var(--, fallback)`.

Ex: `v(margin-variable, 0px)` becomes `var(--margin-variable, 0px)`

### `@use 'scss-color-var/util.scss`
Provides `util.` functions.

`util.lerp($from, $to, $t)`. Take a value and lerps it (`t`: bwteen `0` and `1`) where `0` is `$from` and `1` is `$to`

### `@use 'scss-color-var/var.scss`
Provides the `var.` functions.

#### var helpers:
When using `+` or `-` in these examples, it will become a `calc()` using the operator and value you've used.

These get the individual HSLA values:
```scss
var.H(accent, '+' 25);         // -> calc(var(--color-accent-h) + 25);
var.S(accent, '-' 10%);        // -> calc(var(--color-accent-s) - 10%);
var.L(accent);                 // -> var(--color-accent-l);
var.A(accent);                 // -> var(--color-accent-a);
```

These replace one individual HSLA value or sets a new relative value:
```scss
// Notice these uses calc()
var.Hue(accent, '+' 25);       	   // -> hsla(calc(var(--color-accent-h) + 25), var(--color-accent-s), var(--color-accent-l), var(--color-accent-a));
var.Saturation(accent, '-' 10%);   // -> hsla(var(--color-accent-h), calc(var(--color-accent-s) - 10%), var(--color-accent-l), var(--color-accent-a));

// Notice these just replace the variable
var.Lightness(accent, 10%);        // -> hsla(var(--color-accent-h), var(--color-accent-s), 10%, var(--color-accent-a));
var.Alpha(accent, 0.5);            // -> hsla(var(--color-accent-h), var(--color-accent-s), var(--color-accent-l), 0.5);
```

#### Thorough example
```scss
@use "scss-color-var/var";

* {
    // * Here we're assigning values, which later can be referenced as var.Use(background)
    
    @include var.colors(
        $background: hsla(0, 0%, 10%, 1),
        $color: hsla(0, 0%, 84%, 1),
        $color: hsla(0, 0%, 84%, 1),
        $accent: hsla(216, 83%, 56%, 1);
    )

    // Result

    --color-background-h: 0;
    --color-background-s: 0%;
    --color-background-l: 10%;
    --color-background-a: 1;
    --color-background: hsla(
        var(--color-background-h);
        var(--color-background-s);
        var(--color-background-l);
        var(--color-background-a);
    );
    --color-color-h: 0;
    --color-color-s: 0%;
    --color-color-l: 84%;
    --color-color-a: 1;
    --color-color: hsla(...);
    --color-accent-h: 216;
    --color-accent-s: 83%;
    --color-accent-l: 56%;
    --color-accent-a: 1;
    --color-accent: hsla(...);

    // * Here we're referencing previous values, to new values.
    
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
    
    var.Color(accent);             // -> var(--color-accent);
}

```
