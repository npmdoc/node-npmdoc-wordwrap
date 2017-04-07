# api documentation for  [wordwrap (v1.0.0)](https://github.com/substack/node-wordwrap#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-wordwrap.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-wordwrap) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-wordwrap.svg)](https://travis-ci.org/npmdoc/node-npmdoc-wordwrap)
#### Wrap those words. Show them at what columns to start and stop.

[![NPM](https://nodei.co/npm/wordwrap.png?downloads=true)](https://www.npmjs.com/package/wordwrap)

[![apidoc](https://npmdoc.github.io/node-npmdoc-wordwrap/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-wordwrap_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-wordwrap/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-wordwrap/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-wordwrap/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "James Halliday",
        "email": "mail@substack.net",
        "url": "http://substack.net"
    },
    "bugs": {
        "url": "https://github.com/substack/node-wordwrap/issues"
    },
    "dependencies": {},
    "description": "Wrap those words. Show them at what columns to start and stop.",
    "devDependencies": {
        "tape": "^4.0.0"
    },
    "directories": {
        "lib": ".",
        "example": "example",
        "test": "test"
    },
    "dist": {
        "shasum": "27584810891456a4171c8d0226441ade90cbcaeb",
        "tarball": "https://registry.npmjs.org/wordwrap/-/wordwrap-1.0.0.tgz"
    },
    "gitHead": "9f02667e901f2f10d87c33f7093fcf94788ab2f8",
    "homepage": "https://github.com/substack/node-wordwrap#readme",
    "keywords": [
        "word",
        "wrap",
        "rule",
        "format",
        "column"
    ],
    "license": "MIT",
    "main": "./index.js",
    "maintainers": [
        {
            "name": "substack",
            "email": "mail@substack.net"
        }
    ],
    "name": "wordwrap",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/substack/node-wordwrap.git"
    },
    "scripts": {
        "test": "expresso"
    },
    "version": "1.0.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module wordwrap](#apidoc.module.wordwrap)
1.  [function <span class="apidocSignatureSpan">wordwrap.</span>hard (start, stop)](#apidoc.element.wordwrap.hard)
1.  [function <span class="apidocSignatureSpan">wordwrap.</span>soft (start, stop, params)](#apidoc.element.wordwrap.soft)



# <a name="apidoc.module.wordwrap"></a>[module wordwrap](#apidoc.module.wordwrap)

#### <a name="apidoc.element.wordwrap.hard"></a>[function <span class="apidocSignatureSpan">wordwrap.</span>hard (start, stop)](#apidoc.element.wordwrap.hard)
- description and source-code
```javascript
hard = function (start, stop) {
    return wordwrap(start, stop, { mode : 'hard' });
}
```
- example usage
```shell
...
Pad out lines with spaces out to column 'start' and then wrap until column
'stop'. If a word is longer than 'stop - start' characters it will overflow.

In "soft" mode, split chunks by '/(\S+\s+/' and don't break up chunks which are
longer than 'stop - start', in "hard" mode, split chunks with '/\b/' and break
up chunks longer than 'stop - start'.

wrap.hard(start, stop)
----------------------

Like 'wrap()' but with 'params.mode = "hard"'.
...
```

#### <a name="apidoc.element.wordwrap.soft"></a>[function <span class="apidocSignatureSpan">wordwrap.</span>soft (start, stop, params)](#apidoc.element.wordwrap.soft)
- description and source-code
```javascript
soft = function (start, stop, params) {
    if (typeof start === 'object') {
        params = start;
        start = params.start;
        stop = params.stop;
    }

    if (typeof stop === 'object') {
        params = stop;
        start = start || params.start;
        stop = undefined;
    }

    if (!stop) {
        stop = start;
        start = 0;
    }

    if (!params) params = {};
    var mode = params.mode || 'soft';
    var re = mode === 'hard' ? /\b/ : /(\S+\s+)/;

    return function (text) {
        var chunks = text.toString()
            .split(re)
            .reduce(function (acc, x) {
                if (mode === 'hard') {
                    for (var i = 0; i < x.length; i += stop - start) {
                        acc.push(x.slice(i, i + stop - start));
                    }
                }
                else acc.push(x)
                return acc;
            }, [])
        ;

        return chunks.reduce(function (lines, rawChunk) {
            if (rawChunk === '') return lines;

            var chunk = rawChunk.replace(/\t/g, '    ');

            var i = lines.length - 1;
            if (lines[i].length + chunk.length > stop) {
                lines[i] = lines[i].replace(/\s+$/, '');

                chunk.split(/\n/).forEach(function (c) {
                    lines.push(
                        new Array(start + 1).join(' ')
                        + c.replace(/^\s+/, '')
                    );
                });
            }
            else if (chunk.match(/\n/)) {
                var xs = chunk.split(/\n/);
                lines[i] += xs.shift();
                xs.forEach(function (c) {
                    lines.push(
                        new Array(start + 1).join(' ')
                        + c.replace(/^\s+/, '')
                    );
                });
            }
            else {
                lines[i] += chunk;
            }

            return lines;
        }, [ new Array(start + 1).join(' ') ]).join('\n');
    };
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
