---
layout: post
published: true
title: "Up-Close With Angular: Columnizing items in an ngRepeat"
mathjax: false
featured: true
comments: false
headline: "Making Angular easier for masses"
categories: 
tags: angular javascript
---

On one of my current projects, I recently ran into the following problem in Angular:

I have an array of data that I need to inject into the DOM in two columns, very similar to the way AirBnB does here:

I have a couple of constraints:

I don’t know how many items there will be; it could vary.
I don’t want all the items in the array to display on load, just the first 5.
I do need to be able to display the whole list upon clicking +More, so my solution will need to work for both limited and full view:


The easiest way to solve this problem is by creating an ng-repeat of the array, and filtering the view to show only those that are even or odd in each column.

Let’s say the data that we want to display is the following array of objects:

$scope.amenities =  [{value: ‘Kitchen’}, 
{value: ‘Internet’},
{value: ‘TV’},
{value: ‘Essentials’},
{value: ‘Shampoo’},
{value: ‘Heating’},
{value: ‘Air Conditioning’},
{value: ‘Breakfast’}]

Within our HTML view, the easiest way to columnize this data would be:


<table>
<tr>
<td ng-repeat=”amenity in amenities | limitTo: 3”>
<div ng-if=”$even”>{{amenity.value}}</div>
</td> 
<td ng-repeat=”amenity in amenities | limitTo: 2”>
<div ng-if=”$odd”> {{amenity.value}} </div>
</td>





