API first:
- build the backend first, then build the interfaces around that backend.
- this workd especilly well with MEAN because of the forced separation between back and front end.

Basic Setup (meanapp):
- setup the node.js project
-- make project folder
-- npm init & install express, mongoose, and body-parser and any other dependences like method-override and lodash
-- make server folder
    -- put in node_modules, index.js, and packages.json
Server (server/index.js):
- setup server
-- require all dependences
-- create the app
-- use middlewear for rest api's
    -- (app.use(...);)
-- connect to the database
-- CORS Support to make an open API (if that's what you need for your app)
- register routes
- create models
-- modularizing your code by putting the models in a server/models folder.
-- require mongoose ( var mongoose = require('mongoose'); )
--create a schema ( var ObjectSchema = new mongoose.Schema({ ... }); )
-- export the schema ( module.exports = ObjectSchema; )
- create server/models/index.js
-- require your models
-- export the index Object
-- this acts as a registry of your models for other developers
-- ( module.exports = { ... movie: require('./Movie.js'), ... }; )
- require the models in the server
-- app.models = require('/models/index');
- create controllers
-- (server/controllers/ObjectController.js)
-- require RESTful ( var restful = require('node-restful'); )
-- module.exports = function(app, route){

    // setup the controller for REST
    var rest = restful.model('movie', app.models.movie).methods(['get', 'put', 'post', 'delete']);

    //register this endpoint with the application
    rest.register(app, route);

    //return middleware.
    return function(req, res, next){
        next();
    };
};
- create a registry of routes
-- server/routes.js
- reqister the routes in the server
--  app.models = require('./models/index');

    //load the routes
    var routes = require('./rouets');
    _.each(routes, function(controller, route){
        app.use(route, controller(app, route));
    });
    // __.
