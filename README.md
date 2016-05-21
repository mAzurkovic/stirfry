# Stir Fry #\r\nStir fry is a ___fast___, ___lightweight___, and ___easy to use___ web framework.\r\n#### Creating your first server ##\r\n```javascript\r\nvar StirFry = require('stirfry');\r\nvar server = new StirFry(8080);\r\nserver.request(function (request, response) {\r\n response.send(\"Hello World!\");\r\n});\r\n```\r\nThis example creates a server on port 8080 and sets to to respond with `Hello World!` on any request. When you use `response.send` it appends the input to the response.\r\n#### Static File Servers ##\r\nStir Fry has a static file server method build in. All you need to do is this\r\n```javascript\r\nvar StirFry = require('stirfry');\r\nvar server = new StirFry(8080);\r\nserver.request(StirFry.static('public'));\r\n```\r\nPublic is the folder that the files get served from\r\n\r\n#### Asynchronous Operations ##\r\nStir Fry lets you run multiple asynchronous operations at once. You can do all the preprocessing you want in the `server.process` layer, and then once all of those are done, it runs `server.pre` listeners, and once those are done it runs `server.request` listeners.\r\n```javascript\r\nvar StirFry = require('stirfry');\r\nvar server = new StirFry(8080);\r\n\r\nvar fs = require('fs');\r\n\r\nserver.pre(function (request, response, end, async) {\r\n async.start();\r\n fs.readFile('file.txt', function (err, data) {\r\n response.data = data.toString();\r\n async.done();\r\n });\r\n \r\n});\r\nserver.request(function (request, response) {\r\n response.send(response.data);\r\n});\r\n```\r\nThis program uses `fs.readFile` to add a property to the response object and then sends it to the user. There are loads of more efficient ways to do this, this is just an example of how to use async.\r\n\r\n#### Sending Files ##\r\nStir Fry has a build in `response.sendFile` method, here is an example\r\n```javascript\r\nvar StirFry = require('stirfry');\r\nvar server = new StirFry(8080);\r\nserver.request(function (request, response) {\r\n response.sendFile('file.html');\r\n});\r\n```\r\n#### Responding Only to Certain Requests ##\r\nWhen you create a request, preprocessor, or processor listener, you have the option of limiting it to certain requests by regex matching. Here is an example\r\n```javascript\r\nvar StirFry = require('stirfry');\r\nvar server = new StirFry(8080);\r\nserver.request(/\\/.*?\\/hi/, function (request, response) {\r\n response.send(\"hi\");\r\n});\r\n```\r\nYou can access regex capture groups by accessing `request.params` as an array, `request.params` also processes query strings in the request.\r\n\r\n#### Post Requests ##\r\nYou can access post data by accessing `request.post` as an associative array
