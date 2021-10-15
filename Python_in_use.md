# Python in Use

Now we have the basics of Python down lets put it to use with a quick real world example.

In this example we will use Python to scrape the disclosed meetings of [EU Commission president Ursula von der Leyen](http://ec.europa.eu/transparencyinitiative/meetings/meeting.do?host=c8e208ad-7dc2-4a97-acc9-859463c69ec4&d-6679426-p=1). We will then have this data to analyse and plot. 

## Importing modules

In the last tutorial we briefly touched on modules and importing them. In this tutorial we will be using some very popular and powerful modules. Lets start by importing `pandas` and `time`. 


```python
import time
import pandas as pd
```

Time is a Python package that lets you set timers in your code. We will be using it to make sure our scraper doesn't query the EU site too fast.

Pandas is a great module for working with data. It gives you the ability to create DataFrames which you can then easily analyse.

It also comes with the great feature `read_html` which will read data from an html table and out put a dataframe. This makes scrapping the EU meetings site as easy as running one line of code.

To start lets define a variable `url_start` that we can scrape.


```python
url_start = 'http://ec.europa.eu/transparencyinitiative/meetings/meeting.do?host=c8e208ad-7dc2-4a97-acc9-859463c69ec4&d-6679426-p=1'
```

Now we call the `read_html` function from the pandas module.


```python
df = pd.read_html(url_start)[0] # For some reason read_html returns the dataframe as the only item in a list so we use [0] to get the dataframe out of the list 

```


```python
df
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Location</th>
      <th>Entity/ies met</th>
      <th>Subject(s)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>03/09/2021</td>
      <td>Evian, France</td>
      <td>Allianz SE (Allianz Group)</td>
      <td>Meeting with CEO of Allianz</td>
    </tr>
    <tr>
      <th>1</th>
      <td>29/08/2021</td>
      <td>Brussels</td>
      <td>European Round Table for Industry (ERT)</td>
      <td>Dinner/ meeting with the ERT members on green ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>25/08/2021</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>19/07/2021</td>
      <td>Brussels</td>
      <td>Bill &amp; Melinda Gates Foundation (BMGF)</td>
      <td>Meeting with Co-chairman and co-founder of the...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>09/07/2021</td>
      <td>Brussels</td>
      <td>Global Citizen</td>
      <td>Meeting with the CEO from Global Citizen</td>
    </tr>
    <tr>
      <th>5</th>
      <td>24/04/2021</td>
      <td>Brussels</td>
      <td>Potsdam-Institut für Klimafolgenforschung (PIK)</td>
      <td>Meeting with Founding Director of the Potsdam ...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>29/03/2021</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>19/03/2021</td>
      <td>Videoconference</td>
      <td>Bundesverband der Deutschen Industrie e.V. (BDI)</td>
      <td>Meeting with BDI President</td>
    </tr>
    <tr>
      <th>8</th>
      <td>20/02/2021</td>
      <td>Videoconference</td>
      <td>Global Citizen</td>
      <td>Meeting with the CEO from Global Citizen</td>
    </tr>
    <tr>
      <th>9</th>
      <td>19/02/2021</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Meeting with Chairman of Volvo, Chairman of Si...</td>
    </tr>
  </tbody>
</table>


And ta da! We have the first sheet of meetings. But to analyse all of the president meetings we need to pull the data from all the pages and combine them. First lets turn our little scraping code into a function. This might seem like a waste of time for one line of code, but this is a remarkably small amount of code for a scraper so good practise for building more complex scrapers.


```python
def EU_meeting_scraper(url):

    df = pd.read_html(url)[0]

    return df

```



Notice how the url has `-p=1` at the end of it. A guess and a quick experiment in with the browser confirms the number `p` equals corresponds to the page of meetings. So to get all the pages we need to iterate through the pages using the urls.

Now we can get all the meetings on the site using a loop. Be sure to read the muted text around the code that explains what each bit of code is doing.


```python
page_number = 1 # Start at the first page 

df_list = [] # We create an empty list to add the meeting dataframes to as we go along
while page_number <= 5: # There are only 5 pages of meetings on the page
    print(page_number)

    # Remember what we learnt earlier about how to turn numbers into strings so we can add them to text
    url = 'http://ec.europa.eu/transparencyinitiative/meetings/meeting.do?host=c8e208ad-7dc2-4a97-acc9-859463c69ec4&d-6679426-p=' + str(page_number)

    # Now we pull the data from url and save as the temporary database called df_temp
    df_temp = EU_meeting_scraper(url)

    # Now we add this database to our list of databases
    df_list.append(df_temp)

    time.sleep(2) # Now we use the sleep function to tell python to wait for two seconds so we don't annoy the EU's servers too much

    # Finally we add one to our page number so that we arn't pulling information from the same page over and over again. The EU really wouldn't like that.
    page_number = page_number + 1

```

    1
    2
    3
    4
    5


Now lets combine all the pages of data, which we have saved in `df_list`, into one dataset using the pandas `concat` function. We also add `reset_index` to reset the index so it counts all the rows in the dataframe, but don't worry too much about that.


```python
df = pd.concat(df_list).reset_index(drop=True)
```

And we now have all the meetings!


```python
df
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Location</th>
      <th>Entity/ies met</th>
      <th>Subject(s)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>03/09/2021</td>
      <td>Evian, France</td>
      <td>Allianz SE (Allianz Group)</td>
      <td>Meeting with CEO of Allianz</td>
    </tr>
    <tr>
      <th>1</th>
      <td>29/08/2021</td>
      <td>Brussels</td>
      <td>European Round Table for Industry (ERT)</td>
      <td>Dinner/ meeting with the ERT members on green ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>25/08/2021</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>19/07/2021</td>
      <td>Brussels</td>
      <td>Bill &amp; Melinda Gates Foundation (BMGF)</td>
      <td>Meeting with Co-chairman and co-founder of the...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>09/07/2021</td>
      <td>Brussels</td>
      <td>Global Citizen</td>
      <td>Meeting with the CEO from Global Citizen</td>
    </tr>
    <tr>
      <th>5</th>
      <td>24/04/2021</td>
      <td>Brussels</td>
      <td>Potsdam-Institut für Klimafolgenforschung (PIK)</td>
      <td>Meeting with Founding Director of the Potsdam ...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>29/03/2021</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>19/03/2021</td>
      <td>Videoconference</td>
      <td>Bundesverband der Deutschen Industrie e.V. (BDI)</td>
      <td>Meeting with BDI President</td>
    </tr>
    <tr>
      <th>8</th>
      <td>20/02/2021</td>
      <td>Videoconference</td>
      <td>Global Citizen</td>
      <td>Meeting with the CEO from Global Citizen</td>
    </tr>
    <tr>
      <th>9</th>
      <td>19/02/2021</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Meeting with Chairman of Volvo, Chairman of Si...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>05/02/2021</td>
      <td>Videoconference</td>
      <td>Global Citizen</td>
      <td>Meeting with the CEO from Global Citizen</td>
    </tr>
    <tr>
      <th>11</th>
      <td>31/01/2021</td>
      <td>Videoconference</td>
      <td>AstraZeneca PLC BioNTech SE CureVac AG SANOFI ...</td>
      <td>Meeting with CEOs of the pharmaceutical compan...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>27/01/2021</td>
      <td>Videoconference</td>
      <td>Tony Blair Institute for Global Change (TBI)</td>
      <td>Meeting with CEO Institute for Global Change</td>
    </tr>
    <tr>
      <th>13</th>
      <td>24/01/2021</td>
      <td>Videoconference</td>
      <td>Global Citizen</td>
      <td>Meeting with the CEO from Global Citizen</td>
    </tr>
    <tr>
      <th>14</th>
      <td>22/01/2021</td>
      <td>Videoconference</td>
      <td>Global Citizen</td>
      <td>Meeting with the CEO from Global Citizen</td>
    </tr>
    <tr>
      <th>15</th>
      <td>03/12/2020</td>
      <td>Videoconference</td>
      <td>Union of European Football Associations (UEFA)</td>
      <td>Meeting with the President of UEFA</td>
    </tr>
    <tr>
      <th>16</th>
      <td>20/11/2020</td>
      <td>Videoconference</td>
      <td>Bill &amp; Melinda Gates Foundation (BMGF)</td>
      <td>Meeting with Co-chairman and co-founder of the...</td>
    </tr>
    <tr>
      <th>17</th>
      <td>18/11/2020</td>
      <td>Brussels</td>
      <td>Gemeinnützige Hertie-Stiftung Institut Montaigne</td>
      <td>Meeting with CEO of Hertie Stiftung and Presid...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>13/11/2020</td>
      <td>Videoconference</td>
      <td>WePROTECT Global Alliance</td>
      <td>Videoconference with Chairman of WePROTECT Glo...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>06/11/2020</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>20</th>
      <td>09/10/2020</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>21</th>
      <td>21/09/2020</td>
      <td>Videoconference</td>
      <td>Global Citizen</td>
      <td>Videoconference with CEO Global Citizen (Topic...</td>
    </tr>
    <tr>
      <th>22</th>
      <td>01/09/2020</td>
      <td>Videoconference</td>
      <td>Potsdam-Institut für Klimafolgenforschung (PIK)</td>
      <td>Videoconference with former director of the Po...</td>
    </tr>
    <tr>
      <th>23</th>
      <td>22/07/2020</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>24</th>
      <td>29/06/2020</td>
      <td>Videoconference</td>
      <td>Global Citizen</td>
      <td>Videoconference with CEO Global Citizen (Topic...</td>
    </tr>
    <tr>
      <th>25</th>
      <td>20/06/2020</td>
      <td>Berlin, Germany</td>
      <td>Mercator Institute for China Studies (MERICS)</td>
      <td>Meeting with the Executive Director (Exchange ...</td>
    </tr>
    <tr>
      <th>26</th>
      <td>12/06/2020</td>
      <td>Videoconference</td>
      <td>Teneo Brussels Global Citizen Amgen Inc</td>
      <td>Videoconference CEO of Amgen and CEO of Teneo ...</td>
    </tr>
    <tr>
      <th>27</th>
      <td>12/06/2020</td>
      <td>Videoconference</td>
      <td>Global Citizen</td>
      <td>Videoconfere with CEO of Global Citizen (Topic...</td>
    </tr>
    <tr>
      <th>28</th>
      <td>12/06/2020</td>
      <td>Videconference</td>
      <td>Volvo Car Corporation AB (Volvo Cars)  Siemens...</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>24/05/2020</td>
      <td>Videoconference</td>
      <td>Global Citizen</td>
      <td>Videoconference with CEO Global Citizen (Topic...</td>
    </tr>
    <tr>
      <th>30</th>
      <td>19/05/2020</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>31</th>
      <td>15/05/2020</td>
      <td>Videoconference</td>
      <td>BUSINESSEUROPE</td>
      <td>Meeting with the President and Director-Genera...</td>
    </tr>
    <tr>
      <th>32</th>
      <td>11/05/2020</td>
      <td>Videoconference</td>
      <td>EUROPEAN TRADE UNION CONFEDERATION (ETUC)</td>
      <td>Videoconference with the President and General...</td>
    </tr>
    <tr>
      <th>33</th>
      <td>30/04/2020</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>34</th>
      <td>21/04/2020</td>
      <td>Videoconference</td>
      <td>Deutscher Gewerkschaftsbund (DGB)</td>
      <td>Videoconference with the Chairman of DGB</td>
    </tr>
    <tr>
      <th>35</th>
      <td>17/04/2020</td>
      <td>Videoconference</td>
      <td>Siemens AG (SAG)  Volvo AB (Volvo Group)  A.P....</td>
      <td>Videoconference with Chairman of Volvo, Chairm...</td>
    </tr>
    <tr>
      <th>36</th>
      <td>25/03/2020</td>
      <td>Videoconference</td>
      <td>MONDRAGON CORPORACION COOPERATIVA S. COOP (MON...</td>
      <td>Videoconference with CEOs on COVID-19</td>
    </tr>
    <tr>
      <th>37</th>
      <td>16/03/2020</td>
      <td>Videoconference</td>
      <td>CureVac AG</td>
      <td>Videconderence with CureVac representatives on...</td>
    </tr>
    <tr>
      <th>38</th>
      <td>18/02/2020</td>
      <td>Brussels</td>
      <td>The Wallenberg Foundations AB Investor AB (Inv...</td>
      <td>Meeting with Chair of Wallenberg Foundations, ...</td>
    </tr>
    <tr>
      <th>39</th>
      <td>22/01/2020</td>
      <td>Davos</td>
      <td>Palantir Technologies Inc.</td>
      <td>Meeting with the CEO</td>
    </tr>
    <tr>
      <th>40</th>
      <td>22/01/2020</td>
      <td>Davos</td>
      <td>Apple Inc.</td>
      <td>Meeting with the Apple CEO (Topic: EU Green De...</td>
    </tr>
    <tr>
      <th>41</th>
      <td>21/01/2020</td>
      <td>Davos</td>
      <td>AXA</td>
      <td>Meeting with Group CEO AXA (Topic: EU Green De...</td>
    </tr>
    <tr>
      <th>42</th>
      <td>07/01/2020</td>
      <td>Brussels</td>
      <td>EUROPEAN TRADE UNION CONFEDERATION (ETUC)</td>
      <td>Meeting with General-Secretary ETUC</td>
    </tr>
    <tr>
      <th>43</th>
      <td>19/12/2019</td>
      <td>Brussels</td>
      <td>BUSINESSEUROPE</td>
      <td>Meeting with the President and Director General</td>
    </tr>
  </tbody>
</table>


## Next steps

That concludes the InfluenceMap coding tutorials. If you are eager to learn more here are some good resources:

* Code Hub team - If you want to continue learning code feel free to reach out to the Code Hub team. We have weekly meetings to discuss code and are trying to support the development of coders at InfluenceMap. 

* [First Python Notebook course](https://www.firstpythonnotebook.org/) - This is a free tutorial developed for journalists and makes an excellent next step following the completion of our tutorial.

* The [InfluenceMap Data Visualization guide](https://git.influencemap.org/JakeCarbone/DV-guide) - This is a guide I made to making data visualizations covering both the theory behind good design and a guide to using Plotly in Python.

* [InfluenceMap gitea guide](https://git.influencemap.org/InfluenceMap/Gitea-setup-guide) - InfluenceMap uses a open source code storage system called Gitea, which uses the super popular git framework for saving and sharing code. If you have ever heard of the site Github, that is the most common place for people to store their git repositories. This guide walks you through the setup and a bit of the use on how to use git and our version of Github - Gitea.

