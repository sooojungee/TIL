# [HTML/CSS] overflow 속성

## overflow
 - 내부 요소의 컨텐츠가 외부 요소의 크기보다 클 때 그 컨텐츠를 어떻게 보여주는지를  구성하는 속성이다.

- 이 속성을 사용하기 위해서는 꼭, **height**가 설정되어 있어야한다.
- overflow는 **상속되지 않는다**.
 
### 1. visible
```
	.parent{
		overflow: visible;
		/* 외부 요소의 크기에 영향을 받지 않는다. */
	}
```
 1) overflow를 따로 설정해주지 않을 경우, visible이 기본값으로 들어간다.
 2) 내용이 잘리지 않는다.
 3) 스크롤 바가 생기지 않는다.
 


### 2. scroll
```
	.parent{
		overflow: scroll;
	}
```
 1) 내부 요소의 크기가 외부 요소보다 큰 부분은 보여지지 않는다.
 2) 스크롤 바가 무조건 생긴다.


### 3. hidden
```
	.parent{
		overflow: hidden;
	}
```
1) 내부 요소의 크기가 외부 요소보다 큰 부분은 보여지지 않는다.
2) 스크롤 바가 없다.

### 4. auto
```
	.parent{
		overflow: auto;
	}
```
 1) 내부 요소의 크기가 외부 요소의 크기보다 큰 부분이 없는 경우,
	 스크롤 바가 생기지 않는다.
 2) 내부 요소의 크기가 외부 요소의 크기보다 큰 부분이 있는 경우,
	 스크롤 바가 생긴다.
 2-1.외부 요소를 중심으로 스크롤 바가 생긴다.
 - scoll과의 차이점: scoll은 항상 스크롤 바를 보여주지만, auto는 내부 요소가 외부 요소를 넘어갈 때만 스크롤 바가 생긴다.
 
### 5. hidden 과 auto 활용
```
	<div class="parent">
		<div class="child-hidden">
			<h1> 하이 </h1>
				 :
			<h1> 하이 </h1>
		</div>
		<div class="child-auto">
			<h1> 안녕 </h1>
				 :
			<h1> 안녕 </h1>
		</div>

	</div>

```
```
<style>
	.parent{
		display: flex;
		width: 200px;
		height: 300px;
		background: #ccc;
	}

	.child-auto{
		border: 1px solid white;
		flex: 1;
		overflow: auto;
		/* '안녕'은 auto로 설정 */
		font-size: 12px;
	}

	.child-hidden{
		border: 1px solid white;
		flex: 1;
		overflow: hidden;
		/* '하이'는 hiddend으로 설정 */
		font-size: 18px;
	}
</style>
```
왼쪽 '하이'가 적힌 부분은 **hidden**을 적용하였다.
오른쪽 '안녕'이 적힌 부분은 **auto**을 적용하였다.
따라서, '하이'의 잘린 부분은 보여지지 않았고,
'안녕'은 스크롤 바를 이용하여 잘린 부분을 볼 수 있었다.


<a href="https://imgbb.com/"><img src="https://image.ibb.co/mSpqVy/shot1.png" alt="shot1" border="0"></a>



