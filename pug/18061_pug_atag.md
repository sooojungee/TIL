# [PUG] a 태그 사용법

### 'a태그'란
하이퍼링크를 걸어주는 태그이다.



### a태그 사용법
pug에서 a 태그사용법은 다음과 같다.

```
a(href="www.naver.com")네이버로 가기
```
input 의 속성을 추가할 경우, 다음과 같이 적어준다.
```
a(href="www.naver.com", title="naver") 네이버로 가기

```
### 클래스를 눌렀을 때 a 태그 적용 
 특정 클래스를 클릭했을 때, a태그를 적용시키는 방법은 다음과 같다.
```
a.main(href="www.naver.com", title="naver") 네이버로 가기
```
이때 main이라는 클래스를 클릭하면 www.naver.com 으로 이동한다.
