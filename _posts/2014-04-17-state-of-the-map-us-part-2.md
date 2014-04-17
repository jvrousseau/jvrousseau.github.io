---
icon: fa-globe
layout: post
title: What I learned at State of The Map US
subtitle: Part 2
description: covering day 2 of the talks at sotmus
---
[![SOTMUS](/assets/img/logo.png)](http://stateofthemap.us)

This is part two of two of what I learned at SOTMUS. Part one can be found [here](/2014/04/15/state-of-the-map-us-part-1/)

I want to add some more posts later diving deeper into some topics that interested me.

##Presentations
Here are some random thoughts on some of the presentations. As with the previous post, I would encourage you to check out the linked videos and the presenters' twitter accounts

####[Open Data Alchemy](http://stateofthemap.us/session/making-data-alchemy/) - [Chris Helm](https://twitter.com/cwhelm)
* before the presentation he was looking at [FreshyMap](http://freshymap.com/)
    * adding the wind.io functionality into any interactive map is pretty interesting
    * the dashboard of ski resort images is really slick
* koop GeoJSON -> FeatureService is nice
* koop GFS is badass
* koop in general is really cool
* ESRI maps will now be using OSM data.
    * Does licensing issues (share-alike) come into play here?
    * Is it really stupid that it even has to be asked? I think so, ESRI should be free to improve their maps with the best data around
* koop API piping to geojson.io was really slick

####[Drone Imagery for OpenStreetMap](http://stateofthemap.us/session/future-relationship-between-osm-and-uavs/) - [Bobby Sudekum](https://twitter.com/bobws) and [Baptiste Tripard]()
* you shake the senseFly drone three times to start it up...most likely i would have shaken the thing twice and dropped if from the cliff
    * there's your 'oh shit' moment
* speaking of 'oh shit' moments, I would be absolutely useless if I was up on the Matterhorn for that drone flight
    * actually more than useless, someone would have to carry me down the mountain
* It crossed my mind a few times during the presentation as to how drone imagery could be used to map out response plans after tornadoes.
* the 3D images of the matterhorn were absolutely stunning
* [sensefly](http://www.sensefly.com) you're welcome

####[Advanced CartoCSS Tricks](http://stateofthemap.us/session/advanced-cartocss-tricks/) - [Kate Watkins](https://twitter.com/kateyw), Seth Fitzsimmons and [Alan McConchie](https://twitter.com/mappingmashups)
* using modulo to randomize a slight pitch of the labels were pretty cool
* I really dig the color scheme of the pinterest map
* the nuggetts of cartocss given (and explained) in the presentation was helpful
* when designing your own map in tilemill2, just remember that it's the *ENTIRE* map
* really digging the patterns on backgrounds using the white and alpha quadrants

####[Designing OpenStreetMap](http://stateofthemap.us/session/designing-openstreetmap/) - [Nicki Dlugash](https://twitter.com/nickidlugash)
* function and context are ridiculously important to cartography
* mapbox streets uses a subset of OSM data
* it looks like mapbox is prepping a map soley for driving routes
* the designs showed made me think about what is absolutely necessary on a weather map
    * need to get rid of or deemphasize the unnecessary basemap data, so the weather data will pop
* mapbox outdoors was demoed...very cool...love the elevation lines
* webgl rendering of vector tiles on the client was demoed at the end of the video...just ridiculous

####[Processing OpenStreetMap Into Vector Tiles](http://stateofthemap.us/session/processing-openstreetmap-into-vector-tiles/) - [Dane Springmeyer](https://twitter.com/springmeyer)
* my notes on this was probably 2-3 times longer than any other presentation.
* the ravioli analogy makes a lot of sense.
* the mapbox garage image is awesome.
* the entire world on a USB stick. How f'n big is that USB stick??
* working on FileGDB to vector tiles is awesome!
* delta and zig zag encoding to make the file size smaller is just brilliant.
* overzooming is great, but I wonder if there is a agreed upon zoom level to resolution ie 5km resolution data can stop at zoom level X
* compositing on different overzoomed datasets is very intersting...makes a lot of sense
* shapefile -> vector tile conversion 2000 tiles/second and dropping the dataset size down ~350MB WOW
* [OSM Bright Style](https://github.com/mapbox/osm-bright.tm2) open sourced...will definitely play with this.
* another plug on Browser/WebGL rendering coming soon...I can't wait to get my hands on that!
