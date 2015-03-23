---
icon: fa-leaf
layout: post
title: displaying global obs using turf.js
subtitle:
description: holy hexgrid!
---

<a href="http://rousseau.io/node-turf-obs"><img src="/assets/img/turf-obs.png" width="100%"/></a>

I have blogged about turfjs [before](http://rousseau.io/2014/08/27/turfjs-in-mapbox.js/). Since then, I have been playing around with some large datasets to see how turfjs performs at scale.

At [WDT](http://wdtinc.com), we have a decent amount of current observations at our disposal. I took the [global dataset](http://rousseau.io/okcjs-mapping/demo/turf-obs/obs.html) of observations and created a [hexgrid](http://rousseau.io/okcjs-mapping/demo/turf-obs/hex.html) map. As you can see, the hexgrid map loads _incredibly_ slow. In fact, I had to trim the domain down to North & Central America and increase the hexgrid resolution to 1 degree so it would load a bit __faster__.

Morgan Herlocker (creator of turfjs) tweeted these out around the time I was tinkering with global obs that helped point me into a new direction:

<blockquote class="twitter-tweet" lang="en"><p>This wave of JavaScript GIS is less about browser or realtime than it is about letting you automate your geo workflows easily and at scale.</p>&mdash; Morgan Herlocker (@morganherlocker) <a href="https://twitter.com/morganherlocker/status/557677315237609472">January 20, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p>[cont] realtime GIS is largely just an accidental byproduct of rebuilding tools from scratch that have been wrapped in 30 years of cruft</p>&mdash; Morgan Herlocker (@morganherlocker) <a href="https://twitter.com/morganherlocker/status/557678334076002307">January 20, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Sure, I can shove a shitload of observations into turfjs and process everything in the browser. The GIS analysis will eventually complete and results will show up on the map. However, if this is not preformant, what good does the application do? You just have real-time processing in the browser for the sake of having real-time processing in the browser. This is not to say, do not use turfjs in the browser ([MapBox](http://mapbox.com/blog) has built some great [examples](https://www.mapbox.com/blog/playback-the-iditarod-with-turf/)). Just be smart with where you use turfjs and ensure the best possible experience for the end user.

Turfjs allows you to take GeoJSON and execute geospatial analysis that were only possible using a PostGIS database or, heaven forbid, ESRI. Turfjs is the open-source key to GIS freedom.

I have re-thought my hexgrid map into this [node project](https://github.com/jvrousseau/node-turf-obs). Follow the steps on the readme to take a static version of our global observations and create a beautiful map. If you just want to see the map...you can find it [here](http://rousseau.io/node-turf-obs).
