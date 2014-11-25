<i>This tutorial uses
* [swagger-node-express] version 2.1.2
* [Swagger Specification 1.2]
* [node.js] version 0.10.33
</i>

# Setting up Swagger-UI with node.js



1. Install [node.js]
2. Download the [swagger-node-express] package from github ("Download ZIP" on the right) and extract the files.
3. Open a command line tool and navigate to `/swagger-node-express-master/sample-application`
4. Run the command `npm install` to install project dependencies
5. Run the application with the command `node app.js` 
6. View the doc at http://localhost:8002/docs

### Displaying your own Swagger documentation
##### Local Static Files
If you don't have them already, write up some JSON files to describe your API using [Swagger Specification 1.2].

Make a new folder in the sample-application folder (I called mine "mydocs") and move all of your API Declaration files into it.

Make a new file in that folder called "api-docs.js", copy/paste your Resource Listing into the file, and add "module.exports = " to the very beginning.

It should look something like this:
```
module.exports = {
    "swaggerVersion":"1.2",
    "basePath": "https://...."
    ...(the rest of your Resource Listing)
}
```
To expose these files with your node application, you'll need to edit the "app.js" file in the sample-application folder. Open it, and somewhere around line 123 add this code:
```
app.use('/mydocs/', express.static(__dirname + '/mydocs/'));
app.get('/mydocs', function(req, res, next) {
    res.json(require('./mydocs/api-docs.js'));
});
```
  

(Replace "mydocs" with the name of your new folder in sample-application)

After you've saved your files, you should be able to view your docs at http://localhost:8002/mydocs.

If you want your docs to appear when the UI loads, open /swagger-node-express-master/swagger-ui/index.html and edit the default URL to point to your Resource Listing:
```
30  } else {
31    url = "http://localhost:8002/mydocs"
32  }
```
You can set this URL to point to an external Swagger Resource Listing as long as that server has given your app cross-origin resource sharing permissions.
##### Pointing to externally hosted Swagger files
The Resource Listing for the Swagger pet store demo is hosted at this URL:
```
http://petstore.swagger.wordnik.com/api/api-docs
```
If you type this into the URL bar at the top of your local UI, you should see the complete pet store demo documentation. You can view any external Swagger documentation in the same manner, as long as the server hosting the files has the proper CORS settings enabled. 

To view external docs by default in your local UI, set the "url" parameter in index.html line 31 to the external Resource Listing's URL. 

[Swagger Specification 1.2]:https://github.com/swagger-api/swagger-spec/blob/master/versions/1.2.md
[node.js]:http://nodejs.org
[swagger-node-express]:http://github.com/swagger-api/swagger-node-express
