# github page에 spa배포 (vue.js)

https://github.com/sujinleeme/spa-github-pages-ko

깃허브페이지는 SPA를 지원하지 않는다.
URL이 `github.io/foo`라면 깃헙페이지는 `/foo`를 몰라서 404에러를 반환함.


## 해결방법

### 1.  `404.html`파일에 스크립트를 만든다.
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Single Page Apps for GitHub Pages</title>
		<script type="text/javascript">
// Single Page Apps for GitHub Pages
// https://github.com/rafrex/spa-github-pages
// Copyright (c) 2016 Rafael Pedicini, licensed under the MIT License
// ----------------------------------------------------------------------
// This script takes the current url and converts the path and query
// string into just a query string, and then redirects the browser
// to the new url with only a query string and hash fragment,
// e.g. http://www.foo.tld/one/two?a=b&c=d#qwe, becomes
// http://www.foo.tld/?p=/one/two&q=a=b~and~c=d#qwe
// Note: this 404.html file must be at least 512 bytes for it to work
// with Internet Explorer (it is currently > 512 bytes)
// If you're creating a Project Pages site and NOT using a custom domain,
// then set segmentCount to 1 (enterprise users may need to set it to > 1).
// This way the code will only replace the route part of the path, and not
// the real directory in which the app resides, for example:
// https://username.github.io/repo-name/one/two?a=b&c=d#qwe becomes
// https://username.github.io/repo-name/?p=/one/two&q=a=b~and~c=d#qwe
// Otherwise, leave segmentCount as 0.
			var segmentCount = 1;

			var l = window.location;

			l.replace(

				l.protocol + '//' + l.hostname + (l.port ? ':' + l.port : '') +

				l.pathname.split('/').slice(0, 1 + segmentCount).join('/') + '/?p=/' +

				l.pathname.slice(1).split('/').slice(segmentCount).join('/').replace(/&/g, '~and~') +

				(l.search ? '&q=' + l.search.slice(1).replace(/&/g, '~and~') : '') +

				l.hash

			);
			</script>
		</head>
	<body>
	</body>
</html>
```
커스텀 도메인을 사용하면 `segmentCount = 0`, 페이지로 만들 경우, `segmentCount = 1`


### 2. 스크립트 추가
App.ts 파일에 스크립트 추가
```ts
(function(l) {
	if (l.search) {
		var q = {};
		l.search.slice(1).split('&').forEach(function(v) {
			var a = v.split('=');
			q[a[0]] = a.slice(1).join('=').replace(/~and~/g, '&');
		});
		if (q.p !== undefined) {
			window.history.replaceState(null, null,
			l.pathname.slice(0, -1) + (q.p || '') +
			(q.q ? ('?' + q.q) : '') +
			l.hash
			);
		}
	}
}(window.location))
```

이거하고 git add , git commit, git push