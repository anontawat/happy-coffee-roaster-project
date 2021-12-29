# happy-coffee-roaster-project

This project is a part of Google Data Analytics Professional certificate. 

## Scenario

I am a junior data analyst working for a company for six months. My boss feels that I am ready for more responsibilities. I was asked to lead a project for a brand new client, this will involve everything from finding the business task all the way through presenting my data-driven recommendations. 

## Company Background

Happy Coffee Roaster is a new coffee shop and roastery just landed right next to the Asok area. Happy Coffee Roaster focuses on roasting unique and micro-lot coffee from small producers around the world. They also have both fast and slow-bar coffee for their customers to sit in or take away. With so many competitors in Bangkok, the marketing lead would like to create a promotion to increase sales on the days when there are the fewest customers. Besides, she needs advice on selecting the next lot of good beans to target more budget-conscious home brewers.

## What they asked me to accomplish
Provide a report of recommendations of the followings:
- What days of the week have the fewest visitors?
- The average peak and quiet hours. 
- The highest/ lowest sales volumes in peak hours or quiet hours.
- What is the average basket size? The average number of drinks per basket?
- What time of the day do customers tend to spend more?
- What type of drink should we use to create the promotion?
- What are the top 3 countries that produce the highest average cupping scores? 

The first 6 questions can be answered by using the spreadsheet. For the last question, I used the real data from [this data](https://www.kaggle.com/volpatto/coffee-quality-database-from-cqi) from Kaggle. 

**Here are steps to follow this repo**

1. Check out "**happy_coffee_sales_generator.ipynb**" first. This is a jupyter notebook file contains a Python code to generate sales data. However, in the real-world situation, this may be unnecessary because most of the small businesses should have their own data collected by themselves somehow. Therefore, this generator will produce a random sales data depending on the number of month you input. You can check the generated sales data in this repository (**3_months_happy_coffee_sales_2021.csv**).
2. You can check the **happy_coffee_sales_cleaned.xlsx** to see the steps I took for processing, analysing, and visualising the spreadsheet data to answer most of the questions above. I also create a presentation but I didn't add it to this repository (please let me know if you want!).
3. Lastly, please check **2017_world_coffee_scores.md** used for answering the last question above. The data can also be downloaded in this repo (**arabica_quality_data.csv**).
4. If you would like to check my viz for the last question, please follow [this link](https://public.tableau.com/app/profile/anontawat.wiriyakijja/viz/CoffeeCuppingScore2017/CoffeeScoresDashboard) for a simple dashboard.  
