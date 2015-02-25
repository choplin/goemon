# goemon

![](https://raw.githubusercontent.com/mattn/goemon/master/data/goemon.png)

*Go* *E*xtensible *Mon*itoring

[Wikipedia](http://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License) CC AA3.0 

Speed up your development.
If you updated js files, the page should be reloaded. If you updated go files, the app should be recompiled, and should be restarted. And also, the page should be reloaded

## Expected directory structure

```
+---assets
|   +- index.html
|   +- app.css
|   +- app.js
+- main.go
```

## Usage

```
$ goemon -g > goemon.yml
$ goemon go run main.go
```

## Default configuration

```yaml
# Generated by goemon -g
livereload: :35730
tasks:
- match: './assets/*.js'
  commands:
  - minifyjs -m -i ${GOEMON_TARGET_FILE} > ${GOEMON_TARGET_DIR}/${GOEMON_TARGET_NAME}.min.js
  - :livereload /
- match: './assets/*.css'
  commands:
  - :livereload /
- match: './assets/*.html'
  commands:
  - :livereload /
- match: '*.go'
  commands:
  - go build
  - :restart
  - :livereload /
```

* `match` is wildcard. You can use `./foo/bar/**/*.js` like a shell.
* `commands` is list of commands to run. `:XXX` is internal command.

| internal command  |           behavior          |
|-------------------|-----------------------------|
| :livereload /path | reload `path`               |
| :jsmin            | minify-js(work in progress) |
| :restart          | restart app                 |

Currently, `:jsmin` is workin progress. So you should `mimifyjs` command to do it.
For example, configuration in above works as below.

|     pattern      |             behavior            |
|------------------|---------------------------------|
| ./assets/\*.css  | reload page                     |
| ./assets/\*.js   | minify js, reload page          |
| ./assets/\*.html | reload page                     |
| ./assets/\*.go   | build, restart app, reload page |

## LiveReload

You can use livereload feature.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <script src="http://localhost:35730/livereload.js"></script>
  <script src="/app.css"></script>
  <script src="/app.min.js"></script>
  <title>Your App</title>
</head>
<body>
<!-- Your Code -->  
</body>
</html>
```

## Use goemon as library

```
cat > goemon.go
package main

import (
	_ "github.com/mattn/goemon/auto"
)
^D
```

Then `go build`. You don't need to use `goemon` command.


## Installation

```
$ go get github.com/mattn/goemon/cmd/goemon
```
If you want to minify js, install minifyjs live below.

```
$ npm install -g minifyjs
```

## License

MIT

## Author

Yasuhiro Matsumoto (a.k.a mattn)
