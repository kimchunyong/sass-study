# SASS란?

여러가지 정의들..

* CSS pre-processor (전처리기)
	* css를 확장하는 스크립팅 언어로서, 컴파일러를 통하여 브라우저에서 사용할 수 있는 일반 css 문법 형태로 변환합니다.
* Sass는 css를 만들어주는 언어로 자바스크립트 처럼 특정 속성(ex. color, margin, width ...)의 값(ex. #000, 3px, 400px ...)을 변수로 선언하여 필요한 곳에 변수를 적용할 수도 있고, 반복되는 코드를 한번의 선언으로 여러 곳에서 재사용할 수 있도록 해주는 등의 기능을 가졌다.
* Sass는 복작한 작업을 쉽게 할 수 있게 해주고, 코드의 재활용성을 높여줄 뿐만 아니라, 코드의 가독성을 높여주어 유지보수를 쉽게해줍니다.
* Sass 파일의 확장자는 .scss 이다.

## SASS vs SCSS

Sass 는 SASS 표기법(.sass)과 SCSS표기법(.scss)이 있다.
이전 버전에서는 SASS 표기법이 기본 표기법이었으나 Sass 3.0부터 CSS친화적인 scss 표기법이 기본 표기법이 되었다.

## 주석(Comment)

한 줄 주석(//)과 여러 줄 주석(/* */)이 있다.
기존 css에서는 한줄 주석이 없었다.

sass 에서 한줄 주석(//)은 css로 컴파일 되었을 때 나타나지 않는다.
여러 줄 주석은 컴파일 되었을 때 동일하게 나타난다.

## 데이터타입

sass 에서 사용할 수 있는 데이터타입

* 숫자 : 1,2, 3 ... 10px 도 숫자로 본다.
* 문자열 : 겹따옴표, 홑따옴표, 따옴표가 없는 것 모두 문자열로 인식
* 색상 : blue, #001100, rgba(255, 0, 0, 0,3)
* 불리언 : true, false
* null
* 리스트(lists) : 괄호 속에서 컴마나 공백으로 구분.
	* margin, padding 속성 값 지정에 사용되는 0 auto와 font-family 속성 값 지정에 사용되는 Helvetica, Arial, Sans-serif 등은 공백또는 콤마로 구분된 값의 list이다.
	* 1.5em 1em 0 2em, Helvetica, Arial, sans-serif
* 맵(maps) : 키:값 쌍이 괄호로 구분 (JSON과 유사한 방식)
	* (key1 : value1, key2 : value2, key3 : value3)

## 변수(Variable)

css에 변수 개념을 도입
변수로 사용 가능한 형태 - 숫자, 문자열, 폰트, 색상, null, lists, maps
변수를 선언하고 사용할 때는 $문자를 사용.

변수란? 데이터를 저장하는 곳, 저장해뒀다가 필요할 때 사용할 수 있다.

`$변수명 : 속성값;`


```scss
// sass

$bg-color:#333;

.header{
	background-color:$bg-color;
}

// css compiled

.header{
	background-color:#333;
}
```

변수를 만들어도, 사용하지 않으면 컴파일된 css파일에는 아무것도 나타나지 않음

### 변수 범위(Variable Scope)

Sass의 변수에 변수 범위가 적용된다.
변수를 특정 selector(선택자)에서 선언하면 해당 selector에서만 접근이 가능!(지역변수)
지역변수의 유효범위는 자신이 속한 코드블럭과 하위 코드 블럭이다.


```scss
// sass

$bg-color:#eee;

.header{
	$bg-color:#333;
	background-color:$bg-color;
}
.container{
	background-color:$bg-color;
}

// css compiled

.header {
	background-color: #333;
}

.container {
	background-color: #eee;
}
  ```

### `!global` 플래그

변수를 선언할 때 전역(global)으로 설정 할 때는 `!global` 플래그를 사용한다.
(코드 블럭안에서 선언한 변수를 전역변수로 사용하고자 할 때 사용한다.)

```scss
// sass

$bg-color:#eee;

.header{
	$bg-color:#333 !global;
	background-color:$bg-color;
}
.container{
	background-color:$bg-color;
}

// css compiled

.header {
	background-color: #333;
}

.container {
	background-color: #333;
}
```

### `!default` 플래그

`!default` 플래그는 해당 변수가 설정되지 않았거나 값이 null 일 때 값을 설정한다.


```scss
// sass

$bg-color:#333;
$bg-color:#eee !default;

.header{
	background-color:$bg-color;
}

// css compiled

.header {
	background-color: #333;
}
```

## 수학 연산자(Math Operators)

Sass에서는 수학 연산자들을 사용 할 수 있다.

Operator | Description
------------ | -------------
+ | addition
- | subtracition
/ | division
* | multiplication
% | modulo
== | equality
!= | inequality


```scss
// sass

.container{
	.lnb{
		float:left;
		width:100px / 500px * 100%;
	}
	.contents{
		float:left;
		width:400px / 500px * 100%;
	}
}

// css compiled

.container .lnb {
	float: left;
	width: 20%;
}

.container .contents {
	float: left;
	width: 80%;
}
```

주의할 점은 `+`,`-` 연산자를 사용할 때는 단위를 언제나 통일 시켜야한다.
아래와 같은 코드는 오류이다.

```scss
$width : 100% - 40px;
```

위와 같은 작업을 해야한다면 css의 `calc()` 함수를 사용해야한다.

%, em, rem, vh, vw, vmin, vmax와 같이 상대적인 값을 Sass는 알지 못한다.
상대적인 값의 결과값은 브라우저만이 알 수 있기 때문!
상대적인 값을 갖는 단위의 연산은 동일한 단위를 갖는 값과의 연산만이 유효하다.
```scss
#foo{
	width:5% + 10% ;	// 15%
}
```

### `/` 연산자
```scss
p{
	font: italic bold 12px/30px Georgia, serif;
}
```
css에서의 `/`는 나눗셈의 의미가 아니라 값을 분리하는 의미를 갖는다.
Sass의 `/` 연산자를 사용하기 위해서는 몇가지 조건이 필요하다.
* 변수에 대해서 사용
* 괄호 내에서 사용
* 다른 연산의 일부로서 사용

변수를  css의 `/`와 함께 사용하고자 하는 경우 `#{}`를 사용한다.
```scss
p{
	$font-size:12px;
	$line-height:30px;
	font:#{$font-size}/#{$line-height};	// 12px/30px
}
```

## 내장함수(Built-in Functions)

자바스크립트에서 제공하는 내장함수처럼 Sass에서도 많은 내장함수를 지원해준다.
그중에 `darken()` 함수를 사용해본다.
이 함수는 얼마나 어둡게 색상을 적용할지 인수로 던져주면 자동으로 색상을 계산해서 나타내준다.


```scss
// sass

$buttonColor: #2ecc71;
$buttonDark: darken($buttonColor, 10%);
$buttonDarker: darken($buttonDark, 10%);

.foot_menu{
	li{
		display: inline-block;
		a{
			background-color:$buttonColor;
			color:#fff;
		}
		&:nth-child(2){
			a{
				background-color:$buttonDark;
			}
		}
		&:nth-child(3){
			a{
				background-color:$buttonDarker;
			}
		}
	}
}

// css compiled

.foot_menu li a {
	background-color: #2ecc71;
	color: #fff;
}
.foot_menu li:nth-child(2) a {
    background-color: #25a25a;
}
.foot_menu li:nth-child(3) a {
    background-color: #1b7943;
}
```

이 함수 외에도 유용한 함수들이 엄청 많다.
[여기서](http://jackiebalzer.com/color) 확인할수 있다.

## 중첩(Nesting)

Sass는 선언을 중첩시킬 수 있다.

```scss
// css

.container{
	width:100%;
}
.container h1{
	color:red;
}

// sass

.container{
	width:100%;
	h1{
		color:red;
	}
}
```

css의 선택자가 길면은 계속해서 중복해서 사용해야 하는데 중첩을 하면 중복되는 선택자 코드를 줄일 수 있다.

### `&` Ampersand

* 부모참조선택자(Referencing Parent Selectors)
* 부모선택자를 참조할 때는 `&`문자를 사용한다.

```scss
// sass
a{
	color:#000;
	&:hover{
		text-decoration:underline;
		color:red;
	}
	&:visited{
		color:purple;
	}
}

// css compiled

a {
	color: #000;
}
a:hover {
	text-decoration: underline;
	color: red;
}
a:visited {
	color: purple;
}
```

### `@at-root`
중첩에서 벗어나려면 `@at-root` 지시자를 사용한다.
아래 코드에서 sibling 클래스가 container 클래스 밖에서도 사용한다고 할때 이 지시자를 사용한다.

```scss
// sass

.container{
	.child{
		color:blue;
	}
	@at-root .sibling{
		color:gray;
	}
}

// css compiled

.container .child {
	color: blue;
}
.sibling {
	color: gray;
}
```

### 속성 중첩
font와 같은 속성은 `font-family`, `font-size`, `font-weight` 등의 세부속성으로 나뉘는데, 이들 역시 중첩으로 표현할 수 있다.

```scss
// sass

h1{
	font : {
		family:verdana;
		size:20px;
		weight:bold;
	}
}

// css compiled

h1 {
	font-family: verdana;
	font-size: 20px;
	font-weight: bold;
}
```

### 중첩을 사용할 때 참고할 것
Sass 코드 중첩을 할때, 4레벨 보다 깊게 들어가지 말 것

* 유지보수가 어려워진다.
* 가독성이 떨어진다.

## 불러오기(Import)
import 기능은 스타일들을 여러파일들로 나누고, 다른파일에서 불러와서 사용하는 기능이다.
(1개의 css파일에 모든 스타일을 기술하는 것은 가독성을 나쁘게한다. 따라서 규칙을 정해서 파일을 분리하여 개발하는 것이 유지보수 측면에서 효과적이다.)
`@import` 지시자를 사용하여 분리된scss 파일을 불러올 수 있다.

```scss
@import "layout.scss";
```

확장자는 생략이 가능하다.
```scss
@import "layout";
```

한줄에 여러 scss를 import한다.
```scss
@import "common", "easing";
```

### partial
여러개의 파일로 분할 하는 것 또는 분할된 파일을 partial이라 하며 partial된 sass 파일명의 선두에는 underscore(_)를 붙인다. (_reset.scss, _module.scss, _print.scss)

예를 들어 "_reset.scss"라는 partial된 Sass 파일이 있고 이 파일을 import 할 경우 `@import "reset"` 이처럼 기술할 수 있는데, 파일 명 선두의 _는 생략할 수 있다.

partial된 sass 파일명 선두에 붙인 (_) 의 의미는 import는 수행하되 css로의 컴파일은 수행하지 말라는 의미를 갖는다.
따라서 partial은 import시에는 css파일로 컴파일 되지 않기 때문에 최종적으로 css로 컴파일을 수행할 Sass 파일에서 import한다.

`@import`는 CSS rule 또는 @media rule 내에 포함시키는 것도 가능하다.

```scss

//scss

// _color.scss
.example{
	color:red;
}

#main {
	@import "color";
}

// css compiled

#main .example{
	color:red;
}

```

## 상속(Extend)

### `@extend` 지시자
특정 선택자를 상속 할 때 사용한다.

적용 방법
`@extend .클래스명`
`@extend %클래스명`

```scss
// sass

.txt{
	font-size:12px;
	line-height:1.6;
	letter-spacing: -.5px;
	color:#333;
}
.box_cont{
	@extend .txt;
}

// css compiled

.txt, .box_cont {
	font-size: 12px;
	line-height: 1.6;
	letter-spacing: -.5px;
	color: #333;
}
```

### placeholder 선택자 `%`
`%`를 사용하면 상속은 할 수 있지만 해당 선택자는 컴파일 되지 않는다.

```scss
// sass

%txt{
	font-size:12px;
	line-height:1.6;
	letter-spacing: -.5px;
	color:#333;
}
.box_cont{
	@extend %txt;
}

// css compiled

.box_cont {
	font-size: 12px;
	line-height: 1.6;
	letter-spacing: -.5px;
	color: #333;
}
```

## 믹스인(Mixin)
extend와 비슷하지만 argument(인수)를 받을 수 있습니다.
mixin을 선언 할 때는 `@mixin` directive를 사용하며, 이를 사용할 때는 `@include` directive를 사용한다.

선언 - `@mixin mixin명(인자값){}`
호출 - `@include mixin명(인자값)`

```scss
// sass

@mixin headline($color, $size){
	color:$color;
	font-size:$size;
	font-weight:bold;
	margin-bottom:10px;
	line-height:1;
}
.box_cont{
	h2{
		@include headline(#000, 15px);
	}
}

// css compiled

.box_cont h2 {
	color: #000;
	font-size: 15px;
	font-weight: bold;
	margin-bottom: 10px;
	line-height: 1;
}
```

Mixin을 응용해본다.

```scss
// sass

@mixin media($queryString){
	@media #{$queryString}{
		@content;
	}
}
.wrap{
	@include media("(max-width:767px)"){
		max-width:100%;
	}
}

// css compiled

@media (max-width: 767px) {
	.wrap {
		max-width: 100%;
	}
}
```

mixin을 활용해서 vendor prefix를 적용하는 코드를 만들어본다.

```scss

// sass

@mixin prefix($property, $value){
	@each $prefix in -webkit-, -moz-, -ms-, -o-, '' {
		#{$prefix}#{$property}:$value;
	}
}

.radius{
	@include prefix(transition, 0.5);
}

// css compiled

.radius {
	-webkit-transition: 0.5;
	-moz-transition: 0.5;
	-ms-transition: 0.5;
	-o-transition: 0.5;
	transition: 0.5;
}
```

### `#{}` Interpolation
이 표현은 특정문자열을 따로 처리하지 않고 그대로 출력 할 때 사용
변수는 속성값으로만 사용할 수 있으나 `#{}`을 사용하면 속성값은 물론 셀럭터와 속성명에도 사용할 수 있다.
또한 연산의 대상으로 취급되지 않도록 할 수도 있다.

### `@content` 지시자
선택자 내부의 내용들이 @content부분에 나타나게 된다.

## 함수(Function)
내장함수(Built-in function)과는 달리 이 부분은 임의 함수이다.
mixin은 style markup을 반환하지만, function은 `@return` 지시자를 통하여 값을 반환한다.
`@function` 지시자를 사용해서 함수를 만든다.

```scss
// sass

@function calc-percent($target, $container){
	@return ($target / $container) * 100%;
}
@function cp($target, $container){
	@return calc-percent($target, $container);
}
.container{
	.lnb{
		width:cp(100px, 500px);
	}
}

// css compiled

.container .lnb {
	width: 20%;
}
```

자주 사용할 것 같은 함수는 위와 같이 단축함수를 만들어 사용해라!

## @if
Sass 에서도 조건문을 사용할 수 있다.
`@if , @else if, @else`

```scss
// sass

$box-type:text;
ul{
	li{
		@if $box-type == text {
			font-size:15px;
		} @else if $box-type == thum {
			font-size:12px;
		}
	}
}

// css compiled

ul li {
	font-size: 15px;
}
```

## if() 함수

`if(condition, if_true, if_false)`

```scss
a{
	color:if($type == ocean, bl;ue, black);
}
```

## @for
Sass 에서도 반복문을 사용할 수 있다.
순차적인 값에 대해서 사용한다.
`@for` 지시자를 사용한다.

```scss
// sass

@for $i from 1 through 3{
	.lst-#{$i}{
		padding-left:10px * $i;
	}
}

// css compiled

.lst-1 {
	padding-left: 10px;
}
.lst-2 {
	padding-left: 20px;
}
.lst-3 {
	padding-left: 30px;
}
```

범위 표현은 `from 1 to 3`로 쓸수도 있다.

## @each
`@each`는 리스트나 맵의 각 값을 순회한다.
맵의 경우 변수를 키, 값으로 준다.

```scss
// sass

@each $animal in puma, sea-slug, egret, salamander {
	.#{$animal}-icon {
		background-image: url('/images/#{$animal}.png');
	}
}

// css compiled

.puma-icon {
	background-image: url("/images/puma.png");
}
.sea-slug-icon {
	background-image: url("/images/sea-slug.png");
}
.egret-icon {
	background-image: url("/images/egret.png");
}
.salamander-icon {
	background-image: url("/images/salamander.png");
}
```

```scss
// sass

@each  $animal, $color, $cursor in(puma, black, default), (sea-slug, blue, pointer), (egret, white, move){
	.#{$animal}-icon{
		background-image:url('/images/#{$animal}.png');
		border:2px solid $color;
		cursor:$cursor;
	}
}

// css compiled

.puma-icon {
	background-image: url("/images/puma.png");
	border: 2px solid black;
	cursor: default;
}

.sea-slug-icon {
	background-image: url("/images/sea-slug.png");
	border: 2px solid blue;
	cursor: pointer;
}

.egret-icon {
	background-image: url("/images/egret.png");
	border: 2px solid white;
	cursor: move;
}
```

```scss
// sass

@each $header, $size in (h1:2em, h2:1.5em, h3:1.2em){
	#{$header}{
		font-size:$size;
	}
}

// css compiled

h1 {
	font-size: 2em;
}
h2 {
	font-size: 1.5em;
}
h3 {
	font-size: 1.2em;
}
```

## @while
`@while` 반복문을 사용할 수 있다.

```scss
// sass

$j: 6;
@while $j > 0 {
    .item-#{$j} { width: 2em * $j; }
    $j: $j - 2;
}

// css compiled

.item-6 {
	width: 12em;
}
.item-4 {
	width: 8em;
}
.item-2 {
	width: 4em;
}
```

