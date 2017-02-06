# Project
A user interface for interacting with campaigns.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
# start json-server at localhost:3000
npm start
```

## Features
- **Vue.js 2.1**
- **vue-resource** for XHR requests
- **Create, Read, Update, Delete (CRUD)** campaigns via card's action panel
- **JSON Server** for REST API
    - Default data found at `server/data.js`
    - _**Note**: A `server/data.js` file (instead of .json) is used to prevent `PUT/POST/DELETE` from writing to disk, however it is still kept in-memory until server is stopped_
- **node-sass** for SCSS
- **Fully responsive**
- **Sorting by Created or Name**
- **Order by Ascending or Descending**
- [JS Standard style linting](https://github.com/feross/standard/blob/master/RULES.md#javascript-standard-style)
