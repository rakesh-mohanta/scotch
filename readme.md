#Scotch
###A really classy, dead simple, markdown based, blogging framework for node.js

[![Build Status](https://travis-ci.org/Techwraith/scotch.png?branch=master)](https://travis-ci.org/Techwraith/scotch)

To try Scotch for yourself, make sure that you have node, npm, and mongodb installed (and running), then do this:

    $> npm install -g scotch-blog
    $> scotch create blog
    $> cd blog
    $> sudo scotch serve

Go to `http://0.0.0.0/dashboard/install` to install Scotch.

Your blog should be up and running on `http://localhost`

### Static Site Generation

    $> cd blog
    $> scotch generate

Your you should now have a 'static' directory in your blog's root directory.

Read the [deployment docs](#deployment) on how to configure static site deployment.

#### Dashboard

![Scotch's dashboard](https://dl.dropbox.com/u/7982297/scotch_screens/newdash.png)


#### Writing

![writing in Scotch](https://dl.dropbox.com/u/7982297/scotch_screens/newwrite.png)

#### Reading

![writing in Scotch](https://dl.dropbox.com/u/7982297/scotch_screens/newread.png)

#### Importing From Octopress

```
$ cd my-scotch-blog
$ scotch import ~/Octoblog/source/_posts
```

#### Deployment

Create `config/deployment.js` and fill it with your deployment settings then run `scotch deploy` from your terminal. At the moment we support S3 and FTP deployment, with gh-pages on its way.

##### Sample S3 Settings
```js
module.exports = {
  "destination": "s3"
, "opts": {
    "bucket": "<BUCKET NAME>"
  , "key": "<AWS ACCESS KEY>"
  , "secret": "<AWS SECRET KEY>"
  , "region": "<AWS REGION>"      //us-west-1
  }
};
```

##### Sample FTP Settings
```js
module.exports = {
  "destination": "ftp"
, "opts": {
    "host": "<FTP HOST>"          //ftp.myserver.com
  , "port": <FTP PORT>            //Default: 21
  , "username": "<FTP USERNAME>"
  , "password": "<FTP PASSWORD>"
  }
};
```

#### Plugins

Plugins are installed via npm and enabled via in `config/environment.js`.

```
/*
* Sample config/environment.js
* `npm install readmore`
*/
var config = {
  port: 80,
  model: {
    defaultAdapter: 'mongo'
  },
  db: {
    mongo: {
      dbname: 'blog'
    }
  },
  plugins: {
    formatters: [
      'readmore'
    ]
  }
};

module.exports = config;
```

##### Formatters

Formatter plugins enhance the markdown language.

```js
/*
* An example formatter plugin that replaces the string "charcount" with the number
* of characters in the markdown source when viewing a post in the blog index
*/
var replacer = function (buffer) {
  //`this` in the context of a formatter plugin refers to the post model
  return buffer.replace(/charcount/, this.markdown.length);
};

/*
* You can modify either the `index` or `show` actions.
* By not exporting to exports.show, our plugin will only
* run on posts during the index action
*/
exports.index = replacer;

```

#### Upgrading

A new field was added in ce22e2a4a804245ad81c8c7a82407e6dd830d287 for post publishing timestamps.

To upgrade your database:

```
$ cd my-scotch-blog
$ scotch upgrade
```

It is safe to do this multiple times.

### Things to Do

If you'd like to help out, check out the [issue list](https://github.com/Techwraith/scotch/issues?state=open).
