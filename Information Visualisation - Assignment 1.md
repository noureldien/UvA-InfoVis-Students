# Information Visualisation - D3 Assignment

This first lab assignment for the Information Visualisation course consists of two parts. In the first part you will walk through the preparation and installation and then follow a step-by-step introduction to create your first visualisation using **D3.js**. For the second part of the assignment you will create your own, somewhat more complex, visualisation for which *the result will count towards your final grade*. Before you continue, please check the following information and deadlines.

|  Teaching Assistants  | Name | E-mail |
| ---------------------------- | --------- | ------------|
| Teaching Assistant (Group A) | Tom Runia | runia@uva.nl |
| Teaching Assistant (Group B) | Berkay Kicanaoglu | b.kicanaoglu@uva.nl |

|  Group  | Deadline | Due To | How |
| ---------------------------- | --------- | ------------| ------ |
| Group A | D3.js Assignment (this)     | Tuesday June, 7th 12:59 CET  | Upload to Blackboard.
| Group B | D3.js Assignment (this)     | Wednesday June, 8th 12:59 CET  | Upload to Blackboard.

## Before Starting

Before we start creating our first visualisation using D3.js, we recommend students to download a proper code editor that they are comfortable with for writing HTML, CSS and JavaScript. Some excellent choices we recommend include:

1. [SublimeText](http://www.sublimetext.com/3) (Windows, OS X, Linux)
2. [Atom](https://atom.io) (Windows, OS X, Linux)
3. [NotePad++](https://notepad-plus-plus.org) (Windows)

Students that choose SublimeText as their editor can have a look at the [package repository](https://packagecontrol.io/search/javascript), which contains some very useful plugins for writing JavaScript code. After installing your code editor, we also recommend setting up a *version control system* such as Git. This is optional, but highly recommended as you will later work together on the same code. Students unfamiliar with Git can walk through this [short introduction](https://try.github.io/) to Git.

For this course we only support Google Chrome as browser as it is easier to debug JavaScript. However, when using **Google Chrome** as browser you might encounter problems with loading local files from disk. By default Google Chrome does not allow reading local files from disk due to security reasons. Since we will use D3 to read datasets (e.g., CSV files) from disk we need to disable this security. We can start Chrome by passing the commandline parameter `--allow-file-access-from-files`. Please use the following commands for starting Chrome from the commandline, or have a look at the [documentation page](https://www.chromium.org/for-testers/command-line-flags). Before restarting Chrome with disabled web security, make absolutely sure that all Chrome processes are killed (check by opening your Task Manager `Ctrl+Alt+Del` on Windows).

```bash
chrome.exe --allow-file-access-from-files                                (for Windows)
open -a Google\ Chrome --args --allow-file-access-from-files             (for Mac OS X)
google-chrome --args --allow-file-access-from-files                      (for Linux)
```

For this and the next assigments you will write JavaScript code, and you will most likely run into problems at some point. A common problem is just a blank page when you are trying to view your visualisation in the browser. When this happens you will have to debug your JavaScript code, which can be a little tricky sometimes due to limited or vague error messages. We recommend having a look at this StackOverflow question on **[debugging Javascript](http://stackoverflow.com/questions/988363/how-can-i-debug-my-javascript-code)**. It will be useful to always have the *Chrome Developer Console* open by pressing `Ctrl+Shift+J` and add `console.log()` lines to your code to print intermediate results. Note that if you forget to start Chrome using the `--allow-file-access-from-files` parameter you will see an error similar to this one in the Developer Console.

```
d3.v3.min.js:1 XMLHttpRequest cannot load file:///.../cbs_companies_in_netherlands.csv. Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https, chrome-extension-resource.
```

### D3.js Visualisation Library

**D3.js** is a JavaScript library for producing interactive data visualisations in the browser. The library is built for being very flexible and allows for creating highly complex and visually pleasing graphs, bar charts and animated data visualisations. One of its primary advantages over other visualisation libraries is the possibility to use all the modern web techniques such as SVG, HTML5 and CSS.

The visualisations you will create all exist within the browser and are built using *scalable vector graphics* (SVG). D3.js uses JavaScript functions to select elements, create other elements, perform styling and add animations. In addition, users can easily add their own styling or more complex visualisations using CSS and JavaScript. Central principle when using D3.js is selection of elements from the **[Document Object Model](https://en.wikipedia.org/wiki/Document_Object_Model)** (DOM) on the page and the ability to modify them using operators similar to those used in [jQuery](https://jquery.com/). For example, by using D3.js we can easily select all the rectangles `<rect>` on a page, change their fill color to magenta and update the position:

```javascript
 d3.selectAll("rect")         // select all <p> elements
   .style("color", "magenta") // set style "color" to value "magenta"
   .attr("x", 100);           // set attribute "x" (horizontal position) to value 100px
   .attr("y", 0);             // set attribute "y" (vertical position) to value 0px
```

This example shows how we can select and modify elements based on the *type* (rectangle), but elements can also be selected based on their *class*, *identifier* or *attribute*. These selectors are by far the most common way in D3.js to manipulate elements on the page. We will now continue with a walkthrough in which you will create a simple bar chart.

## Walkthrough: Creating a Simple Bar Chart

In this walkthrough example we are going to create a simple bart chart displaying the total number of companies in The Netherlands. This data is publicly available by the Centraal Bureau voor Statistiek (CBS). If you are interested you can have a look at their webpage [Cijfers in Nederland](https://www.cbs.nl/nl-nl/cijfers). The bar chart you will create looks similar (although not exactly the same) as the following figure.

![Barchart](https://raw.githubusercontent.com/tomrunia/InformationVisualization/master/assets/assignment1-barchart.png?token=AFR5gcK4J5Td2blhpIcYUdRfrZQ3C90mks5XTwGhwA%3D%3D)

The first thing we need to do is setting up an empty HTML document which will hold all the code for our visualisation. If you are familiar with HTML then you will immediatly recognize all the components; if not, you might want to study all the elements in the document. Create an empty document with the name `barchart.html` and put the following code inside it. 

```html
<!DOCTYPE html>
<meta charset="utf-8">

<!-- The visual styling of our barchart is in this file -->
<link rel="stylesheet" href="barchart.css">

<!-- This is the main container to which we append all chart elements -->
<svg class="chart"></svg>

<!-- We include D3 from an external location so that we don't have
     to download it and we are sure we are using the latest version -->
<script src="https://d3js.org/d3.v3.min.js" charset="utf-8"></script>

<script>
  // Here we are going to create a barchart using D3
  // ...
</script>
```

As you can see in `Line 5` a CSS stylesheet is included. The file `barchart.css` contains all the visual styling for the chart you will create. In addition to the HTML file you just created, also create a file `barchart.css` in the same directory and then put the following style elements inside. Study the style attributes for each of the elements as you will use them later.

```css
@charset "UTF-8";

body {
  font: 10px sans-serif;
  padding: 40px 60px;
}

.axis path,
.axis line {
  fill: none;
  stroke: #666;
  stroke-width: 1px;
  shape-rendering: crispEdges;
}

.x.axis line {
  stroke: #000;
  stroke-width: 1.5px;
}

.x.axis .minor {
  stroke-opacity: .5;
}

.y.axis line,
.y.axis path {
  fill: none;
  stroke: #000;
}

.chart rect {
  fill: #2F81A6;
  opacity: 0.8
}

.chart rect:hover {
  opacity: 1.0;
}

.chart text {
  fill: #000;
  font: 10px sans-serif;
  text-anchor: middle;
}
```

If you open `barchart.html` in your browser at this point you will just see an blank page because we did not add any elements to the page. Now that we have a placeholder and styling for the chart, we continue by downloading the dataset that we will use for the visualisation. The dataset is available in [comma separated value](https://en.wikipedia.org/wiki/Comma-separated_values) (CSV) format from Blackboard under the name `cbs_companies_in_netherlands.csv`. Please download the dataset and put it in the same directory as the other two files you have created. Make sure that you download the CSV files as [raw text](https://raw.githubusercontent.com/tomrunia/UvA-InfoVis-Students/master/datasets/cbs_companies_in_netherlands.csv). As you study the contents of the dataset file, you will see that it contains the number of companies in The Netherlands over the period 2012 to 2016. For this simple walkthrough we will only use the total number of companies given in the second column (`Alle`). You will use the other columns denoting the number of companies per category later in this assignment.

We are now going to set up the bar chart, *e.g.* the dimensions, location, axes of the figure. This is the first D3.js code that we will add to the document. Please include the following initialization code inside the empty `<script></script>` tags of `barchart.html`. Also study the code and the comments carefully as this defines the main appearance of our bar chart. In particular, note how we define variables for the dimensions of the graph (`widht`, `height`) to reuse them later for dynamically modifying the chart. Also note the scalings for the axes, for different datasets you might need to change the scalings accordingly. 

```javascript
// Define the size of the visualisation
var margin   = {top: 20, right: 20, bottom: 100, left: 60};
var width    = 800-margin.left-margin.right;
var height   = 500-margin.top-margin.bottom;

// Scaling on x-axis is 'ordinal' indicating we show categories
// Other scales are d3.time.scale() and d3.scale.linear()
// Notice that the time scale might actually be more suited here.
var x = d3.scale.ordinal()
    .rangeRoundBands([0, width]);

// Scaling on y-axis is 'linear'
var y = d3.scale.linear()
    .rangeRound([height, 0], 1.0);

// Set the scaling and orientation of the x-axis
var xAxis = d3.svg.axis()
    .scale(x)
    .orient("bottom");

// Set the scaling, ticks and orientation of the y-axis
var yAxis = d3.svg.axis()
    .scale(y)
    .orient("left")
    .ticks(10)
    .tickFormat(d3.format(".3s"));

// Initialize the barchart and the set position/spacings
var chart = d3.select(".chart")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")")
    .style("padding", "60px")
```

Although we have setup the bar chart, there is still nothing to show if you view the file in your browser. Next, we are going to load the dataset and complete the bar chart based on the data. Since our data is formatted as CSV, we can use the convenient `d3.csv` reader to load it. Below the code you have just added, paste and study the following code:

```javascript
// Here we define the path to the .csv file we are going to load.
// Note that this assumes that the dataset file is in the same directory as barchart.html
var csv_file   = "./cbs_companies_in_netherlands.csv";
d3.csv(csv_file, function(error, data) {
 
    // Everything that uses the data will be inside this callback function.
    // Within this function, you can use the variable 'data' to access the contents of the CSV file.
    // More information on loading data files: https://github.com/d3/d3/wiki/CSV
 
});
```

Now that we have loaded the dataset we can use it to append components to the bar chart based on the data. The following large chunk of code will format the data, map the data to the x/y-axes and add the bars with appropiate height based on the data. Please paste the code inside the callback function you have just added and closely study the comments inline. This is also the final piece of code we need, everything that is neccessary for our bar chart is here, so make sure you understand what is going on and view the chart in the browser.

```javascript
  // Check if an error occured while loading the data
  if (error) throw error;

  // Data formatting. Here we select the columns from the CSV file that we are
  // going to use. For our simple bar chart we only use the 'periode' and 'alle'
  // indicating the total number of companies in The Netherlands. 
  data.forEach(function(d) {
    d.time   =  d["periode"] // read as string
    d.value  = +d["alle"]    // the plus (+) indicates converting to a number
  });

  // Define the width of each bar based on the number of data entries
  var barWidth = width / data.length

  // Data mapping to the x-axis. 
  x.domain(data.map(function(d) { return d.time; }));

  // Data mapping to the y-axis. Note how the axis spans from [0,max(data)] dynamically
  y.domain([0, d3.max(data, function(d) { return d.value; })]);

  // Format the chart such that the width matches the number of data entries
  chart.attr("width", width);

  // Add the histogram bars and set the x-location based on the data
  // Note "g" is a container for grouping other SVG elements
  // Read more on this page: https://goo.gl/KQWB8m
  var bar = chart.selectAll("g")
      .data(data)
    .enter().append("g")
      .attr("transform", function(d, i) { 
        // The x-location of the bar is defined in terms of the
        // number of the bar and the width of the bars
        return "translate(" + (i*barWidth) + ", 0)"; 
      });

  // Here we set the width, height and y-location of the bars
  bar.append("rect")
    .attr("y", function(d) { return y(d.value) })
    .attr("height", function(d) { return height-y(d.value) })
    .attr("width", barWidth - 1);

  // Add text inside the bars to display the number of companies
  bar.append("text")
    .attr("x", barWidth / 2)
    .attr("y", function(d) { return y(d.value) + 10; })
    .attr("dy", ".5em")
    .text(function(d) { return d.value; });

  // Show the x-axis by calling xAxis
  chart.append("g")
    .attr("class", "x axis")
    .attr("transform", "translate(0," + height + ")")
    .call(xAxis);

  // Show the y-axis by calling yAxis
  chart.append("g")
    .attr("class", "y axis")
    .attr("transform", "translate(0, 0)")
    .call(yAxis);

  // Select the labels on the x-axis and rotate them 45 degrees.
  // This is not neccessary but makes the chart easier to read.
  // The numbers -3.0 and 2.8 are somewhat arbitrary to 
  // nicely position the labels.
  chart.selectAll(".x text") 
         .attr("transform", function(d) {
             return "translate(" + (-3.0*this.getBBox().height) + "," + 
                                   ( 2.8*this.getBBox().height) + ")" +
                                   "rotate(-45)"
        });

  // Add axis labels to the chart (Note: these are not built-in to D3)
  // Label x-axis
  chart.append("text")
    .attr("x", width/2.0)
    .attr("y", height+100)
    .style("font-weight", "bold")
    .style("font-size", "14px")
    .text("Periode");

  // Label y-axis
  chart.append("text")
    .attr("text-anchor", "end")
    .attr("y", -60)
    .attr("x", -height/2.0)
    .attr("dy", ".75em")
    .attr("transform", "rotate(-90)")
    .style("font-weight", "bold")
    .style("font-size", "14px")
    .text("Total Number of Companies")


  // Add a title to the chart (Note: these are not built-in to D3)
  chart.append("text")
    .attr("x", (width/2.0))
    .attr("y", -20)
    .style("font-weight", "bold")
    .style("font-size", 18)
    .text("Number of Companies in the Netherlands");
```

We have reached the end of the walkthrough. By now you should have created your first bar chart and understand the basics of D3. Most of the things that we have seen in this walkthrough you will also need for the first assignment. You can find the full code of `barchart.html` on this [page](https://gist.github.com/tomrunia/0bb8bfb84490dfbf9293092034086777) in case something went wrong. Remember to check the Chrome developer console by pressing `Ctrl+Shift+J` if you run into problems: the errors and warnings there will probably point you in the right direction for debugging. This walkthrough was just to get you started using D3.js. You can now continue with the first assignment.

## Assignment Part I: Stacked Bar Chart

In this assignment, you are expected to create another simple chart, namely, stacked bar chart. Stacked bar charts are useful when visualising multiple categories of your data simultaneosly. The following figure shows how your stacked bar chart should look like. 

![StackedBarChart](https://raw.githubusercontent.com/tomrunia/InformationVisualization/master/assets/StackedBarChart.png?token=AMJKHwbnBSCKiuN919pOdjYRhV_QcPPAks5XVYIuwA%3D%3D)

In this assignment, you are going to work on the following CBS dataset regarding the number of companies in the Netherlands:
`cbs_companies_in_netherlands.csv` - which can be found under the `/datasets` directory.

Similar to the walkthrough example, you should first define your visual layout in a file called `stackedbarchart.css`. For this part, you can actually use the `.css` file we have provided you with in the walkthrough example. If you have grasped the idea of D3.js in the walkthrough, you will see that this one is very similar to a regular bar chart in implementation. In case you feel lost, we recommend you to check out examples on this [page](https://github.com/d3/d3/wiki/Gallery).

**Hint-1:** In order to make your data speak for itself, you may want to consider picking only a subset of dimensions and discard the rest. This will basically avoid outputting too much information at the same time and hence your visualisation will make more sense to the viewer (and to you). 

**Hint-2:** By visualising multiple subsets of dimensions, you can gain more insight in the data at hand. Therefore, you may want to create multiple `.csv` files in which only a selected subset of columns (*i.e.*, dimensions) exists and run your `stackedbarchart.html`, by specifying the desired `.csv` file, on your browser.

**Requirements:** The assignment is complete when it has met the following requirements:

1. It is written completely in D3.js
2. It loads and uses the data from the provided CSV file
3. Each period is represented by one stacked bar
4. Period labels appear below the bars
5. The size of each bar is computed dynamically using D3.js 
6. Chart contains dynamically computed y-axis on the left
7. Chart contains dynamically computed x-axis on the bottom

## Assignment Part II: Operating Systems Usage

In the final part of the assignment you will create a visualisation for a small dataset containing statistics on Operating System usage. There are two subsets of data you can choose, one contains numbers on *desktop* operating systems and one contains numbers on *mobile* operating systems. **Please choose one of the datasets and clearly mention which one you use**. Both datasets are publicly available from [NetMarketShare.com](https://www.netmarketshare.com/), but you can download them from Blackboard:

1. Desktop Operating Systems, april 2016: `operating_systems_desktop_april2016.csv`
2. Mobile Operating Systems, april 2016: `operating_systems_mobile_april2016.csv`

You are free to choose the type of visualisation for displaying the data. Pay a close attention to the lecture slides as a pointer to the kind of visualisations you can choose from. 

**Requirements:** The assignment is complete when it has met the following requirements:

1. It is written completely in D3.js
2. It loads and uses the data from one of the provided CSV files
3. Contains proper labels where necessary
4. Presents the data in a visually appealing and useful way

## End of the Assignment

The assignment is handed in through Blackboard: **do not send the deliverables per e-mail, they will get lost.** The deadline for this assignment can be found in the table at the top of this document. When your assignment contains multiple files, please make and upload a ZIP archive file to Blackboard. Your deliverables should include at least the following:

1. A short description of the visualization technique
2. D3.js code used to produce the visualization
3. Visualization result

We should be able to open your visualisation with a single click, *i.e.* open the HTML file in the browser (e.g. dataset file should be in place). 

## References and Further Reading

1. https://github.com/d3/d3/wiki/Gallery
2. http://michaelcrump.net/getting-sublime-3-to-launch-your-html-page-in-a-browser-with-a-key-combo/
3. Introduction to D3.js ([YouTube Video](https://www.youtube.com/watch?v=8jvoTV54nXw))
