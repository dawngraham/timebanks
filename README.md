# Timebanks
> Author: [Dawn Graham](https://dawngraham.github.io/)

## Problem Statement
Timebanking can help individual and community needs be met without relying on money, markets, or the state. What are the best predictors of an active timebank?

## Goal
Build a regression model to predict the number of average daily exchanges in timebanks using the TimeBanks.org platform.


## Data Collection
This project is focused on data from [TimeBanks USA](https://timebanks.org/). While there are other platforms with more timebanks listed (such as [hOurworld](http://hourworld.org/) and [Community Exchange System](https://www.community-exchange.org)), they do not have as much centralized and publicly accessible information about individual timebanks.  

- I created a [Directory Scraper](/notebooks/01_tb_scrape_directory.ipynb) to scrape all pages of the TimeBanks.org directory to get an initial listing of all timebanks on the platform.
- I then created a [Daily Scraper](/notebooks/02_tb_daily_scraper.ipynb) to get updates on the following at the beginning of each day:
	- Numbers for exchanges, hours, members, offers, requests, last exchange
	- Offers, requests, and talents by category
	- All offer and request listings
- I manually gathered data from Facebook and Twitter accounts that were included on timebank pages. This decision was originally prompted by Facebook's restrictions on automated data collection. However, this became an opportunity to learn more about the different timebanks and was feasible given the relatively small number of timebanks with social media accounts.
- I retrieved geolocation and census data for locations of U.S. timebanks using [Geocodio](https://www.geocod.io/). I gathered population estimates for New Zealand from [Stats NZ](https://www.stats.govt.nz/). These will be used for future developments with the project.

See the [Data README.md](/data/README.md) for a description of all data files included in this repo, as well as the data dictionary for the features and target used in modeling.

![Map of timebanks](./images/map.png)
*Map of timebanks on TimeBanks USA (Source: [TimeBanks USA Directory](http://community.timebanks.org/))*

## Processing & Feature Engineering
See the [Cleaning & Feature Engineering notebook](/notebooks/03_tb_cleaning_engineering.ipynb) for full details. In this notebook, I generate new features and combine collected data into a single file to use for modeling.

I got compound sentiment scores (using VADER's `SentimentIntensityAnalyzer`) and word counts for each timebank's mission statement and notes. Dummy variables indicate if each timebank has a phone number, sponsor, secondary website, Facebook account, or Twitter account listed. I also calculated the averages for the number of daily exchanges, number of hours exchanged each day, hours per exchange, number of offers, number of requests, ratio of offers to requests, and number of new members each day. Additional features included the number of parent categories for each timebank, total categories (parent and child), the percent of categories with offers and requests, and total talents per parent category per member.

## Exploratory Data Analysis
Working on this project has been a highly iterative process. I used exploratory data analysis throughout data collection, processing, feature engineering, etc. The [EDA notebook](/notebooks/04_tb_eda.ipynb) contains analysis and visualization of the combined data that will be used for modeling. This includes a look at data for ALL timebanks compared to INACTIVE and ACTIVE timebanks. From this, I anticipated that creating separate models for all timebanks and active timebanks would provide helpful insights.

|![Boxplots for ALL timebanks](./images/boxplots_all.png) | ![Boxplots for ACTIVE timebanks](./images/boxplots_active.png) |
|---|---|
|![Boxplots for ALL timebanks](./images/boxplots2_all.png) | ![Boxplots for ACTIVE timebanks](./images/boxplots2_active.png) |
|<center>**Correlation Heatmap for ALL Timebanks**<br></center>![Boxplots for ALL timebanks](./images/heatmap_all.png) | <center>**Correlation Heatmap for ACTIVE Timebanks**<br></center>![Boxplots for ACTIVE timebanks](./images/heatmap_active.png) |



## An Invitation
Thoughts and feedback are always appreciated. Please feel free to get in touch. Get up-to-date contact info at [dawngraham.github.io](https://dawngraham.github.io/).