# Information Visualisation - D3.js Project

In this group project you will use D3.js to visualize a complex dataset. Below you
will find multiple datasets that you can use for the project. In addition to the datasets that we provide, we encourage you to combine them with other sources of data to create more insightful and information rich visualizations. Some points that will help to get you started:

1. Discuss with the group which of the 7 datasets you want to use for the project. Once you have made a decision, let the teaching assistant know of your choice. *Note that at most two groups can use the same dataset. First come, first serve.*

2. Remember that you can find the course schedule on Blackboard (slides first practical session). This week you will explore the datasets, think about what you want to do with the data, look for additional data sources, create a plan with the group and start working on the code. Next week you will hand in your mid-term report and give a presentation for all students in the group to receive feedback that will help you in improving the final result. For the presentation next week we expect you to have some preliminary visualizations to show. The presentations should be **7 minutes** at maximum with **3 minutes** available for questions. The mid-term report and slides should be uploaded to Blackboard. Please make sure to bring a USB stick with the slides on the day of the presentation, including a backup as PDF in case Powerpoint is not working properly (*e.g.* transitions, images or fonts).

3. The [D3.js example gallery ](https://github.com/d3/d3/wiki/Gallery) might serve as inspiration for what is possible in D3. Of course, you are free to use other sources of inspiration as well. Remember that D3.js and JavaScript are excellent for creating interactive visualizations. If you feel that you need more basic D3.js knowledge, have a look at the Dashing D3.js tutorial posted on Blackboard.

# Datasets

### 1. Events in Amsterdam
The marketing department of the Gemeente Amsterdam maintains an up-to-date list of many events, stores, festivals, museums and other leisure activities in the city. For this project you are free to choose a number of datasets from the website [data.amsterdam.nl](http://data.amsterdam.nl/publisher/amsterdam-marketing), and combine them to create insightful visualizations on the activities in and around Amsterdam. Many of the datasets are updated every day and thus also contain the activities for the coming summer months.

- **Data Source**: Gemeente Amsterdam
- **Data Format**: CSV or JSON
- **External URL**: http://data.amsterdam.nl/publisher/amsterdam-marketing

### 2. Flight Tracker
The OpenFlights organization provides datasets on flights around the world. For this project you could study the flight behavior around the world, the busiest airports and the airline companies. Also, you will hopefully gain insight on the popular destinations by visualizing the frequency of scheduled flights. The website contains the data files and their documentation which should help to get you started.

- **Data Source:** OpenFlights.org
- **Data Format:** Multiple CSV files (as .dat file)
- **External URL:** http://openflights.org/data.html

### 3. United Nations Statistics
This website offers a variety of datasets concerning demographic relations in countries. For inspiration, you might want to have a look the TED video *The Best Stats You've Ever Seen* by Hans Rosling in which he shows how social indicators, wealth and population changes over time. The United Nations website contains lots of publicly available datasets which allows for a great number of possibilities.

- **Data Source:** United Nations
- **Data Format:** multiple CSV files
- **External URL:** http://unstats.un.org/unsd/Demographic/products/default.htm

### 4. Weapons Trade
This dataset contains a timeline of arms transfers which might exhibit interesting correlations with times of wars. This dataset present (recipient/supplier) countries and amount of weaponry in deals. Also, you might want to opt for the types of weapons (this option is available on the page where you download the dataset) so that you can provide insight which is less likely heard of. To download the dataset, go to the SIPRI website using the link below and click `Generate importer/exporter TIV tables`. On the page that opens you can generate the data that you want to download, *e.g.* the range of years to cover.

- **Data Source:** Stockholm International Peace Institute
- **Data Format:** multiple CSV files
- **External URL:** http://www.sipri.org/databases/armstransfers/armstransfers

### 5. CBS Neighborhood Statistics
This dataset includes neighborhood statistics of cites and neighborhoods in The Netherlands. Governments are actively seeking for this type of insights inside vast amount of structured data since it offers ways to analyze their strengths and weaknesses across regions.

- **Data Source:** Centraal Bureau voor Statistiek
- **Data Format:** Excel
- **External URL:** https://www.cbs.nl/nl-nl/dossier/nederland-regionaal/wijk-en-buurtstatistieken

### 6. Bike Sharing Dataset
The Washington bike sharing dataset is a machine learning dataset in which many correlations can be found between the hourly and daily bike rentals and attributes such as the weather condition and the day of the week. For this dataset the focus in on exploring the factors that contribute to more bike rentals. Since the number of columns in the dataset is limited you might find it useful to combine it with other sources of data or study mobility in a broader sense.

- **Data Source**: Capital Bikeshare (Washington D.C.)
- **Data Format**: CSV or JSON
- **External URL**:  https://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset

### 7. Languages in the World
With this dataset you will be able to visualize the birthplaces of many living languages and the families they originate from. It could be very interesting to see how geography determines the communication medium in which people interact with each other. The dataset contains longitude and latitude as well as language family and genus. The dataset can be download from the link below and then clicking on the small gray box with the link to [wals.info](http://wals.info/)

- **Data source**: The World Atlas of Language Structures (WALS)
- **Data Format**: CSV
- **External URL**: http://www.puffpuffproject.com/languages.html


# Tips and Recommendations

### Use of other resources
If you feel that you need other resources, you can use the websites from which the data originates. For example, if you think that the GDP depends on population, you may need the
data on population of each country. Additionally, you can use other sources, such as [Google
Trends](https://www.google.com/trends/) to get regional data related to what people search for in The Netherlands and Europe. We encourage students to explore and combine other data as creativity and information rich visualization will positively contribute towards your final grade.

### Mix and Match

You are free to take different quantities from these datasets in order to investigate their correlation. For example, is the GDP growth of European countries related to unemployment?

### Python is your friend

While D3.js is great for building visually appealing and interactive figures, sometimes it can be hard to read the datasets in a convenient way. If you are more familiar with Python, you could inspect, prepare and pre-process the data using `NumPy`, `Pandas` or `SciPy`. The code snippet below could help to get you started reading a processing a TSV file (*tab seperated value*).

__Read a `.tsv` file in Python__
```python
import csv
with open("source_file.tsv") as tsvfile:
    tsvreader = csv.reader(tsvfile, delimiter="\t")
    for line in tsvreader:
        print(line)
        ''' Do some preprocessing such as:
            - handle missing data
            - correct the data
            - etc.
        '''
```

For reading Excel spreadsheet files (.xls)  in Python you could use the `xlrd` package which can be downloaded from [pypi.python.org](https://pypi.python.org/pypi/xlrd). After reading and processing the data, the `json` package can be used for exporting the data to JSON file which is suitable for reading in D3.js. Make sure to look at the original dataset table, to be sure that you are loading data correctly.

__Write to a `.json` file in Python__
```python
import json
with open("target_file.json", "w") as outfile:
    json.dump(data, outfile)
```

# Other Data Sources

1. EU Open Data Portal http://open-data.europa.eu/en/data/
2. Rijksoverheid Data Portal https://data.overheid.nl/
3. Amsterdam Open Data Portal http://data.amsterdam.nl/
4. NYC Open Data Portal https://data.cityofnewyork.us/
