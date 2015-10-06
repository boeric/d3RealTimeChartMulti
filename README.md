## D3 Based Real Time Chart with Multiple Streams

The real time chart is a resuable Javascript component that accepts real time data. The purpose is to show the arrival time of real time events (or lack thereof), piped to the browser (for example via Websockets). Various attributes of each event can be articulated using **size**, **color** and **opacity** of the object generated for the event.   

The component allows multiple asynchronous data streams to be viewed, each in a horizontal band across the SVG. New data streams can be added dynamically (as they are discovered by the caller over time), simply by calling the `yDomain` method with the new array of data series names. The chart will automatically fit the new data series into the available space in the SVG. 

The chart's time domain is moving with the passage of time. That means that any data placed in the chart eventually will age out and leave the chart. Currently, the chart history is capped at five minutes (but can be changed by modifying the component).

In addition to the main chart, the component also manages a navigation window with a viewport (using d3.brush) that can moved and sized to view an arbitrary portion of the time series data. 

A nice future capability is to allow the caller to dynamically specify the **type of object** to be created for each data item. The infrastructure is in place to dynamically create different svg objects on the fly (using the **document.createElementNS()** function). However, given the current data binding mechanism (lacking a "key function"), the data is just "passing through" already created elements (not necessarily of the same type as what the is specified by the data). 

Another nice capability would be to use **d3 transitions** to create smooth horizontal scrolling, as opposed to the current 200ms leftward jump. Any idea how to implement this is appreciated.

View the component in action at bl.ocks.org [here](http://bl.ocks.org/boeric/6a83de20f780b42fadb9), 
Repos: GitHub Gist [here](https://gist.github.com/boeric/6a83de20f780b42fadb9), GitHub [here](https://github.com/boeric/d3RealTimeChartMulti)

The component is a derivative of [**D3 Based Real Time Chart**](http://bl.ocks.org/boeric/3b57a788a4b96e1af211/). 

The component adheres to the pattern described in [Towards Reusable Chart](http://bost.ocks.org/mike/chart/). 

Use the component like so:


```
// create the real time chart
var chart = realTimeChartMulti()
    .width(900)               // width in pixels of chart; mandatory
    .height(350)              // height in pixels of chart; mandatory
    .yDomain(["Category1"])   // initial categories/data streams (note array),  mandatory
    .title("Chart Title")     // optional
    .yTitle("Categories")     // optional
    .xTitle("Time")           // optional
    .border(true);            // optional

// invoke the chart
var chartDiv = d3.select("#viewDiv").append("div")
    .attr("id", "chartDiv")
    .call(chart);

// create data item
var obj = {
  time: new Date().getTime(), // mandatory
  category: "Category1",      // mandatory
  type: "rect",               // optional (defaults to circle)
  color: "red",               // optional (defaults to black)
  opacity: 0.8,               // optional (defaults to 1)
  size: 5,                    // optional (defaults to 6)
};

// send the data item to the chart
chart.datum(obj);  

// to dynamically add a data series, just call yDomain with a new array of data series names
// (of course, all data passed to the chart will need to reference one of these categories)
// the data series can dynamically be (re-)sorted in arbitrary order; the chart will update accordingly
chart.yDomain(["Category1", "Category2"]);  

```
