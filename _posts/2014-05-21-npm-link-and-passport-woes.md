---
icon: fa-flash
layout: post
title: Debugging issues with passport and npm link
subtitle:
description: trying to keep you from banging your head into the desk...
---

So the team and I have been working on a fairly large project. We have 3-4 large node applications that can either be run as separate applications or together in a 'core' application.

There are two projects that we are using passport. One for session-based authentication `LocalStrategy` and one for basic/token-based authentication `BasicStrategy`

Both applications work just fine as standalone apps as well as inside the core application when using normal npm install. However, as soon as we npm link the two modules inside the core application things go awry.

<pre data-src="prism.js" class="language-javascript"><code class="language-javascript">Error: Failed to serialize user into session
    at pass (/Users/jrousseau/Development/wdt-portal-core/node_modules/passport/lib/authenticator.js:277:19)
    at Authenticator.serializeUser (/Users/jrousseau/Development/wdt-portal-core/node_modules/passport/lib/authenticator.js:295:5)
    at IncomingMessage.req.login.req.logIn (/Users/jrousseau/Development/convergence-api/node_modules/passport/lib/http/request.js:48:29)
    at Strategy.strategy.success (/Users/jrousseau/Development/convergence-api/node_modules/passport/lib/middleware/authenticate.js:228:13)
    at verified (/Users/jrousseau/Development/convergence-api/node_modules/passport-local/lib/strategy.js:83:10)
    at /Users/jrousseau/Development/convergence-api/controller/auth/index.js:62:16)
    at Query.&lt;anonymous&gt; (/Users/jrousseau/Development/convergence-api/model/contact/index.js:306:13)
    at Query._callback (/Users/jrousseau/Development/wdt-portal-configuration/db.js:57:30)
    at Query.Sequence.end (/Users/jrousseau/Development/wdt-portal-configuration/node_modules/mysql/lib/protocol/sequences/Sequence.js:78:24)
    at Query._handleFinalResultPacket (/Users/jrousseau/Development/wdt-portal-configuration/node_modules/mysql/lib/protocol/sequences/Query.js:143:8)
</code></pre>

So what's going on here? I'm not sure the full details...but here's my best guess:

Here is the node_modules directory of the core application (notice the symlinks from the npm link command):

<pre data-src="prism.js" class="language-bash"><code class="language-bash">drwxr-xr-x  16 22252027  37678515  544 May 22 13:58 .
drwxr-xr-x  13 22252027  37678515  442 May 22 13:43 ..
drwxr-xr-x   4 22252027  37678515  136 May  6 14:25 .bin
lrwxr-xr-x   1 22252027  37678515   55 May 22 13:56 convergence-api -> ../../../.nvm/v0.10.26/lib/node_modules/convergence-api
drwxr-xr-x  13 22252027  37678515  442 May  6 10:55 express
drwxr-xr-x  16 22252027  37678515  544 May  6 14:25 jade
drwxr-xr-x   6 22252027  37678515  204 May  6 10:55 load-grunt-tasks
drwxr-xr-x   7 22252027  37678515  238 May  6 10:55 passport
lrwxr-xr-x   1 22252027  37678515   64 May 22 13:56 wdt-portal-configuration -> ../../../.nvm/v0.10.26/lib/node_modules/wdt-portal-configuration
lrwxr-xr-x   1 22252027  37678515   58 May 22 13:58 wdt-portal-imappro -> ../../../.nvm/v0.10.26/lib/node_modules/wdt-portal-imappro
lrwxr-xr-x   1 22252027  37678515   53 May 22 13:58 wdt-portal-ui -> ../../../.nvm/v0.10.26/lib/node_modules/wdt-portal-ui
</code></pre>

Passport is being initialized at the application level

<pre data-src="prism.js" class="language-javascript"><code class="language-javascript">var express = require('express'),
    app = express();
    passport = require('passport');

app.use(passport.initialize());
app.use(passport.session());
</code></pre>

So `require('passport')` in the core application will be pulled from the passport module installed in that application.
If we jump into the convergence-api node_modules you will see this:

<pre data-src="prism.js" class="language-bash"><code class="language-bash">drwxr-xr-x   19 22252027  37678515   646 May 26 17:45 .
drwxr-xr-x   16 22252027  37678515   544 May 23 11:04 ..
drwxr-xr-x    5 22252027  37678515   170 May 25 15:05 .bin
drwxr-xr-x   15 22252027  37678515   510 May 19 10:22 amqp-coffee
drwxr-xr-x    8 22252027  37678515   272 May 19 10:21 async
drwxr-xr-x   13 22252027  37678515   442 May 19 10:22 express
lrwxr-xr-x    1 22252027  37678515    14 May 26 17:45 passport
drwxr-xr-x    7 22252027  37678515   238 May 19 10:21 passport-local
lrwxr-xr-x    1 22252027  37678515    64 May 22 14:20 wdt-portal-configuration -> ../../../.nvm/v0.10.26/lib/node_modules/wdt-portal-configuration
</code></pre>

Notice the passport module is installed here as well...I don't know why, but this is where the issue lies. If we symlink the passport module to the core application module, then everything works like a charm:

<pre data-src="prism.js" class="language-bash"><code class="language-bash">drwxr-xr-x   19 22252027  37678515   646 May 26 17:45 .
drwxr-xr-x   16 22252027  37678515   544 May 23 11:04 ..
drwxr-xr-x    5 22252027  37678515   170 May 25 15:05 .bin
drwxr-xr-x   15 22252027  37678515   510 May 19 10:22 amqp-coffee
drwxr-xr-x    8 22252027  37678515   272 May 19 10:21 async
drwxr-xr-x   13 22252027  37678515   442 May 19 10:22 express
lrwxr-xr-x    1 22252027  37678515    14 May 26 17:45 passport -> ../../passport
drwxr-xr-x    7 22252027  37678515   238 May 19 10:21 passport-local
lrwxr-xr-x    1 22252027  37678515    64 May 22 14:20 wdt-portal-configuration -> ../../../.nvm/v0.10.26/lib/node_modules/wdt-portal-configuration
</code></pre>

Basically, we spent quite a while not npm linking due to this issue causing some massive headaches when it came to debugging. If you delete your node_modules directory of the sub-project, npm link will still fully install all dependencies even if a module exists in a higher level application.

Personally, I think it would be nice whenever you run npm link the node_modules directory in the subfolder would be removed and recreated so modules can be shared (just link doing a clean npm install)
