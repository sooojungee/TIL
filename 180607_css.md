# [HTML/CSS] table을 이용하여 그림 가운데 정렬하기

```
<div class = "content">
	<div class = "content-table">
		<div class = "content-table-cell">
			<div class = "cell-box">
		</div>
	</div>
</div>
```

4개의 클래스가 필요하다.


### 1. content
- 전체 범위를 설정해 준다.
```
.content {  
  width: 100vw  
  height: 100vh
}
```
### 2. content-table
- **display: table**을 사용한다.
```
.content-table {
	/* 현재 width의 값은 정확하지 않아도 된다 */
	width: 200px  
	height: 100%  
	display: table  
	margin: 0 auto
}
```

### 3.content-table-cell
- **display: table-cell**을 사용한다.
- **vertical-align: middle**을 사용한다.
```
.content-table-cell {
	display: table-cell

	/* table의 위치가 전체 화면의 중앙으로 배치된다. */    
	vertical-align: middle 
	width: 200px  
	height: 100%
}
```
**display: table의 하위요소들은 무조건 table 전체를 채워야 한다.**
위의 예시 같은 경우에 content-table의 하위 요소가 하나뿐임으로  height는 어떤 값을 쓰던지 100%로 들어간다.

### 4. cell-box
```
.cell-box {
	width: 200px  
	height: 200px  
	background: gold  
	margin: 0 auto
}
```
원하는 컨텐츠의 크기와 세부사항을 지정해준다.


**<완성본>**

<a href="https://ibb.co/jeU5ho"><img src="https://preview.ibb.co/i9cEa8/2018_06_07_1_42_58.png" alt="2018_06_07_1_42_58" border="0"></a>
