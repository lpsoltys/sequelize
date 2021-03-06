_Please note!_ The github issue tracker should only be used for feature requests and bugs with a clear description of the issue and the expected behaviour (see below). All questions belong on [Slack](https://sequelize.slack.com), [StackOverflow](https://stackoverflow.com/questions/tagged/sequelize.js) or [Google groups](https://groups.google.com/forum/#!forum/sequelize).

# Issues
Issues are always very welcome - after all, they are a big part of making sequelize better. However, there are a couple of things you can do to make the lives of the developers _much, much_ easier:

### Tell us:

* What you are doing?
  * Post a _minimal_ code sample that reproduces the issue, including models and associations
  * What do you expect to happen?
  * What is actually happening?
* Which dialect you are using (postgres, mysql etc)?
* Which sequelize version you are using?

When you post code, please use [Github flavored markdown](https://help.github.com/articles/github-flavored-markdown), in order to get proper syntax highlighting!

If you can even provide a pull request with a failing unit test, we will love you long time! Plus your issue will likely be fixed much faster.

# Pull requests
We're glad to get pull request if any functionality is missing or something is buggy. However, there are a couple of things you can do to make life easier for the maintainers:

* Explain the issue that your PR is solving - or link to an existing issue
* Make sure that all existing tests pass
* Make sure you followed [coding guidelines](https://github.com/sequelize/sequelize/blob/master/CONTRIBUTING.md#coding-guidelines)
* Add some tests for your new functionality or a test exhibiting the bug you are solving. Ideally all new tests should not pass _without_ your changes.
  - Use [promise style](http://bluebirdjs.com/docs/why-promises.html) in all new tests. Specifically this means:
    - don't use `EventEmitter`, `QueryChainer` or the `success`, `done` and `error` events
    - don't use nested callbacks (use [Promise.bind](http://bluebirdjs.com/docs/api/promise.bind.html) to maintain context in promise chains)
    - don't use a done callback in your test, just return the promise chain.
  - Small bugfixes and direct backports to the 1.7 branch are accepted without tests.
* If you are adding to / changing the public API, remember to add API docs, in the form of [JSDoc style](http://usejsdoc.org/about-getting-started.html) comments. See [section 4a](#4a-check-the-documentation  ) for the specifics.
* Add an entry to the [changelog](https://github.com/sequelize/sequelize/blob/master/changelog.md), with a link to the issue you are solving

Still interested? Coolio! Here is how to get started:

### 1. Prepare your environment
Here comes a little surprise: You need [Node.JS](http://nodejs.org).

### 2. Install the dependencies

Just "cd" into sequelize directory and run `npm install`, see an example below:

```console
$ cd path/to/sequelize
$ npm install
```

### 3. Database... Come to me! ###

For MySQL and PostgreSQL you'll need to create a DB called `sequelize_test`.
For MySQL this would look like this:

```console
$ echo "CREATE DATABASE sequelize_test;" | mysql -uroot
```

**CLEVER NOTE:** by default, your local MySQL install must be with username `root` without password. If you want to customize that, you can set the environment variables `SEQ_DB`, `SEQ_USER`, `SEQ_PW`, `SEQ_HOST` and `SEQ_PORT`.

For Postgres, creating the database and (optionally) adding the test user this would look like:

```console
$ psql
# create database sequelize_test;
# create user postgres with superuser;
```

**AND ONE LAST THING:** Once `npm install` worked for you (see below), you'll
get SQLite tests for free :)

#### 3a. Docker

Makes sure `docker` and `docker-compose` are installed.

If running on Mac OSX, install [Docker for Mac](https://docs.docker.com/docker-for-mac/).

Then simply run:

```sh
npm run test-docker
```

And once in a while you might want to run:

```sh
npm run build-docker
```

To rebuild the image (in case of changed dependencies or similar).

If sequelize is unable to connect to mysql you might want to try running `sudo docker-compose up` in a second terminal window.

### 4. Run the tests ###

All tests are located in the `test` folder (which contains the
lovely [Mocha](http://visionmedia.github.io/mocha/) tests).

```console
$ npm run test-all || test-mysql || test-sqlite || test-mssql || test-postgres || test-postgres-native

$ # alternatively you can pass database credentials with $variables when testing
$ DIALECT=dialect SEQ_DB=database SEQ_USER=user SEQ_PW=password npm test
```

#### 4a. Check the documentation
This step only applies if you have actually changed something in the documentation. Please read [Documentation Contribution Guidelines](https://github.com/sequelize/sequelize/blob/master/CONTRIBUTING.DOCS.md) first.
To generate documentation for the `sequelize.js` file, run (in the sequelize dir)

```console
$ npm run docs
```

The generated documentation will be placed in `docs/tmp.md`.

### 5. That's all ###

Just commit and send your pull request. Happy hacking and thank you for contributing.

### Coding Guidelines ###
Have a look at our [.jshintrc](https://github.com/sequelize/sequelize/blob/master/.jshintrc) file for the specifics. As part of the test process, all files will be linted, and your PR will **not** be accepted if it does not pass linting.

#### Spaces ####

Use spaces when defining functions.

```js
function(arg1, arg2, arg3) {
  return 1;
}
```

Use spaces for if statements.

```js
if (condition) {
  // do something
} else {
  // something else
}
```

#### Variable declarations ####

```js
var num  = 1
  , user = new User()
  , date = new Date();
```

#### Semicolons ####
Yes

# Publishing a release

1. Ensure that latest build on master is green
2. Ensure your local code is up to date (`git pull origin master`)
3. `npm version patch|minor|major` (see [Semantic Versioning](http://semver.org))
4. Update changelog to match version number, commit changelog
5. `git push --tags origin master`
6. `npm publish .`
7. Copy changelog for version to release notes for version on github
