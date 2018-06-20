# [SASS] @media screen

스크린 크기에 따라 화면에 보여지는 요소들의 구성이 달라보이게 한다.

이를 통해, pc화면, tablet화면, mobile화면 에서의 요소들의 구성을 다르게 나타낼 수 있다.

## 사용법

### 1. mixin 쓰는법 세가지

- **max-width : n px** (width의 크기가 n 이하일 때)

```
/* width의 크기가 1280px 이하일 때 */
@media screen and (max-width: 1280px)
```
- **min-width: n px** (width의 크기가 n보다 클 때)

```
/*  width의 크기가 780px 이상일 때 */
@media screen and (min-width: 780px)
```

- **(max-width: a px) and (min-width: b px)** (width가 a보다 작고 b보다 클 때 )
```
/* width의 크기가 780px보다 크고 1280px보다 작을 때 */
@media screen and (min-width: 780px) and (max-width: 1280px)
```
### 2. mixin을 통한 사용법

_global.sass에서 사용하면 좋다.
```
@mixin small-view
	@media screen and (max-width: 700px)
		@content

@mixin middle-view
	@media screen and (max-width: 900px)
		@content
```

### 3.  구성 변경이 필요한 sass에서 활용법

```
.content
	background: #f00
	@include small-view
		background: #0f0
```
@mixin small-view의 @content 부분에 background: #0f0이 들어간다.


### 주의할 점

```
.content
	background: #f00
	@inclue small-view
		background: #0f0
	@include middle-view
		background: #0ff
```

코드는 위에서 아래로 읽힌다.

위와 같은 코드를 작성했을 때, width의 크기가 400px가 되면 small-view의 700px보다 작고 middle-view의 900px보다도 작아서 두개의 @media를 모두 만족하여 위에 있는 small-view는 무시되고 아래에있는 middle-view를 따르게 된다.

따라서 크기가 큰 순서부터 적어주는게 올바르다.

```
.content
	background: #f00
	@include middle-view
		background: #0ff
	@inclue small-view
		background: #0f0
```





