# [SASS] @mixin, $사용법

## @mixin

- @mixin은 반복되는 같은 속성의 여러 줄의 코드를 정의하여 사용을 편리하게 한다.

### 1. 받아오는 변수가 없을 경우
[선언]
**@mixin 이름지정** 후 아래에 코드를 적어준다.
```
/* @mixin 이름이 'inline-block'인 코드 */
@mixin inline-block
	display: inline-block
	vertical-align: top
```

[활용]
**@inlcude 이름** 을 통해 사용한다.
```
.content
	width: 100%
	height: 100%
	@include inline-block
```
### 2. 받아오는 변수가 있을 경우
[선언]
**@mixin 이름지정($변수)**
```
@mixin backgroundImage($url)
	background-image:($url)
	background-size: cover
```
[활용]
**@include 이름()** 를 통해 사용한다.
```
.content
	@include backgroundImage('./user/image.gif')
	width: 100%
	height: 100%
```

## $
- 반복되는 속성을 한번에 정의하여 가독성을 높인다.

[선언]
```
$title_color: rgb(20, 52, 78)
```

[활용]
```
.content
	background-color: $title_color
```
