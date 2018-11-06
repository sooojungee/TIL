# [css/sass] 스크롤바 안보이게 하기

스크롤바 안보이게 하는 방법

1. Chrome - webkit
```sass
&::-webkit-scrollbar  
  display: none
```

2. Internet Explorer - ms
```sass
-ms-overflow-style: none
```

3.  Mozilla Firefox - moz

	firefox에는 해당 기능이 없다.