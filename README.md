# web-services-behaviour-research
Attributes combination pattern - web services research


### Background
Distributed Web Services make use of many cloud resources located in different geographic regions. Each region has
several “scale units”, which are copies made for the purpose of serving the variable demand on the
service.
To track the health state and identify malfunctioning parts of a web service, its owners collect a large
amount of telemetry data about different aspects of the functionality of the service, such as:
The duration of a user’s request, did the request fail and many more

### The problem
We have a CSV file contains data of exceptions that occurred in a time span of 6 hours in a web
application. Each row represents a single exception. The row data consists of a few selected attributes
of the exception, such as the geographical region in which the exception occurred, the application’s
deployment ID, etc.

### The goal
Since that spike indicates a problem that affects its customers, the service owner wishes to track the
problem’s source in order to fix it.
We'll help the service owner by finding the highest spike and developing an algorithm that ranks different combinations
of attributes according to how they may explain what happened during the spike.

### The Algorithm
Each column in our data represents a feature that can directly or indirectly affect the amount of service errors / exceptions. each record is indexed according to the exact time the error occurred (timestamp).

We'll separate our data into two different tables - the features of all the data and the features that occurred during a 5-minute window in which the number of errors was the highest.

As noted, each row (record) in each of the above tables represents a possible combination that affects the number of errors. each feature in the table may or may not appear in a particular combination, so we run at `2^number_of_features`.

We'll produce 2 new tables that represent the number of impressions of all the possible combinations of the given data set. One will represent all the data and the other the data given in the time period (5 minutes) in which the number of errors / exceptions was highest.

We then combine the two tables into a single table that will represent impressions of combinations in all of our data. features that didn't appear in a particular combination will become 0 (in order to maintain uniformity of the data).

We'll create two random variables (indicators), the first representing features found (or not) in the specific time window in question (5 minutes spike), while the second represents whether there is a relevant combination or not.
The common variance will represent the relationship between the two indicators that should be the basis for calculating the score of each combination.

`p` represents a table of probabilities for each combination calculated by dividing the common variance by the square root of the two indicators, or, in short, a [correlation](https://en.wikipedia.org/wiki/Correlation_and_dependence). The correlation represents a statistical relationship between these two random variables.
The values of the correlation coefficients usually range from 1 to -1. A value of 0 usually indicates a lack of connection between the indicators (in the sense in which the relationship was defined), and values of 1 or -1 usually indicate a complete relationship between the indicators.

Finally we will present the 10 combinations with the strongest connections that may be a source of problems presented first.