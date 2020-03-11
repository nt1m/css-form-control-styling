# Form control styling

## Authors
* Tim Nguyen

## Participate
* Issue tracker: https://github.com/nt1m/css-form-control-styling

## Introduction

Styling form controls is a pain for web developers since different browser engines can diverge on the following:
* behaviour of the CSS appearance property
* the default styles
* naming and structure of the form control pseudo elements

This proposal aims to focus mostly on the last pain point, by defining a standardized structure and naming for the internals of some form controls. It will also define some standard default styles for these internals to ensure consistent rendering regardless the browser engine.

## Goals

Web developers often style form controls like `<progress>`, `<meter>`, `<input type="range">`, `<input type="search">` which come with internal parts which are exposed via pseudo elements. Unfortunately, these aren't consistent across browsers and the default styling also isn't.

Here is some example code to style `<input type="range">` (for Gecko and WebKit/Blink only, EdgeHTML needs extra styling):

```css
#slider {
  -webkit-appearance: none; /* -moz-appearance: none would also be needed if Gecko didn't support -webkit-appearance */
  height: 3px;
  margin: 0;
  background: none; /* useful for gecko only */
}

/* The slider track */
#slider::-moz-range-track {
  background: #ccc;
  height: 3px;
}
#slider::-webkit-slider-runnable-track {
  background: #ccc;
  height: 3px;
}

/* The thumb */
#slider::-moz-range-thumb {
  border: none; /* overrides gecko's default style */

  background: blue;
  border-radius: 50%; /* gecko's default is 0.5em */

  /* height/width default to 1em in Gecko */
  height: 16px;
  width: 16px;

}
#slider::-webkit-slider-thumb {
  -webkit-appearance: none; /* overrides webkit/blink's default style */
  transform: translateY(-50%); /* Needed because WebKit/Blink push down the thumb when you reduce the height of the track */

  background: blue;
  border-radius: 50%; /* webkit/blink's default is 0 */

  /* height/width are not set by default, and hence the thumb doesn't render when the thumb has -webkit-appearance: none */
  height: 16px;
  width: 16px;
}
```
It's also worth noting that Gecko provides an extra `:-moz-range-progress` pseudo element that styles the "activated" part of slider.

The different pseudo element names unfortunately cause some duplication due to browser engines only being able to understand their own pseudo elements. This snippet also shows divergence between the default styles. These problems also apply to the other form elements mentioned above.
