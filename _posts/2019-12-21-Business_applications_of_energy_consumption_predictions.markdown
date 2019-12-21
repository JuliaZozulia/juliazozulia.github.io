---
layout: post
title:  Business applications of energy consumption predictions
date: 2019-12-21 07:15:20 +0300
description: The developed model allows us to estimate how much more/less will a household consume under new parameters. 
img: helloquence-5fNmWej4tAA-unsplash.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [energy consumption, biils, electricity]
---



Let’s imagine you are a homeowner trying to minimize your utility cost. Everyone knows that, for example, if you insulate your home well, your energy consumption will go down. But by how much? **How much will I save** per year? **Is it worth the investment?** What will allow to save more: installing new windows or changing heating equipment?

In series of previous posts we 
[obtained, cleaned and pre-processed data](https://juliazozulia.github.io/What_affects_our_energy_bills/) and investigated the influence of different home attributes on it,
and [created a model](https://juliazozulia.github.io/Predicting_residential_energy_consumption_based_on_attributes_of_the_house/) to predict total energy consumption. 


The developed model allows us to estimate how much more/less will a household consume under new parameters. 

# To estimate savings for consumer:
Let’s consider an example household: 
### One-story single-family detached house, 2553 sq ft, located in an urban area in Cold/Very Cold climate zone. 

The model estimates total consumption as 127,095 BTU. 

House now is poorly insulated. If the owner will insulate the house, his estimated saving will be as follows:

``` python
house_modified.loc[:, 'level of insulation'].replace('Poorly insulated', 'Well insulated', inplace = True)
model.predict(house_modified)
 
Original consumption: 127095.96 BTU
Modified consumption: 125607.74 BTU
Absolute savings:     1488.22 BTU
Percentage savings:   1.17 %
```

Changing windows to energy star qualified:

``` python
Absolute savings:     1301.38 BTU
Percentage savings:   1.02 %
```
Changing main space heating equipment to newer one (same type):
``` python
Absolute savings:     1926.4 BTU
Percentage savings:   1.52 %
```
Changing main space heating equipment from central furnace to heat pump would save us 6.44%:
``` python
Absolute savings:     8179.43 BTU
Percentage savings:   6.44 %
```
 
 
Another example:
### Rural 1676 sq. ft. single-family house located in hot-humid climate.

Predicted consumption is 90945.05 BTU.

It is already adequately insulated, but, if the owner insulates the house better, he would save 1.01%:
``` python
Original consumption: 90945.05 BTU
Modified consumption: 90024.01 BTU
Absolute savings:     921.04 BTU
Percentage savings:   1.01 %
```
The occupants keep summer temperature when no one is home during the day at 65°F, which is unusually low. By keeping it at 75°F and installing smart thermostat they would save 1.41%:
``` python
Original consumption: 90945.05 BTU
Modified consumption: 89658.33 BTU
Absolute savings:     1286.72 BTU
Percentage savings:   1.41 %
```
Not everyone would benefit from installing smart thermostats equally: our first example home would save only 0.14%.

All predictions here are in BTU, which is defined as the amount of heat required to raise the temperature of one pound of water by one degree Fahrenheit (counterpart in the metric system is the calorie). It is used as a unit of power and can be easily converted to whatever source of power the household is using. By knowing the user’s zip, we can easily acquire the corresponding electricity/gas/oil rate at this moment, thus, converting the prediction to monetary equivalent. 

# To contractors:

Apart from the end consumer, this model can be used by a variety of businesses, like:

* **HVAC contractors**, who would be able to give the customer a very quick estimate of potential savings associated with updating of heating/cooling equipment; 

* **Companies selling/installing windows**;

* **Insulation contractors**;

* **Energy assessment companies**, who would be able to recommend customers which parts of the house would yield to the highest savings.

Of course, each of these constructors would be able to estimate potential savings based on detailed engineering calculations, but those are usually very time consuming and require a qualified engineer, while this model allows making a *very quick estimate*, allowing to *keep potential customers*. 

# To real estate brokers:

Another business application of this model is estimating utilities cost while looking for a new home. **Real estate brokers** can incorporate it into their business process to help their customers to make more informed decisions by estimating heating and electric utilities cost for different homes. 

As to use the model on full subset of variables need gathering a lot of information about the home, and, in case of using it on new unseen homes, it may be a problem, the better approach would be to use a small subset of variables. 

Using subset of top 40 variables (based on tree-based feature importance metrics, and after removal of variable that are hard to estimate for unseen homes), gives us test score of 0.71.

Variables can be split into 3 subsets:

* locations based, can be assessed using zip code;
* family based,  are characteristics of the household and can get them be creating simple questionnaire for the clients, or, if not possible, using mean values;
* real estate based, are basic characteristics of building and are easily accessible in the listings. 

![features]({{site.baseurl}}/assets/img/energy/40_variables.jpg)

This way real estate brokers can have additional valuable information to provide to customers by using very small set of variables and the developed model. 

**New home buyers**, in return, would be able to predict their utilities bills with high precision. This is one unknown factor less when choosing monthly mortgage payment they can afford.  


