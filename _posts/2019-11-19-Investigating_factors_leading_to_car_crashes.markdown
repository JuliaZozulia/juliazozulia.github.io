---
layout: post
title: Investigating factors leading to car crashes.
date: 2019-11-19 09:00:20 +0300
description: Almost all of us drive every day. And we all face the danger of a car accident. Every day make a decision - should I drive to the destination via a highway or using local roads? Should I drive my child to school and let the school bus pick him/her up? Should I buy a sedan, SUV or coupe? Those decisions not only impact our convenience, but also our safety.
img: Jackson_Street_Bridge.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [cars, crash, accident]
---

Almost all of us drive every day. And we all face the danger of a car accident. Every day make a decision - should I drive to the destination via a highway or using local roads? Should I drive my child to school and let the school bus pick him/her up? Should I buy a sedan, SUV or coupe? Those decisions not only impact our convenience, but also our safety. 
The goal of this project is to investigate the factors which led to a car accident, including those, impacting the severity of injuries during the impact, as well as create a model to predict car crashes.
The project could be used by insurance agencies, car dealers, or just by people who are looking for a new, safer, car.
The dataset contains details about 101480  police-reported car accidents that happened in the US, including information about the vehicle, GPS coordinates,  road conditions, weather, information about the car, driver, severity of injury received by passengers. 
These crash reports are chosen from 60 selected areas across the United States that reflect the geography, population, miles driven, and crashes in the United States.
![map]({{site.baseurl}}/assets/img/cars/crss-map-60.jpg)

![sev_per_body_type]({{site.baseurl}}/assets/img/cars/sev_per_body_type.jpg)

This plot shows the normalized distribution of injury severity per body type for accidents happened in 2016-2017 years. 
Different vehicle body types strongly impact the severity of the injury if a crash happened. 
For motorcycles, if a crash happened, there is almost no chance not to receive any injury, while for any car your chances are about 65-85%. 
The safest are buses, followed by trucks. The weight of the vehicle matters a lot!
Coupes, as well as convertibles, shows a large fatality rate.
While the most popular car types - sedans and SUV shows visually similar distributions, Chi-square test of independence shows that distribution of injury severity for sedans and SUV are independent (with p-value 1.77e-28) - in other words, body type indeed affects injury severity:
{% highlight ruby %}
    
    from scipy import stats

    def get_chi2_p(data, filters, column_name, 
                   aggregate_column="MAX_VSEV"):

    props = data.groupby(column_name)
            [aggregate_column].value_counts().unstack()
    chi2, p, dof, ex = stats.chi2_contingency(
                       props[props.index.isin(filters)]
                       )
    return p
    get_chi2_p(df, ["SEDAN", "SPORT UTILITY VEHICLE"], 
                    "BODYSTYL_T")

{% endhighlight %}

![sev_per_make]({{site.baseurl}}/assets/img/cars/sev_per_make.jpg)
Do your chances to get insured, if a crash happens, depend on the manufacturer of the car you are in? Well, this plot shows that it does. Apart from a truck maker, Volvo is by far the safest car, followed by Mercedes, Lexus, Acura. Infinity and BMV. Mercury, Mitsubishi, Pontiac, and Kia among the least safe. 
Now we know that choosing a luxury car is not only about comfort but also about safety. This information could be used by car dealers to promote certain safer brands.
![sev_per_year]({{site.baseurl}}/assets/img/cars/sev_per_year.jpg)

From this plot, we can see those modern cars provide much better protection for its riders against severe and fatal insures. But something happened right after the 2010 year - the percentage of passengers with no or minor injures dropped, while protection against fatal and severe injuries keep improving. So the question of whether to upgrade the car is not only about convenience but also about safety
#### What about factors that lead to an accident?
![timing]({{site.baseurl}}/assets/img/cars/timing.jpg)
It seems that the more tires the drivers are, the more accidents happened. Afterwork hours are more dangerous than during the morning rush, and the furthest we are from the beginning of the workweek, the worse.
![sev_per_alcohol]({{site.baseurl}}/assets/img/cars/sev_per_alcohol.jpg)
Crashes happened when on the drivers is under influence, are much more deadly than those without. 
This statistic can be used to persuade people not to drive under alcohol influence. One thing is to know that it is dangerous, and others - know just how much dangerous it is.
![sev_per_speeding]({{site.baseurl}}/assets/img/cars/sev_per_speeding.jpg)
Same story with speeding. Chances to get killed or to kill someone during the crash when you’re speeding are 8 times larger than when you don’t! 
![sev_per_speed_limit]({{site.baseurl}}/assets/img/cars/sev_per_speed_limit.jpg)
Accidents with severe injuries happen mostly on highways.  
![sev_per_avoidance]({{site.baseurl}}/assets/img/cars/sev_per_avoidance.jpg)
If you found yourself in a pre-accident situation, remember, that accidents happened when driver was trying to maneuver by steering more dangerous than those when you do nothing or just brake.
The chi-squared test below shows that the is no significant difference between steering left or right, but there is a strong difference between braking and steering left (right).
{% highlight ruby %}

    get_chi2_p(df, ["Steering Right", "Steering Left"], 
                    "P_CRASH3")
    # 0.4356781892724938
    get_chi2_p(df, ["Braking", "Steering Left"], "P_CRASH3")
    # 4.4512624856085415e-136

{% endhighlight %}
![sev_per_restr]({{site.baseurl}}/assets/img/cars/sev_per_restr.jpg)
Remember that plots that show that motorcycles are unsafe? Well, driving inside the car without buckling up is even worse! In fact, your chances to get killed are 28 times larger when you are not buckled up! I personally know people who drive without a belt, you can even buy a belt fasteners placeholder on ebay, which will stop your car from beeping in protest. Educating people about the importance of the seat belt is very important.
Also, we can see that a rear-facing child seat is safer than front-facing.
![sev_per_seating]({{site.baseurl}}/assets/img/cars/sev_per_seating.jpg)
