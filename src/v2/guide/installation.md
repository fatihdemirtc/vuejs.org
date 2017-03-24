---
title: Kurulum
type: guide
order: 1
vue_version: 2.2.0
dev_size: "234.23"
min_size: "74.22"
gz_size: "26.87"
ro_gz_size: "18.71"
---

### Uyumluluk Notu

Vue IE8 ve altına destek vermemektedir çünkü ECMAScript 5 özelliklerinden faydalanır ve bu özellikler IE8'de desteklenmemektedir. Ancak Vue diğer [ECMAScript 5 destekleyen tüm tarayıcıları](http://caniuse.com/#feat=es5) desteklemektedir.

### Sürüm Notları

Detaylandırılmış olan tüm sürüm notları [GitHub](https://github.com/vuejs/vue/releases)'da mevcuttur.

## Doğrudan `<script>` Dahil etmek

Basitçe indirin ve bir script tag'i ile dahil edin. `Vue` global bir değişken olarak kayıt edilecektir.

<p class="tip">Geliştirme ortamında sıkıştırılmış versiyonu kullanmayın. Aksi halde sık yapılan hatalar için güzel uyarıları kaçıracaksınız.</p>

<div id="downloads">
<a class="button" href="/js/vue.js" download>Geliştirme Ortamı Versiyonu</a><span class="light info">(Tüm uyarılar ve hata ayıklama modu ile birlikte)</span>

<a class="button" href="/js/vue.min.js" download>`Production` Versiyonu</a><span class="light info">Uyarılar olmadan, {{gz_size}}kb min+gzip</span>
</div>

### CDN

Tavsiye Edilen: [https://unpkg.com/vue](https://unpkg.com/vue), en son sürüm npm üzerinde yayınlandığında güncellenecektir. Ayrıca kaynak dosyasını [https://unpkg.com/vue/](https://unpkg.com/vue/) üzerinden görebilirsiniz.

Ayrıca [jsDelivr](//cdn.jsdelivr.net/vue/latest/vue.js) ve [cdnjs](//cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.js) adreslerinde mevcut, ancak bu iki hizmetin senkronize edilmesi biraz zaman alabilir, bu nedenle en yeni sürüm henüz uygun olmayabilir.

## NPM

Vue ile büyük ölçekli uygulamalar inşa ederken NPM önerilen kurulum metodudur. [Webpack](https://webpack.js.org/) veya [Browserify](http://browserify.org/) gibi modül paketleyicileriyle birlikte iyi çalışır.

``` bash
# son stabil versiyon
$ npm install vue --save
```

## CLI (Komut Satırı)

Vue.js hızlıca SPA yaratmanız için resmi olarak bir [CLI](https://github.com/vuejs/vue-cli) aracı sağlıyor. hot-reload, lint-on-save ve production-ready build'ler alıp ilerlemeniz yalnızca bir kaç dakika sürer;

``` bash
# vue-cli'yi kur
$ npm install --global vue-cli
# "webpack"'i kullanarak yeni bir proje oluştur
$ vue init webpack my-project
# bağımlılıkları kur ve ilerle!
$ cd my-project
$ npm install
$ npm run dev
```

<p class="tip">CLI ile kurulum yaparken, Node.js ve ilgili geliştirme araçları hakkında önceden bilgi sahibi olduğunuz varsayılmaktadır. Vue veya frontend araçlarını kullanmaya yeni başlıyorsanız, CLI'yi kullanmadan önce <a href="./">kılavuzla</a> birlikte oluşturma araçları olmadan kurulumu öğrenmenizi öneririz.</p>

## Farklı Derlemelerin Açıklanması

[NPM paketinin içerisinde ki `dist/` klasöründe](https://unpkg.com/vue@latest/dist/) Vue.js'in farklı build'lerini bulabilirsin. Aşağıda aralarında ki farkı gösteren bi tablo var:

| | UMD | CommonJS | ES Module |
| --- | --- | --- | --- |
| **Tam** | vue.js | vue.common.js | vue.esm.js |
| **Sadece-Runtime** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js |
| **Tam (production)** | vue.min.js | - | - |
| **Sadece-Runtime (production)** | vue.runtime.min.js | - | - |

### Terimler

- **Tam**: Hem derleyiciyi hem de runtime'ı içeren derleme.

- **Derleyici**: Template strings'leri Javascript render fonksiyonu içerisine derlemekle sorumlu kod.

- **Runtime**: Vue instance'ları oluşturmakla, Virtual DOM oluşturma ve patch'leme ile sorumlu kod bloğu. 

- **[UMD](https://github.com/umdjs/umd)**: UMD build'i direkt olarak tarayıcıda bir `<script>` tagi ile kullanılabilir. [https://unpkg.com/vue](https://unpkg.com/vue) CDN'inde bulunan (`vue.js`) Runtime ve Derleyiciyi içerir.

- **[CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1)**: CommonJS build'leri [browserify](http://browserify.org/) veya [webpack 1](https://webpack.github.io) gibi daha eski paketleyiciler ile kullanılmak üzere tasarlanmıştır.

- **[ES Module](http://exploringjs.com/es6/ch_modules.html)**: ES module buid'leri [webpack 2](https://webpack.js.org) veya [rollup](http://rollupjs.org/) gibi daha modern paketleyiciler ile kullanılmak üzere tasarlanmıştır.

### Runtime + Compiler vs. Runtime-only

If you need to compile templates on the fly (e.g. passing a string to the `template` option, or mounting to an element using its in-DOM HTML as the template), you will need the compiler and thus the full build:

``` js
// this requires the compiler
new Vue({
  template: `<div>{{ hi }}</div>`
})

// this does not
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

When using `vue-loader` or `vueify`, templates inside `*.vue` files are pre-compiled into JavaScript at build time. You don't really need the compiler in the final bundle, and can therefore use the runtime-only build.

Since the runtime-only builds are roughly 30% lighter-weight than their full-build counterparts, you should use it whenever you can. If you still wish to use the full build instead, you need to configure an alias in your bundler:

#### Webpack

``` js
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' for webpack 1
    }
  }
}
```

#### Rollup

``` js
const alias = require('rollup-plugin-alias')

rollup({
  // ...
  plugins: [
    alias({
      'vue': 'vue/dist/vue.esm.js'
    })
  ]
})
```

#### Browserify

Add to your project's `package.json`:

``` js
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```

### Development vs. Production Mode

Development/production modes are hard-coded for the UMD builds: the un-minified files are for development, and the minified files are for production.

CommonJS and ES Module builds are intended for bundlers, therefore we don't provide minified versions for them. You will be responsible for minifying the final bundle yourself.

CommonJS and ES Module builds also preserve raw checks for `process.env.NODE_ENV` to determine the mode they should run in. You should use appropriate bundler configurations to replace these environment variables in order to control which mode Vue will run in. Replacing `process.env.NODE_ENV` with string literals also allows minifiers like UglifyJS to completely drop the development-only code blocks, reducing final file size.

#### Webpack

Use Webpack's [DefinePlugin](https://webpack.js.org/plugins/define-plugin/):

``` js
var webpack = require('webpack')

module.exports = {
  // ...
  plugins: [
    // ...
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify('production')
      }
    })
  ]
}
```

#### Rollup

Use [rollup-plugin-replace](https://github.com/rollup/rollup-plugin-replace):

``` js
const replace = require('rollup-plugin-replace')

rollup({
  // ...
  plugins: [
    replace({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
}).then(...)
```

#### Browserify

Apply a global [envify](https://github.com/hughsk/envify) transform to your bundle.

``` bash
NODE_ENV=production browserify -g envify -e main.js | uglifyjs -c -m > build.js
```

Also see [Production Deployment Tips](deployment.html).

### CSP environments

Some environments, such as Google Chrome Apps, enforce Content Security Policy (CSP), which prohibits the use of `new Function()` for evaluating expressions. The standalone build depends on this feature to compile templates, so is unusable in these environments.

On the other hand, the runtime-only build is fully CSP-compliant. When using the runtime-only build with [Webpack + vue-loader](https://github.com/vuejs-templates/webpack-simple) or [Browserify + vueify](https://github.com/vuejs-templates/browserify-simple), your templates will be precompiled into `render` functions which work perfectly in CSP environments.

## Dev Build

**Important**: the built files in GitHub's `/dist` folder are only checked-in during releases. To use Vue from the latest source code on GitHub, you will have to build it yourself!

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

Only UMD builds are available from Bower.

``` bash
# latest stable
$ bower install vue
```

## AMD Module Loaders

All UMD builds can be used directly as an AMD module.
