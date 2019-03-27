# syncfusiontest

## Problem description
The problem is that the template functions written in the App.vue -> getEditCell never gets called

if you look into the console you see:
``` 
template create()
template write()
gridmapper created
gridmapper mounted
gridmapper setValue
template destroy()
```
but the read() method never gets called ;(

the line
```
template read()
```
is missing ;(

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
