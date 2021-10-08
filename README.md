
<img src="images/for-sale-sign.png" />

# King County House Price Model

**Authors:** Wes Newcomb, Noble Tang, Kelsey Lane

## Overview

This project analyzes housing information drawn from the King County area from May 2014 to May 2015 in order to help real estate agencies determine how different metrics can impact the price of a home. After the data was cleaned and encoded, various iterative multiple linear regressions were run in order to converge on a model to use for the house price inference. It was found that the price of a given home in King County could be reasonably modeled using square footage of the house, the condition of the home, and if the house was in an urban, suburban, or rural area.

## Business Problem

This model was created for small real estate agencies in King County during 2016 to help them understand more about how altering certain features of a home could impact the resulting price range of the house. This can help give them insight into dynamics that might have been exclusive only to large companies that had access to this kind of analysis before and help make these smaller real estate agencies more competitive. By focusing in on how location, house condition, and square footage relate to price, real estate agencies can have a digestible understanding of some basic factors to focus on that can help improve their business.

## Data Understanding

This project uses a dataset containing information on 21,597 property sales in King County between May 2014 and May 2015 drawn from the [King County website](https://info.kingcounty.gov/assessor/esales/residential.aspx?openSearchForm=1). Each row represents a different sale and corresponding information about the property sold.


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>price</th>
      <th>bedrooms</th>
      <th>bathrooms</th>
      <th>sqft_living</th>
      <th>sqft_lot</th>
      <th>floors</th>
      <th>waterfront</th>
      <th>view</th>
      <th>...</th>
      <th>grade</th>
      <th>sqft_above</th>
      <th>sqft_basement</th>
      <th>yr_built</th>
      <th>yr_renovated</th>
      <th>zipcode</th>
      <th>lat</th>
      <th>long</th>
      <th>sqft_living15</th>
      <th>sqft_lot15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7129300520</td>
      <td>10/13/2014</td>
      <td>221900.0</td>
      <td>3</td>
      <td>1.00</td>
      <td>1180</td>
      <td>5650</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NONE</td>
      <td>...</td>
      <td>7 Average</td>
      <td>1180</td>
      <td>0.0</td>
      <td>1955</td>
      <td>0.0</td>
      <td>98178</td>
      <td>47.5112</td>
      <td>-122.257</td>
      <td>1340</td>
      <td>5650</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6414100192</td>
      <td>12/9/2014</td>
      <td>538000.0</td>
      <td>3</td>
      <td>2.25</td>
      <td>2570</td>
      <td>7242</td>
      <td>2.0</td>
      <td>NO</td>
      <td>NONE</td>
      <td>...</td>
      <td>7 Average</td>
      <td>2170</td>
      <td>400.0</td>
      <td>1951</td>
      <td>1991.0</td>
      <td>98125</td>
      <td>47.7210</td>
      <td>-122.319</td>
      <td>1690</td>
      <td>7639</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5631500400</td>
      <td>2/25/2015</td>
      <td>180000.0</td>
      <td>2</td>
      <td>1.00</td>
      <td>770</td>
      <td>10000</td>
      <td>1.0</td>
      <td>NO</td>
      <td>NONE</td>
      <td>...</td>
      <td>6 Low Average</td>
      <td>770</td>
      <td>0.0</td>
      <td>1933</td>
      <td>NaN</td>
      <td>98028</td>
      <td>47.7379</td>
      <td>-122.233</td>
      <td>2720</td>
      <td>8062</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2487200875</td>
      <td>12/9/2014</td>
      <td>604000.0</td>
      <td>4</td>
      <td>3.00</td>
      <td>1960</td>
      <td>5000</td>
      <td>1.0</td>
      <td>NO</td>
      <td>NONE</td>
      <td>...</td>
      <td>7 Average</td>
      <td>1050</td>
      <td>910.0</td>
      <td>1965</td>
      <td>0.0</td>
      <td>98136</td>
      <td>47.5208</td>
      <td>-122.393</td>
      <td>1360</td>
      <td>5000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1954400510</td>
      <td>2/18/2015</td>
      <td>510000.0</td>
      <td>3</td>
      <td>2.00</td>
      <td>1680</td>
      <td>8080</td>
      <td>1.0</td>
      <td>NO</td>
      <td>NONE</td>
      <td>...</td>
      <td>8 Good</td>
      <td>1680</td>
      <td>0.0</td>
      <td>1987</td>
      <td>0.0</td>
      <td>98074</td>
      <td>47.6168</td>
      <td>-122.045</td>
      <td>1800</td>
      <td>7503</td>
    </tr>
  </tbody>
</table>
</div>


The information provided includes details like the square footage of lot and living space, number of beds and bathrooms, the date the property was sold, sale price, number of floors in the house, if it was a waterfront property, the zip code the property is in, and other additional columns shown below.

     #   Column         Non-Null Count  Dtype  
    ---  ------         --------------  -----  
     0   id             21597 non-null  int64  
     1   date           21597 non-null  object 
     2   price          21597 non-null  float64
     3   bedrooms       21597 non-null  int64  
     4   bathrooms      21597 non-null  float64
     5   sqft_living    21597 non-null  int64  
     6   sqft_lot       21597 non-null  int64  
     7   floors         21597 non-null  float64
     8   waterfront     19221 non-null  object 
     9   view           21534 non-null  object 
     10  condition      21597 non-null  object 
     11  grade          21597 non-null  object 
     12  sqft_above     21597 non-null  int64  
     13  sqft_basement  21597 non-null  object 
     14  yr_built       21597 non-null  int64  
     15  yr_renovated   17755 non-null  float64
     16  zipcode        21597 non-null  int64  
     17  lat            21597 non-null  float64
     18  long           21597 non-null  float64
     19  sqft_living15  21597 non-null  int64  
     20  sqft_lot15     21597 non-null  int64  

As the model is predicting the price of future homes, price is the target variable used from the dataset. The distribution of the column can be seen below, where there is a right skew, indicating the presence of some very expensive homes in the dataset.

<img src="images/price_hist2.png" />

However, not all the information provided in the original dataset is used in the creation of the model. These factors aren't all significant in helping create an inference around home prices and thus it's beneficial to pair the dataset down to important points. Furthermore, the continuous features that correlate well with the target variable, price, while not correlating strongly with each other would make for viable features to use, as this shows they have some kind of correlation with price with not creating issues of multicollinearity. These features, like sqft_living and sqft_lot, can be seen in the below heatmap.

<img src="images/large_heat_map2.png" />

The data is also limited in it's distribution of information. For example, there are not a lot of higher priced homes included in the set, so the model could become more variable for higher priced homes as there's not enough data to help capture the relationship. Furthermore, the dataset is also limited in its quantity and number of features, which may also impact the model.

## Data Preparation

### Outliers

While most of the data was entered correctly, there does appear to be some mistyped entries. An example is shown below, where one property is listed with 33 bedrooms while only having 1,620 square feet of living space, which isn't possible.

    id               2402100895
    date              6/25/2014
    price                640000
    bedrooms                 33
    bathrooms              1.75
    sqft_living            1620
    sqft_lot               6000
    floors                    1
    waterfront               NO
    view                   NONE
    condition         Very Good
    grade             7 Average
    sqft_above             1040
    sqft_basement         580.0
    yr_built               1947
    yr_renovated              0
    zipcode               98103
    lat                 47.6878
    long               -122.331
    sqft_living15          1330
    sqft_lot15             4700


Similarly, there are examples of data where the total amount of living space (sqft_living) exceeds the lot space of the property, even when floors are taken into account. This isn't possible, as it would mean the house is bigger than the lot it resides on.

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>price</th>
      <th>bedrooms</th>
      <th>bathrooms</th>
      <th>sqft_living</th>
      <th>sqft_lot</th>
      <th>floors</th>
      <th>waterfront</th>
      <th>view</th>
      <th>...</th>
      <th>grade</th>
      <th>sqft_above</th>
      <th>sqft_basement</th>
      <th>yr_built</th>
      <th>yr_renovated</th>
      <th>zipcode</th>
      <th>lat</th>
      <th>long</th>
      <th>sqft_living15</th>
      <th>sqft_lot15</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1547</th>
      <td>8816400885</td>
      <td>10/8/2014</td>
      <td>450000.0</td>
      <td>4</td>
      <td>1.75</td>
      <td>1640</td>
      <td>1480</td>
      <td>1.0</td>
      <td>NO</td>
      <td>NONE</td>
      <td>...</td>
      <td>7 Average</td>
      <td>820</td>
      <td>820.0</td>
      <td>1912</td>
      <td>0.0</td>
      <td>98105</td>
      <td>47.6684</td>
      <td>-122.314</td>
      <td>1420</td>
      <td>2342</td>
    </tr>
    <tr>
      <th>3449</th>
      <td>2559950110</td>
      <td>4/22/2015</td>
      <td>1230000.0</td>
      <td>2</td>
      <td>2.50</td>
      <td>2470</td>
      <td>609</td>
      <td>3.0</td>
      <td>NO</td>
      <td>NONE</td>
      <td>...</td>
      <td>11 Excellent</td>
      <td>1910</td>
      <td>560.0</td>
      <td>2011</td>
      <td>0.0</td>
      <td>98112</td>
      <td>47.6182</td>
      <td>-122.312</td>
      <td>2440</td>
      <td>1229</td>
    </tr>
    <tr>
      <th>5795</th>
      <td>2770604103</td>
      <td>7/31/2014</td>
      <td>450000.0</td>
      <td>3</td>
      <td>2.50</td>
      <td>1530</td>
      <td>762</td>
      <td>2.0</td>
      <td>NO</td>
      <td>NONE</td>
      <td>...</td>
      <td>8 Good</td>
      <td>1050</td>
      <td>480.0</td>
      <td>2007</td>
      <td>0.0</td>
      <td>98119</td>
      <td>47.6420</td>
      <td>-122.374</td>
      <td>1610</td>
      <td>1482</td>
    </tr>
    <tr>
      <th>13240</th>
      <td>2877104196</td>
      <td>12/6/2014</td>
      <td>760000.0</td>
      <td>3</td>
      <td>2.00</td>
      <td>1780</td>
      <td>1750</td>
      <td>1.0</td>
      <td>NO</td>
      <td>AVERAGE</td>
      <td>...</td>
      <td>8 Good</td>
      <td>1400</td>
      <td>380.0</td>
      <td>1927</td>
      <td>2014.0</td>
      <td>98103</td>
      <td>47.6797</td>
      <td>-122.357</td>
      <td>1780</td>
      <td>3750</td>
    </tr>
    <tr>
      <th>13265</th>
      <td>3277800845</td>
      <td>7/11/2014</td>
      <td>370000.0</td>
      <td>3</td>
      <td>1.00</td>
      <td>1170</td>
      <td>1105</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NONE</td>
      <td>...</td>
      <td>7 Average</td>
      <td>1170</td>
      <td>0.0</td>
      <td>1965</td>
      <td>0.0</td>
      <td>98126</td>
      <td>47.5448</td>
      <td>-122.375</td>
      <td>1380</td>
      <td>1399</td>
    </tr>
    <tr>
      <th>15729</th>
      <td>9828702895</td>
      <td>10/22/2014</td>
      <td>700000.0</td>
      <td>4</td>
      <td>1.75</td>
      <td>2420</td>
      <td>520</td>
      <td>1.5</td>
      <td>NO</td>
      <td>NONE</td>
      <td>...</td>
      <td>7 Average</td>
      <td>2420</td>
      <td>0.0</td>
      <td>1900</td>
      <td>0.0</td>
      <td>98112</td>
      <td>47.6209</td>
      <td>-122.302</td>
      <td>1200</td>
      <td>1170</td>
    </tr>
    <tr>
      <th>16917</th>
      <td>5016002275</td>
      <td>6/2/2014</td>
      <td>610000.0</td>
      <td>5</td>
      <td>2.50</td>
      <td>3990</td>
      <td>3839</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NONE</td>
      <td>...</td>
      <td>8 Good</td>
      <td>1990</td>
      <td>2000.0</td>
      <td>1962</td>
      <td>0.0</td>
      <td>98112</td>
      <td>47.6236</td>
      <td>-122.299</td>
      <td>2090</td>
      <td>5000</td>
    </tr>
    <tr>
      <th>17420</th>
      <td>2062600020</td>
      <td>7/8/2014</td>
      <td>530000.0</td>
      <td>2</td>
      <td>2.50</td>
      <td>1785</td>
      <td>779</td>
      <td>2.0</td>
      <td>NO</td>
      <td>NONE</td>
      <td>...</td>
      <td>7 Average</td>
      <td>1595</td>
      <td>190.0</td>
      <td>1975</td>
      <td>0.0</td>
      <td>98004</td>
      <td>47.5959</td>
      <td>-122.198</td>
      <td>1780</td>
      <td>794</td>
    </tr>
  </tbody>
</table>
</div>

Due to the fact that these nine listings don't seem to be possible and are likely present in the dataset due to human error, they were dropped from the final dataset.

### Final Columns

As mentioned above, certain property features correlate better with price while avoiding multicollinearity issues. As a result, the dataset was paired down to only include the columns that would be of interest to both real estate agencies and potential to explain the variability in price: square foot living space, bedrooms, bathrooms, square footage of the lot, location (for now represented by zip code), home condition, and the year the house was built along if it was renovated.


### Missing Data

After selecting the final columns, there is some missing data left in the year renovated column.


    price              0
    bedrooms           0
    bathrooms          0
    sqft_living        0
    sqft_lot           0
    condition          0
    yr_built           0
    yr_renovated    3842
    zipcode            0


As it is unclear if the null values indicate that the house wasn't renovated or that this information just wasn't collected, the safest approach seems to be to fill the empty value with the median value, which in this case is 0 indicating the property wasn't renovated.


### Column Creation

As location is one of the points of interest in the analysis and the zip code column has too many variables to reasonably encode for an inferential model, the zip codes are categorized by being in an urban, suburban, or rural area. The information for this zip code breakdown is drawn from the [King County site](https://kingcounty.gov/~/media/depts/executive/performance-strategy-budget/regional-planning/buildable-lands-report/king-county-buildable-lands-report-2014.ashx?la=en) itself, where rural areas are identified [through this webpage](https://web.archive.org/web/20150911054310/https://kingcounty.gov/services/rural-services/rural/community.aspx) and the mapping of rural, suburban, and urban areas to zip code is obtained by comparing the first map to [the map of zip codes](https://kingcounty.gov/~/media/operations/GIS/maps/vmc/images/zipcodes_westKC_586.ashx?la=en%20-%20zipcode%20to%20place).


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>price</th>
      <th>bedrooms</th>
      <th>bathrooms</th>
      <th>sqft_living</th>
      <th>sqft_lot</th>
      <th>condition</th>
      <th>yr_built</th>
      <th>yr_renovated</th>
      <th>area_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>221900.0</td>
      <td>3</td>
      <td>1.00</td>
      <td>1180</td>
      <td>5650</td>
      <td>Average</td>
      <td>1955</td>
      <td>0.0</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>538000.0</td>
      <td>3</td>
      <td>2.25</td>
      <td>2570</td>
      <td>7242</td>
      <td>Average</td>
      <td>1951</td>
      <td>1991.0</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>180000.0</td>
      <td>2</td>
      <td>1.00</td>
      <td>770</td>
      <td>10000</td>
      <td>Average</td>
      <td>1933</td>
      <td>0.0</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>604000.0</td>
      <td>4</td>
      <td>3.00</td>
      <td>1960</td>
      <td>5000</td>
      <td>Very Good</td>
      <td>1965</td>
      <td>0.0</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>510000.0</td>
      <td>3</td>
      <td>2.00</td>
      <td>1680</td>
      <td>8080</td>
      <td>Average</td>
      <td>1987</td>
      <td>0.0</td>
      <td>Suburban</td>
    </tr>
  </tbody>
</table>
</div>



When renovation is considered in the iterative models below, it is treated as a true/false value where only the fact that the house was renovated matters. This is to help keep the model simple and also because it is ambiguous over how renovations could be accurately binned without introducing any confounding factors.


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>price</th>
      <th>bedrooms</th>
      <th>bathrooms</th>
      <th>sqft_living</th>
      <th>sqft_lot</th>
      <th>condition</th>
      <th>yr_built</th>
      <th>area_type</th>
      <th>is_renovated</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>221900.0</td>
      <td>3</td>
      <td>1.00</td>
      <td>1180</td>
      <td>5650</td>
      <td>Average</td>
      <td>1955</td>
      <td>Urban</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>538000.0</td>
      <td>3</td>
      <td>2.25</td>
      <td>2570</td>
      <td>7242</td>
      <td>Average</td>
      <td>1951</td>
      <td>Urban</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>180000.0</td>
      <td>2</td>
      <td>1.00</td>
      <td>770</td>
      <td>10000</td>
      <td>Average</td>
      <td>1933</td>
      <td>Suburban</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>604000.0</td>
      <td>4</td>
      <td>3.00</td>
      <td>1960</td>
      <td>5000</td>
      <td>Very Good</td>
      <td>1965</td>
      <td>Urban</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>510000.0</td>
      <td>3</td>
      <td>2.00</td>
      <td>1680</td>
      <td>8080</td>
      <td>Average</td>
      <td>1987</td>
      <td>Suburban</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



The final column created was a bin for house condition. In order to make the results clearer, poor, fair, and average results were binned together, as these represented houses that would need some amount of maintence still [according to the King County site](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r). Good and Very Good results are kept seperate as they have a clearer deliniation about maintence of the house. By binning condition this way, it helps narrow condition down to more pertinent features.


<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>price</th>
      <th>bedrooms</th>
      <th>bathrooms</th>
      <th>sqft_living</th>
      <th>sqft_lot</th>
      <th>yr_built</th>
      <th>area_type</th>
      <th>is_renovated</th>
      <th>conditions_bin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>221900.0</td>
      <td>3</td>
      <td>1.00</td>
      <td>1180</td>
      <td>5650</td>
      <td>1955</td>
      <td>Urban</td>
      <td>0</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>1</th>
      <td>538000.0</td>
      <td>3</td>
      <td>2.25</td>
      <td>2570</td>
      <td>7242</td>
      <td>1951</td>
      <td>Urban</td>
      <td>1</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>2</th>
      <td>180000.0</td>
      <td>2</td>
      <td>1.00</td>
      <td>770</td>
      <td>10000</td>
      <td>1933</td>
      <td>Suburban</td>
      <td>0</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>3</th>
      <td>604000.0</td>
      <td>4</td>
      <td>3.00</td>
      <td>1960</td>
      <td>5000</td>
      <td>1965</td>
      <td>Urban</td>
      <td>0</td>
      <td>High</td>
    </tr>
    <tr>
      <th>4</th>
      <td>510000.0</td>
      <td>3</td>
      <td>2.00</td>
      <td>1680</td>
      <td>8080</td>
      <td>1987</td>
      <td>Suburban</td>
      <td>0</td>
      <td>Low</td>
    </tr>
  </tbody>
</table>
</div>





## Modeling and Evaluation


### Baseline Model

The baseline used for comparison of the models is the mean of the price. This is the simplest predictor, and thus works as a good measure of comparison to see if the future models have any kind of inferential power. The R2 for the baseline is around -5.6 x 10^-5, indicating a very small number that does not really explain any of the variability in price.


### First model
The first model looks at two features that come up frequently when talking about real estate - square foot living space and number of bathrooms.

```
Train MSE: 365204642459.9775
Test MSE: 350703089728.4004
```
<img src="images/model1.png" />


<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared (uncentered):</th>       <td>   0.160</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared (uncentered):</th>  <td>   0.160</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>           <td>   1646.</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Thu, 07 Oct 2021</td> <th>  Prob (F-statistic):</th>            <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>15:41:58</td>     <th>  Log-Likelihood:    </th>          <td>-2.5440e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 17270</td>      <th>  AIC:               </th>           <td>5.088e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 17268</td>      <th>  BIC:               </th>           <td>5.088e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     2</td>      <th>                     </th>               <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>               <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
       <td></td>          <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>bathrooms</th>   <td>-4417.6539</td> <td> 7050.011</td> <td>   -0.627</td> <td> 0.531</td> <td>-1.82e+04</td> <td> 9401.082</td>
</tr>
<tr>
  <th>sqft_living</th> <td> 2.672e+05</td> <td> 7050.011</td> <td>   37.898</td> <td> 0.000</td> <td> 2.53e+05</td> <td> 2.81e+05</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>12148.279</td> <th>  Durbin-Watson:     </th>  <td>   0.383</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>483241.210</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 2.903</td>   <th>  Prob(JB):          </th>  <td>    0.00</td> 
</tr>
<tr>
  <th>Kurtosis:</th>       <td>28.256</td>   <th>  Cond. No.          </th>  <td>    2.69</td> 
</tr>
</table><br/><br/>Notes:<br/>[1] R² is computed without centering (uncentered) since the model does not contain a constant.<br/>[2] Standard Errors assume that the covariance matrix of the errors is correctly specified.



### Second model
While the Adjusted R2 value for this first model is better than the baseline, bathrooms does not seem to be a significant predictor despite correlating well with price. Therefore, the next model swaps out bathrooms for bedrooms as a predictor, which is another important factor to consider when looking at home price.

```
Train MSE: 362989735310.17664
Test MSE: 348310084760.00806
```

<img src="images/model2.png" />


<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared (uncentered):</th>       <td>   0.165</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared (uncentered):</th>  <td>   0.165</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>           <td>   1709.</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Thu, 07 Oct 2021</td> <th>  Prob (F-statistic):</th>            <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>15:41:58</td>     <th>  Log-Likelihood:    </th>          <td>-2.5435e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 17270</td>      <th>  AIC:               </th>           <td>5.087e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 17268</td>      <th>  BIC:               </th>           <td>5.087e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     2</td>      <th>                     </th>               <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>               <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
       <td></td>          <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>sqft_living</th> <td> 2.985e+05</td> <td> 5689.039</td> <td>   52.464</td> <td> 0.000</td> <td> 2.87e+05</td> <td>  3.1e+05</td>
</tr>
<tr>
  <th>bedrooms</th>    <td>-5.851e+04</td> <td> 5689.039</td> <td>  -10.284</td> <td> 0.000</td> <td>-6.97e+04</td> <td>-4.74e+04</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>11750.041</td> <th>  Durbin-Watson:     </th>  <td>   0.372</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>   <th>  Jarque-Bera (JB):  </th> <td>425990.817</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 2.790</td>   <th>  Prob(JB):          </th>  <td>    0.00</td> 
</tr>
<tr>
  <th>Kurtosis:</th>       <td>26.683</td>   <th>  Cond. No.          </th>  <td>    1.98</td> 
</tr>
</table><br/><br/>Notes:<br/>[1] R² is computed without centering (uncentered) since the model does not contain a constant.<br/>[2] Standard Errors assume that the covariance matrix of the errors is correctly specified.



### Third model
While the adujsted R2 value is only slightly better than the first model, both coefficients this time are significant. Therefore, this model is the one iterated on as it includes the two coefficients that correlate highly with price and can help real estate agencies get a deeper grip on how changing these factors may impact price range. From here, the next model includes location in an effort to better explain the variability in price and thus improve the adjusted R2 score. This also improves its usefulness for home buyers looking for a house in a specific type of area. As location is treated as a categorical variable, it is one hot encoded and the rural column is dropped to avoid multicollinearity issues.

```
Train MSE: 118549236782.18959
Test MSE: 111929070343.0281
```

<img src="images/model3.png" />


<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared (uncentered):</th>       <td>   0.727</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared (uncentered):</th>  <td>   0.727</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>           <td>1.152e+04</td> 
</tr>
<tr>
  <th>Date:</th>             <td>Thu, 07 Oct 2021</td> <th>  Prob (F-statistic):</th>            <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>15:41:59</td>     <th>  Log-Likelihood:    </th>          <td>-2.4469e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 17270</td>      <th>  AIC:               </th>           <td>4.894e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 17266</td>      <th>  BIC:               </th>           <td>4.894e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     4</td>      <th>                     </th>               <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>               <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
       <td></td>          <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>area__Rural</th> <td> 4.129e+05</td> <td> 9633.897</td> <td>   42.863</td> <td> 0.000</td> <td> 3.94e+05</td> <td> 4.32e+05</td>
</tr>
<tr>
  <th>area__Urban</th> <td> 5.704e+05</td> <td> 3108.185</td> <td>  183.504</td> <td> 0.000</td> <td> 5.64e+05</td> <td> 5.76e+05</td>
</tr>
<tr>
  <th>sqft_living</th> <td> 3.356e+05</td> <td> 3271.920</td> <td>  102.566</td> <td> 0.000</td> <td> 3.29e+05</td> <td> 3.42e+05</td>
</tr>
<tr>
  <th>bedrooms</th>    <td>-6.312e+04</td> <td> 3258.593</td> <td>  -19.369</td> <td> 0.000</td> <td>-6.95e+04</td> <td>-5.67e+04</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>6123.024</td> <th>  Durbin-Watson:     </th> <td>   1.818</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>  <th>  Jarque-Bera (JB):  </th> <td>54454.732</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 1.452</td>  <th>  Prob(JB):          </th> <td>    0.00</td> 
</tr>
<tr>
  <th>Kurtosis:</th>       <td>11.200</td>  <th>  Cond. No.          </th> <td>    4.65</td> 
</tr>
</table><br/><br/>Notes:<br/>[1] R² is computed without centering (uncentered) since the model does not contain a constant.<br/>[2] Standard Errors assume that the covariance matrix of the errors is correctly specified.


### Fourth model
This third model has a drastic increase in the adjusted R2 value, hopping up from around 16% to now explaining almost 82% of the variability in price. All coefficients also remain significant, indicating they would be good predictors to use. However, there are issues with both the homoskedasticity and normality assumptions. In an effort to address this, the next model eliminates houses with a price outside three standard deviations from the mean based on the training data from both the train and test sets. This should help decrease the effect of home price outliers on the data and help correct some issues with the model.

```
Train MSE: 103656106075.60287
Test MSE: 104120129333.86363
```

<img src="images/model4.png" />


<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared (uncentered):</th>       <td>   0.734</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared (uncentered):</th>  <td>   0.734</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>           <td>1.190e+04</td> 
</tr>
<tr>
  <th>Date:</th>             <td>Thu, 07 Oct 2021</td> <th>  Prob (F-statistic):</th>            <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>15:42:00</td>     <th>  Log-Likelihood:    </th>          <td>-2.4279e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 17218</td>      <th>  AIC:               </th>           <td>4.856e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 17214</td>      <th>  BIC:               </th>           <td>4.856e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     4</td>      <th>                     </th>               <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>               <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
       <td></td>          <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>area__Rural</th> <td> 4.172e+05</td> <td> 9012.912</td> <td>   46.284</td> <td> 0.000</td> <td> 3.99e+05</td> <td> 4.35e+05</td>
</tr>
<tr>
  <th>area__Urban</th> <td>  5.58e+05</td> <td> 2911.291</td> <td>  191.652</td> <td> 0.000</td> <td> 5.52e+05</td> <td> 5.64e+05</td>
</tr>
<tr>
  <th>sqft_living</th> <td> 2.885e+05</td> <td> 3071.659</td> <td>   93.935</td> <td> 0.000</td> <td> 2.83e+05</td> <td> 2.95e+05</td>
</tr>
<tr>
  <th>bedrooms</th>    <td>-5.003e+04</td> <td> 3057.629</td> <td>  -16.361</td> <td> 0.000</td> <td> -5.6e+04</td> <td> -4.4e+04</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>2927.221</td> <th>  Durbin-Watson:     </th> <td>   1.793</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>  <th>  Jarque-Bera (JB):  </th> <td>7131.709</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 0.958</td>  <th>  Prob(JB):          </th> <td>    0.00</td>
</tr>
<tr>
  <th>Kurtosis:</th>       <td> 5.504</td>  <th>  Cond. No.          </th> <td>    4.65</td>
</tr>
</table><br/><br/>Notes:<br/>[1] R² is computed without centering (uncentered) since the model does not contain a constant.<br/>[2] Standard Errors assume that the covariance matrix of the errors is correctly specified.



### Fifth model
This fourth model shows a slight increase in adjusted R2 after cutting out the outliers, increasing by around 2%. Furthermore, compared to the third model with the same predictors, there is a lower mean squared error with less of a difference between the training and test sets, indicating this model is a bit more accurate than the older one. However, the metric for bedrooms is unintuitive for an inference model like this. Therefore, other  metrics were tested to see how they would impact price. As home condition is another factor real estate agencies could consider when looking at homes, it was substituted for bedrooms to see how it would impact the model. 

```
Train MSE: 97965821583.35643
Test MSE: 98179373271.56746
```

<img src="images/model5.png" />


<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared (uncentered):</th>       <td>   0.749</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared (uncentered):</th>  <td>   0.749</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>           <td>1.028e+04</td> 
</tr>
<tr>
  <th>Date:</th>             <td>Thu, 07 Oct 2021</td> <th>  Prob (F-statistic):</th>            <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>15:42:00</td>     <th>  Log-Likelihood:    </th>          <td>-2.4231e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 17218</td>      <th>  AIC:               </th>           <td>4.846e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 17213</td>      <th>  BIC:               </th>           <td>4.847e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     5</td>      <th>                     </th>               <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>               <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
        <td></td>          <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>area__Rural</th>  <td> 3.802e+05</td> <td> 8842.925</td> <td>   42.999</td> <td> 0.000</td> <td> 3.63e+05</td> <td> 3.98e+05</td>
</tr>
<tr>
  <th>area__Urban</th>  <td> 4.947e+05</td> <td> 3324.418</td> <td>  148.798</td> <td> 0.000</td> <td> 4.88e+05</td> <td> 5.01e+05</td>
</tr>
<tr>
  <th>cond__High</th>   <td> 2.104e+05</td> <td> 8985.391</td> <td>   23.415</td> <td> 0.000</td> <td> 1.93e+05</td> <td> 2.28e+05</td>
</tr>
<tr>
  <th>cond__Medium</th> <td> 1.609e+05</td> <td> 5316.909</td> <td>   30.257</td> <td> 0.000</td> <td>  1.5e+05</td> <td> 1.71e+05</td>
</tr>
<tr>
  <th>sqft_living</th>  <td> 2.626e+05</td> <td> 2401.788</td> <td>  109.350</td> <td> 0.000</td> <td> 2.58e+05</td> <td> 2.67e+05</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>2618.193</td> <th>  Durbin-Watson:     </th> <td>   1.823</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>  <th>  Jarque-Bera (JB):  </th> <td>6511.337</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 0.859</td>  <th>  Prob(JB):          </th> <td>    0.00</td>
</tr>
<tr>
  <th>Kurtosis:</th>       <td> 5.474</td>  <th>  Cond. No.          </th> <td>    4.00</td>
</tr>
</table><br/><br/>Notes:<br/>[1] R² is computed without centering (uncentered) since the model does not contain a constant.<br/>[2] Standard Errors assume that the covariance matrix of the errors is correctly specified.



### Sixth model
While the fifth model has a drop of .1% in the adujsted R2 value, the resulting model is more intuitive of an explanation for real estate agencies. However, while this is the final model settled on, further iterations were done with other inputs to see if there would be a pursuable increase in adjusted R2 with different metrics as well. For the sake of brevity, only one of the models is included as none of them are further pursued.

```
Train MSE: 95890428030.35956
Test MSE: 96919709464.90942
```

<img src="images/model6.png" />


<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared (uncentered):</th>       <td>   0.754</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared (uncentered):</th>  <td>   0.754</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>           <td>   8810.</td>  
</tr>
<tr>
  <th>Date:</th>             <td>Thu, 07 Oct 2021</td> <th>  Prob (F-statistic):</th>            <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>15:42:01</td>     <th>  Log-Likelihood:    </th>          <td>-2.4212e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 17218</td>      <th>  AIC:               </th>           <td>4.843e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 17212</td>      <th>  BIC:               </th>           <td>4.843e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     6</td>      <th>                     </th>               <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>               <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
        <td></td>          <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>area__Rural</th>  <td> 3.719e+05</td> <td> 8759.579</td> <td>   42.461</td> <td> 0.000</td> <td> 3.55e+05</td> <td> 3.89e+05</td>
</tr>
<tr>
  <th>area__Urban</th>  <td>  4.84e+05</td> <td> 3334.936</td> <td>  145.140</td> <td> 0.000</td> <td> 4.77e+05</td> <td> 4.91e+05</td>
</tr>
<tr>
  <th>cond__High</th>   <td> 2.147e+05</td> <td> 8892.806</td> <td>   24.146</td> <td> 0.000</td> <td> 1.97e+05</td> <td> 2.32e+05</td>
</tr>
<tr>
  <th>cond__Medium</th> <td> 1.643e+05</td> <td> 5263.453</td> <td>   31.217</td> <td> 0.000</td> <td> 1.54e+05</td> <td> 1.75e+05</td>
</tr>
<tr>
  <th>sqft_living</th>  <td>   2.6e+05</td> <td> 2380.088</td> <td>  109.256</td> <td> 0.000</td> <td> 2.55e+05</td> <td> 2.65e+05</td>
</tr>
<tr>
  <th>is_renovated</th> <td>  2.53e+05</td> <td> 1.31e+04</td> <td>   19.301</td> <td> 0.000</td> <td> 2.27e+05</td> <td> 2.79e+05</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>2446.281</td> <th>  Durbin-Watson:     </th> <td>   1.824</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>  <th>  Jarque-Bera (JB):  </th> <td>5911.339</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 0.817</td>  <th>  Prob(JB):          </th> <td>    0.00</td>
</tr>
<tr>
  <th>Kurtosis:</th>       <td> 5.359</td>  <th>  Cond. No.          </th> <td>    5.65</td>
</tr>
</table><br/><br/>Notes:<br/>[1] R² is computed without centering (uncentered) since the model does not contain a constant.<br/>[2] Standard Errors assume that the covariance matrix of the errors is correctly specified.



## Final Model Evaluation

As mentioned earlier, the fifth model is the final model used as further iterations did not show a big enough increase in R2 to consider pursuing further. For interpretability, the fifth model is again included below, but this time with the continuous variable being unscaled. The takeaway for this model seems to be that location, square foot living space, and condition of the home can be used to explain around 75% of the variability of price for homes in King County from 2014-2015. This is an improvement on the baseline, where the R2 score was quite small, indicating this model does a much better job of explaining the variability. With everything else held steady, an increse in 1 sqaure foot living space increases price by around 247 dollars. Other factors still held constant, living in an urban rather than suburban area increases price by 25,670 dollars and living in a rural area rather than suburban decreases price by 88,320 dollars. Of these factors, location is the most significant predictor, followed by square foot living and then home condition.

When it comes to the model assumptions, the model does not do that well. While linearity and multicollinearity are checked before with the correlation plot and are accounted for, the QQ plot showing normality still has issues at the tail ends despite culling out outliers. Similarly, the plot looking at heteroskedasticity retains a cone shape, indiacting issues there as well.

While this is a decent fit for the dataset provided, it isn't clear how well it would generalize to different areas and different time periods, as house prices and price distribution would be affected by these factors. Furthermore, the fact that the model does not fullfill the assumptions would also make it inaccurrate in more rigorous price inferences. As for how beneficial this model is for the stakeholders, given the adjusted R2 score of 75%, it would do a decent job at helping predict home prices variability for real estate agencies in King County.

```
Train MSE: 53075303270.234314
Test MSE: 54786684337.00644
```

<img src="images/fin_model.png" />


<table class="simpletable">
<caption>OLS Regression Results</caption>
<tr>
  <th>Dep. Variable:</th>          <td>price</td>      <th>  R-squared (uncentered):</th>       <td>   0.864</td>  
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared (uncentered):</th>  <td>   0.864</td>  
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th>           <td>2.188e+04</td> 
</tr>
<tr>
  <th>Date:</th>             <td>Thu, 07 Oct 2021</td> <th>  Prob (F-statistic):</th>            <td>  0.00</td>   
</tr>
<tr>
  <th>Time:</th>                 <td>15:42:01</td>     <th>  Log-Likelihood:    </th>          <td>-2.3703e+05</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td> 17218</td>      <th>  AIC:               </th>           <td>4.741e+05</td> 
</tr>
<tr>
  <th>Df Residuals:</th>          <td> 17213</td>      <th>  BIC:               </th>           <td>4.741e+05</td> 
</tr>
<tr>
  <th>Df Model:</th>              <td>     5</td>      <th>                     </th>               <td> </td>     
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>               <td> </td>     
</tr>
</table>
<table class="simpletable">
<tr>
        <td></td>          <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>area__Rural</th>  <td>-8.832e+04</td> <td> 7061.421</td> <td>  -12.507</td> <td> 0.000</td> <td>-1.02e+05</td> <td>-7.45e+04</td>
</tr>
<tr>
  <th>area__Urban</th>  <td> 2.567e+04</td> <td> 3399.205</td> <td>    7.552</td> <td> 0.000</td> <td>  1.9e+04</td> <td> 3.23e+04</td>
</tr>
<tr>
  <th>cond__High</th>   <td> 9.068e+04</td> <td> 6638.249</td> <td>   13.660</td> <td> 0.000</td> <td> 7.77e+04</td> <td> 1.04e+05</td>
</tr>
<tr>
  <th>cond__Medium</th> <td>   1.9e+04</td> <td> 3950.004</td> <td>    4.810</td> <td> 0.000</td> <td> 1.13e+04</td> <td> 2.67e+04</td>
</tr>
<tr>
  <th>sqft_living</th>  <td>  247.8315</td> <td>    1.295</td> <td>  191.388</td> <td> 0.000</td> <td>  245.293</td> <td>  250.370</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td>7046.745</td> <th>  Durbin-Watson:     </th> <td>   1.984</td> 
</tr>
<tr>
  <th>Prob(Omnibus):</th>  <td> 0.000</td>  <th>  Jarque-Bera (JB):  </th> <td>53585.263</td>
</tr>
<tr>
  <th>Skew:</th>           <td> 1.785</td>  <th>  Prob(JB):          </th> <td>    0.00</td> 
</tr>
<tr>
  <th>Kurtosis:</th>       <td>10.871</td>  <th>  Cond. No.          </th> <td>9.30e+03</td> 
</tr>
</table><br/><br/>Notes:<br/>[1] R² is computed without centering (uncentered) since the model does not contain a constant.<br/>[2] Standard Errors assume that the covariance matrix of the errors is correctly specified.<br/>[3] The condition number is large, 9.3e+03. This might indicate that there are<br/>strong multicollinearity or other numerical problems.


## Conclusions
When real estate agencies are looking at home price inferences during this timeframe, the location and square foot living space are the most important factors that could impact home price, followed by the home condition. Therefore, when considering the price range of houses a real estate agency is looking for, these factors can help give a more numeric understanding of variability in price range.

Limitations of this data are obviously the timeframe and location, as home price prediction would vary based on these factors so this model wouldn't be applicable outside of this consideration. Building on that, the model is unstable for homes of a higher price and actually cuts off high-priced outliers, so buyers looking for more expensive or extravegant houses wouldn't get accurrate results. The model also doesn't include all the variables, so there may be factors influencing price that are overlooked.

Finally, going forward it would be good to work with more data from different areas to see if the model could be adapted to different locations, as well as integrating data from more recent years and a more robust number of features.

## Project Structure
```
├── King_County_Analysis.ipynb
├── README.md
├── presentation.pdf
├── images
└── data
```