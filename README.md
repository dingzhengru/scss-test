# scss-test

測試、練習 scss

## variable

參考: https://sass-lang.com/documentation/variables

```scss
$base-color: #c6538c;
```

```scss
// Scope、Shadowing
$variable: global-value;

.content {
  $variable: local-value;
  value: $variable;
}

.sidebar {
  value: $variable;
}

// 編譯後
.content {
  value: local value;
}

.sidebar {
  value: global value;
}
```

## extend

參考: https://icguanyu.github.io/scss/scss_2  
繼承整個樣式，使用時機：整包樣式都要一樣的時候

```scss
%aBlock {
  text-decoration: none;
}
.button {
  @extend %aBlock;
}
.big_button {
  @extend %aBlock;
}

// 編譯後
.button,
.big_button {
  text-decoration: none;
}
```

## mixin

參考: https://icguanyu.github.io/scss/scss_2  
使用時機：需要大量重複使用到的屬性

與 extend 不同之處，參考: https://stackoverflow.com/a/30744854/5134658

- 編譯後結果不同，extend 會用 , 放一起；mixin 則會各別放(單純繼承，用 extend 比較好)
- mixin 能接變數來使用

雖然都可以做到一樣的事情，但通常是，需要接收變數就使用 mixin，不需要就用 extend

```scss
@mixin line($height: 20px) {
  height: $height;
  line-height: $height;
}
div {
  @include line(20px);
}

// 編譯後
div {
  height: 20px;
  line-height: 20px;
}
```

## function

參考: https://icguanyu.github.io/scss/scss_2  
搭配變數、mixin 使用，可以回傳，適合用於回傳計算屬性

```scss
$font: 8px 12px 16px 24px 30px;
$base-line: 10px;

@function line-height($index: 1) {
  // nth(陣列, 選擇陣列的第幾個(初始是1)) 參考: https://sass-lang.com/documentation/modules/list
  // floor(無條件捨去) 參考: https://sass-lang.com/documentation/modules/math
  $font-size: list.nth($font, $index);
  @return (math.floor($font-size / 10px) + 1) * 10px;
}

@mixin font($index: 1) {
  font-size: nth($font, $index);
  line-height: line-height($index);
}

.myFont {
  @include font(4);
}
```

## debug、warn、error

參考: https://sass-lang.com/documentation/at-rules/debug

```scss
@mixin inset-divider-offset($offset, $padding) {
  $divider-offset: (2 * $padding) + $offset;
  @debug 'divider offset: #{$divider-offset}';
}
```

## if

參考: https://www.w3cplus.com/preprocessor/Sass-control-directives-if-for-each-while.html

```scss
$boolean: true !default;
@mixin simple-mixin {
  @if $boolean {
    @debug '$boolean is #{$boolean}';
    display: block;
  } @else {
    @debug '$boolean is #{$boolean}';
    display: none;
  }
}
.some-selector {
  @include simple-mixin;
}

// 編譯後
.some-selector {
  display: block;
}
```

## for loop

參考: https://www.w3cplus.com/preprocessor/Sass-control-directives-if-for-each-while.html

### @for

- @for \$i from 0 through 4 (0 ~ 4)
- @for \$i from 0 to 4 (0 ~ 3)

### @each

```scss
$list: adam john wynn mason;

@each $author in $list {
  .photo-#{$author} {
    background-image: url('/images/avatars/#{$author}.png');
  }
}

// 編譯後
.photo-adam {
  background: url('/images/avatars/adam.png') no-repeat;
}
.photo-john {
  background: url('/images/avatars/john.png') no-repeat;
}
.photo-wynn {
  background: url('/images/avatars/wynn.png') no-repeat;
}
.photo-mason {
  background: url('/images/avatars/mason.png') no-repeat;
}
```

### @while

```scss
$types: 4;
$type-width: 20px;
@while $types > 0 {
  .while-#{$types} {
    width: $type-width + $types;
  }
  $types: $types - 1;
}

// 編譯後
.while-4 {
  width: 24px;
}
.while-3 {
  width: 23px;
}
.while-2 {
  width: 22px;
}
.while-1 {
  width: 21px;
}
```
