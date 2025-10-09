# intro_to_data_vis_lab_5_leveragingAI
This is my 5th lab for the intro to data visualization class. In this class we are assigned to work with AI to create some visualizations given data.

Link to AI Chat: https://chatgpt.com/share/68e701b9-b6d4-800c-b626-c208c0f01049

(You, 10 points) data preparation. Generate the small and the large scale CSV.  
  
  I did the data preparation in R, the code will be attatched as a screenshot.
  
(You, 15 points) apply core vis principles to begin with a task and use one task only.
Start by choosing a question you’d like a visualization to answer. e.g., what is the country with the largest life-expectancy? or is there a correlation between life expectancy and region over the year? Is there outlier in the data?   

The task I would like a visualization to answer the question: How does GDP per Capita vary across regions?

(AI, 5 points) Ask AI to take the small data as an input and generate a chart and the code in D3 that can answer your question. 
(AI, 5 points) Ask AI to take the large data as an input and generate a chart and the code in D3 that can answer your question. 
(You, 10 points) Is AI aware of different data size and does it give you the same code/visualization style? 

I do think it is aware of the different size data. It suggested that for the larger data set, I could incorporate life expectancy as well but ultimately changed to just create two barcharts. It did give the same code and style for both.

(You, 15 points) After these generation step, 
practice critique AI-generated ideas and D3 code by listing the set of guidelines it should have followed, e.g., need legend when color is used; draw a different chart when you don't think the chart representation fits your task. 
I think these charts do pretty well. I did end up making some suggestions and fixes, I added in the button so that you can click to the small data set and the large. AI didn't create axis labels so I told AI in and made sure they were good and decided to color code the bars with a key so that it is easier to understand.
(AI, 5 points) ask AI to add items you think it does not do properly one-by-one. Please fix one at a time. 
These were all fixed one-by-one
(AI + You, 20 points) ask AI to fix one-by-one. When it cannot, please fix it yourself. In this assignment, you only need to fix one of the things AI cannot fix. 
The AI had some problems with spacing the legend as well as it kept pushing the graphs down the page making you have to scroll. Thus I took over and adjusted it. 
(AI, 5 points) ask AI to justify its design choices. 
When asking it to explain it's design choices this is what it said:
1. Margin-based Layout
const margin = { top: 40, right: 40, bottom: 100, left: 100 };
const innerWidth = width - margin.left - margin.right;
const innerHeight = height - margin.top - margin.bottom;


Why:

Using margins ensures that axes, labels, and bars do not get clipped by the edges of the SVG.

It separates the charting area (innerWidth/innerHeight) from space reserved for axes, labels, and legends.

This fixes the “bars off-screen” problem we had before and gives the chart a more professional, centered layout.

2. Separate Chart Group (g)
const chart = svg.append("g")
  .attr("transform", `translate(${margin.left},${margin.top})`);


Why:

Wrapping all bars and axes in a group allows us to apply the same translation once instead of adjusting every element individually.

Simplifies scaling and keeps the chart neatly inside the margins.

3. Scales
const x = d3.scaleBand().range([0, innerWidth]).padding(0.3);
const y = d3.scaleLinear().range([innerHeight, 0]);
Why:

Band scale for x-axis: Perfect for categorical data like “Region” because it automatically handles spacing between bars.

Linear scale for y-axis: Maps GDP values to pixel heights correctly. The inversion (innerHeight → 0) ensures taller GDPs appear higher on the chart.

Padding: Adds space between bars so they don’t stick together, improving readability.

4. Color Coding Bars
const color = d3.scaleOrdinal(d3.schemeTableau10);
.attr("fill", d => color(d.Region));
Why:

Each region gets a consistent color, making it easy to differentiate categories.

Chose Tableau10 because it’s visually distinct and colorblind-friendly, improving accessibility.

Using the same color scale in the legend ensures intuitive mapping between bars and labels.

5. Legend Placement
const legend = svg.append("g")
  .attr("transform", `translate(${margin.left}, ${height - 60})`);
Why:

Placing the legend below the chart avoids covering any bars (problem we had before).

Spacing items in rows (i % 4) ensures multiple regions fit neatly without overlapping.

Keeping it in SVG coordinates relative to the chart makes layout predictable and responsive to margin changes.

6. Axes

x-axis: tickFormat(() => "") removes redundant category labels since we have a legend.

y-axis: Standard numeric scale with .nice() rounds the maximum value for a cleaner appearance.

Axis labels: Added “Region” (x-axis) and “Average GDP per Capita (2020, US$)” (y-axis) for clarity.

Why: Simplifies the visual; no repeated text clutter under the bars, still readable.

7. Value Labels on Bars
.attr("y", d => y(d.GDP) - 5)
.text(d => d3.format(",.0f")(d.GDP));


Why:

Provides exact GDP numbers directly above bars without requiring a tooltip.

-5 offset ensures the text doesn’t overlap the bar.

Formatted with commas for readability.

8. Grouping and Transform for Legend Items
.attr("transform", (d, i) => `translate(${(i % 4) * 150}, ${Math.floor(i / 4) * 20})`);


Why:

Dynamically lays out legend items in rows of 4, avoiding overlap regardless of the number of regions.

Keeps chart responsive if you swap datasets (small vs. large CSV).
(You, 10 points), what did you learn through the process? Critically evaluate
This process showed me how difficult it can be to work with AI while still being fun. There were a lot of processes that AI made coding more efficient and less time consuming. Writing correct code all the time can be difficult and takes time, AI can take off some of the burden of writing code. However, we cannot fully rely on AI, artificial intelligence still doesn't know how to create incredible visualizations because it does not have it's own eyes. A lot of the choices it makes don't create a visually useful graphic. With the help from humans directing it, AI can be a very neat tool to utilize.
