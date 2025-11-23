---
title: 'Top 10 Most Subscribed Youtube Channel Visualization[2013-2018]'
date: 2018-11-24
permalink: /posts/2018/11/youtube-channel-visualization/
tags:
  -JavaScript
  -Tutorials
  -Web
---

Motion Visualization is intriguing and explains a lot in itself. I have put together an example visualization of the Top 10 Youtubers(by subscribers) from November 2013 to November 2018.
I am not very informed of whether or not flash based visualizations are still relevant, but I found Google Motion Chart to have explained very well the data set I've taken into consideration. As always the codes remain public so people can take ideas from it and produce an even better visualizations for themselves.

It is especially interesting to produce a motion chart for data points where entities have varying rates of growth overtime. I had difficulties being able to view the visualizations on some versions of chrome although I had flash installed. Make sure you have installed adobe flash in-order to view the visualizations. Here is a link to the visualization.

I've also prepared a video from the visualization for the ones having trouble viewing the viz. You should watch it anyways, it's fun.

https://youtu.be/vMeKjwV15K8

The code below contains only a few portion of the dataset to make it much readable.

```
html
<!doctype html>
<html>
<head>
    <script type="text/javascript" src="https://www.google.com/jsapi"></script>
    
    <script type="text/javascript">
        google.load("visualization", "1", {packages:["motionchart"]});
        google.setOnLoadCallback(drawChart);


        function drawChart() {
            var data = new google.visualization.DataTable();

            data.addColumn('string', 'Organization');
            data.addColumn('date', 'Date');
            data.addColumn('number', 'Subscribers');
            loadYoutubeData(data);

        }


        function loadYoutubeData(data) {
            data.addRows([
                ['PewDiePie',  new Date (2013, 11, 1), 14881729],
                //['PewDiePie', new Date (2018, 11, 1), 71145119],
                ['Smosh', new Date (2013, 11, 1), 13014827],
                ['HolaSoyGerman', new Date(2013, 11, 1), 12063901],
                ['JennaMarbles', new Date(2013, 11, 1), 10961919],
                ['RihannaVEVO', new Date(2013, 11, 1), 10617173],
                ['nigahiga', new Date(2013, 11, 1), 10504418],
                ['RayWilliamJohnson', new Date(2013, 11, 1), 10344107],
                ['Machinima', new Date(2013, 11, 1), 9771825],
                ['OneDirectionVEVO', new Date(2013, 11, 1), 9029788],
                ['ERB', new Date(2013, 11, 1), 7691601],

                ['PewDiePie', new Date(2015, 1, 1), 34062473],
                ['HolaSoyGerman', new Date(2015, 1, 1), 21010264],
                ['Smosh', new Date(2015, 1, 1), 19663985],
                ['RihannaVEVO', new Date(2015, 1, 1), 15295585],
                ['OneDirectionVEVO', new Date(2015, 1, 1), 15174217],
                ['KatyPerryVEVO', new Date(2015, 1, 1), 14845896],
                ['JennaMarbles', new Date(2015, 1, 1), 14617456],
                ['EminemVEVO', new Date(2015, 1, 1), 14492570],
                ['nigahiga', new Date(2015, 1, 1), 13661249],
                ['Machinima', new Date(2015, 1, 1), 12248125],


                ['PewDiePie', new Date(2017, 8, 1), 57232583],
                ['HolaSoyGerman', new Date(2017, 8, 1), 32338930],
                ['JustinBieberVEVO', new Date(2017, 8, 1), 31353721],
                ['elrubiusOMG', new Date(2017, 8, 1), 25620604],
                ['RihannaVEVO', new Date(2017, 8, 1), 25463556],
                ['KatyPerryVEVO', new Date(2017, 8, 1), 24574565],
                ['TaylorSwiftVEVO', new Date(2017, 8, 1), 24328186],
                ['T-Series', new Date(2017, 8, 1), 24324185],
                ['Fernanfloo', new Date(2017, 8, 1), 23691057],
                ['EminemVEVO', new Date(2017, 8, 1), 23211015],
                
                ['PewDiePie', new Date(2018, 11, 1), 71267456],
                ['T-Series', new Date(2018, 11, 1), 71125778],
                ['Canal KondZilla', new Date(2018, 11, 1), 43081613],
                ['Justin Bieber', new Date(2018, 11, 1), 43073669],
                ['5-Minute Crafts', new Date(2018, 11, 1), 41455546],
                ['WWE', new Date(2018, 11, 1), 36799586],
                ['Dude Perfect', new Date(2018, 11, 1), 36688630],
                ['SET India', new Date(2018, 11, 1), 36046873],
                ['Ed Sheeran', new Date(2018, 11, 1), 35539034],
                ['HolaSoyGerman', new Date(2018, 11, 1), 34264571],

            ]);
            var chart = new google.visualization.MotionChart(document.getElementById('chart_div'));

            chart.draw(data, {width: 1200, height:600});

        }
    </script>

</head>
<body>
<div id="chart_div" style="width: 100%; height: 100%;"></div>
</body>
</html>
```