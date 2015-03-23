---
icon: fa-flash
layout: post
title: visualizing a month of lightning
subtitle:
description: tippecanoe & mapbox studio at work with wdt data
---

[Weather Decision Technologies](http://wdtinc.com){:target="_blank"} has access to some fascinating weather data. One of those datasets that I enjoy playing around with is lightning. We ingest around 2,000 lightning strikes per minute. While the processing of that data is interesting, I wanted to attempt to visualize the sheer volume of lightning strikes. I pulled down an archive of lightning strikes from May 2013 and created a [map of 80,305,421 lightning strikes](https://a.tiles.mapbox.com/v4/weatherdecisiontechnologies.lgk0k0f6/page.html?access_token=pk.eyJ1Ijoid2VhdGhlcmRlY2lzaW9udGVjaG5vbG9naWVzIiwiYSI6IkhLM1dMUDgifQ.ykvRdysqB6BgpQKZ_x7dhg#4/23.32/-103.32){:target="_blank"}

<iframe width='100%' height='450px' frameBorder='0' src='https://a.tiles.mapbox.com/v4/weatherdecisiontechnologies.lgk0k0f6/attribution,zoompan.html?access_token=pk.eyJ1Ijoid2VhdGhlcmRlY2lzaW9udGVjaG5vbG9naWVzIiwiYSI6IkhLM1dMUDgifQ.ykvRdysqB6BgpQKZ_x7dhg#4/23.32/-103.32'></iframe>

In October of last year, MapBox open sourced a tool called [tippecanoe](https://github.com/mapbox/tippecanoe){:target="_blank"}. Tippecanoe builds vector tilesets from large GeoJSON features. This was one of the tools behind the [most detailed tweet map ever](https://www.mapbox.com/blog/twitter-map-every-tweet/){:target="_blank"}.

So I have a large dataset of lightning strikes combined with a tool that takes large GeoJSON features. Match made in heaven, right? Our lightning provider has an archive available to us broken out in gzipped csv files. The size of these daily archive files range from around 200MB - 2GB. I pulled down a couple of month's worth of archive files (~50GB) and started to tinker.

My first step was to take those files and create some GeoJSON. I took the quick and dirty approach. So I brute forced converted ~180M strikes from compressed CSV to single day GeoJSON files. The GeoJSON was not valid (to spec), but tippecanoe requires an array of features.

So here comes the work of tippecanoe. There are some other [options](https://github.com/mapbox/tippecanoe#options){:target="_blank"} that are available for usage. The following command is what I settled on to create this dataset:

`cat json/201305* | tippecanoe -z15 -g2 -o may-2013-ltg.mbtiles`

So the waiting game began. It doesn't take too long (5 or 6 hours) to process with 8GB of memory, but of course the more you have, the faster it is. I would not recommend limping in using 4GB of memory (it's just not going to happen).

So that worked, and I tweeted out a picture of it:

<blockquote class="twitter-tweet" lang="en"><p>just a month of lightning (&gt; 80 million strikes). processed with tippecanoe and styled with <a href="https://twitter.com/hashtag/mapbox?src=hash">#mapbox</a> studio. <a href="http://t.co/HYkW1HSiI9">pic.twitter.com/HYkW1HSiI9</a></p>&mdash; Jordan Rousseau (@jvrousseau) <a href="https://twitter.com/jvrousseau/status/578223533618163712">March 18, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

But there were some weird things that I couldn't get past. The concentration of the data was in the US (midwest). This was not the only area in the world that had a large amount of lightning strikes during May, 2013. Tippecanoe's progressive disclosure was too aggressive for non-concentrated areas of lightning.


![ltg-diff](/assets/img/ltg-diff.png){:width="100%"}

I started tweaking the settings and ran a single day (~4m strikes) through tippecanoe to make sure I could correct the problem. I landed on a pretty good combination to allow the features to still come out at low zoom levels while not going over some limits that MapBox has put into place (mbtile size limit, number of features within a vector tile, number of composite datasetsets to name a few).

`cat json/201305* | tippecanoe -z14 -g3 -r1.25 -X -o may-2013-ltg.mbtiles`

Now I have a huge .mbtiles file sitting here that needs to get itself to MapBox. The folks at MapBox created a nice tool to help me along my way: [mapbox-upload](https://github.com/mapbox/mapbox-upload){:target="_blank"}. MapBox built out a nice javascript and cli interface to use.

`export MapboxAccessToken={sk.secret-key-from-your-mb-account}`

`mapbox-upload {mb-account-name}.{mapid} {filename}.mbtiles`

After that, I moved on to styling. In taking [Eric Fischer's](https://www.mapbox.com/about/team/#eric-fischer){:target="_blank"} advice from his tweet map blog post, I used the colorize-alpha image-filter. Then it was just small tweaks until I was happy with the output for each zoom level. My weekend involved a lot of this view:

![ltg-studio-style](/assets/img/ltg-studio-style.png){:width="100%"}

Once it was all said and done, it made for a beautiful map to highlight the scale of just one single dataset that [Weather Decision Technologies](http://wdtinc.com){:target="_blank"}  provides to it's customers. If you're interested in lightning or any other weather data, hit Renny Vandewege up at [rvandewege@wdtinc.com](mailto:rvandewege@wdtinc.com)

lightning in west Africa:
![null-island-ltg](/assets/img/null-island-ltg.png){:width="100%"}
lightning in the Mediterranean:
![med-ltg](/assets/img/med-ltg.png){:width="100%"}
lightning in Southeast Asia:
![se-asia-ltg](/assets/img/se-asia-ltg.png){:width="100%"}
