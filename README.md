# Webpack Sandbox

## NPM Scripts
To add scripts to our project, we can edit ```package.json``` under the ```scripts``` property to build complex commands.
Once is saved in there, we can run ```npm run <nameOfScript>```.

```
Ex:

```javascript
...
scripts: {
  "webpack": "webpack",
  "dev": "npm run webpack -- --mode development",
  "prod": "npm run webpack -- --mode production"
}
...
```
In order to run these scripts we can do the following:
```
npm run webpack
npm run dev
npm run prod
```

### How are we able to run executables such as webpack in the scripts?
Webpack builds a directory called ```.bin``` where they hoisted to be available for us to use them.


