# itranswarp.js

![icon](https://raw.githubusercontent.com/michaelliao/itranswarp.js/master/resource/icon-64x64.png)

A nodejs powered website containing blog, wiki, discuss and search engine.

[![Build Status](https://travis-ci.org/michaelliao/itranswarp.js.svg?branch=master)](https://travis-ci.org/michaelliao/itranswarp.js)

* based on koa2 with ES7 async/await
* OAuth2 integration (weibo, QQ, facebook, etc.)
* SEO support
* REST api
* customized css with uikit2
* fully tested (using mocha/chai)

### Environment

Nodejs: >= 8.x

MySQL: 5.6 ~ 5.7（Set root password as password）

Memcache （run memcached -m 32 -p 11211 -d start）

Nginx（run and change the port form 8080 to 80）

### Configurations

You should make a copy of `config_default.js` to `config_<NODE_ENV>.js`, and override some of the settings you needed.

For example, if NODE_ENV=production, you need create `config_/usr/local/bin/node.js`（In the directory of www）:

    $ cp www/config_default.js www/config_/usr/local/bin/node.js

You can safely remove any settings you do not changed.

### Install packages

Run `npm install`（In the directory of www） to install all required packages:

    $ npm install

### Initialize database

Run `node  script/init-db.js > init_db.sql` （In the directory of www）to generate initial schema as well as administrator's email and password.

You will get `init_db.sql` file in current directory. Run this SQL script by(www):

    $ mysql -u root -p < init_db.sql

NOTE: 
* Running the command above needs ur input of email & password, which would be required when you login as admin user, `SO BEAR THAT IN UR MIND `
* Re-run this SQL file will remove all existing data.

### Compile CSS
Run `sudo npm install less -gd` to install less globally;
Run `sudo npm install less-watch-compiler`to install less-watch-compiler
Then draging the less.sh to Terminal and press return
The CSS files should be compiled properly.

### Run

    $ node start.js

You should able to see the home page in the browser with address `http://localhost:2017/`.

If you want to sign in to management console, go to `http://localhost:2017/manage/signin`, and sign in using the email and password you entered when running `node script/node-db.js`.

### Maintaining
If there is an error occured that is `Invalid parameter: image`,u need to run `brew install GraphicsMagick ImageMagick` in Terminal.  Visiting [Issues](https://github.com/michaelliao/itranswarp.js/issues?q=is%3Aissue+is%3Aclosed) for more information.

### Test

iTranswarp.js is fully tested. To run tests, make sure:

* run MySQL in localhost and set root password as `password`.
* run Memcache in localhost.

Then run:

    $ mocha

Schema will be created in MySQL `test` database before run tests.


### Changelog

2.1 - 14 Oct 2017

* collapsable tree view for wiki
* AD support

Database schema update:

```
DROP TABLE `randoms`;

CREATE TABLE `randoms` (
  `id` varchar(50) NOT NULL,
  `name` varchar(50) NOT NULL,
  `value` varchar(50) NOT NULL,
  `expired_at` bigint(20) NOT NULL,
  `created_at` bigint(20) NOT NULL,
  `updated_at` bigint(20) NOT NULL,
  `version` bigint(20) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `uni_rnd_value` (`value`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE `adslots` (
  `id` varchar(50) NOT NULL,
  `alias` varchar(50) NOT NULL,
  `name` varchar(100) NOT NULL,
  `description` varchar(1000) NOT NULL,
  `price` bigint(20) NOT NULL,
  `width` bigint(20) NOT NULL,
  `height` bigint(20) NOT NULL,
  `num_slots` bigint(20) NOT NULL,
  `num_auto_fill` bigint(20) NOT NULL,
  `auto_fill` text NOT NULL,
  `created_at` bigint(20) NOT NULL,
  `updated_at` bigint(20) NOT NULL,
  `version` bigint(20) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `uni_adslot_alias` (`alias`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE `adperiods` (
  `id` varchar(50) NOT NULL,
  `user_id` varchar(50) NOT NULL,
  `adslot_id` varchar(50) NOT NULL,
  `display_order` bigint(20) NOT NULL,
  `start_at` varchar(10) NOT NULL,
  `end_at` varchar(10) NOT NULL,
  `created_at` bigint(20) NOT NULL,
  `updated_at` bigint(20) NOT NULL,
  `version` bigint(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE `admaterials` (
  `id` varchar(50) NOT NULL,
  `user_id` varchar(50) NOT NULL,
  `adperiod_id` varchar(50) NOT NULL,
  `cover_id` varchar(50) NOT NULL,
  `weight` bigint(20) NOT NULL,
  `start_at` varchar(10) NOT NULL,
  `end_at` varchar(10) NOT NULL,
  `geo` varchar(100) NOT NULL,
  `keywords` varchar(100) NOT NULL,
  `url` varchar(1000) NOT NULL,
  `created_at` bigint(20) NOT NULL,
  `updated_at` bigint(20) NOT NULL,
  `version` bigint(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

2.0 - 15 Jul 2017

* fully async/await support
* markdown plugin support
* based on koa 2.x

1.11 - 21 Jul 2015

* support article, wiki, discuss.
* based on koa 1.x


