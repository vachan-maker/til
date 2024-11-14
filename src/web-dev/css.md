# CSS
## Improving CSS Reset

How you can make your site a little bit more better by using the css below. [Source](https://www.youtube.com/watch?v=eWmDW4zEXt4)
```css
h1,
h2,
h3,
h4,
h5,
h6 {
  text-wrap: balance;
}

p,
li,
figcaption {
  text-wrap: pretty;
  max-width:65ch;
}
 
```
## Dark Theme
How to add dark theme to a website using only css.
```css 
@media (prefers-color-scheme: dark) {
  img {
    opacity: .75;
    transition: opacity .5s ease-in-out;
  }
  img:hover {
    opacity: 1;
  }
}
```

[Source](https://css-tricks.com/dark-modes-with-css/)