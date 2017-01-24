---
icon: fa-trophy
layout: post
title: running in mapbox-gl
subtitle:
description: a quick hack to see my runs.
---

<br/><br/>
![okc2](/assets/img/run/disney.png){:width="100%"}<br/>
<sub><sup>*Walt Disney World in 2016*</sup></sub>

I run. Well, I attempt to run, but it's fun! There are plenty of posts that walk through how to map out runs. [Catalina marathon](https://www.mapbox.com/blog/run-the-catalina-eco-marathon-in-3d/){:target="_blank"}, [Runkeeper routes](https://www.mapbox.com/blog/runkeeper-routes){:target="_blank"}, and [the one that got me hooked](https://www.mapbox.com/blog/2012-08-28-running-maps/){:target="_blank"} are some of the ones that come to mind.

I played around with the entire dataset of my runs a while back (which was fun): 

<iframe width='100%' height='450px' frameBorder='0' src='https://api.mapbox.com/v4/jvrousseau.lgbcme65/zoompan,attribution,share.html?access_token=pk.eyJ1IjoianZyb3Vzc2VhdSIsImEiOiJYYUNlcVRZIn0.lp0867Jn5ynlj72kMwICSA#13/35.2017/-97.4476'></iframe>

However, the Catalina marathon blog post inspired me to start visualizing my individual runs. 

I started with a fundamental thought of "automated" as much as possible. From the point I export a run from [Strava](https://www.strava.com/){:target="_blank"}, [Runkeeper](https://runkeeper.com){:target="_blank"}, or your tracker of choice as a [GPX](http://www.topografix.com/gpx.asp){:target="_blank"} file all the way to viewing the dataset on a map.

![tulsa1](/assets/img/run/tulsa.png){:width="100%"}<br/>
<sub><sup>*Tulsa Route-66 in 2014*</sup></sub>

From the file I use [togeojson](https://github.com/mapbox/togeojson){:target="_blank"} to spit out a geojson version of the run. When uploading the tileset to Mapbox, I did not have much control over the metadata (elevation, heart rate, segment time, etc.). The output of togeojson are [parallel arrays](https://en.wikipedia.org/wiki/Parallel_array){:target="_blank"}: one of the arrays is the [linestring](http://geojson.org/geojson-spec.html#linestring){:target="_blank"} of the route and the other arrays are the metadata sitting in the properties object of the [feature](http://geojson.org/geojson-spec.html#feature-objects){:target="_blank"}. In comes [Turf](http://turfjs.org/){:target="_blank"} to help out.


Using Turf [meta](http://turfjs.org/docs.html#coordall){:target="_blank"}, I merged the parallel arrays to an array of objects. 

From there I create segments of the run by grabbing the n and n+1 datapoint. I wanted to minimize overlapping segments, so I used [turf.distance](http://turfjs.org/docs.html#distance){:target="_blank"} and [turf.lineSliceAlong](http://turfjs.org/docs.html#lineslicealong){:target="_blank"} to crop out 2 feet of each segment. After that, all  the segments are stuffed into a [feature collection](http://geojson.org/geojson-spec.html#feature-collection-objects){:target="_blank"}.

![okc1](/assets/img/run/okc.png){:width="100%"}<br/>
<sub><sup>*OKC Memorial in 2016*</sup></sub>

I wrote the geojson to disk and uploaded via [Mapbox](https://www.mapbox.com/studio/){:target="_blank"}. That's great, but it's still a manual process. Fortunately for me, Mapbox provides an [API](https://www.mapbox.com/api-documentation/#uploads){:target="_blank"} and a [JavaScript SDK](https://github.com/mapbox/mapbox-sdk-js){:target="_blank"} to take care of my manual process. From there I was able automatically upload the dataset and I build a quick map to display the run. 

With the metadata of the runs attached to each segment, I was able to take advantage of some great gl styling features. I used [data driven styling](https://www.mapbox.com/blog/data-driven-styling/){:target="_blank"} to visualize my heart rate using the same heartrate colorization of Strava. I also used [3D extrusions](https://www.mapbox.com/mapbox-gl-style-spec/#layers-fill-extrusion){:target="_blank"} to visualize the elevation of the run, but mapping my Oklahoma runs will not show much.

Here is the finished product, my run of the [OKC Memorial](http://okcmarathon.com/){:target="_blank"} Half Marathon back in April of 2016. If you would like to check out the code, it can be found [here](https://github.com/jvrousseau/mapbox-gpx){:target="_blank"}. Click [here](http://rousseau.io/mapbox-gpx){:target="_blank"} to view the standalone map.

<iframe width='100%' height='450px' frameBorder='0' src='http://rousseau.io/mapbox-gpx'></iframe>



