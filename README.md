enb-borschik
==========

[![NPM version](http://img.shields.io/npm/v/enb-borschik.svg?style=flat)](https://www.npmjs.org/package/enb-borschik)
[![Build Status](http://img.shields.io/travis/enb-make/enb-borschik/master.svg?style=flat&label=tests)](https://travis-ci.org/enb-make/enb-borschik)
[![Coverage Status](https://img.shields.io/coveralls/enb-make/enb-borschik.svg?style=flat)](https://coveralls.io/r/enb-make/enb-borschik?branch=master)
[![devDependency Status](http://img.shields.io/david/enb-make/enb-borschik.svg?style=flat)](https://david-dm.org/enb-make/enb-borschik)

Предоставляет технологии `borschik`, `css-borschik-chunks` и `js-borschik-include`.

borschik
========

Обрабатывает файл Борщиком (раскрытие borschik-ссылок, минификация, фризинг).

Настройки фризинга и путей описываются в конфиге Борщика (`.borschik`) в корне проекта
(https://github.com/bem/borschik/blob/master/README.md).

**Опции**

* *String* **sourceTarget** — Исходный таргет. Например, `?.js`. Обязательная опция.
* *String* **destTarget** — Результирующий таргет. Например, `_?.js`. Обязательная опция.
* *String* **dependantFiles** — Замораживаемые борщиком файлы. Например `['?.css', '?.js']`.
* *Boolean* **minify** — Минифицировать ли в процессе обработки. По умолчанию — `true`.
* *Boolean* **freeze** — Использовать ли фризинг в процессе обработки. По умолчанию — `true`.
* *String* **tech** — Технология сборки. По умолчанию — соответствует расширению исходного таргета.
* *Object* **techOptions** — Параметры для технологии сборки. Возможные значения зависият от конкретной технологии.

**Пример**

```javascript
nodeConfig.addTech([ require('enb-borschik/techs/borschik'), {
  sourceTarget: '?.css',
  destTarget: '_?.css',
  minify: true,
  freeze: false,
  tech: 'css+'
} ]);
```

css-borschik-chunks
===================

Из *css*-файлов по deps'ам, собирает `css-chunks.js`-файл, обрабатывая инклуды, ссылки.
Умеет минифицировать и фризить.

`css-chunks.js`-файлы нужны для создания bembundle-файлов или bembundle-страниц.

Технология bembundle активно используется в bem-tools для выделения
из проекта догружаемых кусков функционала и стилей (js/css).

**Опции**

* *String* **target** — Результирующий таргет. По умолчанию `?.css-chunks.js`.
* *String* **filesTarget** — files-таргет, на основе которого получается список исходных файлов
  (его предоставляет технология `files`). По умолчанию — `?.files`.
* *Boolean* **minify** — Минифицировать ли в процессе обработки. По умолчанию — `true`.
* *Boolean* **freeze** — Использовать ли фризинг в процессе обработки. По умолчанию — `true`.
* *String* **tech** — Технология сборки. По умолчанию — соответствует расширению исходного таргета.

**Пример**

```javascript
nodeConfig.addTech([ require('enb-borschik/techs/css-borschik-chunks'), {
  minify: true,
  freeze: false
} ]);
```

js-borschik-include
===================

Собирает *js*-файлы инклудами борщика, сохраняет в виде `?.js`.
Технология нужна, если в исходных *js*-файлах используются инклуды борщика.

В последствии, получившийся файл `?.js` следует раскрывать с помощью технологии `borschik`.

**Опции**

* *String* **target** — Результирующий таргет. Обязательная опция.
* *String* **filesTarget** — files-таргет, на основе которого получается список исходных файлов
 (его предоставляет технология `files`). По умолчанию — `?.files`.
* *String[]* **sourceSuffixes** — суффиксы файлов, по которым строится files-таргет. По умолчанию — ['js'].

**Пример**

```javascript
nodeConfig.addTechs([
  [ require('enb-borschik/techs/js-borschik-include') ],
  [ require('enb-borschik/techs/borschik'), {
      source: '?.js',
      target: '_?.js'
  } ]);
]);
```
