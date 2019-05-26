---
title: S0014 - Building a roadways graph from OpenStreetMap data
author: Nick Larsen
categories: streams
date: 2019-02-14 13:48:31
youtube_url: https://www.youtube.com/watch?v=arbcioDjbQs
youtube_embed: https://www.youtube.com/embed/arbcioDjbQs
---


Today we extracted a graph of roadways from the [OpenStreetMap data dump](https://wiki.openstreetmap.org/wiki/Planet.osm#Country_and_area_extracts) for the city of [Lima, Peru](https://www.google.com/maps/place/Lima,+Peru/@-12.0262676,-77.1278649,11z/data=!3m1!4b1!4m5!3m4!1s0x9105c5f619ee3ec7:0x14206cb9cc452e4a!8m2!3d-12.0463731!4d-77.042754).  Why Lima?  It's fairly large but not whole world so we can ramp up the problem size.  This process turned out to be pretty straightforward and after cleaning up a few minor mistakes we got the problem running.

First we tried IDA* and that turned out to be really slow to make progress because there are a lot of very minor improvements in the distance with this larger dataset.  We'll have to consider some alternatives to that in the future but in order to keep making progress today we just switched over to vanilla A*.  There are roughly 800,000 edges in this final graph so A* shouldn't take very long even if it has to explore a large number of them.  For the heuristic we used [Haversine formula](https://www.movable-type.co.uk/scripts/latlong.html) to calculate straight line distance on the surface of a sphere.

Guess what? IT WORKED!  And we plotted the route on Google Maps!  This felt like such an accomplishment and it was really incredible, so thanks to everyone who joined me on this journey so far.  &lt;3