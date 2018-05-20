#### Belly Button Biodiversity app
#### Flask API

A Flask API was used to present the given dataset and serve my HTML/CSS/JS files required for this dashboard landing page. A sqlite database was included, and a bootstrap grid system was utilized.

* The following routes have been created and these endpoints were used to plot the plotly charts. 

```python
@app.route("/")
    """Return the dashboard homepage."""
```
```python
@app.route('/names')
    """List of sample names.

    Returns a list of sample names in the format
    [
        "BB_940",
        "BB_941",
        "BB_943",
        "BB_944",
        "BB_945",
        "BB_946",
        "BB_947",
        ...
    ]

    """
```
```python
@app.route('/otu')
    """List of OTU descriptions.

    Returns a list of OTU descriptions in the following format

    [
        "Archaea;Euryarchaeota;Halobacteria;Halobacteriales;Halobacteriaceae;Halococcus",
        "Archaea;Euryarchaeota;Halobacteria;Halobacteriales;Halobacteriaceae;Halococcus",
        "Bacteria",
        "Bacteria",
        "Bacteria",
        ...
    ]
    """
```
```python
@app.route('/metadata/<sample>')
    """MetaData for a given sample.

    Args: Sample in the format: `BB_940`

    Returns a json dictionary of sample metadata in the format

    {
        AGE: 24,
        BBTYPE: "I",
        ETHNICITY: "Caucasian",
        GENDER: "F",
        LOCATION: "Beaufort/NC",
        SAMPLEID: 940
    }
    """
```
```python
@app.route('/wfreq/<sample>')
    """Weekly Washing Frequency as a number.

    Args: Sample in the format: `BB_940`

    Returns an integer value for the weekly washing frequency `WFREQ`
    """
```
```python
@app.route('/samples/<sample>')
    """OTU IDs and Sample Values for a given sample.

    Sort your Pandas DataFrame (OTU ID and Sample Value)
    in Descending Order by Sample Value

    Return a list of dictionaries containing sorted lists  for `otu_ids`
    and `sample_values`

    [
        {
            otu_ids: [
                1166,
                2858,
                481,
                ...
            ],
            sample_values: [
                163,
                126,
                113,
                ...
            ]
        }
    ]
    """
```

---
#### Plotly.js

Plotly.js was used to build interactive charts for this dashboard. The endpoints created pulled data through JS callbacks. Plotly uses D3 components which the bubble, pie, and gauge plots were created with. 

* Used the route `/names` to populate a dropdown select element with the list of sample names.

  - [x] use `document.getElementById`, `document.createElement` and `append` to populate the create option elements and append them to the dropdown selector.

  - [x] use the following HTML tag for the dropdown selector

  ```html
  <select id="selDataset" onchange="optionChanged(this.value)"></select>
  ```
  - [x] create a function called `optionChanged` to handle the change event when a new sample is selected (i.e. fetch data for the newly selected sample)

* Created a PIE chart that uses data from your routes `/samples/<sample>` and `/otu` to display the top 10 samples.

  - [x] used the Sample Value as the values for the PIE chart

  - [x] used the OTU ID as the labels for the pie chart

  - [x] used the OTU Description as the hovertext for the chart

  - [x] used `Plotly.restyle` to update the chart whenever a new sample is selected


* Created a Bubble Chart that uses data from your routes `/samples/<sample>` and `/otu` to plot the __Sample Value__ vs the __OTU ID__ for the selected sample.

  - [x] used the OTU IDs for the x values

  - [x] used the Sample Values for the y values

  - [x] used the Sample Values for the marker size

  - [x] used the OTU IDs for the marker colors

  - [x] used the OTU Description Data for the text values

  - [x] used `Plotly.restyle` to update the chart whenever a new sample is selected

* Display the sample metadata from the route `/metadata/<sample>`

  - [x] display each key/value pair from the metadata JSON object somewhere on the page

  - [x] update the metadata for each sample that is selected

* Made updates to the dashboard index page to take in some styles. Used the class '''container-fluid''' to adjust the chart window sizes to fit a smaller screen. 

- [ ] WIP: I started to deploy this Flask app to Heroku which is why I included a Procfile, but did not complete this task yet.

---
#### optional gauge chart

* a gauge Chart was adapted from [https://plot.ly/javascript/gauge-charts/](https://plot.ly/javascript/gauge-charts/) to plot the Weekly Washing Frequency obtained from the route `/wfreq/<sample>`

* gauge code was updated to account for values ranging from 0 - 9.

* used `Plotly.restyle` to update the chart when a new sample is selected

#### heroku port set-up

Steps for heroku + flask environment

1. `app.run(debug=True, port=33507)` which is a reserved port for Flask apps or you can add the port to the heroku ENV with `heroku config:add PORT=33507`

2. An alternate option is using `port = int(os.environ.get('PORT', 33507))`. This gets the value of PORT from the environment if it is set, otherwise it will use 33507. 