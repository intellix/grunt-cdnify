# grunt-cdnify [![Build Status](https://secure.travis-ci.org/callumlocke/grunt-cdnify.png?branch=master)](http://travis-ci.org/callumlocke/grunt-cdnify) [![Dependency Status](https://gemnasium.com/callumlocke/grunt-cdnify.png)](https://gemnasium.com/callumlocke/grunt-cdnify)

> Grunt plugin for rewriting static resource URLs found in your HTML and CSS.

## What it does
The task looks through your specified files for URLs to rewrite, in the following places:

* `<img src="____">`
* `<script src="____"></script>`
* `<link rel="stylesheet" href="____">`
* `background-image: url(____);` in your CSS (including inside `<style>` tags in your HTML)

See options below for how it modifies them.


## Options
You should set either `base` **or** `rewriter` (not both).

### `base`
For the most common use case, just set a `base` string for your URLs – eg, `'//cdn.example.com/'`. The cdnify task will automatically search for all **local** URLs in your files, and prefix them with this string. (It will automatically avoid double-slashes.)

Example:

```javascript
cdnify: {
  someTarget: {
    options: {
      base: '//cdn.example.com/stuff/'
    },
    files: [{
      expand: true,
      cwd: 'app',
      src: '**/*.{css,html}',
      dest: 'dist'
    }]
  }
}
```

### `rewriter`
For more control, you can specify a custom `rewriter` function instead. In this case, the task will search for **all** URLs (not just local ones) and run your function on each one. Your function should return the new value.

Example:

```javascript
cdnify: {
  someTarget: {
    options: {
      rewriter: function (url) {
        if (url.indexOf('data:') === 0)
          return url; // leave data URIs untouched
        else
          return url + '?12345'; // add query string to all other URLs
      }
    },
    files: [{
      expand: true,
      cwd: 'app',
      src: '**/*.{css,html}',
      dest: 'dist'
    }]
  }
}
```
