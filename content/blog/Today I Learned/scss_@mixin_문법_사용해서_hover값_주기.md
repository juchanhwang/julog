---
title: 'scss @mixin 문법 사용해서 hover값 주기'
date: 2020-03-16 16:00:00
category: 'Today I Learned'
draft: false
---

```scss
$icons: (
  sprite-instagram: (
    width: 40px,
    height: 40px,
    background-position-x: -4px,
    background-position-y: -56px,
  ),
)
$icon-width: 1000px;

@mixin icon($name, $width, $height) {
  $rate: (map-deep-get($icons, $name, "width") / $width);
  display: block;
  background-image: url('/assets/img/image-sprite/icons.png');
  background-size: $icon-width / $rate auto;
  background-position-x: map-deep-get($icons, $name, "background-position-x")/$rate;
  background-position-y: map-deep-get($icons, $name, "background-position-y")/$rate;
  width: $width;
  height: $height;
}

@mixin icon_hover($name, $width) {
  $rate: (map-deep-get($icons, $name, "width") / $width);
  background-position-x: map-deep-get($icons, $name, hover, "background-position-x")/$rate;
  background-position-y: map-deep-get($icons, $name, hover, "background-position-y")/$rate;
}

@mixin resize-icons($resize-icons) {
  @each $name, $width, $height in $resize-icons {
    .#{$name} {
      @include icon($name, $width, $height);

      @if map-deep-get($icons, $name, hover) {
        &:hover {
          @include icon_hover($name, $width);
        }
      }
    }
  }
}
```
