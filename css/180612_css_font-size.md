# [HTML/CSS] font-size 크기 최소
각 브라우저나 화면 크기에 따라 나타낼 수 있는 **font-size의 최소 크기가 정해져있다.**

``font-size: 2px``를 적었을 때 실제로 2px로 표시하지는 않는다.

실제로는 ```font-size: xx-small```의 값을 갖는다.

**font-size의 최소 값**은 ``font-size: xx-small``을 사용하면 알 수 있는데,

``xx-small``는 font-size의 default값인 ``medium``에 대한 상대적인 크기로 브라우저에서 나타낼 수 있는 가장 작은 값이다.

## font-size 작게 만드는 방법

**transform: scale()을 사용한다.**

```
.text {
	font-size: 15px
	transform: scale(0.5)
}
```
이 때, transform: scale을 사용할 경우, .text의 모든 요소의 비율이 0.5로 줄어드는 문제점이 있다.

이 때 .text는 글씨 크기와 관련된 부분만 잡아주고,
배경색과 같은 다른 부분을 정해주는 상위 클래스를 만든다. 
```
<div class = "content">
	<div class = "text"> 작은 글씨</div>
</div>
```
