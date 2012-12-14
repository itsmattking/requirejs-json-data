# json-data.js RequireJS plugin

## Example Usage

This RequireJS plugin allows you to load JSON via AJAX as a dependency. Includes optional auth support and caching scheme.

    define('main', ['json-data!/data.json'], function(data) {
      // code here...
    });

Or an external URL (make sure CORS is enabled!):

    define('main', ['json-data!http://api.foo/data.json'], function(data) {
      // code here...
    });

There is an storage option that will cache the fetched data in the client localStorage.

    define('main', ['json-data!/data.json?store=true'], function(data) {
      // code here...
    });
    
It could be useful for caching large amounts of information you don't want to fetch on every request.

You can also extend json-data to provide baseURL and auth configuration if you don't want to set those options globally. An example:

    define('foo-data', ['json-data'], function(jsonData) {

      return {
        load: function(name, req, onLoad, config) {
          // Probably a good idea to clone the config object so we don't modify global options
          var newConfig = Object.create(config);
          newConfig.jsonData = {
            baseURL: 'http://api.foo'
          };
          jsonData.load(name, req, onLoad, newConfig);
        }
      };

    });

Global options for jsonData:

    require.config({
      jsonData: {
         // username and password to use in auth (both optional,
         // you can have an empty username or password)
        username: '[username]',
        password: '[password]',
        // by default the URL base will be the same as the origin server of your scripts,
        // you can specify a default base URL as well (make sure the endpoint supports CORS)
        baseURL: 'http://api.foo/',
        // How long in seconds any cached data should be able to live
        storeExpire: 86400 // 1 day
      }
    });

