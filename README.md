(Project was from August-December 2023 and was done in a team of 5, and this was a continuation of a previous CoffeeMaker version done with a previous team of 3, where I worked mainly on the frontend stuff with a little bit on the backend as well)

# csc326-OBP-204-3

See https://github.ncsu.edu/engr-csc326-staff/iTrust2-v11/wiki/ for more info.

### Intro to Maven

To run Maven, navigate to the `./CoffeMaker-Individual/` directory.

#### Maven CLI

If you have Maven installed on your computer:

```sh
mvn <target>
```

#### Maven wrapper

If you do not have Maven installed on your computer, and would like to use the bundled binary version:

```sh
./mvnw <target>
```

### Compilation

To compile the project:

```sh
mvn compile
```

### Development

To develop:

```sh
mvn spring-boot:run
```

This should launch [`spring-boot-devtools`](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.devtools)

Note that Eclipse probably does this automatically upon running the main class.

#### Reloading resources

Resources include the `application.yml`, as well as static web resources and dynamic Thymeleaf templates.

##### Manually

This copy resources from `src` to `target`:

```sh
mvn process-resources
```

##### Automatically

`spring-boot-maven-plugin` has been configured with the `addResources` option set to `true`, per [the docs](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto.hotswapping). This should have Maven automatically copy all resources from `src` to `target`. This should also trigger an application reload when running in an IDE or on the command line with `spring-boot-devtools`.

Eclipse should also be doing this automatically upon saving a resource. If Eclipse is bugging and the `addResources` options does not seem to be working, then there is a workaround which reconfigures web resources from `target` to `src`. This skips the need for relying on Eclipse or Maven to update to `target` with web resources. Note this workaround will not make Live Reload work, but manually reloading the page in the web browser will work.

###### Workaround A: `application.yml` configuration

`CoffeeMaker-Individual/src/main/resources/application.yml`
```yaml
spring:
  thymeleaf:
    prefix: file:src/main/resources/templates/
  resources:
    static-locations: file:src/main/resoures/static/
```

###### Workaround B: `mvn` command line options

```sh
mvn spring-boot:run \
    -Dspring-boot.run.jvmArguments="-Dspring.thymeleaf.prefix=file:src/main/resources/templates/ -Dspring.resources.static-locations=file:src/main/resources/static/"
```

###### Workaround C: by updating livereload conf?

todo?

https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.devtools.restart.excluding-resources

###### References

- https://github.com/spring-projects/spring-boot/issues/5136
- https://github.com/thymeleaf/thymeleaf/issues/614

#### Live Reload

When CoffeeMaker is running, it is possible to get the browser to automatically refresh the page when a resource is changed. This only happens if [Resource reloading](#reloading-resources) is working, and the `target` directory is being updated.

Once the backend is configured to support Live Reload, each page must load the `livereload.js` script. When `spring-boot-devtools` is running, this script should be available locally at [http://localhost:35729/livereload.js](http://localhost:35729/livereload.js).

##### JavaScript Script

Add the following HTML snippet to load the script on the page you want to use Live Reload from.

```html
<script src="http://localhost:35729/livereload.js"></script>
```

##### Browser Extension

Install the appropriate browser extension for Live Reload. Note the project is very dated and extensions may not be working.

[Project page](https://github.com/livereload/livereload-extensions)

[Firefox Extension](https://addons.mozilla.org/en-US/firefox/addon/livereload-web-extension/)

There seems to also be an extension available for Chrome.

### Integration

To get the green ball:

```sh
mvn clean test checkstyle:checkstyle
```

The Checkstyle report will be saved to `CoffeeMaker-Individual/target/checkstyle-result.xml`
