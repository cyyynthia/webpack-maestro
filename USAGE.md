# Usage
## Table of Contents
 - [Basic usage](#basic-usage)
 - [Shareable configs](#shareable-configs)
 - [API Reference](#api-reference)

## Basic usage
```js
// file: webpack.config.js
const maestro = require('webpack-maestro')

module.exports = maestro()
  .entry('src/main.js')
  .output('dist/out.js')
  .loader(/\.js$/, 'babel-loader')
  .loader(/\.css$/, [ 'style-loader', 'css-loader' ])
  // TBD: method or just an array [ name, opts ]?
  .loader(/\.png$/, [ maestro.loader('file-loader', { name: '[hash:20].[ext]' }) ])
  .build() // Generates the webpack config
```

## Shareable configs
Shareable configs are npm packages that your config can extend to quickly have a base config, with only little to no
modifications to do. We'll assume here the config we want to extend is `@cyyynthia/my-epic-webpack-config`.

### Use a shared config
```js
// file: webpack.config.js
const maestro = require('webpack-maestro')

module.exports = maestro()
  .extends('@cyyynthia/my-epic-webpack-config') // npm package
  .build()
```

Shareable configs may also have "fragments", to have more specific rules, such as for a specific framework:
```js
// file: webpack.config.js
const maestro = require('webpack-maestro')

module.exports = maestro()
  .extends('@cyyynthia/my-epic-webpack-config') // Import main config
  .extends('@cyyynthia/my-epic-webpack-config#react') // Import the "react" fragment
  .build()

// Alternatively, it can also be written as a single extend:
module.exports = maestro()
  .extends('@cyyynthia/my-epic-webpack-config', [ 'main', 'react' ])
  .build()
```

Shareable configs are required to set friendly names for the loaders and plugins they add. Those can be used to
modify or remove rules from a sharable config:
```js
const maestro = require('webpack-maestro')

module.exports = maestro()
  .extends('@cyyynthia/my-epic-webpack-config')
  .removeLoader('@cyyynthia/my-epic-webpack-config:png') // source:name
  .build()
```

### Publish a shared config
As mentioned earlier, shareable configs are required to set friendly names to the loaders and plugins they add, to
allow the end-user to quickly modify and/or remove things the shareable added. You don't need to set a super-specific
name, just something preferably descriptive and not too long.

```js
// file: index.js
const maestro = require('webpack-maestro')

module.exports = maestro.shareable()
  .loader(/\.js$/, 'babel-loader', { name: 'javascript' })
  .loader(/\.css$/, [ 'style-loader', 'css-loader' ], { name: 'css' })
  .loader(/\.png$/, [ maestro.loader('file-loader', { name: '[hash:20].[ext]' }) ], { name: 'png' })
  .export()
```

You can also export specific fragments, if you want to not bloat the main config chunk but provide support for
specific frameworks:
```js
// file: index.js
const maestro = require('webpack-maestro')

// Main config
module.exports = maestro.shareable()
  .loader(/\.js$/, 'babel-loader', { name: 'javascript' })
  .loader(/\.css$/, [ 'style-loader', 'css-loader' ], { name: 'css' })
  .loader(/\.png$/, [ maestro.loader('file-loader', { name: '[hash:20].[ext]' }) ], { name: 'png' })
  .export()

// Export a fragment named "react" which contains react-specific rules
module.exports.react = maestro.shareable()
  .loader('...')
  .export()
```

Or, using ES modules:
```js
// file: index.js
import maestro from 'webpack-maestro'

export default maestro.shareable()
  .loader(/\.js$/, 'babel-loader', { name: 'javascript' })
  .loader(/\.css$/, [ 'style-loader', 'css-loader' ], { name: 'css' })
  .loader(/\.png$/, [ maestro.loader('file-loader', { name: '[hash:20].[ext]' }) ], { name: 'png' })
  .export()

export const react = maestro.shareable()
  .loader(/\.jsx$/, '...', { name: 'react' })
  .export()
```

**Notes**:
 - It is not possible to name a fragment "main", since this is a reserved keyword for the `extend` method.
 - It is possible to only export fragments and have no main config

TBD: utility cli to test and validate the config

## API Reference
TBD
