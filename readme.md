# Happy Grid
A grid system should be simple and make your inner designer happy. Create rows, make columns, offset them, and change their source ordering. Easy-to-use Stylus mixin library. Forked and simplified from Cory Simmon's [Lost](https://github.com/corysimmons/lost) grid before it was converted to PostCSS. Thanks to Cory Simmons for sorting out the grid math.

This is intentionally stripped down to the basics. If you want more features, check out https://github.com/corysimmons/lost.

Uses CSS `calc` so it supports modernish browsers, IE9+. To support IE8, you can use a calc polyfill, like this one: https://github.com/closingtag/calc-polyfill.

## Example
With this markup...

```html
<section>
  <div><h2>Top Level Grid 1</h2></div>
  <div>
    <h2>Top Level Grid 2</h2>
    <div><h3>Nested Grid 1</h3></div>
    <div><h3>Nested Grid 2</h3></div>
  </div>
</section>
```

And this style...

```stylus
section
  clearfix()

  div
    column('1/2')
```

Gives you a perfect nested grid. And makes you happy!

## Installation
```bash
npm install --save happy-grid
```

Like other Stylus libraries you need to `use()` it when calling Stylus. Here's an example Gulp config using two other awesome Stylus libraries: [Rupture](https://github.com/jenius/rupture) and [Axis](https://github.com/jenius/axis).

```js
var stylus = require('gulp-stylus');
var grid = require('happy-grid');
var rupture = require('rupture');
var axis = require('axis');

gulp.task('style', function() {
  gulp.src('styles/main.styl')
    .pipe(stylus({use: [rupture(), axis(), grid()]}))
    .pipe(gulp.dest('./compiled/css'))
});
```

Then in your `main.styl` just `@import 'happy-grid`.

## Mixin Documenation
There are no rendered classes. Just use the mixins.

### Settings
Gutter is used to set padding on rows and margin-right on columns. The `max-site-width` gives you a default for the `center()` mixin width. Typically you would set it to the max-width of your site.

```stylus
gutter = 3%
max-site-width = 60em
```

### Clearfix
Used on a parent container to clear floated children elements. Based on [Nicolas Gallagher's micro clearfix](http://nicolasgallagher.com/micro-clearfix-hack). If you use the `center()` mixin it's already applied for you. Takes no arguments.

Aliased as `cf()` and `group()` as well.

```stylus
.parent
  clearfix()
```

### Center
Horizontally center a container element and apply a clearfix and optional padding to it. Pass any unit for the max-width and padding. Uses the default `max-site-width` from settings if called without any arguments.

Aliased as `row()` as well.

```stylus
section
  center(30em, gutter)
```

### Column
Here's the star of the show. Creates a column that is a fraction of the size of it's containing element with a gutter. You don't need to pass any additional ratios (fractions) as the grid system will make use of CSS `calc()`. Note that the ratio must always be a **fraction wrapped in quotes**... i.e. `column('1/2')`, NOT `column(1/2)` and NOT `column(.5)`.

**Cycle**: The grid works by assigning a margin-right to all elements except the last in the row. It does this by default by using the denominator of the fraction you pick. To override this default pass a `cycle` parameter. e.g. `column('2/4', cycle: 2)` or more simply `column('2/4' 2)`

**Gutter**: The margin on the right side of the element used to create a gutter. Typically this is left alone and the global gutter setting will be used, but you can override it here if you want certain elements to have a particularly large or small gutter (pass 0 for no gutter at all).

Aliased as `col()` also.

```stylus
.element
  column('1/3')

.cycle-override
  column '2/6' 3

.cycle-and-gutter-override
  column '2/6' 3 2em
```

### Offset
Margin to the left or right of an element depending on if the fraction passed is positive or negative.

```stylus
.two-elements
  column('1/3')

  &:first-child
    offset('1/3')
```

### Move
Source ordering. Shift elements left or right by passing a positive or negative fraction. Aliased as `shift()` also.

```stylus
.reversed-order
  column('1/2')

  &:first-child
    move('1/2')

  &:last-child
    move('-1/2')
```

