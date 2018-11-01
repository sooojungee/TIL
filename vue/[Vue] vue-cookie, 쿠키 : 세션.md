# [Vue] vue-cookie, 쿠키 / 세션

## [npm] vue-cookie
https://www.npmjs.com/package/vue-cookie

### 설치 방법
```
npm install vue-cookie --save
```
```js
const VueCookie = require('vue-cookie');
// 플러그인 사용
Vue.user(VueCookie);
```

### 사용 방법
플러그 인은 컴포넌트의 `this.$cookie` 또는 `Vue.cookie`을 통해 사용할 수 있다.

```js
// 'test'라는 이름과 'hello world'라는 값을 가지고 있고 하루 후에 만료되는 쿠키를 만든다.
this.$cookie.set('test', 'hello world', 1);

// 'test'의 값을 가져온다.
this.$cookie.get('test');

// 'test'라는 이름을 가진 쿠키를 지운다.
this.$cookie.delete('test');
```

## 쿠키 (Cookie)
- 쿠키는 클라이언트 로컬에 저장되는 키와 값이 들어있는 작은 데이터 파일이다.
- 쿠키에는 이름, 값, 만료날짜, 경로 정보가 들어있다.
- 쿠키는 일정 시간동안 데이터를 저장할 수 있다.
- 쿠키는 클라이언트에 상태 정보를 로컬에 저장했다가 참조한다.
- 로그인 상태를 유지하거나 사용자 정보를 일정 시간동안 유지해야하는 경우에 주로 사용한다.

쿠키는 사용자가 따로 요청하지 않아도 브라우저가 Request 시에 Request Header를 넣어서 자동으로 서버에 전송한다.

### 목적

- 세션 관리 (Session management)
	로그인, 게임 점수 등을 서버가 기억한다.
- 개인화 (Personalization)
	사용자 기본 설정, 테마 및 기타 설정
- 트래킹 (Tracking)
	사용자 동작 기록 및 분석

쿠키는 모든 요청과 함께 전송되기 때문에 클라이언트 측 스토리지로 사용하는데에는 성능상의 부담이 될 수 있다.

### 쿠키 보는 법
브라우저 띄움 -> 검사 -> Application -> Cookies

## 세션 (Session)

- 세션은 서버 메모리에 저장되는 정보다.
- 웹 크라우저를 통해 웹 서버에 접속한 이후로 브라우저를 종료할 때 까지 유지되는 상태이다.
- 클라이언트가 Request를 보내면 해당 서버의 엔진이 클라이언트에게 유일한 ID를 부여하는것이 세션 ID다.
- 서버에 저장되기 때문에 쿠키와 달리 사용자 정보가 노출되지 않는다.

### 로그인 처리 과정

1. 사용자가 로그인 페이지에 ID / PW 를 입력하고 로그인 버튼 클릭
2. 서버에서 사용자가 보낸 ID / PW 정보를 확인하고 존재하는 사용자면 서버 메모리에 유일한 세션 ID를 생성하고 사용자 ID와 매핑 정보를 저장
3. 클라이언트에 세션 ID를 쿠키로 저장
4. 요청할 떄마다 서버는 Request Header의 쿠키 정보를 확인하고 세션 ID와 매핑되는 id의 사용자로 인식