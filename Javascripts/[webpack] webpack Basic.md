# [webpack] webpack Basic

## 1. webpack이란?

자바스크립트에서 응용 프로그램을 위한 정적 모듈 번들(module bundler)이다.
> **번들(bundler)**
> 필요한 의존성에 대해 정확하게 추적하여 해당하는 의존성들을 그룹핑 해주는 도구이다.

- 모듈 번들은 각각의 모듈들에 대해 의존성 관계를 파악하여 그룹핑해주는 것

서버에서 처리하는 로직을 JavaScript로 구현하는 부분이 많아지면서 웹 서비스 개발에서 코드의 양이 많아진다. 그러면 코드의 유지보수가 쉽도록 코드를 모듈로 나누어 관리하는 모듈 시스템이 필요하다.

webpack으로 모듈 시스템을 구성할 수 있다.

### 모듈 정의와 모듈 사용

모듈을 작성하고 다음과 같이 module.exports 속성에 외부에 배포할 모듈을 전달해 모듈을 정의한다.
```js
module.exports = {message: 'webpack'};
```

모듈로 만든 파일은 바로 웹 페이지에 넣어 브라우저에서 실행할 수 없다.
webpack으로 컴파일해 브라우저에서 실행할 수 있는 형태로 바꿔야 한다.

## 2. 핵심 개념

webpack은 기본적으로 `webpack.config.js`파일에서 설정할 수 있다.

### Entry

Entry point는 내부 종속성 그래프 구축을 시작하기 위해 어떤 모듈 웹팩을 사용해야 하는지 나타낸다.
웹팩은 Entry point가 종속된 다른 모듈과 라이브러리를 확인한다.

기본적으로 값은 `./src/index.js`이지만 웹팩 구성에서 항목 속성을 구성하여 다른 엔트리 포인트를 지정할 수 있다.
```js
module.exports = {
	entry: './path/to/my/entry/file.js'
}
```
- entry 프로퍼티는 bundling하고 싶은 파일들을 선언해주면 된다.

Entry 웹팩 구성에서 속성을 정의하는 여러가지 방법이 있다.
1. **Single Entry [Shorthand] Syntax**
	 용법 : `entry: string | Array<string>`
	 <webpack.config.js>
	 ```js
	 module.exports = {
		 entry: {
			 main: './path/to/my/entry/file.js'
		 }
	 }
	 ```
	 > 배열을 전달하면 여러 종속 파일을 함께 주입하고 종속성을 하나의 'chunk'로 그려보고 싶을 때 좋음
	 
	 하나의 엔트리 포인트가 있는 응용프로그램 또는 도구의 웹팩 구성을 신속하게 설정하려는 경우에 가장 적합하다. 하지만 이 구문으로 구성을 확장하거데 유연성이 없다.
	 
2. **Object Syntax**
	용법: `entry: {[entryChunkName: string]: string|Array<string>}`
	<webpack.config.js>
	```js
	module.exports= {
		entry: {
			app: './src/app.js'
			adminApp: './src/adminApp.js'
		}
	}
	```
	응용 프로그램에서 entry/entrys를 정의하는 확장 가능한 방법이다.
	
### Output

Output 속성은 웹팩에 생성된 번들을 어디에 방출해야 하는지, 그리고 이러한 파일의 이름을 지정하는 방법을 알려준다.

여러 entry point가 있을 수 있지만 하나의 output 구성만 지정된다.
webpack이 컴파일된 파일을 디스크에 기록하는 방법을 알 수 있다.

기본적으로 출력 파일의 경우 `./dist/main.js`이고 생성된 다른 파일의 경우 `./dist` 폴더로 설정된다.

```js
const path = require('path');

module.exports = {
	entry: './path/to/my/entry/file.js',
	output: {
		path: path.resolve(__dirname, 'dist'),
		filename: 'bundle.js'
	}
}
```

- output 프로퍼티는 bundling되고 만들어진 파일에 대한 정보를 명시해 주면 된다.

1. **Multiple Entry Points**
```js
module.exports = {
	entry: {
		app: './src/app.js',
		search: './src/search.js'
	},
	output: {
		filename: '[name].js',
		path: __dirname + '/dist'
	}
}

// ./dist/app.js , ./dist/search.js
```
여러개의 entry point가 있거나 CommonsChunkPlugin과 같은 플러그인을 사용할 때와 같이 하나 이상의 "Chunk"가 생성되는 경우 각 파일이 고유한 이름을 갖도록 해야한다.

### loader

다양한 리소스를 JavaScript에서 바로 사용할 수 있는 형태로 로딩하는 기능이다.

로더의 종류에 따라 JavaScript에서 사용할 수 있는 다양한 형태의 결과를 얻을 수 있다.
<예시>
```
json-loader (데이터) -> 데이터 객체
handlebars-loader (템플릿) -> 템플릿 함수
coffescript (개발 언어) -> JavaScript
```



웹팩은 JavaScript와 JSON파일만 이해한다.
loader는 웹팩이 다른 유형의 파일을 처리하고 응용프로그램에서 사용하고 종속성 그래프에 추가할 수 있는 유효한 모듈로 변환할 수 있게 한다.
이렇게 하기 위해선 필요한 로더를 설치해야 한다.

```js
module.exports = {
	modules: {
		rules: [
			{ test: /\.txt$/, use: 'raw-loader'}
		]
	}
}
// test프로퍼티는 파일을 식별한다.
// use프로퍼티는 변환을 수행하는데 사용해야 하는 로더를 나타낸다.
// .txt 파일로 확인되는 경로를 발견하면 번들에 추가하기 전에 `raw-loader`를 이용해서 변경해라.
```

응용프로그램에서 로드를 사용하는 방법
1. Configuration (recommended): webpack.config.js 파일에 지정하기
	`module.rules` 웹팩 구성 내에 여러 로더를 지정할 수 있다.
	```js
	module.exports = {
		module: {
			rules: [
				test: /\.css$/,
				use: [
					{ loader: 'style-loader'},
					{
						loader: 'css-loader',
						options: {
							modules: true
						}
					},
					{ loader: 'sass-loader'}
				]
			]
		}
	}
	// sass-loader -> css-loader -> style-loader 순으로
	```
	로더는 오른쪽에서 왼쪽으로 실행된다. 체인은 역순으로 실행된다.
2. Inline: 각 `import` 문에서 명시적으로 지정하기
	`import` 명령문에 loader을 지정하거나 "importing" 메소드를 지정할 수 있다.
	`!`를 사용하여 리소스로부터 로더를 분리한다.
	```js
	import Styles from 'style-loader!css-loader?modules!./styles.css';
	```
	전체 규칙 앞에 접두어 `!`를 붙여서 모든 로더를 무시할 수 있다.
	옵션은 쿼리 매개 변수를 사용하여 전달할 수 있다.
3. CLI: shell 명령 내에서 지정하기
	```
	webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
	```
	이렇게 하면 `.jade` 파일용 `jade-loader`와 `.css` 파일용 `style-loader` 및 `css-loader`가 사용된다.

**로더 기능**
1. 로더는 연결될 수 있다. 체인은 역순으로 실행된다. 
2. 로더는 동기식 또는 비동기식일 수 있다.
3. 로더는 Node.js에서 실행된다.
4. 로더는 `options` 오브젝트로 구성될 수 있다.
5. 플러그인은 로더에게 더 많은 기능을 제공할 수 있다.
6. 로더는 임의의 추가 파일을 export 할 수 있다.  

### plugin

로더는 특정 유형의 모듈을 변환하는데 사용되지만 플러그인을 사용하면 번들 최적화와 같은 광범위한 작업을 수행할 수 있다.

플러그인을 사용하려면 플러그인에 `require`을 추가해야한다. 

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const webpack = require('webpack')

module.exports = {
	modules: {
		rules: [
			{ test: /\.test$/, use: 'raw-loader'}
		]
	},
	plugins: [
		new HtmlWebpackPlugin({template: './src/index.html'})
	]
}
``` 
`html-webpack-plugin`은 생성된 모든 번들을 자동으로 주입하여 애플리케이션에 사용할 html 파일을 생성한다.


webpack 플러그인은 `apply` 메소드를 가진 JavaScript 객체이다. 
이 `apply` 메소드는 webpack 컴파일러에서 호출하여 전체 컴파일 수명주기에 액세스 할 수 있다.

```js
const pluginName = 'consoleLogOnBuildWebpackPlugin'

class COnsoleLogOnBuildWebpackPlugin {
	apply(compiler) {
		compiler.hooks.run.tap(pluginName, compliation => {
			console.log('The webpack build process is srarting!!')
		})
	}
}
```
컴파일러 훅의 `tap` 메소드의 첫번째 매개변수는 camelized 버전의 플러그인 이름이어야한다.

플러그인은 arguments/options 사항을 취할 수 있으므로 웹팩 구성의 플러그인 속성에 새 인스턴스를 전달해야 한다.

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const webpack = require('webpack')
const path = require('path')

module.exports = {
	entry: './path/to/my/entry/file.js',
	output: {
		filename: 'my-first-webpack.bundle.js',
		path: path.resolve(__dirname, 'dist')
	},
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				user: 'babel-loader'
			}
		]
	},
	plugins: [
		new webpack.ProgressPlugin(),
		new HtmlWebpackPlugin({ template: './src/index.html' })
	]
}
```