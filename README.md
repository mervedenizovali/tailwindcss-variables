![Tests](https://img.shields.io/github/workflow/status/mertasan/tailwindcss-variables/tests?label=tests)
[![Build Status](https://img.shields.io/travis/mertasan/tailwindcss-variables/master.svg?label=travis-ci)](https://travis-ci.com/mertasan/tailwindcss-variables)
[![Examples](https://img.shields.io/github/workflow/status/mertasan/tailwindcss-variables/build?label=examples)](https://github.com/mertasan/tailwindcss-variables/tree/master/examples)
![Dependencies](https://status.david-dm.org/gh/mertasan/tailwindcss-variables.svg)
[![license](https://img.shields.io/badge/License-GPLv3-green.svg?label=license)](//github.com/mertasan/tailwindcss-variables/tree/master/LICENSE)

# tailwindcss CSS variables

Bu eklenti, tailwindcss için CSS değişkenlerini `tailwind.config.js` dosyası aracılığıyla kolayca yapılandırmaya
olanak sağlar.

Kullanım şekli, tailwindcss'in varsayılan yapılandırmaları ile benzer şekildedir. Dark Mode için ayrı değişken
grubu belirleyebilmek, plugin API aracılığıyla kendi paketlerinize kolayca entegre edebilmek de mümkün.

## Öne çıkan özellikler

- Değişkenler, tailwindcss renklerini tanımlamak kadar basit.
- Değişkenleri `:root` ya da custom CSS seçicilere tanımlayabilirsiniz.
- Değişkenler iç içe geçmiş obje notasyonu (nested object notation) kullanılarak oluşturulabilir.
- Dark Mode için farklı değişkenler oluşturulabilir.
- Dark Mode değişkenleri, yapılandırmanızdaki `class` ya da `media` moduna göre otomatik tanımlanır.
- Yapılandırmadaki `darkMode` ayarı eğer `class` olarak belirtilmiş ise özel seçiciler tanımlanabilir.
- Plugin API aracılığıyla kendi eklentinizi oluştururken tema yapılandırması yapmanıza olanak sağlar.
- Değişkenler için prefix tanımlaması yapılabilir. (plugin API için faydalı)
- Değişkenler, yapılandırma dosyasında veya .css vb. stil dosyalarında kullanılabilir.
- Çoklu tema gibi gereksinimlerinizi ek bir eklentiye gerek kalmadan kendiniz yapılandırabilirsiniz!

## Dökümantasyonlar

| Dil | Dökümantasyon linki |
| --- | --- |
| English | [**Documentation**](./README.md) |
| Türkçe | Dokümantasyon |

## Kurulum

```cli
npm install -D @mertasan/tailwindcss-variables
```

## Basit Kullanım

```javascript
// tailwind.config.js

module.exports = {
  theme: {
    colors: {
        red: {
            50: 'var(--colors-red-50)'
        }
    }
    variables: {
      DEFAULT: {
        sizes: {
          small: '1rem',
          button: {
            size: '2rem'
          }
        },
        colors: {
          red: {
            50: '#ff3232',
          },
        },
      },
      '.container': {
        sizes: {
          medium: '1.5rem',
        },
      },
    },
  },
  plugins: [
    require('@mertasan/tailwindcss-variables')
  ]
}
```

**Output:**

```css
:root {
  --sizes-small: 1rem;
  --sizes-button-size: 2rem;
  --colors-red-50: #ff3232
}

.container {
  --sizes-medium: 1.5rem
}
```

## Dark Mode

### `class` modu ile

```javascript
// tailwind.config.js

module.exports = {

  darkMode: 'class',

  theme: {
    variables: {
      DEFAULT: {
        sizes: {
          small: '1rem',
        },
        colors: {
          red: {
            50: 'red',
          },
        },
      },
      '.container': {
        colors: {
          red: {
            50: 'indigo',
          },
        },
      },
    },
    darkVariables: {
      DEFAULT: {
        colors: {
          red: {
            50: 'blue',
          },
        },
      },
      '.container': {
        colors: {
          red: {
            50: 'green',
          },
        },
      },
    },
  },
  plugins: [
    require('@mertasan/tailwindcss-variables')
  ]
}
```

**Output:**

```css
:root {
  --sizes-small: 1rem;
  --colors-red-50: red
}

.container {
  --colors-red-50: indigo
}

:root.dark {
  --colors-red-50: blue
}
+
:root.dark .container {
  --colors-red-50: green
}
```

#### `darkToRoot` ve `darkSelector` ayarları ile

Eğer tailwindcss yapılandırmanızda `darkMode: 'class'` olarak tanımlıysa, eklentinin `darkToRoot` ve `darkSelector`
ayarlarını kullanarak özelleştirebilirsiniz.

| option       	| type   	| default 	| description                                                             	|
|--------------	|--------	|---------	|-------------------------------------------------------------------------	|
| darkSelector 	| string 	| .dark   	| Dark mode için kullanılan CSS seçici.                                   	|
| darkToRoot   	| bool   	| true    	| `darkSelector` ayarında tanımlanan seçici :root olarak mı kullanılıyor? 	|

```javascript
// tailwind.config.js

module.exports = {

  darkMode: 'class',

  theme: {
    variables: {
      DEFAULT: {
        sizes: {
          small: '1rem',
        },
        colors: {
          red: {
            50: 'red',
          },
        },
      },
      '.container': {
        colors: {
          red: {
            50: 'indigo',
          },
        },
      },
    },
    darkVariables: {
      DEFAULT: {
        colors: {
          red: {
            50: 'blue',
          },
        },
      },
      '.container': {
        colors: {
          red: {
            50: 'green',
          },
        },
      },
    },
  },
  plugins: [
    require('@mertasan/tailwindcss-variables')({
      darkToRoot: false,
      darkSelector: '.custom-dark-selector',
    })
  ]
}
```

**Output:**

```css
:root {
    --sizes-small: 1rem;
    --colors-red-50: red
}

.container {
    --colors-red-50: indigo
}

.custom-dark-selector {
    --colors-red-50: blue
}

.custom-dark-selector .container {
    --colors-red-50: green
}
```

### `media` modu ile

```javascript
// tailwind.config.js

module.exports = {

  darkMode: 'media',

  theme: {
    variables: {
      DEFAULT: {
        sizes: {
          small: '1rem',
        },
        colors: {
          red: {
            50: 'red',
          },
        },
      },
      '.container': {
        colors: {
          red: {
            50: 'indigo',
          },
        },
      },
    },
    darkVariables: {
      DEFAULT: {
        colors: {
          red: {
            50: 'blue',
          },
        },
      },
      '.container': {
        colors: {
          red: {
            50: 'green',
          },
        },
      },
    },
  },
  plugins: [
    require('@mertasan/tailwindcss-variables')
  ]
}
```

**Output:**

```css
:root {
  --sizes-small: 1rem;
  --colors-red-50: red
}

.container {
    --colors-red-50: indigo
}

@media (prefers-color-scheme: dark) {
  :root {
    --colors-red-50: blue
  }

  .container {
    --colors-red-50: green
  }
}
```

## Prefix Kullanımı

```javascript
// tailwind.config.js

module.exports = {
  theme: {
    variables: {
      DEFAULT: {
        sizes: {
          small: '1rem',
          button: {
            size: '2rem'
          }
        },
        colors: {
          red: {
            50: '#ff3232',
          },
        },
      },
      '.container': {
        sizes: {
          medium: '1.5rem',
        },
      },
    },
  },
  plugins: [
    require('@mertasan/tailwindcss-variables')({
      variablPrefix: '--admin'
    })
  ]
}
```

**Output:**

```css
:root {
  --admin-sizes-small: 1rem;
  --admin-sizes-button-size: 2rem;
  --admin-colors-red-50: #ff3232
}

.container {
    --admin-sizes-medium: 1.5rem
}
```

## İç içe geçmiş obje notasyonu (Nested)

```javascript
// tailwind.config.js

module.exports = {
  theme: {
    variables: {
      DEFAULT: {
        sizes: {
          small: '1rem',
          admin: {
            buttons: {
              colors: {
                red: {
                  500: '#ff0000',
                  600: '#e60000',
                }
              }
            }
          }
        },
      }
    },
  },
  plugins: [
    require('@mertasan/tailwindcss-variables')
  ]
}
```

**Output:**

```css
:root {
  --sizes-small: 1rem;
  --sizes-admin-buttons-colors-red-500: #ff0000;
  --sizes-admin-buttons-colors-red-600: #e60000
}
```


## Key adlandırma kuralları

Değişken keyleri yalnızca belirli karakterlere sahip olabilir. Diğer karakterler otomatik olarak temizlenir.
Objelerde alt tire (_) kullanımı mümkün olduğundan, alt tireler de orta çizgiye (-) dönüştürülür.

Rule:
````jsregexp
/[^a-zA-Z0-9\-]+/gi
````

| öncesi        | sonrası |
|--------------	|--------	|
| hello[$&+,:;=?@#'<>.-^*()%!]world   | hello-world                     	|
| hello__world                       	| hello-world   	                  |
| css_variables_for-tailwindcss   	  | css-variables-for-tailwindcss   	|

İşte bir örnek:

```javascript
// tailwind.config.js

module.exports = {
  theme: {
    variables: {
      DEFAULT: {
        colors: {
          'hello[$&+,:;=?@#|\'<>.-^*()%!]world': '100%',
          underscore_to_dash: '100%',
          'underscore_to_dash-with-dash': '100%',
          auto_dash: '100%',
        },
      },
      '[type=\'button\']': {
        'hello[$&+,:;=?@#|\'<>.-^*()%!]world': '100%',
        underscore_to_dash: '100%',
        'underscore_to_dash-with-dash': '100%',
        auto_dash: '100%',
        nested_auto_dash: {
          color_primary: '100%',
        },
      },
    },
  },
  plugins: [
    require('@mertasan/tailwindcss-variables')
  ]
}
```

**Output:**

```css
:root {
  --colors-hello-world: 100%;
  --colors-underscore-to-dash: 100%;
  --colors-underscore-to-dash-with-dash: 100%;
  --colors-auto-dash: 100%
}

[type='button'] {
  --hello-world: 100%;
  --underscore-to-dash: 100%;
  --underscore-to-dash-with-dash: 100%;
  --auto-dash: 100%;
  --nested-auto-dash-color-primary: 100%
}
```

## Kendi eklentileriniz için API örneği

- [Ayrıntılı açıklama](#gerçek-kullanım-örneği-detaylı)

```javascript
// tailwind.config.js
const plugin = require('tailwindcss/plugin')
const variablesApi = require('@mertasan/tailwindcss-variables/api')

let variableOptions = {
  variablePrefix: '--myplugin'
}

const pluginVariables = {
  DEFAULT: {
    colors: {
      primary: 'black',
      secondary: 'white',
      warning: 'orange',
    },
  },
}

const pluginDarkVariables = {
  DEFAULT: {
    colors: {
      primary: 'red',
      secondary: 'yellow',
      warning: 'green',
    },
  },
}

module.exports = {
  plugins: [
    plugin(function({ addComponents, config }) {

      addComponents(variablesApi.variables(pluginVariables, variableOptions))

      addComponents(variablesApi.darkVariables(pluginDarkVariables, variableOptions, config('darkMode'))) // darkMode: class

    })
  ]
}
```

**Output:**

```css
:root {
  --myplugin-colors-primary: black;
  --myplugin-colors-secondary: white;
  --myplugin-colors-warning: orange
}

:root.dark {
  --myplugin-colors-primary: red;
  --myplugin-colors-secondary: yellow;
  --myplugin-colors-warning: green
}
```

### API Component helper

tailwindcss-variables plugin API'yi bileşenlerinizi kayıt etmek için de kullanabilirsiniz.

```javascript
// tailwind.config.js
const plugin = require('tailwindcss/plugin')
const variablesApi = require('@mertasan/tailwindcss-variables/api')

let variableOptions = {
  variablePrefix: '--myplugin'
}

const pluginVariables = {
  DEFAULT: {
    colors: {
      primary: 'black',
      secondary: 'white',
      warning: 'orange',
    },
  },
}

const pluginDarkVariables = {
  DEFAULT: {
    colors: {
      primary: 'red',
      secondary: 'yellow',
      warning: 'green',
    },
  },
}

module.exports = {
  plugins: [
    plugin(function({ addComponents, config }) {
      const formComponents = {
        select: {
          DEFAULT: {
            backgroundColor: 'var(--myplugin-colors-primary)',
          },
          multi: {
            '&.default-multi': {
              backgroundColor: 'var(--myplugin-colors-secondary)',
            },
            '&.other-multi': {
              backgroundColor: 'var(--myplugin-colors-warning)',
            },
          },
        },
      }

      addComponents(variablesApi.variables(pluginVariables, variableOptions))

      addComponents(variablesApi.darkVariables(pluginDarkVariables, variableOptions, config('darkMode'))) // darkMode: class

      // Automatically register components via API.
      addComponents(variablesApi.getComponents('.form', formComponents))
    })
  ]
}
```

**Output:**
```css
:root {
  --myplugin-colors-primary: black;
  --myplugin-colors-secondary: white;
  --myplugin-colors-warning: orange;
}

:root.dark {
  --myplugin-colors-primary: red;
  --myplugin-colors-secondary: yellow;
  --myplugin-colors-warning: green;
}

.form-select {
    background-color: var(--myplugin-colors-primary);
}

.form-select.default-multi {
    background-color: var(--myplugin-colors-secondary);
}

.form-select.other-multi {
    background-color: var(--myplugin-colors-warning);
}
```

## Gerçek kullanım örneği (detaylı)

**Avantajları neler?**

Laravel için bir form oluşturucu (PHP) paketi oluşturduğunuzu hayal edin. Bu durumda özelleştirmeniz gereken
birçok stil olacağına eminim. Ancak en gerekli olan şeylerden bir tanesi renkler! Bileşenleri kendi belirlediğiniz
renklerle oluşturursunuz. Elbette bu renkler `vendor:publish` komutu ile kullanıcılar tarafından özelleştirilebilir ama
bunu herkes için daha basit hale getirebilirsiniz. Kullanıcılar renkleri kendileri yapılandırabilir, isterlerse
eklentinizi dark mode için yapılandırabilirler. Böylelikle, kullanıcılar bazı basit değişiklikler için
`.css` veya `.blade.php` dosyalarını özelleştirmek zorunda kalmazlar. Bu sayede eklentinizi güncel şablonlarıyla
birlikte kullanarak, gelecekteki versiyon güncellemelerine uyum sağlayabilirler. Bu açıklamayı okuduysanız eğer,
bu eklentininin ortaya çıkma nedenini artık biliyorsunuz demektir. :)


**Dezavantajları neler?**

Bir fikriniz varsa eğer, lütfen PR göndermekten çekinmeyin.

**Bu örnek ile ilgili kaynaklar:**

- [kaynak](https://github.com/mertasan/tailwindcss-variables/tree/master/examples/api-examples/readme-source)
- [test](https://github.com/mertasan/tailwindcss-variables/tree/master/__tests__/readme.test.js)

**Your own plugin themes:**
```javascript
// myplugin/themes.js
module.exports = (theme) => ({
  themes: {
    DEFAULT: {
      colors: {
        primary: 'black',
        secondary: 'white',
        warning: 'orange',
      },
    }
  }
})
```

**Your own plugin components:**
```javascript
// myplugin/components.js
module.exports = (theme) => ({
  select: {
    DEFAULT: {
      backgroundColor: 'var(--forms-colors-primary)',
    },
    multi: {
      '.default-multi': {
        backgroundColor: 'var(--forms-colors-secondary)',
      },
      '.other-multi': {
        backgroundColor: 'var(--forms-colors-warning)',
      },
    },
  },
})
```

**Your own plugin source:**
```javascript
// myplugin/index.js
const plugin = require('tailwindcss/plugin')
const _ = require('lodash')
const variablesApi = require('@mertasan/tailwindcss-variables/api')
const pluginComponents = require('./components')
const pluginThemes = require('./themes')

module.exports = plugin.withOptions(
  function (options) {
    return function ({addComponents, theme, config}) {

      let variableOptions = {
        variablePrefix: theme('myPlugin.prefix', '--forms')
      };

      addComponents(variablesApi.variables(_.merge(pluginThemes(theme).themes, {DEFAULT: theme('myPlugin.options', {})}), variableOptions))

      let darkVariables = theme('myPlugin.darkOptions', {});
      if (!_.isEmpty(darkVariables)) {
        addComponents(variablesApi.darkVariables(darkVariables, variableOptions, config('darkMode')))
      }

      // Automatically register components via API.
      addComponents(variablesApi.getComponents('.form', pluginComponents(theme)))

    }
  }
)
```

**User config:** (`tailwind.config.js`)
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    myPlugin: {
      options: {
        colors: {
          primary: 'indigo', // custom color instead of default color
        }
      }
    },
  },
  variants: {},
  plugins: [require('my-plugin')],
}
```

**Output:**
```css
:root {
  --forms-colors-primary: indigo; /* <<< default color changed via root configuration */
  --forms-colors-secondary: white;
  --forms-colors-warning: orange;
}

.form-select {
    background-color: var(--forms-colors-primary);
}

.form-select .default-multi {
    background-color: var(--forms-colors-secondary);
}

.form-select .other-multi {
    background-color: var(--forms-colors-warning);
}
```

Bu örnekteki gibi bir kurgu sayesinde, eklentinizin stilleri için, sizin ek .css dosyaları yayınlamanıza,
kullanan kişilerin ise paketlerinizi kullanabilmek için stil dosyaları derlemelerine gerek kalmayacak.


## Örnekler ve testler

Hem kullanım şekli konusunda yardımcı olması için, hem de sunulan tüm özellikleri test ederek
doğru çalıştığından emin olmak için örnekler hazırladım.


| Kaynak | Durum |
| --- | --- |
| [Örnekler](https://github.com/mertasan/tailwindcss-variables/tree/master/examples/examples) | ![Examples](https://img.shields.io/github/workflow/status/mertasan/tailwindcss-variables/build?label=examples)	|
| [Plugin API örnekleri](https://github.com/mertasan/tailwindcss-variables/tree/master/examples/api-examples) | ![API Examples](https://img.shields.io/github/workflow/status/mertasan/tailwindcss-variables/build?label=api-examples) |
| [Testler](https://github.com/mertasan/tailwindcss-variables/tree/master/__tests__) | ![Tests](https://img.shields.io/github/workflow/status/mertasan/tailwindcss-variables/tests?label=tests) |
| [Travis CI](https://travis-ci.com/mertasan/tailwindcss-variables) | ![Tests](https://img.shields.io/travis/mertasan/tailwindcss-variables/master.svg?label=travis-ci) |

> Örneklere ve testlere ait dosyalar pull-request, push, release vb. etkinliklerde otomatik olarak yeniden
> derlenmektedir. Bu nedenle, örnek dosyalarda `require(../index)` gibi dosya yolları kullanıldı.
> Örnekleri kullanacaksanız eğer, ilgili yerleri `require('@mertasan/tailwindcss-variables')` şeklinde değiştirmeniz gerekiyor.


## Yardım

Lütfen GitHub issues aracılığıyla tüm soru ve sorunlarınızı iletin. Size yardımcı olmaya çalışacağım.

## Katkı

Herhangi bir özelliği iyileştirir veya yeni özellikler eklerseniz eğer, lütfen pull-request göndermekten çekinmeyin.

## License

The GPL-3.0 License (GNU General Public License 3.0)

Please see License File for more information.
