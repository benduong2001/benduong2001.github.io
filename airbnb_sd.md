# ArcGIS San Diego Airbnb Project

* A DSC170 Class Project with Tyson Tran. It uses ArcGIS's Python API to do data analysis and machine learning on San Diego Airbnb listings. We analyzed the following:
    * Complex Geo-enrichment of zipcode census data including household income, and amount of leisuire businesses established, were all used to develop choropleth maps showing the geospatial relationships it had with the nearby AirBnb Locations by their price (overlaid as a scatterplot), highlighting that those closer to the beach were pricier.
    * Non-Spatial factors in the context of airbnb pricing.
    * Buffering of well-known city landmarks in proximity to these AirBnb's.
    * A linear regression model to predict Airbnb Listing Prices based on the surrounding geodata was trained, and achieved a near accuracy of 80%. Feature Importance bar-plots showed that Accomodation and various geo-enriched factors (such as amount of recreational and leisure businesses within the zipcode) were the most influential.
* [Github Repo](https://github.com/benduong2001/DSC170_Airbnb)
* [(Presentation)](https://docs.google.com/presentation/d/1oIXAt-b-P-pWBr-IgK-vk3GEkhqwqTrLX_ksc_a72ME/edit?usp=sharing)

![](images/images_airbnb_sd/dsc170img1.png)

<!--![](images/images_airbnb_sd/dsc170img2.png)

![](images/images_airbnb_sd/dsc170img3.png)

![](images/images_airbnb_sd/dsc170img4.png)
-->

To summarize our results, the biggest factors affecting pricing in a positive or negative trend are location and size.
In general, location and size are the biggest factors that affect the expenses of an investor because they have to pay more for a better location or a bigger home leading to a bigger mortgage each month or having to put more of their money out of pocket. The goal of an investor is to make a profit, so they would need to charge more if they have more expenses. When we look at the pricing of an Airbnb from tourist attractions, we can see that bigger tourist attractions do have an effect on the Airbnb pricing. If there are two properties of similar features near a large tourist attraction like the San Diego convention center but one is super close and one is further away, we would expect the closer one would be able to charge more. We also saw that homes that are able to accommodate more are able to charge more. Both of these findings are in line with our hypothesis.

![](images/images_airbnb_sd/dsc170img5.png)

![](images/images_airbnb_sd/dsc170img6.png)

![](images/images_airbnb_sd/dsc170img13.png)

* There is a lot of multi-collinearity between the businesses columns

![](images/images_airbnb_sd/dsc170img14.png)

* There seems to be a direct relationship between the amount of accomodation businesses within the zipcodes and high airbnb pricing.

![](images/images_airbnb_sd/dsc170img7.png)

![](images/images_airbnb_sd/dsc170img8.png)

* We decided to look at some concentric buffers around 3 specific, famous San Diego landmarks: 
    * SeaWorld, 
    * Convention Center, 
    * Balboa Park/the Zoo. 
* We have found that the airbnb's with the top 1000 highest prices tend to be within the closer buffer rings of these landmarks


![](images/images_airbnb_sd/dsc170img9.png)

![](images/images_airbnb_sd/dsc170img10.png)

![](images/images_airbnb_sd/dsc170img11.png)

![](images/images_airbnb_sd/dsc170img12.png)
