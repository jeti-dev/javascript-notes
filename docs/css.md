---
title: CSS
layout: default
---


# CSS

## Methodologies

| **Methodology** | **Description** | **Key Concepts** | **Pros** | **Cons** |
|-----------------|-----------------|------------------|----------|----------|
| **BEM (Block, Element, Modifier)** | A naming convention for writing CSS that makes the structure of components clear. | Block (`.block`), Element (`.block__element`), Modifier (`.block__element--modifier`) | - Improves code readability and consistency <br> - Encourages reusability <br> - Scales well with large projects | - Can lead to long class names <br> - Initial setup and learning curve |
| **SMACSS (Scalable and Modular Architecture for CSS)** | A guideline-based approach to structuring CSS into five categories (Base, Layout, Module, State, Theme). | Base, Layout, Module, State, Theme | - Flexible and scalable <br> - Provides clear separation of concerns | - No strict naming conventions <br> - May require more discipline to enforce |
| **OOCSS (Object-Oriented CSS)** | Promotes reusability and separation of structure and skin to create modular, reusable components. | Structure vs. Skin, Separate container and content styles | - Encourages DRY (Don't Repeat Yourself) <br> - Makes components more reusable | - Requires careful planning <br> - Can lead to over-generalization |
| **ITCSS (Inverted Triangle CSS)** | Organizes CSS in a layered structure to improve maintainability and scalability. | Settings → Tools → Generic → Elements → Objects → Components → Utilities | - Highly scalable <br> - Improves CSS organization <br> - Reduces specificity conflicts | - Steeper learning curve <br> - Requires consistent implementation |
| **Atomic CSS** | Creates small, single-purpose utility classes to compose styles directly in HTML. | Utility-first classes (`.p-4`, `.text-center`) | - Highly flexible <br> - Fast development <br> - Encourages consistency | - Can clutter HTML with many classes <br> - Less semantic |
| **Utility-First (Tailwind CSS)** | Similar to Atomic CSS, focuses on small utility classes that directly style elements. | Utility classes (`.flex`, `.bg-blue-500`) | - Highly customizable <br> - Encourages reusability <br> - Strong community support | - Can result in large HTML files <br> - Requires configuration |
| **CSS-in-JS** | Embeds CSS directly into JavaScript files, often using libraries like Styled Components or Emotion. | Inline styles, JS variables for styling | - Scoped styles <br> - Dynamic styling based on component state | - Increases bundle size <br> - Can reduce performance |

### BEM

```css
/* Block */
.card {
  background-color: #f9f9f9;
  padding: 16px;
  border-radius: 4px;
}

/* Element */
.card__header {
  font-size: 20px;
  font-weight: bold;
}

.card__body {
  font-size: 16px;
}

/* Modifier */
.card__footer--highlighted {
  background-color: #e0e0e0;
  color: #333;
}
```

### SMACSS
```css
/* Base (generic styles) */
body {
  font-family: Arial, sans-serif;
}

/* Layout */
.container {
  display: flex;
  justify-content: center;
}

/* Module */
.btn {
  padding: 10px 20px;
  border-radius: 4px;
}

/* State */
.btn--primary {
  background-color: #007bff;
  color: white;
}

/* Theme */
.theme-dark {
  background-color: #333;
  color: #fff;
}

```

### OOCSS
``` css
/* Structure */
.box {
  padding: 10px;
  border: 1px solid #ccc;
}

/* Skin */
.blue-box {
  background-color: #007bff;
  color: white;
}

```

### ITCSS
``` css
/* Settings */
:root {
  --primary-color: #007bff;
}

/* Tools */
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Generic */
* {
  box-sizing: border-box;
}

/* Elements */
body {
  font-family: Arial, sans-serif;
}

/* Objects */
.container {
  @include flex-center;
  padding: 20px;
}

/* Components */
.button {
  background-color: var(--primary-color);
  color: white;
  padding: 10px 20px;
}

/* Utilities */
.hidden {
  display: none;
}

```
### Atomic
```css
/* Example from Tailwind CSS */
.p-4 {
  padding: 1rem;
}

.m-2 {
  margin: 0.5rem;
}

.text-center {
  text-align: center;
}

.bg-blue-500 {
  background-color: #007bff;
}

.text-white {
  color: #fff;
}

```

### Utility first
``` css
/* Flex utilities */
.flex {
  display: flex;
}

.justify-center {
  justify-content: center;
}

.items-center {
  align-items: center;
}

```

### CSS-in-JS
``` js
import { css } from '@emotion/react';

const containerStyle = css`
  display: flex;
  justify-content: center;
  background-color: #f9f9f9;
`;

export default function App() {
  return <div css={containerStyle}>Hello World</div>;
}

```

### Flexbox Table  

| Property | Description | Example | Notes |  
|----------|-------------|---------|-------|  
| `display: flex` | Enables flexbox layout on an element | `display: flex;` | Applied to the container |  
| `flex-direction` | Defines the main axis direction | `flex-direction: row;` | `row`, `row-reverse`, `column`, `column-reverse` |  
| `justify-content` | Aligns items along the main axis | `justify-content: center;` | `flex-start`, `flex-end`, `center`, `space-between`, `space-around`, `space-evenly` |  
| `align-items` | Aligns items along the cross axis | `align-items: center;` | `flex-start`, `flex-end`, `center`, `stretch`, `baseline` |  
| `align-self` | Aligns a specific item on the cross axis | `align-self: flex-end;` | Overrides `align-items` for an individual item |  
| `flex-wrap` | Defines if items should wrap onto multiple lines | `flex-wrap: wrap;` | `nowrap`, `wrap`, `wrap-reverse` |  
| `flex-grow` | Specifies how much a flex item can grow | `flex-grow: 1;` | `0` (no growth), `1` (grow equally), or custom value |  
| `flex-shrink` | Specifies how much a flex item can shrink | `flex-shrink: 1;` | `0` (no shrink), `1` (shrink equally), or custom value |  
| `flex-basis` | Defines the initial size of a flex item | `flex-basis: 200px;` | Works with `width` and `height` |  
| `order` | Defines the order of flex items | `order: 2;` | Default is `0` |  
| `gap` | Defines the space between flex items | `gap: 10px;` | Works with `row-gap` and `column-gap` |  

---

### CSS Grid Table  

| Property | Description | Example | Notes |  
|----------|-------------|---------|-------|  
| `display: grid` | Enables grid layout on an element | `display: grid;` | Applied to the container |  
| `grid-template-columns` | Defines the number and size of columns | `grid-template-columns: 1fr 2fr;` | `fr` is a fractional unit |  
| `grid-template-rows` | Defines the number and size of rows | `grid-template-rows: 100px auto;` | Can use fixed or dynamic values |  
| `grid-auto-flow` | Defines the automatic flow of items | `grid-auto-flow: row;` | `row`, `column`, `dense` |  
| `grid-gap` | Defines the gap between grid items | `grid-gap: 10px;` | Works with `row-gap` and `column-gap` |  
| `justify-items` | Aligns grid items along the row axis | `justify-items: center;` | `start`, `end`, `center`, `stretch` |  
| `align-items` | Aligns grid items along the column axis | `align-items: stretch;` | `start`, `end`, `center`, `stretch` |  
| `justify-content` | Aligns the entire grid along the row axis | `justify-content: space-between;` | `start`, `end`, `center`, `space-between`, `space-around`, `space-evenly` |  
| `align-content` | Aligns the entire grid along the column axis | `align-content: center;` | `start`, `end`, `center`, `space-between`, `space-around`, `space-evenly` |  
| `grid-column` | Specifies the start and end position of a grid item (columns) | `grid-column: 1 / 3;` | Can use `span` to stretch |  
| `grid-row` | Specifies the start and end position of a grid item (rows) | `grid-row: 1 / span 2;` | Can use `span` to stretch |  
| `place-items` | Shorthand for `align-items` and `justify-items` | `place-items: center;` | Combines two properties into one |  
| `place-content` | Shorthand for `align-content` and `justify-content` | `place-content: center;` | Combines two properties into one |  


## SASS  

| Feature | Description | Example | Notes |  
|---------|-------------|---------|-------|  
| **Variables** | Store reusable values (like colors, fonts, etc.) | `$primary-color: #ff5733;` | Can store any value (color, size, string, etc.) |  
| **Nesting** | Nest selectors within each other | `.nav { .item { color: red; } }` | Reduces repetitive code and improves readability |  
| **Partials** | Create reusable SASS files and import them | `_variables.scss` → `@import 'variables';` | `_` prefix prevents compiling into CSS |  
| **Mixins** | Create reusable blocks of CSS with parameters | `@mixin border($size) { border: $size solid black; }` | Can take arguments and be included in selectors |  
| **Inheritance** | Share styles between selectors using `@extend` | `%button { padding: 10px; } .primary { @extend %button; }` | Reduces duplication and improves organization |  
| **Operators** | Perform math operations within styles | `width: (100% / 3);` | Supports `+`, `-`, `*`, `/`, and `%` |  
| **Control Directives** | Use `@if`, `@else`, `@for`, `@each`, and `@while` for logic | `@if $theme == dark { background: black; }` | Enables conditional and loop-based styling |  
| **Functions** | Define reusable functions | `@function double($n) { @return $n * 2; }` | Returns computed values |  
| **Color Manipulation** | Adjust colors dynamically | `darken($color, 10%)` | Supports `lighten`, `darken`, `saturate`, `desaturate`, `adjust-hue`, etc. |  
| **Maps** | Store key-value pairs | `$theme: (primary: #ff5733, secondary: #ccc);` | Use `map-get($theme, primary)` to retrieve values |  
| **Interpolation** | Use variables inside selectors or properties | `#{$class-name} { color: red; }` | Allows dynamic property names and values |  
| **Placeholder Selectors** | Define non-rendering selectors for `@extend` | `%button { padding: 10px; }` | Saves CSS output size |  
| **Import** | Import SASS files into other files | `@import 'variables';` | Helps organize code into smaller modules |  
| **Modules** | Use `@use` to load modules with namespace | `@use 'colors' as c;` | Replaces `@import` for better organization |  
| **@forward** | Re-export styles from another file | `@forward 'base';` | Improves modularization |  

## PostCSS  

| Feature | Description | Example | Notes |  
|---------|-------------|---------|-------|  
| **Plugins** | Modular plugins that add or modify CSS behavior | `postcss([autoprefixer])` | Most PostCSS features are implemented as plugins |  
| **Autoprefixing** | Automatically add vendor prefixes for better browser support | `autoprefixer` → `display: flex;` → `-webkit-display: flex;` | Simplifies handling cross-browser compatibility |  
| **Nesting** | Use nested selectors like in SASS | `postcss-nested` → `.nav { .item { color: red; } }` | Allows cleaner, organized CSS |  
| **CSS Variables** | Support for native CSS variables | `postcss-custom-properties` → `--main-color: #ff5733;` | Enables dynamic theming and reusable styles |  
| **Custom Media Queries** | Define custom media queries and reuse them | `postcss-custom-media` → `@custom-media --small-viewport (max-width: 600px);` | Makes media queries easier to maintain |  
| **Color Functions** | Manipulate colors with functions | `postcss-color-mod-function` → `color: color-mod(var(--main-color) lightness(+10%));` | Dynamic color adjustments |  
| **Mixins** | Use reusable code blocks | `postcss-mixins` → `@mixin border { border: 1px solid black; }` | Reduces repetition and improves consistency |  
| **Imports** | Import CSS files within CSS | `postcss-import` → `@import 'styles.css';` | Combines multiple CSS files into one |  
| **Preset Env** | Use future CSS features today | `postcss-preset-env` | Transforms modern CSS to be compatible with older browsers |  
| **PurgeCSS** | Remove unused CSS from final bundle | `@fullhuman/postcss-purgecss` | Reduces final CSS size |  
| **PostCSS CLI** | Command-line interface for PostCSS | `npx postcss input.css -o output.css` | Automates PostCSS processing |  
| **Scope Isolation** | Isolate styles to avoid conflicts | `postcss-modules` → `@scope(.component) { color: red; }` | Improves component-based styling |  
| **Linter** | Catch and fix CSS errors | `stylelint` → `postcss([stylelint])` | Ensures consistent code quality |  
| **Source Maps** | Generate source maps for debugging | `postcss-sourcemaps` | Helps with debugging compiled CSS |  
| **RTL Support** | Generate right-to-left (RTL) styles | `postcss-rtl` → `float: left;` → `float: right;` | Improves multi-language support |  

## CSS Transition vs Animation Table 

| Aspect | CSS Transition | CSS Animation |  
|--------|----------------|---------------|  
| **Definition** | Enables smooth changes between two states over a defined duration | Allows keyframe-based, complex, multi-step animations |  
| **Syntax** | `transition: property duration timing-function delay;` | `@keyframes` and `animation` properties |  
| **Trigger** | Triggered by user interaction (e.g., hover, focus, click) or DOM state change | Can start automatically or based on user interaction |  
| **Complexity** | Simple, limited to starting and ending states | Can define multiple steps using keyframes |  
| **Control** | Limited control over intermediate states | Full control over intermediate states using keyframes |  
| **Repeatability** | Cannot repeat; ends once the state change finishes | Can loop indefinitely using `animation-iteration-count` |  
| **Performance** | High performance when using `transform` and `opacity` | High performance when using `transform` and `opacity` |  
| **Example** | 

``` css  
#box {  
  transition: transform 0.5s ease-in-out;  
}  
#box:hover {  
  transform: translateX(100px);  
}  
```  

```css  
@keyframes slide {  
  from { transform: translateX(0); }  
  to { transform: translateX(100px); }  
}  
#box {  
  animation: slide 1s forwards;  
}  
``` 

