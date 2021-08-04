---
layout: default
title: CSS Toggles with Form Elements
description: A useful technique for toggling CSS styles on form elements as well as toggling mobile menus without JavaScript.
tags: HTML CSS
---
# CSS Toggles with Form Elements

Sometimes called a "checkbox hack", you can use checkboxes and radio buttons as a kind of toggle to switch between states and this lets you also toggle between styles. I originally came across this while trying to find a way to style form elements themselves, which can be a lot tricker than styling other HTML elements. Basically, it works by adding a label and styling that based on the state of the checkbox or radio button, and then hiding the element itself.

It works with both checkboxes and radio buttons. It can be used to build things like mobile menus (where a hamburger icon is the toggle built with this method) or even things like modals (though I prefer [using :target for that](/articles/css-toggles-with-target)), but I like it in particular for things like building a form with a lot of options that can be used to filter the output of some form submission.

I used it for that particular case in an electron app I built, which takes in some Japanese text and lets you build a vocabulary list with just the words you want from it (like the easy words, or just the nouns, etc.):

![text](/assets/images/projects/japanese-vocab-app/vocab-viewer-screenshot.png)

It worked really well because in this particular case you might want to quickly customize the list and so just by being able to see all the options as a panel where buttons are either active or inactive you can easily communicate to the user which options will be applied or not with just a few changes in things like color. If you want to see more about this project you can read about it [here](/projects/japanese-vocab-app) or check it out in [my portfolio](https://jamesquillin.github.io) where you can see a demo or download it for windows.

## Code Examples

<!-- TODO: two examples: filter panel and hamburger menu -->

```js
console.log('asdf');
```