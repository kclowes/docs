# wercker-docs

[![wercker status][wercker-image]][wercker-url]
[![semistandard-style][semistandard-image]][semistandard-url]

Wercker docs built with [gulp][gulp] and [metalsmith][metalsmith].

## Install
```sh
$ hub clone wercker/docs
```
Make sure to have [`npm>=2.0.0`][npm] installed to build local modules.

## Usage
```txt
Lifecycle scripts included in dev:
  postinstall
    echo 'linklocal' && linklocal link -r && linklocal list -r | bulk -c 'npm install'
  start
    npm run build && npm run open
  test
    semistandard

available via `npm run-script`:
  build
    gulp build
  clean
    rm -rf {cache,release,coverage}
  content
    gulp content
  open
    open http://localhost:1337 && httpster -d build -p 1337
  watch
    watchify index.js -t brfs -o build/bundle.js
  format
    semistandard-format -w
```

## License
[MIT](https://tldrlegal.com/license/mit-license)

[gulp]: http://gulpjs.com
[metalsmith]: http://www.metalsmith.io/
[npm]: http://npmjs.com

[wercker-image]: https://app.wercker.com/status/05eb642a41844e42b392d2db39bb7552/s "wercker status"
[wercker-url]: https://img.shields.io/wercker/ci/wercker/docs.svg
[semistandard-image]: https://img.shields.io/badge/code%20style-semistandard-brightgreen.svg?style=flat-square
[semistandard-url]: https://github.com/Flet/semistandard
