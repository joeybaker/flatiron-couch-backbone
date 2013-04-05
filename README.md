flatiron-couch-backbone
=======================

# DEPRECATED
This plugin is now *wheelhouse-couch*

https://npmjs.org/package/wheelhouse-couch
https://github.com/joeybaker/wheelhouse-couch

A flatiron plugin that allows you to use Backbone server side, and uses Couch for it's data store. This is largely a rewrite of [backbone-couch](https://github.com/developmentseed/backbone-couch) to get it to play nicely with [flatiron](https://github.com/flatiron/flatiron) and use the excellent [cradle](https://github.com/cloudhead/cradle) CouchDB library.

## Usage
```js
var flatiron = require('flatiron')
  , app = flatiron.app
  , dbPlugin = require('flatiron-couch-backbone')

app.use(flatiron.plugins.http, {})
app.use(dbPlugin, {
  name: 'database-name'
  , host: 'localhost' // default
  , port: 5984 // default
  // also takes cradle options
  , cache: true // disabled by default in development
  , raw: false // cradle default
  , callback: function(){} // do something after the database connection has been established
  , getId: function(model){ // used to get the DB id from the model. the default behavior follows: {{collectionName}}/{{UUID}}
    return model.url().substring(1) // default
  }
  , getCollectionName: function(collection){
    return collection.url.substring(1) // default
  }
})
app.start(8999)
```

After using the plugin, `Backbone.sync` is overridden to use CouchDB.

### Important note:
Your models should override the default `id` attribute to use `_id`.

```js
var model = Backbone.Model.extend({
  idAttribute: '_id'
})
```

## tests
You must have [grunt-cli](https://github.com/gruntjs/grunt-cli) installed: `sudo npm i -g grunt-cli`

### Run tests
`npm test`
