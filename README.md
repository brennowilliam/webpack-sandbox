# Webpack Sandbox

## NPM Scripts
To add scripts to our project, we can edit ```package.json``` under the ```scripts``` property to build complex commands.
Once is saved in there, we can run <br />
```> npm run <nameOfScript>```.

Ex:

```javascript
scripts: {
  "webpack": "webpack",
  "dev": "npm run webpack -- --mode development",
  "prod": "npm run webpack -- --mode production"
}

In order to run these scripts we can do the following:
```
```> npm run webpack``` <br/>
```> npm run dev``` <br />
```> npm run prod``` <br />


### How are we able to run executables such as webpack in the scripts?
Webpack builds a directory called ```.bin``` where they hoisted to be available for us to use them.

### Debugging
To set up debug with webpack, do the following:
```javascript
"scripts": {
  ...
  "debugexample": "node --inspect --inspect-brk ./src/index.js"
}
// NOTE: --inspect-brk is not available on node v6.## during my tests. If you don't have a newer version installed,
// replace --inspect-brk with --debug-brk
```

#### Debugging node on Chrome Dev Tools
So, if you do ```> run npm debugexample```, you will notice that a url is provided in the console
as an output. <br />

If you're using Chrome, you can enter the following in the address bar ```chrome://inspect```
and it will take you DevTools with Device section open.

Notice at the bottom, you will see a section called ```Remote Target``` that must contain an entry for
the file you specified in the scripts. In this case, ```./src/index.js```.

Now, if you click on ```Open dedicated DevTools for Node```, you will be able to start debugging node,
right on the browser.

#### Watch flag
It makes no sense to keep running the same commands over and over again when we make some changes to our code. To make it easier on us, we can add a watch flag to our scripts to watch for changes automatically.

```javascript
"scripts": {
  ...
  "dev": "npm run webpack -- --mode development --watch"
  ...
}
```

### ES Module Syntax

### Exports
We can export a single default export from a file.
```javascript
export default function() {
  console.log('I am a default export')
}

// In some other file
import myFunc from "./someFileName"; // Export default we can name it whatever we like.
```

Or we can have multiple exports with the ```export``` keyword.
```javascript
export const ADD = "ADD"
export const add = (a, b) => a + b

// In some other file
import {ADD, add} from "./someFileName";
// We don't need to specify all the exports in between {}. We can choose what we like
```

## Customizing Webpack

Webpack would not be as popular as it is if we could not overwrite its default.

This is where the magic begins.

To do that, Webpack looks for a file name ```webpack.config.js```.

To start customizing create the the file mentioned above and declare a default export.

```javascript
  module.exports = {
     all the configuration goes here...
  }
```

## Webpack Core Concepts
There are 4 core concepts
### 1. Entry
It is like the kick off point of your application. The root of the tree.
Tells webpack what (files) to load for the browser;
Compliments the Output property.
```javascript
module.exports = {
  entry: './app.ts', // Just a relative path
  //...
}
```
### 2. Output
Tell webpack where and how to distribute bundles (compilation)
A simple example below
```javascript
module.exports = {
  // ...
  output: {
    path: './dist',
    filename: './bundle.js',
  }
}
```

### 3. Loaders + Rules
It tells webpack how to process the files before adding to the dependency graph.

Loaders are also javascript modules that takes the source file, and returns it in
a [modified] state.

```javascript
  module: {
    rules: [
      {test: /\.ts$/, use: 'ts-loader'},
      {test: /\.js$/, use: 'babel-loader'},
      {test: /\.css$/, use: 'css-loader'},
      /*
        {
          test: regex,
          use: (Array|String|Function)
          include: RegExp[],
          exclude: RegExp[],
          issuer: (RegExp|String)[],
          enforce: "pre"|"post"
        }
      */
    ]
  }
```

This is a per file process. Not a bulk process.

#### Chaining Loaders
```javascript
rules: [
  {
    test: /\.less$/,
    use:['style', 'css', 'less'],
    exclude: [/\.less.test$/]
  }
]
```

Rules are ran from right to left. 
Image like this: style(css(less('sourcefile'))) Each function call
returns a new modified source.

Loaders tells webpack how to interpret and translate files. Files translated are per-file
basis before adding to the dependency graph.

### 4. Plugins

How to use plugings:
require() plugin from node modules.
add new instance of plugin to plugins key in config object.
provide additional information for arguments.

```javascript
var BellOnBundlerErrorPlugin = require('bell-on-error');
module.exports = {
  //...
  plugins: [
    new BellOnBundlerErrorPlugin(),
    //...
  ]
}
```

