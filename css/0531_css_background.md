# [HTML/CSS] background 속성

color, clip, origin, blend-mode, image, repeat, position, size
## 1. color
 배경의 색을 결정한다.
 > 따로 속성을 지정하지 않을 경우, 배경색은 나타나지 않는다.
상속 X
### color 속성
```
/* hex 색상 코드*/
background-color : #f00;

/* 지정되어 있는 색상 */
background-color: red;

/* rgb 색상 코드 */
background-color: rgb(255, 0, 0);

/* rgb에 투명도(alpha)가 추가된 색상 코드 */ 
background-color: rgb(255, 0, 0, 1);
```

## 2. clip
배경의 보여지는 부분을 결정한다.

### clip 속성

```
/* 기본값이며, 테두리를 포함한 부분을 배경으로 보여준다.. */
background-clip: border-box;

/* 테두리를 제외하고 padding부분부터 배경으로 보여준다.*/
background-clip: padding-box;

/* 테두리, padding 영역을 제외한 부분을 배경으로 보여준다. 
(내용이 포함된 부분) */
backgound-clip: content-box;

/* 이 속성을 기본값으로 설정한다.*/
backgound-clip: initial;

/* 상위 요소의 속성을 상속받는다. */
backgound-clip: inherit;

```

## 3. origin
 배경의 시작위치를 결정한다.
> background-attachment 속성값이 fixed 일 경우, origin속성은 무시된다.
```
/* 기본값이며, 배경이 padding 바깥 가장자리 왼쪽 위에서 시작된다.
background-origin: padding-box;

/* 배경이 테두리 왼쪽 위에서 시작된다. */
background-origin: border-box;

/* 배경이 내용의 왼쪽 위에서 시작된다. */
background-origin: content-box;

/* 이 속성을 기본값으로 설정한다. */
background-origin: initial;

/* 상위 요소의 속성을 받는다. */
background-origin: inherit;

```

## 4. blend-mode
배경 레이어(색상 및 이미지)의 혼합모드를 결정한다.
배경 두 가지를 혼합하는 속성이다.
```
    background-image: linear-gradient(to right,blue 0%,red 100%),url("abc.gif');
```
위와 같은 경우, "abc.gif"의 이미지만 보여지게 된다.

아래와 같이 blend-mode를 사용하여 이미지와 배경이 혼합된 이미지로 보여주는 효과를 줄 수 있다.
```
background-blend-mode: normal;

background-blend-mode: multiply;

background-blend-mode: screen;

background-blend-mode: overlay;

background-blend-mode: darken;

background-blend-mode: lighten;

background-blend-mode: color-dodge;

background-blend-mode: saturation;

background-blend-mode: color;

background-blend-mode: luminosity;

```
 

## 5. image
 배경 이미지를 결정한다.

>기본 값이 없다.
```
/* 이미지 파일이 저장되어 있을 경우 */
background-image: url("computer.gif");

/* "" 없어도 가능하다 */
background-image: url(computer.gif);

/* 이미지 파일이 저장되어 있지 않을 경우, 이미지 주소를 가져온다. */
background-image: url("http://trendw.kr/sites/default/files/styles/contents_large/public/Screen%20Shot%202015-09-03%20at%208.12.25%20AM.png?itok=zvrMvhvF");

/* "" 없어도 가능하다 */
background-image: url(http://trendw.kr/sites/default/files/styles/contents_large/public/Screen%20Shot%202015-09-03%20at%208.12.25%20AM.png?itok=zvrMvhvF);

```
## 6. repeat

```
/* 기본값이며, 배경 이미지가 수직, 수평으로 반복된다.
 영역을 벗어나는 부분은 잘려서 보여진다. */
background-repeat: repeat;

/* 배경 이미지가 x축(수평)으로만 반복된다.
 영역을 벗어나는 부분은 잘려서 보여진다.*/
background-repeat: repeat-x;

/* 배경 이미지가 y축(수직)으로만 반복된다.
 영역을 벗어나는 부분은 잘려서 보여진다.*/
background-repeat: repeat-y;

/* 배경 이미지가 반복되지 않는다. */
background-repeat: no-repeat;

/* 이미지가 수직, 수평으로 반복된다.
 이미지가 들어갈만큼 채우고 여백은 공백처리한다. 
 (균일하게 이미지 보여줌)*/
background-repeat: space;

/* 배경 이미지가 수직, 수평으로 반복된다.
 이미지의 반복으로 공간을 여백없이 채운다.
 공간을 채우기 위해 이미지가의 길이가 변경된다. 
 (적합한 크기로 이미지 보여줌)*/
background-repeat: round;

/* 이 속성을 기본 값으로 설정한다. */
background-repeat: initial;

/* 상위 요소의 속성을 가진다. */
background-repeat: inherit;

```
## 7. position
 배경의 왼쪽 위 모서리 위치를 결정한다.

>애니메이션: o
```
/* 왼쪽/오른쪽 위/아래의 값을 지정한다.*/
background-position: left top;

/* 하나만 적어줄 경우, 나머지 값은 center가 된다.
 (아래의 경우, left center로 적용된다.) */
background-position: left;

/* 이렇게도 사용이 가능하다. */
background-position: center;

/* 기본 값은 0% 0%이며, x와 y의 값은 (0~100)*/
background-position: x% y%;

/* px 및 다른 단위를 단위를 혼합할 수 있다.*/
background-position: xpos ypos;

/* 이 속성을 기본 값으로 설정한다. */
background-position: initial;

/* 상위 요소의 속성을 가진다. */
background-position: inherit;
```
## 8. size
 배경의 이미지의 크기를 결정한다.
```
/* 기본값이며, 배경 이미지가 원래 크기대로 나타난다. */
background-size: auto;

/* 배경 이미지의 너비와 높이를 설정한다.
첫 번재 값은 넓이, 두번째 값은 높이이다. */
background-size: xpos ypos;
/* 값이 하나만 주어지면 두 번째 값은 자동으로 설정된다. */
background-size: length;

/* 배경 이미지의 너비와 높이를 설정한다.
첫 번재 백분율은 넓이, 두번째 백분율은 높이이다. */
background-size: x% n%;
/* 값이 하나만 주어지면 두 번째 값은 자동으로 설정된다. */
background-size: percentage;

/* 배경 이미지의 크기 비율은 유지한 상태에서, 
전체 이미지가 들어가도록 해당 영역의 가로/세로 중 큰 값에 맞춰
 이미지를 보여준다. */
background-size: cover;

/* 배경 이미지의 크기 비율은 유지한 상태에서, 
전체 이미지가 들어가도록 가로/세로 중 큰 값에 맞춰 
이미지를 보여준다. */
background-size: contain;

/* 이 속성을 기본 값으로 설정한다. */
background-size: initial;

/* 상위 요소의 속성을 가진다. */
background-size: inherit;

```
