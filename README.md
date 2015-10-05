## D3 Based Real Time Chart with Multiple Inputs

The real time chart is a resuable Javascript component that accepts real time data. It's a derivative of **D3 Based Real Time Chart**. This chart allows multiple asynchronous data streams to be viewed, in horizontal bands (one for each stream). Each data item sent to the chart can be customized in terms of type of object (rectangle or circle), color, opacity or size. 

The chart's time domain is moving with the passage of time, which means that any data placed in the chart eventually will age out and leave the chart. In addition to the main chart, the component also manages a "focus" window with a viewport (d3.brush) that can moved and sized to view an arbitrary portion of the time series data. 

The component adheres to the pattern described in [Towards Reusable Chart](http://bost.ocks.org/mike/chart/). 

The following options are currently supported:

- **width** and **height** in pixels (Number)
- **border** (Boolean)
- **title**, **xTitle**, **yTitle** (String)
- **yDomain** contains an array of categories/data streams (Array of Strings)


Use the component like so:

```
// create the real time chart
var chart = realTimeChartMulti()
    .title("Chart Title")
    .yTitle("Categories")
    .xTitle("Time")
    .yDomain(["Category1"]) // initial y domain (note array)
    .border(true)
    .width(900)
    .height(350);

// invoke the chart
var chartDiv = d3.select("#viewDiv").append("div")
    .attr("id", "chartDiv")
    .call(chart);

// create data item
var obj = {
  time: new Date().getTime(), // mandatory
  category: "Category1",      // mandatory
  type: "circle",             // optional
  color: "black",             // optional
  opacity: 1,                 // optional
  size: 5,                    // optional
};

// send the data item to the chart
chart.datum(obj);  

// dynamically add data series, just call yDomain with new array
// (of course, all data passed to the chart need to reference one of these categories)
chart.yDomain(["Category1", "Category2"]);  

```
