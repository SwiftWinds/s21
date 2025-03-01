---
layout: lab
num: lab04	
ready: true
desc: "New type and predicates (for sorting)"
assigned: 2021-04-27 
due: 2021-05-05 23:59
github_org_url: https://github.com/ucsb-cs32-s21
---

Goals
=====

Learning objectives. At the end of this lab, students should be able to:

- read example C++ code and understand the data representations used in the base code
- apply OO design and implementation to a real world data set
- design and implement a new class to represent police shootings at two regional levels (county and state).  State level data requires aggregation.
- use a new data representation for racial demographics (including integrating this data into exisiting code and classes)
- aggregate regional data to state level data using a hashmap
- implement various *compare predicates* to sort state demographic data on various data fields. For example, list the top 10 states in order based on highest number of police shooting incidents and print out the demographic data for these states or list the 5 states with the lowest percentage of people below the poverty line and the associated police shooting information for those states


Modality
============
This is a pair lab.  You may choose to work alone or with another student.  When working in a pair, please always follow good pair programming methodologies.  In general, set aside time to work together and share your screen to share the work and learning.  

You will need to identify you partner (pair) in your submission (add a header to your main with both your names!).  The remaining labs will also be optional pair labs and you will be asked to only partner with the same person TWICE.

Orientation
============

In order to expand the questions/queries about the US regional data we have been working with, we will be adding additional data to our data project. Over the past year there has been a rising awareness about tragic encounters when policing authorities use deadly force resulting in the death of civilians. There is a growing national dialogue around these incidents and the related civil rights issues. Each of these incidents is tragic and complex. We will be working with data documenting police shootings to try to understand regional and demographic trends, in conjunction with the US census data already a part of our project. As computer scientists, we can use computing to help reveal patterns and information in data. 

We are working with police shooting data in the spirit of applying technical tools to a socially relevant data set. Specifically, we will be adding regional data reporting of police shootings (which is present in the CORGIS datasets: https://corgis-edu.github.io/corgis/csv/police_shootings/), however, there is a more up to date version of this data, maintained by the Washington Post that has been cleaned by UCSB students working on a related data project (thanks to: JO & BZF). The exact csv file we will be using is included in the STARTER code and is called: police_shootings_cleaned.csv. We will use the cleaned file for consistency, however, I encourage you to read more about the Washington Post's project here: https://www.washingtonpost.com/graphics/investigations/police-shootings-database/.

The data captured by the Washington Post includes several data fields relevant to each incident, and the data does have missing fields (as expected in 'real world' data). Part of working with such data in these labs is to gain experience with imperfect data. For this lab, we will focus on creating a C++ class representation for the following data fields:<br>
```
name, age, gender, race, county, state, signs_of_mental_illness, flee
``` 
<br>
Note there is other data associated with each incident, but you will need to design a class to represent the specified data listed above. Name the class psData. We will be representing and aggregating this data into state level information (similar to lab03), called psState. This link to the original WP data, explains the data fields: https://github.com/washingtonpost/data-police-shootings - however, the city field has been replaced with county in the cleaned csv you are asked to work with in this lab (this is due to issues with the original location information in the original data - i.e. cities were not always formal city names: 'Hollywood Hills' vs. Los Angeles - which has been handled for you in the cleaned data mapped to counties).

<br>
An important data component for both the demographic census data and the police shooting data is racial demographic data. To support this, you are provided with a new (mostly complete) data type raceDemogData. Personal and community identity with respect to race and ethnicity are complex topics. This page describes the categories used in the US Census and some reasons why this data is collected: https://www.census.gov/topics/population/race/about.html
Developing an understanding of civil rights issues faced by our nation involves representing and examining data related to race. One of your tasks for this lab is to integrate this data type into your code for both demographic data and the aggregated (state) police shooting data.

Once you have expanded your code to represent this new data (police shootings at the county level and aggregated to the state level) and you have integrated the raceDemogData, we will practice using the standard sorting algorithm, by writing some specified compare predicates to sort the data on various fields.  Your program will also need to report various data findings.  *For example, list the top 10 states in order based on highest number of police shooting incidents and print out the demographic data for these states or list the 5 states with the lowest percentage of people below the poverty line and the associated police shooting information for those states

To support reading in the new csv data (police_shootings_cleaned.csv), there is a new parse.cpp and parse.h, which reads in this new data (it is incomplete until you design and integrate the new types).  *Note that if you look carefully at parse.cpp you will see that there is a good deal of redundancy in the functions for reading police data or demographic data. This is something that will be addressed in next week's lab.*

Tasks
============
<p>Starting with your lab03 code take a look at the new files and integrate the new files (and modified files): https://github.com/ucsb-cs32-s21/Lab04-STARTER
 Then you will need to work through the following tasks (you may tackle them in the order that makes sense to you:
</p>
    
**Task 0:** 
<p>
Familiarize yourself with the **raceDemoData.h/cpp** and be sure you understand what it represents.  You will need to complete the one operator overload that is not complete in raceDemogData.h (clearly listed).  In general, this data type is intended to represent the racial counts for a specific total population.  It is not a data type for one person's race, but the counts of various racial identities for an entire population.  Most of the categories are derived from the US census, however, as the police shooting data has some variance (for example the police shooting uses *other* and the US census does not, this class has combined representation for the racial categories used in both the regional data sources we are using (US census and Washington Post shooting database).<br>
</p>
    
**Task 1:** 
<p>
    Design and implment the Police Data representation in **psData.h/cpp** for individual incidents.  This must represent the following data for each incident: name, age, gender, race, county, state, signs_of_mental_illness, flee.  You need to make decisions about types to use to represent these data fields, however, make sure you understand the related constraints and reporting necessities in later steps of this lab.  Also look at psState.h and be sure you understand all the getters you will need to support for the aggregated psData.  They may imply some specific type representations for some of this data.<br>
</p>
    
**Task 2:** 
<p>Design the state level Police Data representation in **psState.h/cpp**.  This aggregated data will be similar to the individual incident data, however, as it represents aggregate data, you will need to make decisions about how to handle the aggregated data.  For example, instead of just representing the race of a single incident victim, you will want the state level police data to include a field for raceDemogData (that is the racial counts for a demographic population; in this case the population is the victims of police shootings aggregated to a state level).  See below list for exact getter methods that must be supported and be sure to consider what design choices you want to make in deciding on variables and types associated with this class.<br>

State level police class must support the following methods:</p>


```
        int getNumMentalI();
        int getFleeingCount();
        int getCasesOver65();
        int getCasesUnder18();
        raceDemogData getRacialData();
        int getnumMales();
        int getnumFemales();
        int getNumberOfCases();
        string getState();
```

For 'fleeing' - an incident is counted as fleeing if it is logged in the police shooting incident report as: "Foot", "Car" or "Other"
    
 **Task 3:** 
 <p>
Modify **parse.cpp** to support the use of the new data types (add raceDemogData to demogData) and fill in any code to read all necessary data for police incidents and construct policeData properly.<br>
</p>

    
 **Task 4:**
 <p>
Modify **dataAQ.h/cpp** to support aggregating the police data to the the state level. This code may look very similar to what you completed for lab03.  Note that to distinguish between the two functions, dataAQ should now support the following two methods (included in the STARTER code dataAQ.h):
 </p>


```
    void createStateDemogData(std::vector<shared_ptr<demogData>> theData);
    void createStatePoliceData(std::vector<shared_ptr<psData>> theData);
```

Similar to lab03, add any data members to dataAQ that you need to store and aggregate the state data.  Also add the getter:


```
    shared_ptr<psState> getStatePoliceData(string stateName);  
```
<p>
Add methods to **dataAQ.h/cpp** in order to use sort to sort state data based on various criteria and report.  Specifically, write the required compare predicates and use sort in each of the following methods:<br>
 </p>

 ```
     //sort and report the top ten states in terms of number of police shootings 
    void reportTopTenStatesPS();
    //sort and report the top ten states with largest population below poverty 
    void reportTopTenStatesBP();
 ```

Note that each of these methods should sort the full data, print a short report on the top 10 (see below) and write out a full listing for both the police shooting and demographic data for the top three. Example output would look like the following with all blanks filled in (prior example included the output string from testBPSort - now removed for clarity):

```
Top ten states sorted on Below Poverty data & the associated police shooting data:
MS
Total population: 2994079
Percent below poverty: 22.63
Police shooting incidents: 75
--
Total population: --
Percent below poverty: --
Police shooting incidents: --
--
Total population: --
Percent below poverty: --
Police shooting incidents: --
...//print all ten
...
**Full listings for top 3 Below Poverty data & the associated police shooting data for top 3:
State Info: MS
Number of Counties: 82
Population info: 
(over 65): 14.31% and total: 428361
(under 18): 24.42% and total: 731075
(under 5): 6.48% and total: 193957
Education info: 
(Bachelor or more): 20.43% and total: 611741
(high school or more): 81.62% and total: 2443639
persons below poverty: 22.63% and total: 677656
community racial demographics: Racial Demographics Info: 
% American Indian and Alaska Native percent: 0.58 count: 17264
% Asian American percent: 1.02 count: 30490
% Black/African American percent: 37.50 count: 1122847
% Hispanic or Latinx percent: 2.97 count: 88779
% Native Hawaiian and Other Pacific Islander percent: 0.05 count: 1628
% Two or More Races percent: 1.17 count: 35124
% White (inclusive) percent: 59.66 count: 1786119
% White (nonHispanic) percent: 57.26 count: 1714550
total Racial Demographic Count: 2994079
Total population: 2994079
**Police shooting incidents:State Info: MS
Number of incidents: 75
Incidents with age 
(over 65): 2
(19 to 64): 64
(under 18): 9
Incidents involving fleeing: 31
Incidents involving mental illness: 12
Male incidents: 73 female incidents: 2
Racial demographics of state incidents: Racial Demographics Info: 
% American Indian and Alaska Native count: 0
% Asian American percent: 1.33 count: 1
% Black/African American percent: 34.67 count: 26
% Hispanic or Latinx percent: 1.33 count: 1
% Native Hawaiian and Other Pacific Islander count: 0
% Two or More Races count: 0
% White (inclusive) percent: 56.00 count: 42
% White (nonHispanic) percent: 56.00 count: 42
% Other percent: 6.67 count: 5
total Racial Demographic Count: 75

```

Note that the starter code has partial test cases, that will then have further assessment conducted on gradescope.
    
 **Task 5:**
 <p>
 Modify **main.cpp** in order to read in, create and aggregate all data and confirm that your output can match the example below.<br>
</p>


Example report
============
For your reference, here is a partial example reporting of expected output from the overloaded stream operators for stateDemog and statePolice data:

```
State Demographic Info: State Info: AK
Number of Counties: 29
Population info: 
(over 65): 9.44% and total: 69541
(under 18): 25.32% and total: 186508
(under 5): 7.45% and total: 54857
Education info: 
(Bachelor or more): 27.29% and total: 201059
(high school or more): 91.51% and total: 674168
persons below poverty: 9.86% and total: 72611
community racial demographics: Racial Demographics Info: 
% American Indian and Alaska Native percent: 14.79 count: 108971
% Asian American percent: 6.06 count: 44679
% Black/African American percent: 3.90 count: 28745
% Hispanic or Latinx percent: 6.81 count: 50165
% Native Hawaiian and Other Pacific Islander percent: 1.27 count: 9367
% Two or More Races percent: 7.10 count: 52282
% White (inclusive) percent: 66.86 count: 492593
% White (nonHispanic) percent: 61.95 count: 456415
total Racial Demographic Count: 736732
Total population: 736732
**Police shooting incidents:State Info: AK
Number of incidents: 44
Incidents with age 
(over 65): 0
(19 to 64): 42
(under 18): 2
Incidents involving fleeing: 18
Incidents involving mental illness: 7
Male incidents: 42 female incidents: 2
Racial demographics of state incidents: Racial Demographics Info: 
% American Indian and Alaska Native percent: 20.45 count: 9
% Asian American percent: 4.55 count: 2
% Black/African American percent: 6.82 count: 3
% Hispanic or Latinx count: 0
% Native Hawaiian and Other Pacific Islander count: 0
% Two or More Races count: 0
% White (inclusive) percent: 61.36 count: 27
% White (nonHispanic) percent: 61.36 count: 27
% Other percent: 6.82 count: 3
total Racial Demographic Count: 44
```

Note that 'W' race reported in the police shooting data is added to both White (inclusive) and White (nonHispanic), however, racial demographic count should only add one member to the community for each category.  *In addition, any empty race reportings from police shootings should be counted as other for the Lab 04*

    
Grading
================
(70) autograder tests passed (GS):  https://www.gradescope.com/courses/259718/assignments/1218415/submissions <br>
(5) code review reasonable psData <br>
(10) code review reasonable psState (aggregation and representation - data type use, method design) <br>
(5) racial demographic data integration<br>
(10) use of compare predicates and use of sort (sort report in general fits specs)<br>



    
Acknowledgements
================

Spring`21 vesion: Zoë Wood.  Thank you to JO and BZF for assistance with the data exploration project and the cleaned Washington Post data.

