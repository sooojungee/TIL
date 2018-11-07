# [css/sass] 그라데이션 넣기

width/ height에 맞게 그라데이션 넣는 방법

## 수평: 왼쪽에 색 ~ 오른쪽 색 (투명)
< 왼쪽은 rgba(255, 255, 0, 1) 파란색 왼쪽은 (255, 255, 255, 0) 투명색 >

```sass
/* FF3.6-15 */
background: -moz-linear-gradient(left, rgba(255,0,0,1) 0%, rgba(255,255,255,0) 100%)
```
`left`: 왼쪽부터
`rgba(255, 0, 0, 1)`: 색상을
`0%`: width * 0% 의 위치부터 시작해서
`rgba(255, 255, 255, 0)`: 색상이
`100%`: width * 100%의 위치까지

```sass
/* Chrome 10-25, Safari 5.1-6 */
background: -webkit-linear-gradient(left, rgba(255,0,0,1) 0%,rgba(255,255,255,0) 100%)
```
`left`: 왼쪽부터
`rgba(255, 0, 0, 1)`: 색상을
`0%`: width * 0% 의 위치부터 시작해서
`rgba(255, 255, 255, 0)`: 색상이
`100%`: width * 100%의 위치까지

```sass
/* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
background: linear-gradient(to right, rgba(255,0,0,1) 0%,rgba(255,255,255,0) 100%)
```
`to right`: 오른쪽으로
`rgba(255, 0, 0, 1)`: 색상을
`0%`: width * 0% 의 위치부터 시작해서
`rgba(255, 255, 255, 0)`: 색상이
`100%`: width * 100%의 위치까지
```sass
/* IE6-9 */
filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff0000',endColorstr='#00ffffff',GradientType=1 )
```
`startColor`: 그라데이션 시작 색(왼쪽)

## 수직: 위쪽 색 ~ 아래쪽 색

< 위쪽은 rgba(255, 255, 0, 1) 파란색 아래쪽은 (255, 255, 255, 0) 투명색 >

```sass
/* FF3.6-15 */
background: -moz-linear-gradient(top, rgba(255,0,0,1) 0%, rgba(255,255,255,0) 100%)
```
`top`: 위쪽부터
`rgba(255, 0, 0, 1)`: 색상을
`0%`: height * 0% 의 위치부터 시작해서
`rgba(255, 255, 255, 0)`: 색상이
`100%`: height * 100%의 위치까지 아래로

```sass
/* Chrome 10-25, Safari 5.1-6 */
background: -webkit-linear-gradient(top, rgba(255,0,0,1) 0%,rgba(255,255,255,0) 100%)
```
`top`: 위쪽부터
`rgba(255, 0, 0, 1)`: 색상을
`0%`: height * 0% 의 위치부터 시작해서
`rgba(255, 255, 255, 0)`: 색상이
`100%`: height * 100%의 위치까지 아래로 

```sass
/* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
background: linear-gradient(to right, rgba(255,0,0,1) 0%,rgba(255,255,255,0) 100%)
```
`to bottom`: 위쪽으로
`rgba(255, 0, 0, 1)`: 색상을
`0%`: height * 0% 의 위치부터 시작해서
`rgba(255, 255, 255, 0)`: 색상이
`100%`: height * 100%의 위치까지 아래로
```sass
/* IE6-9 */
filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ff0000',endColorstr='#00ffffff',GradientType=1 )
```
`startColor`: 그라데이션 시작 색(위쪽)

## 왼쪽 색 ~ 가운데 색 (투명) ~ 오른쪽 색

< 오른쪽은 rgba(0, 255, 0, 1) 초록색 중간은 rgba(255, 255, 255, 1)투명색 오른쪽은 rgba(255, 0, 0, 1) 빨간색>

전체 width의 0~12% : 초록색에서 투명색
전체 width의 12 ~ 85%: 투명색에서 투명색
전체 width의 85 ~ 100%: 투명색에서 빨간색

```sass
/* FF3.6-15 */
background: -moz-linear-gradient(left, rgba(0,255,0,1) 0%, rgba(255,255,255,0) 12%, rgba(255,255,255,0) 85%, rgba(255,255,255,0.07) 86%, rgba(255,0,0,1) 100%)
```
```sass
/* Chrome 10-25, Safari 5.1-6 */
background: -webkit-linear-gradient(left, rgba(0,255,0,1) 0%,rgba(255,255,255,0) 12%,rgba(255,255,255,0) 85%,rgba(255,255,255,0.07) 86%,rgba(255,48,48,1) 100%)
```
```sass
/* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
background: linear-gradient(to right, rgba(0,255,0,1) 0%,rgba(255,255,255,0) 12%,rgba(255,255,255,0) 85%,rgba(255,255,255,0.07) 86%,rgba(255,48,48,1) 100%)
```
```sass
/* IE6-9 */
filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#00ff00', endColorstr='#ff3030',GradientType=1 )
```

## 위쪽 색 ~ 가운데 색 (투명) ~ 아래쪽 색

< 위쪽은 rgba(0, 255, 0, 1) 초록색 중간은 rgba(255, 255, 255, 1)투명색 아래쪽은 rgba(255, 0, 0, 1) 빨간색>

전체 height의 0~12% : 초록색에서 투명색
전체 height의 12 ~ 85%: 투명색에서 투명색
전체 hegiht의 85 ~ 100%: 투명색에서 빨간색

```sass
/* FF3.6-15 */
background: -moz-linear-gradient(top, rgba(0,255,0,1) 0%, rgba(255,255,255,0) 12%, rgba(255,255,255,0) 85%, rgba(255,255,255,0.07) 86%, rgba(255,0,0,1) 100%)
```
```sass
/* Chrome 10-25, Safari 5.1-6 */
background: -webkit-linear-gradient(top, rgba(0,255,0,1) 0%,rgba(255,255,255,0) 12%,rgba(255,255,255,0) 85%,rgba(255,255,255,0.07) 86%,rgba(255,48,48,1) 100%)
```
```sass
/* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
background: linear-gradient(to bottom, rgba(0,255,0,1) 0%,rgba(255,255,255,0) 12%,rgba(255,255,255,0) 85%,rgba(255,255,255,0.07) 86%,rgba(255,48,48,1) 100%)
```
```sass
/* IE6-9 */
filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#00ff00', endColorstr='#ff3030',GradientType=0 )
```
