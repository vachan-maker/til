# CSS
## Dark Theme
How to add dark theme to a website using only css.
``` 
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