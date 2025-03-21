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