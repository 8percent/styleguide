CSS
====

## Base

* BEM Class 명명규칙
* Flex를 활용한 반응형으로 작성, 모바일과 데스크탑이 상이한 경우에는 적응형
* 자식 컴퍼넌트의 스타일 수정은 `/deep/` 셀렉터를 사용한다. (style을 props로 내려주는 방식은 잘 사용되지 않는다)
  * /deep/ selector will be deprecated soon 
  * 대안으로 `::v-deep`, `>>>` 셀렉터가 있으나 불안정하여 스타일바인딩을 통해 해결할 필요가 있다.

## Layout

* flex 반응형 레이아웃을 기본으로 한다.
* 복잡한 케이스가 아니라면 모바일 First 스타일을 가질 수 있도록 한다.
* 적응형 레이아웃 Vue 디바이스별로 컴퍼넌트를 분리한다.

## 기본구조
```scss
// Color 변수, Text Mixin, Media Mixin을 포함
@import '@/styles/common-style';

.block-class {
  @include Paragraph-14;

  &__element {
    @include Text-Size-3;
    color: $color-gr-700;

    &--modifier {

    }
  }

  // 반응형
  @include respond-to(sm) {
    right: 20px;
  }
}

```

## 참조 (자주 사용되는 공통 속성)
1. 반응형 구분
```scss
@import "@/styles/mixins/_media.scss";
$breakpoints: (
  'xs': ( max-width: 359px ),
  'sm': ( max-width: 767px ),
  'md': ( min-width: 768px ) and ( max-width: 1079px ),
  'lg': ( min-width: 768px ),
);

@import '@/styles/base/_media.scss';
$breakpoints: (
  'xs': ( max-width: 359px ),
  'ss': ( max-width: 479px ),
  'sm': ( max-width: 767px ),
  'md': ( max-width: 1079px ),
  'lg': ( min-width: 768px ),
);
```

2. color
- 사용방법
```scss
@import '@/styles/base/_colors.scss';

body {
  color: $color-gr-900;
}
```
- 참조
```css
/* palette
 * 색을 추가할 때는 color-$name 으로 추가하고 적당히 scale 에 맞는 위치에 넣습니다.
   예시: $color-Sunshade: #FFA526; -> $color-Sunshade-500
 * 참고: https://davidwalsh.name/sass-color-variables-dont-suck
 * 색이름은 Name that color 기준 http://chir.ag/projects/name-that-color/
 * 2019.10.16 에 추가된 palette를 사용합니다.
 */
 @import './open_colors';

// white, black, transparent 는 css color name 을 사용한다.

// gray scale
$color-alabaster: #FAFAFA;
$color-wild-sand: #f4f4f4;
$color-gallery: #F0F0F0;
$color-concrete: #F2F2F2;
$color-mercury: #E6E6E6;
$color-alto: #D2D2D2;
$color-silver: #C5C5C5;
$color-silver-chalice: #B2B2B2;
$color-dusty-gray: #9B9B9B;
$color-gray-darken: #929292;
$color-gray: #8A8A8A;
$color-scorpion: #606060;
$color-mineshaft: #3C3C3C;

// brand
$color-white-lilac: #F8F5FB;
$color-selago: #F6F1FD;
$color-prelude: #C6B2DF;
$color-purple-heart: #6C3AD3;
$color-butterfly-bush: #5f4fa4;
$color-daisy-bush: #5929AA;
$color-martinique: #3e3661;
$color-port-core: #241d43;
$color-melrose: #cdbeff;
$color-heliotrope: #ac7aff;
$color-fuchsia-blue: #674BBE;
$color-login-studio: #5D41B4;
$color-purple: #6300CC;
$color-eight-purple: #6741D9;

// blue
$color-royal-blue: #2980e4;
$color-cerulean-blue: #2a69b2;
$color-cornflower-blue: #4a90e2;
$color-dodger-blue: #4e7bfa;
$facebook-chambray: #3B5998;
$facebook-bay-of-many: #23407E;

// red
$color-cinnabar: #E33838;
$color-mandy: #e75757;
$color-mango-tango: #e67700;

// green
$color-jungle-green: #2cb783;

// yellow
$color-mustard: #FFE65E;


/*
 * 2019.10.16 New Color Code
 */

// gray scale
$color-gr-50: #f8f9fa;
$color-gr-100: #f1f3f5;
$color-gr-200: #dee2e5;
$color-gr-300: #ced4da;
$color-gr-500: #9ca5ad;
$color-gr-600: #858d94;
$color-gr-700: #4b525a;
$color-gr-900: #1d2024;

// brand
$color-hola: #6741d9;

// blue
$color-malibu: #6da5ff;
$color-dodger-blue: #3282f0;

// red
$color-valencia: #d73434;
$color-persimmon: #ff5a5a;
$color-busil: #d5725e;

// green
$color-fern: #68be66;
$color-sharmrock: #32caa5;

// yellow
$color-saffron-mango: #fcc04b;
$color-sunshade: #ffa526;

```

1. 문단(TEXT) 스타일
* 사용방법
```scss
@import '~styles/scss/mixins/text';

@include Body1(bold);
@include Body1(400);

// (X)
@include Body1($fontWeight: 400);
```
* 현재 정의된 스타일
```scss
@mixin Text-Size-0 ($fontWeight: bold) {
  font-weight: $fontWeight;
  font-size: 40px;
  line-height: 58px;
  letter-spacing: -1px;
}

@mixin Text-Size-1 ($fontWeight: bold) {
  font-weight: $fontWeight;
  font-size: 32px;
  line-height: 48px;
  letter-spacing: -1px;
}

@mixin Text-Size-2 ($fontWeight: bold) {
  font-weight: $fontWeight;
  font-size: 28px;
  line-height: 40px;
  letter-spacing: -0.8px;
}

@mixin Text-Size-3 ($fontWeight: bold) {
  font-weight: $fontWeight;
  font-size: 24px;
  line-height: 36px;
  letter-spacing: -0.6px;
}

@mixin Text-Size-4 ($fontWeight: bold) {
  font-weight: $fontWeight;
  font-size: 20px;
  line-height: 30px;
  letter-spacing: -0.5px;
}

@mixin Text-Size-5 ($fontWeight: bold) {
  font-weight: $fontWeight;
  font-size: 18px;
  line-height: 26px;
  letter-spacing: -0.4px;
}

@mixin Body1 ($fontWeight: normal) {
  font-weight: $fontWeight;
  font-size: 16px;
  line-height: 24px;
  letter-spacing: -0.4px;
}


@mixin Body2 ($fontWeight: normal) {
  font-weight: $fontWeight;
  font-size: 14px;
  line-height: 24px;
  letter-spacing: -0.6px;
}

@mixin Sub-Paragraph ($fontWeight: normal) {
  font-size: 12px;
  font-weight: $fontWeight;
  line-height: 18px;
  letter-spacing: -0.4px;
}

@mixin Link-Text {
  font-size: 14px;
  font-weight: normal;
  line-height: 24px;
  letter-spacing: -0.6px;
  color: #3282F0;
}


@mixin Error-Text {
  font-size: 12px;
  font-weight: normal;
  line-height: 18px;
  letter-spacing: -0.4px;
  color: #D73434;
}


// legacy
@mixin Text-Size-6 {
  font-weight: bold;
  font-size: 16px;
  line-height: 28px;
  letter-spacing: -0.3px;
}

@mixin Text-Size-7 {
  font-weight: bold;
  font-size: 14px;
  line-height: 24px;
  letter-spacing: -0.3px;
}

@mixin Paragraph-14 {
  font-size: 14px;
  font-weight: normal;
  line-height: 24px;
  letter-spacing: -0.6px;
}

@mixin Paragraph-16 {
  font-size: 16px;
  font-weight: normal;
  line-height: 24px;
  letter-spacing: -0.4px;
}
```

