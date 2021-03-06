---
'@verdaccio/ui-theme': major
'@verdaccio/cli-standalone': major
'@verdaccio/web': major
---

feat: flexible user interface generator

**breaking change**

The UI does not provide a pre-generated `index.html`, instead the server generates
the body of the web application based in few parameters:

- Webpack manifest
- User configuration details

It allows inject html tags, javascript and new CSS to make the page even more flexible.

### Web new properties for dynamic template

The new set of properties are made in order allow inject _html_ and _JavaScript_ scripts within the template. This
might be useful for scenarios like Google Analytics scripts or custom html in any part of the body.

- metaScripts: html injected before close the `head` element.
- scriptsBodyAfter: html injected before close the `body` element.
- bodyAfter: html injected after _verdaccio_ JS scripts.

```yaml
web:
  scriptsBodyAfter:
    - '<script type="text/javascript" src="https://my.company.com/customJS.min.js"></script>'
  metaScripts:
    - '<script type="text/javascript" src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>'
    - '<script type="text/javascript" src="https://browser.sentry-cdn.com/5.15.5/bundle.min.js"></script>'
    - '<meta name="robots" content="noindex" />'
  bodyBefore:
    - '<div id="myId">html before webpack scripts</div>'
  bodyAfter:
    - '<div id="myId">html after webpack scripts</div>'
```

### UI plugin changes

- `index.html` is not longer used, template is generated based on `manifest.json` generated by webpack.
- Plugin must export:
  - the manifest file.
  - the manifest files: matcher (array of id that generates required scripts to run the ui)
  - static path: The absolute path where the files are located in `node_modules`

```
exports.staticPath = path.join(__dirname, 'static');
exports.manifest = require('./static/manifest.json');
exports.manifestFiles = {
  js: ['runtime.js', 'vendors.js', 'main.js'],
  css: [],
  ico: 'favicon.ico',
};
```

- Remove font files
- CSS is inline on JS (this will help with #2046)

### Docker v5 Examples

- Move all current examples to v4 folder
- Remove any v3 example
- Create v5 folder with Nginx Example

#### Related tickets

https://github.com/verdaccio/verdaccio/issues/1523
https://github.com/verdaccio/verdaccio/issues/1297
https://github.com/verdaccio/verdaccio/issues/1593
https://github.com/verdaccio/verdaccio/discussions/1539
https://github.com/verdaccio/website/issues/264
https://github.com/verdaccio/verdaccio/issues/1565
https://github.com/verdaccio/verdaccio/issues/1251
https://github.com/verdaccio/verdaccio/issues/2029
https://github.com/verdaccio/docker-examples/issues/29
