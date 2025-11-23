---
title: 'Visualization with VisJS'
date: 2018-08-19
permalink: /posts/2018/08/visjs-visualization/
tags:
  - JavaScript
  - Visualization
  - VisJS
  - Web Development
---

The intentions of this post is to host example code snippets so people can take ideas from it to make great visualization for themselves using visJS. VisJS is a dynamic, browser based visualization library. The library is designed to be easy to use, to handle large amounts of dynamic data, and to enable manipulation of and interaction with the data. The library consists of the components DataSet, Timeline, Network, Graph2d and Graph3d.

## VisJS network [Nodes as images with label]

```html
<!doctype html>
<html>
<head>
  <title>Bhishan's Services | TheTaraNights</title>

  <style type="text/css">

    body {
      font: 10pt arial;
    }
    #mynetwork {
      width: 100%;
      height: 900px;
      border: 1px solid lightgray;
      background-color:#333333;
    }
  </style>

  <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/vis/4.21.0/vis.js"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/vis/4.21.0/vis-network.min.css" rel="stylesheet" type="text/css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
  <script type="text/javascript">
    var nodes = null;
    var edges = null;
    var network = null;

    // Called when the Visualization API is loaded.
    function draw() {
      // create people.
      var DIR = 'https://www.thetaranights.com/wp-content/uploads/2018/fiverr_reviews/';

      var ratings = '<span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><br>';
      nodes = [
        {id: 1, shape: 'circularImage', image: "https://avatars3.githubusercontent.com/u/6163574?s=400&u;=c396866f2c1ca2a709c1ece0d5a352e0f6e1a865&v;=4", label:"Bhishan", size:60},
        // {id: 1, shape: 'circularImage', image: DIR + "bhishan.png", label:"Bhishan", size:60},

        {id: 2,  shape: 'circularImage', image: DIR + 'Benf97/cropped(1).png', label:"Benf97" },
        {id: 3,  shape: 'circularImage', image: DIR + 'Bigyoyo55/cropped(2).png', label:"Bigyoyo55"},
        {id: 4,  shape: 'circularImage', image: DIR + 'Btpope22/cropped(3).png', label:"Btpope22"},
        {id: 5,  shape: 'circularImage', image: DIR + 'Chrisraven/cropped(4).png', label: "Chrisraven"},
        {id: 6,  shape: 'circularImage', image: DIR + 'Cnnsbs/cropped(5).png', label:"Cnnsbs"},
        {id: 7,  shape: 'circularImage', image: DIR + 'Danielemariotto/cropped(6).png', label:"Danielemariotto"},
        {id: 8,  shape: 'circularImage', image: DIR + 'Davideguerrini/cropped(7).png', label:"Davideguerrini"},
        {id: 9,  shape: 'circularImage', image: DIR + 'Den_bdt/cropped(8).png', label:"Den_bdt"},
        {id: 10, shape: 'circularImage', image: DIR + 'Devinsays/cropped(9).png', label:"Devinsays"},

      ];

      var container = document.getElementById('mynetwork');
      var data = {
        nodes: nodes,
      };
      var options = {

        font:{
          size: 100,
        },
        physics: {
            barnesHut: {
              avoidOverlap: 1,
              centralGravity: 0.2,
            },
      },
      nodes: {
          size:40,
          color: {
            background: '#006400'
          },
          font:{color:'#eeeeee', "size": 30},

        },

      };
      network = new vis.Network(container, data, options);

    }
  </script>

</head>

<body onload="draw()">

<div id="mynetwork"></div>

</body>
</html>
```

## VisJs Edges between nodes

```html
<!doctype html>
<html>
<head>
  <title>Bhishan's Services | TheTaraNights</title>

  <style type="text/css">

    body {
      font: 10pt arial;
    }
    #mynetwork {
      width: 100%;
      height: 900px;
      border: 1px solid lightgray;
      background-color:#333333;
    }
  </style>

  <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/vis/4.21.0/vis.js"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/vis/4.21.0/vis-network.min.css" rel="stylesheet" type="text/css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
  <script type="text/javascript">

    var nodes = null;
    var edges = null;
    var network = null;

    // Called when the Visualization API is loaded.
    function draw() {
      // create people.
      var DIR = 'https://www.thetaranights.com/wp-content/uploads/2018/fiverr_reviews/';

      var ratings = '<span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><br>';
      nodes = [
        {id: 1, shape: 'circularImage', image: "https://avatars3.githubusercontent.com/u/6163574?s=400&u;=c396866f2c1ca2a709c1ece0d5a352e0f6e1a865&v;=4", label:"Bhishan", size:60},
        {id: 2,  shape: 'circularImage', image: DIR + 'Benf97/cropped(1).png', label:"Benf97" },
        {id: 3,  shape: 'circularImage', image: DIR + 'Bigyoyo55/cropped(2).png', label:"Bigyoyo55"},
        {id: 4,  shape: 'circularImage', image: DIR + 'Btpope22/cropped(3).png', label:"Btpope22"},
        {id: 5,  shape: 'circularImage', image: DIR + 'Chrisraven/cropped(4).png', label: "Chrisraven"},
        {id: 6,  shape: 'circularImage', image: DIR + 'Cnnsbs/cropped(5).png', label:"Cnnsbs"},
        {id: 7,  shape: 'circularImage', image: DIR + 'Danielemariotto/cropped(6).png', label:"Danielemariotto"},
        {id: 8,  shape: 'circularImage', image: DIR + 'Davideguerrini/cropped(7).png', label:"Davideguerrini"},
        {id: 9,  shape: 'circularImage', image: DIR + 'Den_bdt/cropped(8).png', label:"Den_bdt"},
        {id: 10, shape: 'circularImage', image: DIR + 'Devinsays/cropped(9).png', label:"Devinsays"},

      ];

      // create connections between people

      edges = [];
      for(var i =2; i<11; i++){
        edges.push({from: 1, to: i});
      }

      var container = document.getElementById('mynetwork');
      var data = {
        nodes: nodes,
        edges: edges
      };
      var options = {

        nodes: {
            size:40,
              color: {
              background: '#006400'
            },
            font:{color:'#eeeeee', "size": 10},
        },

      };
      network = new vis.Network(container, data, options);

    }
  </script>

</head>

<body onload="draw()">

<div id="mynetwork"></div>

</body>
</html>
```

## VisJS onclick() and onhover()

```html
<!doctype html>
<html>
<head>
  <title>Bhishan's Services | TheTaraNights</title>

  <style type="text/css">

    body {
      font: 10pt arial;
    }
    #mynetwork {
      width: 100%;
      height: 900px;
      border: 1px solid lightgray;
      background-color:#333333;
    }
  </style>

  <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/vis/4.21.0/vis.js"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/vis/4.21.0/vis-network.min.css" rel="stylesheet" type="text/css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
  <script type="text/javascript">
    // var DIR = 'https://www.thetaranights.com/wp-content/uploads/2018/fiverr_reviews/';

    var nodes = null;
    var edges = null;
    var network = null;

    // Called when the Visualization API is loaded.
    function draw() {
      // create people.
      var DIR = 'https://www.thetaranights.com/wp-content/uploads/2018/fiverr_reviews/';

      var ratings = '<span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><br>';
      nodes = [
        {id: 1, shape: 'circularImage', image: "https://avatars3.githubusercontent.com/u/6163574?s=400&u;=c396866f2c1ca2a709c1ece0d5a352e0f6e1a865&v;=4", label:"Bhishan", size:60},
        {id: 2,  shape: 'circularImage', image: DIR + 'Benf97/cropped(1).png', label:"Benf97", "title": ratings + 'This guy is honestly amazing! <br>I have never experienced such pure dedication and commitment to a task, despite struggles, and a final product of which is outstanding!'},
        {id: 3,  shape: 'circularImage', image: DIR + 'Bigyoyo55/cropped(2).png', label:"Bigyoyo55", "title": ratings + "Very good seller mastering python and script development ! <br>Thanks for all<br>" + ratings + "Very good python developper, and very patient !<br> Thanks for all"},
        {id: 4,  shape: 'circularImage', image: DIR + 'Btpope22/cropped(3).png', label:"Btpope22", "title": ratings + "I really needed help getting anaconda installed on my laptop - it wasn't working correctly.<br> He was available right away and quickly assessed then fixed my issue."},
        {id: 5,  shape: 'circularImage', image: DIR + 'Chrisraven/cropped(4).png', label: "Chrisraven", "title": ratings + "Outstanding Experience!"},
        {id: 6,  shape: 'circularImage', image: DIR + 'Cnnsbs/cropped(5).png', label:"Cnnsbs", "title": ratings + "Awesome!!! I want to ask for work next time. <br>He wokred exact what I want."},
        {id: 7,  shape: 'circularImage', image: DIR + 'Danielemariotto/cropped(6).png', label:"Danielemariotto", "title": ratings + "Outstanding Experience!<br>" + ratings + "Outstanding Experience!"},
        {id: 8,  shape: 'circularImage', image: DIR + 'Davideguerrini/cropped(7).png', label:"Davideguerrini", "title": ratings + "A really great experience.<br> Bhishan Is very quick and professional. <br>A pleasure to work with him!"},
        {id: 9,  shape: 'circularImage', image: DIR + 'Den_bdt/cropped(8).png', label:"Den_bdt", "title": ratings + "Outstanding Experience!"},
        {id: 10, shape: 'circularImage', image: DIR + 'Devinsays/cropped(9).png', label:"Devinsays", "title": ratings + "Bhishan built a script for us to get Facebook data through their API.<br> Delivered quickly, quality job.<br>" + ratings + "Bhishan got the Alexa rank for about 2,000 websites we had in a spreadsheet."},

      ];

      // create connections between people

      edges = [];
      for(var i =2; i<11; i++){
        edges.push({from: 1, to: i});
      }

      var container = document.getElementById('mynetwork');
      var data = {
        nodes: nodes,
        edges: edges
      };
      var options = {

        nodes: {
            size:40,
              color: {
              background: '#006400'
            },
            font:{color:'#eeeeee', "size": 10},
        },

      };
      network = new vis.Network(container, data, options);

      network.on("click", function (params) {
          params.event = "[original event]";

      });
      network.on("hoverNode", function (params) {
          params.event = "[original event]";

      });


    }
  </script>

</head>

<body onload="draw()">

<div id="mynetwork"></div>

</body>
</html>
```

## Putting it all together [Bhishan's Freelance Network]

```html
<!doctype html>
<html>
<head>
  <title>Bhishan's Services | TheTaraNights</title>

  <style type="text/css">
  .checked {
    color: orange;
},
    body {
      font: 10pt arial;
    }
    #mynetwork {
      width: 100%;
      height: 900px;
      border: 1px solid lightgray;
      background-color:#333333;
    }
  </style>

  <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/vis/4.21.0/vis.js"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/vis/4.21.0/vis-network.min.css" rel="stylesheet" type="text/css" />
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
  <script type="text/javascript">

    var nodes = null;
    var edges = null;
    var network = null;

    // Called when the Visualization API is loaded.
    function draw() {
      // create people.

      var DIR = 'https://www.thetaranights.com/wp-content/uploads/2018/fiverr_reviews/';


      var ratings = '<span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><span class="fa fa-star checked"></span><br>';
      nodes = [
        {id: 2,  shape: 'circularImage', image: DIR + 'Benf97/cropped(1).png', label:"Benf97", "title": ratings + 'This guy is honestly amazing! <br>I have never experienced such pure dedication and commitment to a task, despite struggles, and a final product of which is outstanding!'},
        {id: 3,  shape: 'circularImage', image: DIR + 'Bigyoyo55/cropped(2).png', label:"Bigyoyo55", "title": ratings + "Very good seller mastering python and script development ! <br>Thanks for all<br>" + ratings + "Very good python developper, and very patient !<br> Thanks for all"},
        {id: 4,  shape: 'circularImage', image: DIR + 'Btpope22/cropped(3).png', label:"Btpope22", "title": ratings + "I really needed help getting anaconda installed on my laptop - it wasn't working correctly.<br> He was available right away and quickly assessed then fixed my issue."},
        {id: 5,  shape: 'circularImage', image: DIR + 'Chrisraven/cropped(4).png', label: "Chrisraven", "title": ratings + "Outstanding Experience!"},
        {id: 6,  shape: 'circularImage', image: DIR + 'Cnnsbs/cropped(5).png', label:"Cnnsbs", "title": ratings + "Awesome!!! I want to ask for work next time. <br>He wokred exact what I want."},
        {id: 7,  shape: 'circularImage', image: DIR + 'Danielemariotto/cropped(6).png', label:"Danielemariotto", "title": ratings + "Outstanding Experience!<br>" + ratings + "Outstanding Experience!"},
        {id: 8,  shape: 'circularImage', image: DIR + 'Davideguerrini/cropped(7).png', label:"Davideguerrini", "title": ratings + "A really great experience.<br> Bhishan Is very quick and professional. <br>A pleasure to work with him!"},
        {id: 9,  shape: 'circularImage', image: DIR + 'Den_bdt/cropped(8).png', label:"Den_bdt", "title": ratings + "Outstanding Experience!"},
        {id: 10, shape: 'circularImage', image: DIR + 'Devinsays/cropped(9).png', label:"Devinsays", "title": ratings + "Bhishan built a script for us to get Facebook data through their API.<br> Delivered quickly, quality job.<br>" + ratings + "Bhishan got the Alexa rank for about 2,000 websites we had in a spreadsheet."},
        {id: 11, shape: 'circularImage', image: DIR + 'Dxbst18/cropped(10).png', label:"Dxbst18", "title": ratings + "Outstanding Experience!"},
        {id: 12, shape: 'circularImage', image: DIR + 'Elirosenblatt/cropped(11).png', label:"Elirosenblatt", "title": ratings + "Bhishan, as always, was quick to help troubleshoot and went beyond my expectations.<br> A++ highly recommended!<br>" + ratings + "bhishan did a FANTASTIC job. Great communication, and script worked perfectly.<br> Would for sure do business with him again!"},
        {id: 13, shape: 'circularImage', image: DIR + 'Everinearnest/cropped(12).png', label:"Everinearnest", "title": ratings + "Bhishan is a skilled troubleshooter who had a lot of patience with the unusual problems I was experiencing.<br> He is a very good communicator who explains the rationale for what he is doing, and is a pleasure to work with. <br>Highly recommended."},
        {id: 14, shape: 'circularImage', image: DIR + 'Faizallala/cropped(13).png', label:"Faizallala", "title": ratings + "Outstanding Experience!"},
        {id: 15, shape: 'circularImage', image: DIR + 'Geoman2222/cropped(14).png', label:"Geoman2222", "title": ratings + "SOOOO Fast He was Awsome<br>" + ratings + "Very Knowledgeable and concurred any task I gave him"},
        {id: 16, shape: 'circularImage', image: DIR + 'Gmorinan/cropped(15).png', label:"Gmorinan", "title": ratings + "Outstanding"},
        {id: 17,  shape: 'circularImage', image: DIR + 'Babasanfoor/cropped.png', label:"Babasanfoor", "title":ratings + "great experience<br>" + ratings + 'Excellent communication, excellent proficiency, will (and already have) order again.'},
        {id: 18,  shape: 'circularImage', image: DIR + 'Goaway77/cropped(16).png', label:"Goaway77", "title": ratings + "He solved the problem! Thanks a lot, A+++ coder! Highly recommended! "},
        {id: 19,  shape: 'circularImage', image: DIR + 'Hartlepool/cropped(17).png', label:"Hartlepool", "title": ratings + "Great job, very helpful"},
        {id: 20,  shape: 'circularImage', image: DIR + 'Hkinq1/cropped(18).png', label:"Hkinq1", "title": ratings + "Outstanding Experience!"},
        {id: 21,  shape: 'circularImage', image: DIR + 'Ikeqian/cropped(19).png', label:"Ikeqian", "title": ratings + "Pretty good. <br>Very fast and helpful!"},
        {id: 22,  shape: 'circularImage', image: DIR + 'Iveksl2/cropped(20).png', label:"Iveksl2", "title": ratings + "Outstanding Experience!<br>" + ratings + "bhishan wrote readable / concise code. <br>Did a good job"},
        {id: 23,  shape: 'circularImage', image: DIR + 'Jamoxie/cropped(21).png', label:"Jamoxie", "title": ratings + "Bhishan is the best coder that I've worked with: clear and thorough communication, well documented code, fast working and delivers before deadlines.<br> Also he exceeded my request with a feature that I didn't think could be added yet, so I could not be happier with this project. <br>Will be back soon!"},
        {id: 24,  shape: 'circularImage', image: DIR + 'Jeamer/cropped(22).png', label:"Jeamer", "title": ratings + "Outstanding Experience!"},
        {id: 25,  shape: 'circularImage', image: DIR + 'Jeffmactech/cropped(23).png', label:"Jeffmactech", "title": ratings + "Delivered EXACTLY what I needed! Thanks!"},
        {id: 26,  shape: 'circularImage', image: DIR + 'Jlondonuk/cropped(24).png', label:"Jlondonuk", "title": ratings + "Absolutely fantastic service! <br>Definitely buying again. I am so impressed!<br> Thank you Seller was kind, assuring, professional and completed it quickly!"},
        {id: 27,  shape: 'circularImage', image: DIR + 'Kiwibloke11/cropped(25).png', label:"Kiwibloke11", "title": ratings + "absolute legend. nailed it quickly, lots of communication will use again"},
        {id: 28,  shape: 'circularImage', image: DIR + 'Lauro1986/cropped(26).png', label:"Lauro1986", "title": ratings + "He was great. Project was delivered very quickly, very knowledgeable.<br> I recommend!"},
        {id: 29,  shape: 'circularImage', image: DIR + 'Manuel13/cropped(27).png', label:"Manuel13", "title": ratings + "Creative and outstanding work!<br> A real pro within this realm."},
        {id: 30,  shape: 'circularImage', image: DIR + 'Matheusroriz/cropped(28).png', label:"Matheusroriz", "title": ratings + "Awesome! He was very helpful with everything!<br> We will certainly do business again!"},
        {id: 31,  shape: 'circularImage', image: DIR + 'Michaelgrinberg/cropped(29).png', label:"Michaelgrinberg", "title": ratings + "I needed to parse a string using Python inside Zapier. <br>The final goal was to make sure that the utm parameters get passed on to the CRM.<br> Bhishan was able to deliver a script that works well, does the job.<br> He was ahead of time and very friendly. <br>I would recommend the gig and its owner to everybody who needs to do a similar job."},
        {id: 32,  shape: 'circularImage', image: DIR + 'Newenglandmedia/cropped(30).png', label:"Newenglandmedia", "title": ratings + "The code is very well written and works.<br> Bhishan parameterized the items that I was looking for in the script so that I could take it from here.<br> Look forward to working with him again. "},
        {id: 33,  shape: 'circularImage', image: DIR + 'Nielsdo/cropped(31).png', label:"Nielsdo", "title": ratings + "Perfect!"},
        {id: 34,  shape: 'circularImage', image: DIR + 'Nilsbor/cropped(32).png', label:"Nilsbor", "title": ratings + "Everything was good"},
        {id: 35,  shape: 'circularImage', image: DIR + 'Paulolpduarte/cropped(33).png', label:"Paulolpduarte", "title": ratings + "Excellent job! Recommended!"},
        {id: 36,  shape: 'circularImage', image: DIR + 'Picture93/cropped(34).png', label:"Picture93", "title": ratings + "Outstanding Experience!<br>" + ratings + "Outstanding Experience!"},
        {id: 37,  shape: 'circularImage', image: DIR + 'Qms123456/cropped(35).png', label:"Qms123456", "title": ratings + "Outstanding Experience!"},
        {id: 38,  shape: 'circularImage', image: DIR + 'Rayrock2014/cropped(36).png', label:"Rayrock2014", "title": ratings + "As usual excellent<br>" + ratings + "Brilliant<br>" + ratings + "Awesome engineer"},
        {id: 39,  shape: 'circularImage', image: DIR + 'Realrocker/cropped(37).png', label:"Realrocker", "title": ratings + "Once again great service...Thanks</br>" + ratings + "Great Seller! <br>Exactly as requested, will hire him again and again"},
        {id: 40,  shape: 'circularImage', image: DIR + 'Ritesh2407/cropped(38).png', label:"Ritesh2407", "title": ratings + "I have previously worked with Bishan and he is great Python Guy to go for. <br>Always recommended.<br>" + ratings + "Outstanding Experience!"},
        {id: 41,  shape: 'circularImage', image: DIR + 'Roverfanclub/cropped(39).png', label:"Roverfanclub", "title": ratings + "Bhishan provided an excellent programmatic solution in a very short turnaround. <br>Will definitely look to him for solutions to future programming problems. "},
        {id: 42,  shape: 'circularImage', image: DIR + 'Samartking/cropped(40).png', label:"Samartking", "title": ratings + "Great seller! Highly recommended!<br>" + ratings + "!!!!Outstanding experience!!!!! <br>This seller is a great person and his work is Outstanding! <br>I definitely buy again!"},
        {id: 43,  shape: 'circularImage', image: DIR + 'Shahaleelal/cropped(41).png', label:"Shahaleelal", "title": ratings + "outstanding"},
        {id: 44,  shape: 'circularImage', image: DIR + 'Somo_king/cropped(42).png', label:"Somo_king", "title": ratings + "he is a life saver. thank you"},
        {id: 45,  shape: 'circularImage', image: DIR + 'Stirling198/cropped(43).png', label:"Stirling198", "title": ratings + "Outstanding Experience!"},
        {id: 46,  shape: 'circularImage', image: DIR + 'Subzerom/cropped(44).png', label:"Subzerom", "title": ratings + "Excellent Seller! Highly Skilled, great communication, very fast"},
        {id: 47,  shape: 'circularImage', image: DIR + 'Topdrawersrq/cropped(45).png', label:"Topdrawersrq", "title": ratings + "Outstanding Experience!"},
        {id: 48,  shape: 'circularImage', image: DIR + 'Vatsaldesai/cropped(46).png', label:"Vatsaldesai", "title": ratings + "quick delivery..excellent work"},
        {id: 49,  shape: 'circularImage', image: DIR + 'Vivekgarg172/cropped(47).png', label:"Vivekgarg172", "title": ratings + "Outstanding Experience!<br>" + ratings + "Great experience! <br>Bishan is very responsive and quality of work is great."},
        {id: 50,  shape: 'circularImage', image: DIR + 'Vseomedia/cropped(48).png', label:"Vseomedia", "title": ratings + "Great. Several works with him.<br>" + ratings + "i'll contact him again.<br> Great job as i request it."},
        {id: 51,  shape: 'circularImage', image: DIR + 'Webm87/cropped(49).png', label:"Webm87", "title": ratings + "Outstanding Experience!"},
        {id: 52,  shape: 'circularImage', image: DIR + 'Xspdo2/cropped(50).png', label:"Xspdo2", "title": ratings + "Excellente!!<br>" + ratings + "Excellente!!<br>" + ratings + "Quality Work!! Excelente Dev!"},



      ];


      var container = document.getElementById('mynetwork');
      var data = {
        nodes: nodes,
      };
      var options = {
        interaction:{hover:true},
        font:{
          size: 100,
        },

        physics: {
          barnesHut: {
            avoidOverlap: 1,
            centralGravity: 0.2,
          },
          repulsion:{
            nodeDistance: 1000,
          },
        },

        nodes: {
          size:90,
            color: {
            background: '#006400'
          },
          font:{color:'#eeeeee', size: 30},
        },

      };
      network = new vis.Network(container, data, options);
      network.on("click", function (params) {
          params.event = "[original event]";
 
      });
      network.on("hoverNode", function (params) {
          params.event = "[original event]";

      });

      window.onresize = function() {network.fit();}

    }
  </script>

</head>

<body onload="draw()">

<div id="mynetwork"></div>

</body>
</html>
```

I post awesome content on programming tutorials and computer science in general. Subscribe to never miss an article and programming resources.