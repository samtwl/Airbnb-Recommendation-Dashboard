# Airbnb-Recommendation-Dashboard
This repository contains the code and methodology for building a recommendation app to recommend listings to users based on experiences of other similar users.

# 1.0 Introduction
Airbnb is an online marketplace and hospitality service where people can rent short-term lodging such as apartments, hostel beds, hotel rooms and cottages. People can also organise or participate in holiday activities and experiences such as walking tours, concerts, workshops and restaurant dining. There are more than 4 million accommodation listings on Airbnb in 191 countries and 65000 cities, with over 260 million check-ins facilitated (“Airbnb - Wikipedia,” n.d.).

Airbnb can be accessed via its website or mobile apps. Accommodation listings are generated when users search by destination and use filters such as Dates, No. of Guests, Home Type, Price and Trip Type. Airbnb is popular among travellers, especially those who are budget-conscious, because of its various advantages. For example, travellers have the option to stay in an entire apartment which offers greater flexibility compared to a hotel room. They are also able to have a more authentic travel experience by staying in a local’s home, and prices are generally lower than hotels (“10 Pros and Cons Of Using Airbnb,” 2017).

One downside of relying on Airbnb for travel planning, however, is that listings can be fully booked very quickly, especially those in desirable locations and during peak travel periods (“10 Pros and Cons Of Using Airbnb,” 2017). In addition, travellers would have to look through reviews of listings carefully to ensure safety and security as well as to be better informed about the amenities provided by the host. 

This project aims to build an interactive dashboard for Airbnb users that not only addresses the shortcomings of the platform, but also value-adds to the Airbnb experience. It makes use of datasets provided by Inside Airbnb, which are sourced from publicly available information from the Airbnb site (“Inside Airbnb. Adding data to the debate,” n.d.). These datasets include detailed information on listings and reviews by Airbnb users, for a number of cities and countries. The project focuses on accommodation listings in London.  

# 2.0 Overall Concept
The first part of our application addresses the problem of availability by providing statistics that inform users about listing availability according to the time of the year. On top of that, statistics are provided about the price of listings and overall review scores, based on characteristics such as location. This aims to help users make better decisions concerning their travel plans. 

Our application also addresses the problem of users spending time looking through individual reviews of listings by generating a word cloud featuring the most common words used to describe particular listings in user reviews. Additionally, our application aims to ease users’ selection of listings simply by implementing a Sentiment Analysis which aids users in identifying the proportion of positive or negative words spoken about the selected listing.

Finally, our application takes advantage of the community marketplace feature of Airbnb by providing a recommendation system that recommends listings to users based on the experiences of other similar users, using the user-based collaborative filtering technique. This aims to help users in decision-making by narrowing down the number of listings to consider.

# 3.0 Data Sources
The following datasets for London are used in the scope of this project: 

***Detailed listings data (listings.csv.gz)***
This dataset includes information on London listings. The variables are:
![report1](https://user-images.githubusercontent.com/50171205/60257931-ad6d8480-9906-11e9-87e3-0de04f2ec8e2.png)

***Detailed calendar data for listings (calendar.csv.gz)***
This dataset includes information on available dates and price for each London listing. The variables are:
![report2](https://user-images.githubusercontent.com/50171205/60257932-ad6d8480-9906-11e9-8350-4fdcef3cf27e.png)

***Detailed review data for listings (reviews.csv.gz)***
This dataset includes information on reviews given for listings. There are 6 variables: 
![report3](https://user-images.githubusercontent.com/50171205/60257934-ae061b00-9906-11e9-8578-f701af044e4d.png)

# 4.0 Specific Methodology
**Data Preparation**
First, the datasets are cleaned in R using the dplyr library. As the listings dataset consists of 96 variables, too many for analysis, variables that will not be used are removed. Rows with missing values are then removed. 

Each row in the the calendar dataset represents a particular listing on a single date of the year and its availability and price. As a result, there are millions of rows. Since we are only interested in looking at availability and prices by month, the dataset is modified by filtering only the dates during which listings are available, creating a new variable for month and removing duplicates which exist because some listings have different prices within the same month. The variables in the new dataset are the unique listing ID, price and corresponding month. 

The price variable is recognized as a factor data type due to the “$” included in the data, and is converted to numeric data type in order for analysis to be carried out. 

**Exploratory Data Analysis**
With the clean datasets, we carry out exploratory data analysis to reveal insights on the prices and availability of the listings based on the different months of the year, location, type of accommodation, using the ggplot2 library. London neighbourhoods with the highest overall review scores are identified. We also found the top 10 listings with the most number of reviews, which is the best indicator for popularity.

**Multivariate Analysis**
Multivariate analysis is carried out to explore the correlation between price and a number of variables: overall review scores, number of review per month, number of guests allowed, number of bathrooms, number of bedrooms and number of beds. 

**Multiple Linear Regression**
Multiple linear regression is carried out to determine the relationship between the price of listings and the number of guests, bedrooms, bathrooms and beds. 

**Word Cloud**
A Word Cloud is generated to provide a visual representation on the frequency of the words used. In this case, our word cloud serves to highlight popular terms used by hosts to communicate important information to potential users. Words are highlighted and depicted in different sizes depending on the frequency of use - The larger the size of the word, the higher its prominence and usage. In this case, our word cloud will allow users to identify the most spoken/written words by the owners of the accommodations by selecting two options: (1) Property Type; and (2) Room Type.

Additionally, a bar chart is also generated to provide an alternative visualization of word frequency by plotting the most spoken words against the number of times the words appear in total.

**Collaborative Filtering Recommender System**
In order to provide a comprehensive system where users are not only able to explore the visualized historical data from Airbnb, we have implemented an interactive system where users will be able to obtain recommendations on their next stay with Airbnb. Recommended accommodations are suggested based on the accommodations’ review ratings given by other users (similar users) with similar preferences. First, the system will identify these similar users with similar preferences by distinguishing users who have stayed in the same accommodations as our user, who is using this recommender system (current user). Thereafter, the system will identify the accommodations (selected accommodations) that these similar users have stayed in that the current user have yet to stay in. The mean review ratings of these selected accommodations will be calculated. The highest rated accommodation will thus be the top ranking recommended accommodation for our current user.

**Sentiment Analysis**
In order to ensure that users will be able to infer the emotional content of the text, we will employ sentiment analysis, where scores will be assigned for the positive and negative words such that they will be appropriately tabulated and characterised accordingly. This will allow users to obtain greater inferences and thus be able to understand better on how they can use the number of sentiment gathered to make a better decision on their accommodation choice.

**Putting It Together In Shiny**
In order to optimize user experience in visualising Airbnb’s data, we have integrated all aspects of exploratory and inferential analysis into R Shiny to ensure effective presentation. First, we want users to be able to visualise a general overview of the available listings by property type and room type in a singular tab. 

Next, we want to let users be able to use the data in consideration of their next stay. Therefore, we have placed essential data sets under a second tab labelled “Plan you Staycation Here!” to allow users to determine the best price point and to identify the availability of listings. We have also included review scores assigned to listings by other users by the type of neighbourhood the listings are located in.

Lastly, users who are not fond of planning and considering multiple factors in their staycation but still want to stay in a listing to their liking, will find the most value in our last tab in the Shiny application. They will be able to obtain recommendations on the best listing and the general sentiment of the recommended listing based on past users’ reviews.

# 5.0 Results and Findings
**Exploratory Data Analysis**
An overview of the application is as follows: 
![report5](https://user-images.githubusercontent.com/50171205/60257936-ae9eb180-9906-11e9-8279-9bd7feae2757.png)

The first tab is “Dashboard”. Here, on the “Top 10 Listings” sub-tab, users are able to view the Top 10 listings by number of reviews. Most of these listings are located in central London. 
![report6](https://user-images.githubusercontent.com/50171205/60257937-af374800-9906-11e9-935e-7adfec3e8168.png)

At the Overall View of Property sub-tab, users are able to view the distribution of property types and room types in London. Most listings in London are apartments, and offer private rooms or entire apartments for rent. 
![report7](https://user-images.githubusercontent.com/50171205/60257940-af374800-9906-11e9-8e38-293b47082f6b.png)

The next main tab, “Plan Your Staycation Here!” is where users can select a property and room type of their preference, followed by their accommodation budget and see various statistics relating to their choice. 

The sub-tab “Price Dynamics” shows the the distribution of prices and availability of rooms throughout the year. As shown above, for a price range of between $120 and $560 a night for an entire apartment, December is the most expensive month while March has the most number of available listings. This is expected, since December is the peak travel period for most destinations. 
![report8](https://user-images.githubusercontent.com/50171205/60257941-afcfde80-9906-11e9-9e0b-5adbe7600f7a.png)

The next sub-tab “Neighbourhood” shows the number of entire apartment listings by neighbourhood, according to the chosen price range of between $120 and $560 per night. Westminster has the most number of such listings, and it has the highest average accommodation prices. 
![report9](https://user-images.githubusercontent.com/50171205/60257943-afcfde80-9906-11e9-8cce-6f68a9d63463.png)

The “Scores” sub-tab shows the distribution of review scores across neighbourhoods in London. There are overall review scores and individual review scores for accuracy, cleanliness, check-in, communication and location.  

**Word Cloud**
![report10](https://user-images.githubusercontent.com/50171205/60257944-b0687500-9906-11e9-9448-52b589e86d12.png)

When a user selects “All” for both Property Type and Room Type, the most spoken word by owners is “London”, as it appears for a total of more than 250,000 times. The second most spoken word is “Flat”, which appears in an approximately 100,000 times.

On the other hand, when a user selects “Apartment” under Property Type while Room Type remains as “All”, the top two most spoken words are still “London”, at a word frequency of approximately 200,000 words, and “Flat”, at a word frequency of approximately 180,000 words. The third most frequently spoken word is “Apartment”, which was spoken approximately 90,000 times.

**Collaborative Filtering Recommender System**
Upon implementation of the Recommender System, a user, who must already be an Airbnb user and has stayed in at least one accommodation, can input his unique identification number in order for the system to retrieve accommodations recommendations for this user. Upon keying in his unique identification number, the system will churn out the top five accommodations based on the average of review ratings that have been given by other users who have stayed in similar accommodations as our current user. 
![report11](https://user-images.githubusercontent.com/50171205/60257945-b0687500-9906-11e9-9b1c-dccde6fcf9d6.png)

Of the listing of the top five accommodation recommendation, the following information of the recommended accommodations will be available for our current user:
1.	Accommodation unique identification number
2.	Image of the accommodation
3.	Name of the accommodation
4.	Neighbourhood of the accommodation
5.	Room type
6.	Number of people it accommodates
7.	Price of the accommodation

![report12](https://user-images.githubusercontent.com/50171205/60257947-b1010b80-9906-11e9-9332-f13aa2c17e57.png)

Thereafter, based on this information, the user will also be able to check out the “ConnectionPlot”, where he/she will be able to identify the number of other users who have similar preferences as him/her. 

**Sentiment Analysis**
From the results that we have obtained from the Collaborative Filtering Recommender System, users are able to select their preferred listing and obtain the results of sentiment analysis for that listing.
![report13](https://user-images.githubusercontent.com/50171205/60257948-b1010b80-9906-11e9-8763-306222a6be21.png)

# 6.0	Conclusion / Summary 
The interactive dashboard serves to maximise consumer value creation by simplifying, calibrating and consolidating Airbnb’s historical data to provide fascinating insights on the travel and booking habits of past users, and the characteristics of all listed properties. To facilitate our users in identifying the best accommodation choice, the dashboard also provides an in-depth analysis on the best time to travel based on optimal price points, accommodation availability and the host’s textual description of his listing. 

Alternatively, to alleviate our users from all the vexatious difficulties in finding the perfect place to stay in, we have integrated a recommender service for our users to pick their preferred listing with ease.

All in all, our application provide a well-rounded service to different users who have difference preferences in travel planning.

# 7.0	References
10 Pros and Cons Of Using Airbnb. (2017). Retrieved April 11, 2018, from https://www.fitnancials.com/10-pros-and-cons-of-using-airbnb/

Airbnb - Wikipedia. (n.d.). Retrieved April 11, 2018, from https://en.wikipedia.org/wiki/Airbnb

Inside Airbnb. Adding data to the debate. (n.d.). Retrieved April 11, 2018, from http://insideairbnb.com

# Appendix I
The images below shows an executive summary of this project, and all tabs in the interactive dashboard.

![ab1](https://user-images.githubusercontent.com/50171205/60255682-d2f88f00-9902-11e9-8b6d-fe702f60d44e.png)

![ab2](https://user-images.githubusercontent.com/50171205/60255684-d3912580-9902-11e9-95e2-aca8e477c1c3.png)

![ab3](https://user-images.githubusercontent.com/50171205/60258651-25887a00-9908-11e9-9d3c-610dcab31294.png)

![ab4](https://user-images.githubusercontent.com/50171205/60255689-d4c25280-9902-11e9-9028-7ce643f5e8ac.png)

![ab5](https://user-images.githubusercontent.com/50171205/60255693-d55ae900-9902-11e9-8e17-6cbafc0b56a7.png)

![ab6](https://user-images.githubusercontent.com/50171205/60255695-d5f37f80-9902-11e9-8693-6be76136c301.png)

![ab7](https://user-images.githubusercontent.com/50171205/60255699-d724ac80-9902-11e9-9956-dee4fe61fca9.png)

![ab8](https://user-images.githubusercontent.com/50171205/60255701-d724ac80-9902-11e9-964d-70196f9ec722.png)

![ab9](https://user-images.githubusercontent.com/50171205/60255702-d7bd4300-9902-11e9-85aa-2561a817bf62.png)

![ab10](https://user-images.githubusercontent.com/50171205/60255707-d855d980-9902-11e9-9b81-8ca0ef66d093.png)

![ab11](https://user-images.githubusercontent.com/50171205/60255708-d855d980-9902-11e9-80cb-5c0764d050fe.png)

![ab12](https://user-images.githubusercontent.com/50171205/60255709-d8ee7000-9902-11e9-95d7-4763d6e77b7a.png)

![ab13](https://user-images.githubusercontent.com/50171205/60255710-d9870680-9902-11e9-9cce-6576d20baff7.png)

![ab14](https://user-images.githubusercontent.com/50171205/60255712-d9870680-9902-11e9-9da6-f159fbf13c8d.png)

![ab15](https://user-images.githubusercontent.com/50171205/60255713-da1f9d00-9902-11e9-97bf-74001daf5b31.png)

![ab16](https://user-images.githubusercontent.com/50171205/60255716-da1f9d00-9902-11e9-84fa-78d0adccae59.png)

![ab17](https://user-images.githubusercontent.com/50171205/60255717-da1f9d00-9902-11e9-842d-779b12ff5a1c.png)

![ab18](https://user-images.githubusercontent.com/50171205/60255719-dab83380-9902-11e9-8535-8503a4dde40a.png)

![ab19](https://user-images.githubusercontent.com/50171205/60255720-db50ca00-9902-11e9-84f9-71a3dc39da82.png)

![ab20](https://user-images.githubusercontent.com/50171205/60255726-dc81f700-9902-11e9-8002-71fb84c499c8.png)

![ab21](https://user-images.githubusercontent.com/50171205/60255729-de4bba80-9902-11e9-9b65-f1af606b6713.png)

![ab22](https://user-images.githubusercontent.com/50171205/60255732-dee45100-9902-11e9-92c7-cb1cb2dacb6c.png)

![ab23](https://user-images.githubusercontent.com/50171205/60255736-e0157e00-9902-11e9-8954-3e4768d5fa91.png)

![ab24](https://user-images.githubusercontent.com/50171205/60255742-e1df4180-9902-11e9-8c41-175002501c5e.png)

![ab25](https://user-images.githubusercontent.com/50171205/60255744-e277d800-9902-11e9-9565-eb44ea6cdc32.png)
