# Nimbus Java API

This project contains some reusable libraries originally build for one of my projects called `Nimbus`.

## Routing API

Here is an example of the routing library available in this project.

```java
Router router = new Router()
	.get("/ping",        (req, res) -> Render.string("pong"))
	.get("/hello/:name", (req, res) -> Render.string("Hello " + req.pathParameter(":name") + "!"));

JettyServer server = new JettyServer(8443)
	.https("/path/to/keystore/file", "KeystorePassword")
	.multipart("/path/to/upload/temp/folder", ...)
	.start(router);

// ...
server.stop();
```

It should look familiar to those who know [Spark](http://sparkjava.com/), [WebMotion](https://github.com/webmotion-framework/webmotion) or [Express.js](https://expressjs.com/).

## Web Server Application

This [application](./src/fr/techgp/nimbus/server/impl/WebServerApplication.java) is a simple web server built using the routing API.

The default behaviour is this :

- run on port `10001` in HTTP and share the `public` folder as root (and only) folder
    - check access to http://localhost:10001/index.html with default configuration
- read configuration from `webserver.conf`
    - use `-Dwebserver.conf=another-file.conf` to change it's location
    - use `-Dwebserver.conf=default.conf:customized.conf` to use both files
- write traces in `webserver.log`
    - use `-Dwebserver.log=another-file.log` to change it's location
    - use `-Dwebserver.log=none` to disable file logging and write to the output
- write process id in `webserver.pid` when application is started
    - use `-Dwebserver.pid=another-file.pid` to change it's location
    - this should make termination easier, like ``kill -9 `cat webserver.pid` ``

The configuration can be easily customized. For instance, to share 2 folders with HTTPS enabled, you could use this configuration :

```properties
server.port=10001
server.keystore=webserver.pkcs12
server.keystore.password=CHANGEME
static.0.folder=public
static.0.prefix=/public
static.1.folder=/path/to/folder2
static.1.prefix=/public2
```

If you want to give it a try, go for it !

```bash
git clone https://github.com/guillaumeprevot/nimbus-java-api.git
cd nimbus-java-api
mvn compile
cd webserver
java -cp ../bin:../lib/* fr.techgp.nimbus.server.impl.WebServerApplication
```

If you want to share any thoughts about it, feel free to do so !
