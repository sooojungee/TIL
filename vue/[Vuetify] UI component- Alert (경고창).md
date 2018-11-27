# [Vuetify] UI component: Alert (경고창)

`v-alert` 컴포넌트는 중요한 정보를 유저에게 전하는데 쓰인다.
종류는 4가지가 있다.
**success**, **info**, **warning**, **error**
기본 아이콘은 각기 다른 동작을 나타내고 바꿀 수 있다.

사이트: https://vuetifyjs.com/ko/components/alerts

## 1. 사용법

```html
<v-alert :value="true" type="success"> This is a success alert. </v-alert>
```

## 2. API

### 2-1. color
- **디폴트** : undefined
- **타입**: string
- **설명**: 배경 색상/ 아이콘 색상을 지정한다.

```html
<v-alert :value="true" :color:"'#efefef'"> This is a success alert. 	</v-alert>
```
### 2-2. dismissible

- **디폴트** : false
- **타입** : boolean
- **설명** :경고창을 닫을 수 있게 지정한다.
					경고창에 close 버튼을 만들어서 닫을 수 있게 한다.
```html
<v-alert :value="true" :dismissible="true"> This is a success alert. </v-alert>
```

### 2-3. icon
- **디폴트** : undefined
- **타입** : string

- **설명** : 특정 아이콘을 지정한다.
type을 정하면 아이콘이 지정된다.
icon과 type을 정하지 않으면 아이콘이 보이지 않는다.
icon은 material icon을 가져오는 것이므로 material icon의 이름을 사용하면 된다.

```html
<v-alert :value="true" icon="bookmark"> This is a success alert. </v-alert>
```

### 2-4. mode
- **디폴트** : undefined
- **타입** : string

- **설명**: 트랜지션 모드를 설정한다. (transition-group에는 적용되지 않는다.)

### 2-5. origin
- **디폴트** : undefined
- **타입** : string

- **설명** : 트랜지션의 중심을 설정한다.


### 2-6. outline

- **디폴트** : undefined
- **타입** : boolean

- **설명** : 경고창의 테두리를 설정한다. (배경 색은 없어지고 배경색상이 테두리 색이 된다.)
		- 바인딩 된 값이 false (boolean) 일 때만 테두리가 보이지 않는다.
```html
<!-- 테두리가 안보이는 경우 -->
<v-alert :value="true" :outline="false"> This is a success alert. </v-alert>
<v-alert :value="true"> This is a success alert. </v-alert>

<!-- 테두리가 보이는 경우 -->
<v-alert :value="true" outline="false"> This is a success alert. </v-alert>
<v-alert :value="true" :outline="'false'"> This is a success alert. </v-alert>
<v-alert :value="true" outline="true"> This is a success alert. </v-alert>
<v-alert :value="true" outline="hi"> This is a success alert. </v-alert>
<v-alert :value="true" :outline="01234"> This is a success alert. </v-alert>
```

### 2-7. transition

- **디폴트** : undefined
- **타입** : string
- 
- **설명** : 컴포넌트 트랜지션을 설정한다. 내장 트랜지션과 사용자 트랜지션 모두 가능
- **값** (내장 트랜지션)
	- `fade-transition` : 서서히 생기고 서서히 없어진다.
	- `scroll-x-transition`: 왼쪽 -> 오른쪽으로 생기고 오른쪽으로 사라짐
	- `scroll-x-reverse-transition`: 오른쪽 -> 왼쪽으로 생기고 왼쪽으로 사라짐
	- `scroll-y-transition`: 위 -> 아래로 생기고 아래으로 사라짐
	- `scroll-y-reverse-transtion`: 아래 -> 위로 생기고 아래로 사라짐
	- `slide-x-transition` : 왼쪽 -> 오른쪽으로 생기고 왼쪽으로 사라짐
	- `slide-x-reverse-transition` 오른쪽 -> 왼쪽으로 생기고 오른쪽으로 사라짐
	- `slide-y-transition` : 위 -> 아래로 생기고 위로 사라짐
	- `slide-y-reverse-transition` : 아래 -> 위로 생기고 아래로 사라짐
	- `scale-transition` : 크기가 점점 커지면서 생기고 크기가 점점 작아지면서 사라짐

```html
<v-alert :value="true" transition="slide-x-transition"> This is a success alert. </v-alert>
```


### 2-8. type
- **디폴트** : undefined
- **타입** : string

- **설명** :success, info, warning, error 를 지정한다. 
상황별 색상과 미리 정의된 아이콘을 사용한다.

```html
<v-alert :value="true" type="success"> This is a success alert. </v-alert>
```
타입 | 색상 | 아이콘 (material icon)
------|-----|--------
success| 초록색 | check_circle
info| 파랑색|info
warning| 노란색|priority_high
error| 빨강색 | warning

> **type을 지정하지 않았을 때**
> 아이콘은 나오지 않고 배경/아이콘 색상은 `type: error`와 같은 빨강색이 나온다.


### 2-9. value
- **디폴트** : undefined
- **타입** : any

- **설명** :경고창의 표시 여부를 조절한다.
value을 적지 않으면 경고창이 표시되지 않는다.

	- 경고창이 표시되지 않는 경우
	value의 값이 false, 0 이거나 값이 없을 때
	- 경고창이 표시되는 경우
	value의 값이 true, 0이 아닌 숫자일 때
	
```html
<!-- 알림창이 보이는 경우 -->
<v-alert :value="true"> This is a success alert. </v-alert>
<v-alert :value="234"> This is a success alert. </v-alert>

<!-- 알림창이 보이지 않는 경우 -->
<v-alert > This is a success alert. </v-alert>
<v-alert :value="false"> This is a success alert. </v-alert>
<v-alert :value="0"> This is a success alert. </v-alert>
```