# Assignment-5.1

Will a Customer Accept the Coupon?
## Context

Imagine driving through town and a coupon is delivered to your cell phone for a restaraunt near where you are driving. Would you accept that coupon and take a short detour to the restaraunt? Would you accept the coupon but use it on a sunbsequent trip? Would you ignore the coupon entirely? What if the coupon was for a bar instead of a restaraunt? What about a coffee house? Would you accept a bar coupon with a minor passenger in the car? What about if it was just you and your partner in the car? Would weather impact the rate of acceptance? What about the time of day?

Obviously, proximity to the business is a factor on whether the coupon is delivered to the driver or not, but what are the factors that determine whether a driver accepts the coupon once it is delivered to them? How would you determine whether a driver is likely to accept a coupon?

## Overview

The goal of this project is to use what you know about visualizations and probability distributions to distinguish between customers who accepted a driving coupon versus those that did not.

## Data

This data comes to us from the UCI Machine Learning repository and was collected via a survey on Amazon Mechanical Turk. The survey describes different driving scenarios including the destination, current time, weather, passenger, etc., and then ask the person whether he will accept the coupon if he is the driver. Answers that the user will drive there ‘right away’ or ‘later before the coupon expires’ are labeled as ‘Y = 1’ and answers ‘no, I do not want the coupon’ are labeled as ‘Y = 0’. There are five different types of coupons -- less expensive restaurants (under $20), coffee houses, carry out & take away, bar, and more expensive restaurants ($20 - $50).

## Deliverables

This final product shows a brief report that highlights the differences between customers who did and did not accept the coupons. 

## Data Description
Keep in mind that these values mentioned below are average values.

The attributes of this data set include:

### User attributes
Gender: male, female

Age: below 21, 21 to 25, 26 to 30, etc.

Marital Status: single, married partner, unmarried partner, or widowed

Number of children: 0, 1, or more than 1

Education: high school, bachelors degree, associates degree, or graduate degree

Occupation: architecture & engineering, business & financial, etc.

Annual income: less than $12500, $12500 - $24999, $25000 - $37499, etc.

Number of times that he/she goes to a bar: 0, less than 1, 1 to 3, 4 to 8 or greater than 8

Number of times that he/she buys takeaway food: 0, less than 1, 1 to 3, 4 to 8 or greater than 8

Number of times that he/she goes to a coffee house: 0, less than 1, 1 to 3, 4 to 8 or greater than 8

Number of times that he/she eats at a restaurant with average expense less than $20 per person: 0, less than 1, 1 to 3, 4 to 8 or greater than 8

Number of times that he/she goes to a bar: 0, less than 1, 1 to 3, 4 to 8 or greater than 8

### Contextual attributes
Driving destination: home, work, or no urgent destination

Location of user, coupon and destination: we provide a map to show the geographical location of the user, destination, and the venue, and we mark the distance between each two places with time of driving. The user can see whether the venue is in the same direction as the destination.

Weather: sunny, rainy, or snowy

Temperature: 30F, 55F, or 80F

Time: 10AM, 2PM, or 6PM

Passenger: alone, partner, kid(s), or friend(s)

### Coupon attributes

time before it expires: 2 hours or one day

## Data and codes

All codes can be found here https://github.com/CarolTeo11/Assignment-5.1/blob/main/assignment_5_1_starter/Carolprompt.ipynb

The data can be found here https://github.com/CarolTeo11/Assignment-5.1/tree/main/assignment_5_1_starter/data

## Data cleaning

1.  While the data is relatively structured and consistent, there is a need to correct for spelling error so that there is no downstream coding issues.  In particular, passenger is spelt "passanger" and is corrected using 
data = data.rename(columns={'passanger': 'passenger'})
Also, the data in "passenger" contains parenthesis and are removed as parenthesis are regular expressions and can cause downstream issues with codes. 

<img width="215" alt="image" src="https://user-images.githubusercontent.com/130137674/230892009-ced8b603-2445-423d-9bde-a6e712f27d3f.png">

2.  I always run data.isnull().sum().sort_values() to determine which dataset has missing (NaN) data.  This is to correct for data errors and to understand if missing data should be totally removed from the analysis.  In the end, I replaced all NaN entries in the follow categories with "never". 
data[['Bar','RestaurantLessThan20', 'CarryAway', 'Restaurant20To50','CoffeeHouse']]= data[['Bar','RestaurantLessThan20', 'CarryAway', 'Restaurant20To50','CoffeeHouse']].fillna('never')

<img width="344" alt="image" src="https://user-images.githubusercontent.com/130137674/230891969-6170d68b-e05e-44bc-af2a-19db6ff26543.png">


3. There are missing data in "car" but i ignored it as there was not a lot to go by with car data and it was not used in this analysis.


## Analysis

1.  I first studied the count of coupon being given out and there was at least 1500 coupons given out per category.  This implies we have sufficient data to make conclusive analysis.  

![image](https://user-images.githubusercontent.com/130137674/230888293-8bf5138f-979e-4c0f-bfb3-d6fc44ebef06.png)

2.  Then, I studied the acceptance rate (%) by coupon type.  Since bar coupon acceptance and restaurant(20-50), hereby, referred to as expensive restaurant,  was the lowest, i decided to investigate these 2 coupon types.  

<img width="505" alt="image" src="https://user-images.githubusercontent.com/130137674/230888436-439accb9-e580-49e1-93ab-4d7166ed4f1c.png">

### Bar Coupon Acceptance 

3.  A few plots were made to determine which factors have the highest impact on the bar coupon acceptance.  

![image](https://user-images.githubusercontent.com/130137674/230888142-3470c911-3c6b-4d93-a2ac-87d4218b980e.png)

Based on the plots, i hypothesise that bar coupon acceptance are dependent on the number of bar visits and presence of kids as passenger.  

#### Bar visits

4.  The bar coupon acceptance rate generally increases with the number of previous bar visits.  The acceptance rate is more than 70% when the number of bar visits is at least 4.

<img width="448" alt="image" src="https://user-images.githubusercontent.com/130137674/230889715-03739d5f-0424-4368-b536-e850124d1ec5.png">

#### Studying the reverse why driver do not accept the bar coupon 

5.  Intead of focusing on just the factors affecting the acceptance of bar coupons, I hypothesised that given a driver has been to a bar at least 4 times, the reason for him/ her to reject the bar coupon was due to either (a) the presence of kid(s) as passenger or (b) the location of the pub was not "along the walk" and hence, inconvenient to get to. First, we filter out the drivers who have been to a bar at least 4 times accepted the bar coupon.  

(a)  The first hypothesis that the bar coupon was rejected due to kids was not correct.  In fact, most of them were alone.  This could suggest that most people enjoy having a glass with others instead of being alone.  

<img width="424" alt="image" src="https://user-images.githubusercontent.com/130137674/231116774-57b8d1c0-3e69-4311-a6ea-a1f8c613b00a.png">

(b) However, the second hypothesis that the bar coupon was rejected due to its being in the same direction as where the car was driving to appears to be correct.  In fact, 961 out of 1,190 (~81%) was heading in the opposite directions.   

<img width="287" alt="image" src="https://user-images.githubusercontent.com/130137674/231118735-e01b189b-78d8-455f-9fed-b42e51f4e16e.png">

#### Presence of kids as passengers 

6.  Often, a driver is less likely to accept the bar coupon if they had passengers who are kids. this is likely a reflection of social behaviour where adults try to disassociate kids from "drinking holes".

<img width="448" alt="image" src="https://user-images.githubusercontent.com/130137674/230890074-9f9d529e-24c1-4ed4-8d50-89572a26822e.png">

Indeed, the driver is more likely to accept the bar coupon when they are with friends and least likely to accept when they are with kids.  

### Expensive Restaurant Coupon Acceptance

7.  Since the results of bar coupon shows that the coupon acceptance rate generally increases with the number of previous bar visits, we first check if the same observation applies to expensive restaurant.  

<img width="463" alt="image" src="https://user-images.githubusercontent.com/130137674/230890497-07e95b6e-6269-4a27-bdf8-97488b05f71d.png">

Indeed, those who have been to expensive restaurants at least 4 times are more likely to accept the coupon.  This observation is consistent with that for bar coupons.

8.  Because the restaurant is relatively expensive, we further hypothesis that income has an impact on the coupon acceptance rate.  

<img width="455" alt="image" src="https://user-images.githubusercontent.com/130137674/230890861-b5221208-6db0-4118-92be-e34c7536059c.png">

As I could not observe a discerning pattern, I re-categorise the income into 2 groups, i.e. <$50,000 income and income of at least $50,000.  

<img width="449" alt="image" src="https://user-images.githubusercontent.com/130137674/230890983-ceb5f4ab-cfc0-493a-890e-ad346aeb9478.png">

Interestingly, it appears that those of lower and higher income are equally likely to accept the expensive restaurant coupon.  

9.  In the case of bar, it appears that having kids as passenger affects their decision to visit the bar.  While I do not think the same observation will be made, I nevertheless, decided to test for this. 

<img width="448" alt="image" src="https://user-images.githubusercontent.com/130137674/230891078-2becb304-2e2a-4774-8857-96c5e45be81c.png">

Interestingly, a person is more likely to go to an expensive restaurant with their partner while they are more likely to visit a bar with their friends.  In both scenarios, presence of kids as passengers reduces their chance of accepting the coupon. 

10.  Of course, more analyses were done with other factors but the results were not presented as the outcome was not as significant.  

# Conclusions

1. In general, to encourage coupon acceptance, we should give bar/ expensive restaurant coupons to drivers who have been to such places at least 4 times. 

2. Second, we should observe the passenger.  If there are kids, it is to give them pub coupons.  If the drivers are with their partners, then it is ideal that expensive restaurant coupons be given.

3. Third, we try to reduce the need for the driver to detour.  As far as possible, the direction of the pub/ restaurant should be in the same direction as his/ her intended destination.  Convenience appears to be a key motivation for their acceptance of the coupon.  
