e-mission phone app
--------------------

[![Join the chat at https://gitter.im/e-mission/e-mission-phone](https://badges.gitter.im/e-mission/e-mission-phone.svg)](https://gitter.im/e-mission/e-mission-phone?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

This is the phone component of the e-mission system.

Installing
---
We are using the ionic v3.19.1 platform, which is a toolchain on top of the apache
cordova project. So the first step is to install ionic using their instructions.
http://ionicframework.com/docs/v1/getting-started/

NOTE: Since we are still on ionic v1, please do not install v2 or v3, as the current codebase will not work with it.
Issue the following commands to install Cordova and Ionic instead of the ones provided in the instruction above.

```
$ npm install -g cordova@8.0.0
$ npm install -g ionic@3.19.1
```

Install gradle (https://gradle.org/install/) for android builds.

Then, get the current version of our code

Fork this repo using the github UI

Clone your fork

```
$ git clone <your repo URL>
```

```
$ cd e-mission-phone
```

Enable platform hooks, including http on iOS9

```
$ git clone https://github.com/driftyco/ionic-package-hooks.git ./package-hooks
```

Install all javascript components using bower

```
$ bower update
```

Make sure to install the other node modules required for the setup scripts.

```
npm install
```

Create a remote to pull updates from upstream

```
$ git remote add upstream https://github.com/e-mission/e-mission-phone.git
```

Setup cocoapods. For all versions > 1.9, we need https://cocoapods.org/ support. This is used by the push plugin for the GCM pod, and by the auth plugin to install the GTMOAuth framework. This is a good time to get a cup of your favourite beverage.

```
$ sudo gem install cocoapods
$ pod setup
```

To debug the cocoapods install, or make it less resource intensive, check out troubleshooting guide for the push plugin.
https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md#cocoapods

Configure values if necessary - e.g.

```
ls www/json/*.sample
cp www/json/setupConfig.json.sample www/json/setupConfig.json
cp ..... www/json/connectionConfig.json
```

Restore cordova platforms and plugins

```
$ cordova prepare
```

Installation is now complete. You can view the current state of the application in the emulator

    $ cordova emulate ios

    OR 

    $ cordova emulate android

The android build and emulator have improved significantly in the last release
of Android Studio (3.0.1).  The build is significantly faster than iOS, the
emulator is just as snappy, and the debugger is better since chrome saves logs
from startup, so you don't have to use tricks like adding alerts to see errors
in startup.

End to end testing
---
A lot of the visualizations that we display in the phone client come from the server. In order to do end to end testing, we need to run a local server and connect to it. Instructions for:

1. installing a local server,
2. running it, 
3. loading it with test data, and
4. running analysis on it

are available in the [e-mission-server README](https://github.com/e-mission/e-mission-server/blob/master/README.md).

In order to make end to end testing easy, if the local server is started on a HTTP (versus HTTPS port), it is in development mode and it has effectively no authentication. It expects the user token to contain the user email *in plaintext*.

By default, the phone app connects to the local server (localhost on iOS,
[10.0.2.2 on
android](https://stackoverflow.com/questions/5806220/how-to-connect-to-my-http-localhost-web-server-from-android-emulator-in-eclips))
by default. To connect to a different server, or to use a different
authentication method, you need to create a `www/json/connectionConfig.json`
file. You can find sample files for connecting physical devices to the local
server (`www/json/connectionConfig.physical_device2localhost.json.sample`), and
to production (www/json/connectionConfig.production.json.sample).

So when the phone app connects to a server that is in development mode, it is also in development mode. This means that any user email can be entered without a password. Developers should use one of the emails that they loaded test data for in step (3) above. So if the test data loaded was with `-u shankari@eecs.berkeley.edu`, then the login email for the phone app would also be `shankari@eecs.berkeley.edu`.


JS Testing
---
From the root directory run

    $ npm install karma --save-dev
    $ npm install karma-jasmine karma-chrome-launcher --save-dev

Write tests in www/js/test
To run tests if you have karma globally set, run 

    $ karma start my.conf.js 
    
in the root directory. If you didn't run the -g command, you can run
tests with 

    $ ./node_modules/karma/bin/karma start
    
in the root directory

Troubleshooting
---

Troubleshooting tips have now been moved to their own wiki page
https://github.com/e-mission/e-mission-phone/wiki/Troubleshooting-tips-(FAQ)

Debugging
---
If users run into problems, they have the ability to email logs to the
maintainer. These logs are in the form of an sqlite3 database, so they have to
be opened using `sqlite3`. Alternatively, you can export it to a csv with
dates using the `bin/csv_export_add_date.py` script.

```
<download the log file>
$ mv ~/Downloads/loggerDB /tmp/logger.<issue>
$ pwd
.../e-mission-phone
$ python bin/csv_export_add_date.py /tmp/loggerDB.<issue>
$ less /tmp/loggerDB.<issue>.withdate.log
```

Contributing
---

Create a new branch (IMPORTANT). Please do not submit pull requests from master

    $ git checkout -b mybranch

Make changes to the branch and commit them

    $ git commit

Push the changes to your local fork

    $ git push origin mybranch

Generate a pull request from the UI

Address my review comments

Once I merge the pull request, pull the changes to your fork and delete the branch
```
$ git checkout master
$ git pull upstream master
$ git push origin master
$ git branch -d mybranch
```
