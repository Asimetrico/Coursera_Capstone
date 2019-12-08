
<h1>Introduction (week 4)</h1>

<h2>A.1. Description & Disscusion of the Background</h2>
<p>
The aim of this work is to predict the best locations in Barcelona for businesses related to the bicycle sector.
</p>
<p>
For several years now, Barcelona, like other major European cities, has been committed to diversifying the mobility of citizens and the people who move around it from their daily vicinity. The aim of these public policies is to curb or at least mitigate the congestion of mobility in the city and avoid all negative effects.
</p>
<p>
In the case of Barcelona, there has been an increase in public transport, although there is some debate as to whether this is enough. But there is no doubt that there has been a strong commitment on the part of the different governments and political sensitivities to promote the use of bicycles. From the creation of a public bicycle rental service to the creation of an extensive network of bicycle lanes. Part of these policies is explained in this <a href="http://ajuntament.barcelona.cat/bicicleta/en/opting-for-bicycles" target="_blank" >public website of the city</a>.
</p>
<p>
This capstone would try to detect if these policies have made different businesses in the bicycle sector flourish in the city and use these data to determine the best areas to establish new businesses.<br>
In order to establish a model that predicts a good location, it will be assumed that the existence of businesses in the sector and his location is due to profitability rather than a short-term trend. The analysis will also be limited to geographic data without taking into account other values such as demographics, purchasing power or orography. New mobility systems such as electric scooters will also not be taken into account. In the section 'Discussion' we will analyse what these decisions represent. 

</p>

<h2>A.2 Data Description </h2>
<p>
Different data sources will be used to complete the work.
    </p>
    
<p>
One point to be decided by Barcelona to carry out this work is the commitment to the open data of the city, which publishes different types of data on different topics, including urban planning: <a href="https://opendata-ajuntament.barcelona.cat/en" target="_blank">Barcelona's open data service</a>
</p>
<p>
From this portal the geopositioned data will be used to determine the organization in neighborhoods of the city, and if the models obtained support them, determine which are the best neighborhoods to set up a business. Using Folium the data is represented by:
</p>
<img src="https://raw.githubusercontent.com/Asimetrico/Coursera_Capstone/master/week4-5/bcn_neighbourhood.PNG" style="width:70%">

<p>
Link to dataset and field description: <a href="https://opendata-ajuntament.barcelona.cat/data/en/dataset/taula-direle">City organization by neighbourhoods</a>
 </p>
 <p>
It will also use data from existing bike lanes in the city. This data is also geopositioned. Using Folium the data is represented by:
</p>
<img src="https://raw.githubusercontent.com/Asimetrico/Coursera_Capstone/master/week4-5/bcn_bicyngrail.PNG" style="width:70%">
<p>
Link to dataset and field description: <a href="https://opendata-ajuntament.barcelona.cat/data/en/dataset/vies-ciclables">Cycle paths in the city of Barcelona</a>
</p>

<p>
For data referring to businesses and their segmentation by sector, the Foursquare API will be used, which allows us to obtain business data classified by categories that related to bicycling and located geographically. The categories in Foursquare related to business and bikes are:<br>
    <ul>
        <li>bike stores:  '4bf58dd8d48988d115951735' </li>
        <li>renting bikes:'4e4c9077bd41f78e849722f9'</li>
    </ul>
</p>

<p>
   Using <a href="https://developer.foursquare.com/docs/api/venues/search" target="_blank"> the venues search api endpoint </a> it's possible to find by category venues.
    
    
</p>
<p>
The list of venues categories can be found here: <a href="https://developer.foursquare.com/docs/resources/categories" target="_blank" >Foursquare categories</a>
</p>
<p>
    An example of result of the API call can be seen here:<br>
    <img src="https://raw.githubusercontent.com/Asimetrico/Coursera_Capstone/master/week4-5/bcn_foursquare.PNG" style="width:70%">
    </p>
<p>
With these different data sources we will try to apply the different techniques to detect models that could predict a good location for new business.
</p>

 

<h2>B.Methodology</h2>

<p>
The data used to validate whether public policies to create bicycle lanes have encouraged the creation of businesses around the bicycle sector should get the following data:
    </p>
    <ul>

<li>The urban data of Barcelona that allow us to group geographically the data that we will have from Foursquare.</li>

<li>The bicycle lanes currently existing in Barcelona.</li>

<li>Businesses related to the world of the bicycle from the API of Foursqueare.</li>
</ul>

As it will be necessary to work with geographic data, the Shapely library of Python has been used.  

<p>
    <strong>The detailed work for the handling of dataframes and machine learning models can be seen in <a href="https://github.com/Asimetrico/Coursera_Capstone/blob/master/week4-5/notebook-The%20Battle%20of%20Neighborhoods.ipynb">this notebook</a></strong>.
</p>
<br>
<h3>Used data and processing data</h3> 
<br>
<ins>Urban data:</ins>
<p>
    The urban data for Barcelona have been obtained in 'geojson' format. They have been imported as a dataframe. 
Barcelona is made up of 73 neighbourhoods and 11 districts. The data of each neighbourhood are from the geopositioning point of view polygons.<br/> 
By loading the data we have taken advantage of this to obtain the area of each district and the most centred point within the neighbourhood. <br/>
This data will be used to obtain the different points where to make calls to the Foursquare API.
</p>

<ins>The bike lanes:</ins>
<p>
The current bike lane data is also available in 'geojson' and in this case are Linestrings. It has been loaded into a dataframe and will be used to make calculations on each store obtained.
</p>

<ins>Bicycle shops in Foursquare:</ins>
<p>
A call has been made to the Foursquare API focusing on latitude and longitude in the centre of Barcelona and results have been obtained for the bike rental and sale categories. In the first inspection it was detected that in the rental category, Foursquare also includes the points where you can take a bicycle from the public service offered by the city council of Barcelona. An attempt could be made to detect these public points, but there are several different cases and the study was preferred to be limited to bicycle sales centers.
</p>
<p>
In order to have an important sample of Foursquare's data, it has been necessary to make calls in different parts of the city. The limits per Foursquare call are 100 results. To obtain sufficient results it has been decided to make a call in each neighborhood with a relatively low radius (3,500 meters). This results had to be cleaned properly to eliminate the null results and to eliminate the repeated results for appearing several times within the chosen radius.
</p>
<br>
<p>
Once the tents have been obtained, the following data has been obtained for each one:
</p>
<ul>    
<li>Neighbourhood and District to which it belongs: For each centre it is validated if it belongs to the neighbourhood, the definition of neighbourhood as a polygon has been taken advantage of and it has been checked if the centre exists inside.</li>

<li>Minimum distance to a bicycle lane: The minimum distance from each shop to one of the bicycle lanes has been calculated.</li>
</ul>

<p>It has been tried to obtain the density of the cycle lane for each neighbourhood but it has not been achieved. Surely it would be a more powerful measure for our model than the minimum distance but in practice I have not been able to obtain it... 
</p>

<p>
Once each store was associated with a neighbourhood and district as well as a distance, it was possible to obtain global results for each neighbourhood.  The data have been included in each neighborhood as:
</p>
<ul>
<li>Number of stores per neighborhood.</li>
<li>Average distance of shops from a bicycle lane.</li>
</ul>
<p>
With these data it has already been possible to apply different models to validate our hypothesis.
</p>

<h3>Modeling data</h3>
<p>    
Given the type of variables with which we work, the most appropriate seemed to use supervised methods.  We have started with linear regression to relate the average distance with the number of stores but we have obtained negative scores.
We have also tested more complex models with polynomial regressions but we have also obtained negative or very low score results.
</p>
<p>
In view of the low result, it was preferred not to continue investigating with other linia regression methods such as Ridge or Lasso, or other variants.
</p>
<p>
It has also been tested with logistic regression to try a more consistent model but it has not worked, also obtaining very discrete results to be acceptable.
</p>    
<p>
Since it seemed difficult to have results with supervised methods it has been proven at least to obtain a segmentation that could be useful to be able to have a simple model. The unsupervised method KMeans has been tested. The Elbow method has been used to obtain the ideal number of clusters and it has been seen that they were 3:
 </p>
 <img src="https://raw.githubusercontent.com/Asimetrico/Coursera_Capstone/master/week4-5/kmeans-elbow-method.PNG" with="40%">
<p>    With this information the clusters have been created but in their analysis it has been seen that the geographical proximity relative to the centroid of each neighborhood predominated, so the segmentation has not been considered valid. 
</p>
<p>
Finally, clustering has been tried by eliminating geographic data and clusters have become more asymmetric and without a clear segmentation that can be used for predictive purposes.
</p> 

<h2>C.Results section</h2>

First of all, we must remember that initially it was proposed to include also bicycle rental venues but in the Foursquare data his mixed the public rental points and commercial venues. It could modify our results to find where it is best place to implement a business.  So finally only data from bicycle selling shops has been used.</p> 
<p> After processin the api results we have obtained 190 bike selling shops in Barcelona(<a href="https://github.com/Asimetrico/Coursera_Capstone/blob/master/week4-5/bikeshopvenuesedited.csv">curated file can be dowloaded here</a>). which should be a fairly approximate figure of reality and perhaps we can think something small to create a model with good predictive capacity.

After cleaning the data, a dataframe was obtained containing the number of shops and the average distance to a bicycle lane for each neighbourhood. Visually it is already visible that it will be difficult to find a model that helps our initial proposal:<br/>
<img src="https://raw.githubusercontent.com/Asimetrico/Coursera_Capstone/master/week4-5/bcn_shops_lanestown.PNG" style="width:70%">

In the first instance we have tried to use a supervised model that would allow us to validate our model since we have the data that fit well within the supervised models.

We have started by finding a linear or polynomial relationship in our data. Applying the models through SKLearn we see that in all of them the 'score' is negative, which tells us that 
the chosen model does not follow the trend of the data, so fits worse than a horizontal line. 
Due to this, these types of models have been discarded and have not been tested with methods such as Ridge or Lasso of this type of regressions. 

A logistic regression has also been tested and a score of 0.27 has been obtained. For these models the score is :action of correct predictions): correct predictions / total number of data points. Therefore the model does not allow us to corroborate our proposal.

Finally it has been tested with an unsupervised K-Means method. The analysis of the clusters should serve to create a geographical segmentation that would allow us to see the existence of a relationship between the data obtained. 

After applying the elbow method it has been obtained that the optimal number of clusters is 3 and with this model three clusters have been created that have been able to draw on the map:<br/>
<img src="https://raw.githubusercontent.com/Asimetrico/Coursera_Capstone/master/week4-5/bcn_clusted_withgeodata.PNG" style="width:70%">

In the first instance we see a separation based on geographical proximity (the dataframe contains the location of the centroid of each neighborhood) and vaguely the number of stores.

Trying to determine if the segmentation is based on geography, a new dataframe has been created without the geographical data and a new cluster has been obtained that has a more irregular segmentation and that makes it even more complex to carry out an analysis:
<img src="https://raw.githubusercontent.com/Asimetrico/Coursera_Capstone/master/week4-5/bcn_clusted_withoutgeodata.PNG" style="width:70%">


For all these reasons, our proposal to relate where the shops have been installed with respect to the existing cycle lanes in the city is discarded.


<h2>D.Discussion section</h2>
<p>
As has been said during the report, the model seems too simple. The main problems detected are:
</p>
<ul>
    <li>    
An excessively simple model that does not take into account other options such as purchasing power, orography, population density or the floor price.
    </li>
    <li>
The density of bicycle lanes would have been a better option than the average distance to a bicycle lane.
    </li>
    <li>The clusters created do not allow a clear segmentation of the results and it is difficult to establish useful for commercial purposes.
    </li>
 
<p>
Even including all these nuances it is not possible to guarantee that there is a consistent model that allows to determine a better option to choose a good site to install this type of stores.     
</p>    

<h2>E.Conclusions</h2>
<p>
After analysing the data, it seems that the proposed model is too basic to explain which is the best neighbourhood to set up a shop for the sale of bicycles according to the proximity of the existing bicycle lane. This model has also failed to define a segmentation that relates it to the density of shops in certain neighbourhoods.
</p>
<p>
    We cannot aspire to find a simple relationship in the establishment of bicycle shops in Barcelona or in a weak version.  If it is necessary to improve the study several data have been proposed that could improve the options to find better models although, as it has been commented, the quantity of stores surely is small so that our model had predictive capacity.
    </p>
<p>
If the data cannot be used in a more correct way than the one carried out, it is more likely that if the number of bicycle lanes in the city implies a growth of bicycle shops in an integral way without a concrete impact in certain geographical areas. 
</p>
