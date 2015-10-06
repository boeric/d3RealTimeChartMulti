## D3 Based Real Time Chart with Multiple Inputs

The real time chart is a resuable Javascript component that accepts real time data. It's a derivative of **D3 Based Real Time Chart**. This chart allows multiple asynchronous data streams to be viewed, in horizontal bands (one for each stream). Each data item sent to the chart can be customized in terms of color, opacity or size. Dynamic injection of type SVG object (rectangle or circle) may be added in the future (will require the use of a data key function)

The chart's time domain is moving with the passage of time, which means that any data placed in the chart eventually will age out and leave the chart. In addition to the main chart, the component also manages a navigation window with a viewport (d3.brush) that can moved and sized to view an arbitrary portion of the time series data. 

A nice future capability is to allow the caller to dynamically specify the type of object to be created for each data item. The infrastructure is in place for such feature to dynamically create different svg object on the fly (using the **document.createElementNS()** function), but given the current data binding mechanism (without a key function), the data is just "passing through" already created elements (not necessarily of the same type as what the is specified by the data). 

Another nice capability would be to use **d3 transitions** to create smooth horizontal scrolling, as opposed to the current 200ms leftward jump. Any idea how to implement this is appreciated.

The component adheres to the pattern described in [Towards Reusable Chart](http://bost.ocks.org/mike/chart/). 

The following options are currently supported:

- **width** and **height** in pixels (Number)
- **yDomain** contains an array of categories/data streams (Array of Strings)
- **title**, **xTitle**, **yTitle** (String)
- **border** (Boolean)

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
