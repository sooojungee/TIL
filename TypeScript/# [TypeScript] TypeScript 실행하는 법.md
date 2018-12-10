# [TypeScript] TypeScript 실행하는 법

기존에 ts파일을 실행하기 위해서는 ts파일을 js 파일로 변환한 후에 js파일을 실행해야 한다.

< hello.ts 파일을 실행시킬 때>
```
tsc hello.ts
node hello.js
```

## ts-node

ts-node 명령어를 이용하면 한 번의 입력으로 실행 결과를 확인할 수 있다.
https://www.npmjs.com/package/ts-node

- 설치하기
```
npm install ts-node
```

< ts-node로 hello.ts 파일 실행하기 >
```
ts-node hello.ts
```

ts-node는 hello.ts을 읽어들여 내부적으로 컴파일하고 실행 결과만 출력한다.
ts-node는 js파일을 생성하지 않아 파일 관리에 도움을 준다.