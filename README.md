# DNA Parallax / CSS Keyframes Scroll Animator
Pure CSS-animation defined parallax effect. No Javascript skills required, just HTML/CSS.

**There is not much to say. If you know how to use `@keyframes` CSS then you know how to use this.**

Define your CSS `@keyframes` animation and put its name into `parallax` attribute on the element to be animated. You can use multiple white-space separated animation names and they will get merged into one final animation. 

If you add `role="parallax-container"` on any of element's parent then that element's scrolling position will govern animation timeline progress[[?]](https://webdevelopers-eu.github.io/DNA-Parallax/tutorial/#s7).

Animation timeline key points as determined by scrolled container are as follows[[?]](https://webdevelopers-eu.github.io/DNA-Parallax/tutorial/#s7):
* **0%** container is just bellow the viewport
* **50%** container is in the very middle of the viewport
* **100%** container is just above the viewport

The best way is to look at [tutorial](https://webdevelopers-eu.github.io/DNA-Parallax/tutorial/) or experiment with editable [CodePen](https://codepen.io/webdevelopers/pen/PVGKKO?editors=1100) examples.

## Simple Example
```HTML
<html>
  <head>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
	<script src="https://webdevelopers-eu.github.io/DNA-Parallax/jquery.parallax.js"></script>
	<style>
	  @keyframes my-animation {
		0%, 100% {
		  transform: translateX(100vw) scale(0.2) rotate(360deg);
		  opacity: 0;
		}
		50% {
		  transform: translateX(0vw) scale(1) rotate(0deg);
		  opacity: 1;
		}
	  }
	</style>
  </head>
  <body>
	<div role="parallax-container" style="height: 400vh; margin: 100vh 0px; overflow-x: hidden;">
	  <h1 parallax="my-animation" style="position: fixed; top: 50vh;">
		I will roll in and out as parent <code>div</code> scrolls!
	  </h1>
	</div>
  </body>
</html>
```
Play with this example on [CodePen](https://codepen.io/webdevelopers/pen/XOaoog?editors=1000)!

## Syntax

Animated element:
```
<element parallax="CSS_KEYFRAMES_NAME[:MODIFIER[:...]] [CSS_KEYFRAMES_NAME[:MODIFIER[:...]] ...]" [parallax-container="CSS_SELECTOR"]>...</element>
```
Container that determines progress:
```
<element role="parallax-container">...</element>
```

Supported modifiers[[?]](https://webdevelopers-eu.github.io/DNA-Parallax/tutorial/#s4):
* **reverse** - play animation backwords. E.g. ```parallax="fly-in-top:reverse"``` will fly out up.
* **shift(n%)** - delay or speed up start of the animation. E.g. ```parallax="fly-in-top:shift(10%)``` if animation starts at 0% this will make it start at 10%.
* **scale(n)** - delay or speed up start of the animation. E.g. ```parallax="fly-in-top:scale(0.5)``` if animation starts at 50% and ends at 100% it will make it start at 25% and finish at 50%.

## Limitations
* **media queries** are not supported yet. I will add them when I see there is a demand. E.g. it is possible to support: ```@media (min-width: 700px) { @keyframes test {...} }```
* **units** CSS animated properties must have same units. E.g. you cannot use `transition: rotate(360`**deg**`)` in one keyframe and `transition: rotate(9`**rad**`)` in next.
* **IE** is not supported (yet). But it should be easy to create standard CSS fallback styles and all animation-related CSS styles put into `0%` key of the `@keyframe` - they will overwrite CSS defaults if browser is supported...

## Design Recommendations
Due to modern browser's decision to decouple scrolling from the main responsive browser thread it is not possible to make reliable super-smooth scroll-linked effects. Browsers may run CSS Keyframes Animator after screen update which may result in laggy, janky, or jittery behavior.

Browser vendors currently effectivelly disabled [precise scroll-linked javascript effects](https://developer.mozilla.org/en-US/docs/Mozilla/Performance/Scroll-linked_effects) and they seek feedback and solutions. Ehm. Nice. Developers first, right? ;-) There is no reliable working solution to this problem.

The only reliable workaround for supersmooth scroll-linked animations is to use `position: fixed` on animated elements. When browser scrolls the page it will leave the element in place and following call to CSS Keyframes Animator will update the element's new position. That simple trick will avoid jittery experience.

You can also consider CSS to hide elements that are off-screen.
```css
[parallax][parallax-status="off"] {
	display: none !imortant;
}
```

## Feedback
Let me know what problems did you run in.
