# Timebanks: Process Log
> Author: [Dawn Graham](https://dawngraham.github.io/)

The purpose of this log is to document the process as I go through this project, including challenges, questions, and ideas.

#### Sections
- [Next Steps](#steps)
- [Updates](#updates)  
- [Challenges](#challenges)
- [Other Notes](#notes)
- [Resources / References](#resources) 

## <a name="steps"></a>Next Steps
### Ideas for Future Developments
- Collect updated directory info regularly to gather new timebank info. (Added 2/8/19)
- Do timeseries analysis once more data is collected. (Added 2/8/19)
- Consider use of active / passive voice in mission statement / notes. (Added 2/4/19)
- Use NLP to take a deeper dive into what requests/offers are exchanged, what goes unmet, and other patterns. (Added 1/21/19)
- Deeper look at connection with language use, framing, and descriptiveness of mission statements and about pages. (Added 1/21/19)
- There are some timebank "hot spots". Should investigate if areas with more in closer proximity also have more activity. (Added 1/21/19)
- Look at demographic info in relation to timebank locations. (Added 1/21/19)

## <a name="updates"></a>Updates

### February 10, 2019
- **Modeling**
	- Tried using sklearn `PolynomialFeatures` to see if it would improve models. It did not.

### February 9, 2019
- **Modeling**
	- Completed initial models using `combined` csv. Tried linear regression, ridge, lasso, elastic net, and decision trees using gridsearch for ALL timebanks and ACTIVE timebanks only. Selected linear regression as best model for both, using backward feature selection.

### February 8, 2019
- **TimeBanks.org purge**
	- Looked into TimeBanks.org [fee information](https://timebanks.org/community-weaver-fee-schedule/). The cost to timebanks are based on number of members, ranging from $0.88 to $30 per member per pay period.
		- Timebank fees are charged on a biannual basis (that is, two times a year), due March 31 and September 30.
		- "The range for your timebank is determined by the number of members, counted at a point in the period Jan 7 to Jan 15 and again in the period July 1 to July 15."
	- My guess is that `crookedriver` (and potentially other timebanks) received their bill due March 31, then dropped inactive members. Some timebanks that had been inactive chose not to renew and were removed from the directory.

### February 7, 2019
- **TimeBanks.org purge**
	- Noticed that membership for `crookedriver` timebank dropped by almost 100 in one day. This may have something to do with the inactive timebank purge that I noticed on February 3. It could also be that the timebank itself dropped inactive members to save on fees.

### February 6, 2019
- **Feature Engineering** - Main dataset now includes:

	| Name | Description |
	| --- | --- |
	| `timebank` | Shortname for timebank. Each **timebank main page** can be accessed at `{timebank}.timebanks.org`.|
	| `sponsor` | 0: No sponsor listed on **timebank directory info page** (the page you are sent to if selecting a timebank from the directory page at `community.timebanks.org`)<br>1: Has sponsor listed |
	| `phone` | 0: No phone number listed on timebank directory info page<br>1: Has phone number listed |
	| `url_2` | 0: No additional website listed on timebank directory info page<br>1: Has additional website listed |
	| `facebook` | 0: No Facebook page listed on timebank main page<br>1: Has Facebook page listed |
	| `twitter` | 0: No Twitter account listed on timebank main page<br>1: Has Twitter account listed |
	| `mission_word_count` | Number of words in statement on mission page at `{timebank}.timebanks.org/mission` |
	| `mission_sentiment` | Compound score from Vader sentiment analysis of statement on mission page, ranging from -1 to 1.<br>-1: Negative sentiment<br>1: Positive sentiment |
	| `notes_word_count` | Number of words in "Notes" section on timebank directory info page. |
	| `notes_sentiment` | Compound score from Vader sentiment analysis of "Notes" section, ranging from -1 to 1.<br>-1: Negative sentiment<br>1: Positive sentiment |
	| `avg_daily_exchanges` | ("Number of Exchanges"  on timebank directory info page on first date of data collection - "Number of Exchanges" on last date of collection) / number of days between first and last date of collection |
	| `avg_daily_hours` | ("Hours Exchanged" on timebank directory info page on first date of data collection - "Hours Exchanged" on last date of collection) / number of days between first and last date of collection |
	| `hours_per_exchange` | `avg_daily_hours` / `avg_daily_exchanges` |
	| `avg_offers` | The mean of "Active Offers" on timebank directory info page for all dates of collection
	| `avg_requests` | The mean of "Active Requests" on timebank directory info page for all dates of collection
	| `offer_request_ratio` | `avg_offers` / `avg_requests` |
	| `members_starting` | "Number of Users" on timebank directory info page on first date of collection |
	| `members_daily_new` | ("Number of Users" on timebank directory info page on last date of collection - `members_starting`) / number of days between first and last date of collection |
	| `facebook_likes` | Number of "Likes" on Facebook page listed on timebank main page |
	| `facebook_followers` | Number of Followers on Facebook page listed on timebank main page |
	| `twitter_tweets` | Number of tweets on Twitter account listed on timebank main page |
	| `twitter_following` | Number of "Following" on Twitter account listed on timebank main page |
	| `twitter_followers` | Number of "Followers" on Twitter account listed on timebank main page |
	| `categories_total` | Total number of unique categories (including parent and child categories) included in "Timebankers' Talents", "Offers by Category", and "Requests by Category" from timebank main page |
	| `categories_parent` | Total number of unique parent categories included in "Timebankers' Talents", "Offers by Category", and "Requests by Category" from timebank main page |
	| `categories_with_offers` | The percent of `categories_total` that have offers |
	| `categories_with_requests` | The percent of `categories_total` that have requests |
	| `talent_per_cat_per_member` | The mean total talents for all timebank parent categories / `categories_parent` / `members_starting` |
		

### February 5, 2019
- **Address changes**: Found observations with missing census data due to addresses with multiple lines or P.O. Boxes. Partial list of addresses that could be adjusted:

	| Timebank | Original | Updated | Reason |
	| --- | --- | --- | --- |
	| atx | 123 Uptown, ATHENS, Ohio 45701, United States | 5 N Court St, Athens, OH 45701, United States | False address. Changed to address that Google Maps provides when given original address. |
	| jeffersoncommunity | PO Box 162, Charles Town, West Virginia 25414, United States | 101 W Washington St, Charles Town, WV 25414, United States | PO Box. Changed to PO street address. |
	| mttimeexchange | P O Box 243, Great Cacapon, West Virginia 25422, United States | 5010 Central Ave, Great Cacapon, WV 25422, United States | PO Box. Changed to PO street address. |
	| rvtb | 1327 Grandin Rd. SW, Roanoke, Virginia 24016, United States | not changed | Seems to be a valid address. May be an area that does not have ACS reporting? |

		

### February 4, 2019
- **Census Data**
	- Selected desired columns from data provided by geocodio (33 out of 600+).
	- Changed address for `atx` timebank from invalid address (123 Uptown, ATHENS, Ohio 45701, United States) to address that comes up when you enter original address in Google maps (5 N Court St, Athens, OH 45701).

### February 3, 2019
- **Geocodio**
	- Used [geocodio](https://www.geocod.io/) to get geolocation information for timebanks. Also got [census data](https://www.geocod.io/docs/hipaa/#census-block-tract-fips-codes-amp-msa-csa-codes) for those locations. All saved in `geocodio.csv`.
	- Reminder through geocodio that it may make most sense to limit research to U.S. for now due to GDPR restrictions. 
- **Facebook / Twitter followers**
	- Re-ran the directory scraper to also capture Facebook and Twitter links. Noticed a discrepancy between the number of timebanks captured. It appears TimeBanks.org has removed about 20 timebanks that have not been active in 2 years or more.
	- In order to not lose information of timebanks that were removed, I have appended the social media link information to the original `directory.csv` file.
	- Captured social media stats in `social.csv`. Visited pages and manually entered data.
		- `porirua` timebank has a Facebook group instead of page. # of group members used for both `facebook_likes` and `facebook_followers` columns.

### January 23, 2019
- **Data**
	- Consolidated data gathered so far.
	- Fixed issues where timebank shortname (`timebank`) had been extracted incorrectly.
	- Removed data from demo and test-launch timebanks that had already been gathered.
- **Data collection improvements**
	- Fixed error with extracting `timebank`.
	- Added `timebank` column to all datasets.
	- Added `timestamp` to all datasets except `directory.csv`.
	- In directory: fixed issues with collecting `phone`, `url_2`, and `sponsor`. Updated to not collect info from demo and test-launch timebanks.
	- Changed to append new data to existing csv's rather than create new ones.
- **Notebooks**
	- Consolidated to two main notebooks:
		- Create directory
		- Get daily updates

### January 22, 2019
- **Timebank name error**
	- Caught issue with extracting timebank shortname from `url` where `t`'s were dropped if first letter. Need to update daily scraper.

## <a name="challenges"></a>Challenges
- **Modeling** (Added 2/8/19)
	- The limited number of timebanks. 

- **Limited timebanks** (Added 2/5/19)
	- If I limit the timebanks to those in the U.S. (to include census data) that have been active since I have started collecting data, there are only 57.
		- **Important**: May be no need to filter out inactive timebanks, since this is about seeing what makes for active timebanks. However, need to be careful about what features are used.
	- I can try to collect population data for ALL timebank areas. This means understanding if the areas being reported on are comparable. I also won't be able to include other measures (% rental units, single-parent households, etc.) at this time.
	- For New Zealand: [Subnational population estimates (TA, ward), by age and sex, at 30 June 2013-18 (2018 boundaries)](http://nzdotstat.stats.govt.nz/wbos/Index.aspx?DataSetCode=TABLECODE7505)
	- For other countries: [City population by sex, city and city type](http://data.un.org/Data.aspx?d=POP&f=tableCode%3a240)
	
- **Facebook / Twitter followers** (Added 1/21/19)
	- I had planned to get the follower count for each (if available) from each timebank's main page. However, the follower numbers are hidden within the widgets. I could scrape the Facebook page link, but would not be allowed by Facebook TOS to scrape the number from there.

- **Other timebank directories** (Added 1/21/19)
	- `hourworld.org` has 44,262 members, 542 communities, and 2,284,276 hours of service received (as of a few days ago), but does not allow scraping.
	- `community-exchange.org` (worldwide), `timebanking.org` (UK), and other regional sites include listings of timebanks, but with varying amounts of information and platforms. Many timebanks have independent sites and would have to be visited individually.

## <a name="notes"></a>Other Notes
- **Mission framing / NLP** (Added 2/5/19)
	- Language and framing matter! Original intent behind timebanks was to encourage reciprocity, putting equal value to both giving and receiving time. Many timebanks are intentional about differentiating what they do from volunteerism. But others do use language of volunteerism and helping. In the People Fixing the World podcast, they discuss how many people give time, but may not plan to "cash in" their credits. They also discuss that many timebanks have more offers than requests, raising the point that people do not often like to ask for help. This raises multiple questions:
		- Are timebanks that use volunteerism language more likely to have more offers than requests (and potentially be less active overall)?
		- Are those that use language focused on skillshares, sharing economy, etc. more likely to have more even offers and requests?
		- How do the language choices (and - sidenote - the images used on websites, social media, etc.) affect who participates?
	- Edgar S. Cahn is often cited as starting timebanks because he wrote about the subject and trademarked the terms "TimeBank" and "Time Credit". Yet similar informal and formal systems had been around under different names before then. The [Cincinnati Time Store](https://en.wikipedia.org/wiki/Cincinnati_Time_Store) used time-based currency from 1827-1830. This and other earlier initiatives were often anarchist and/or socialist projects. In terms of framing, curious if this history is lesser known or intentionally passed over to "protect" timebanks from negative perception. Many do still use language about mutual aid, etc.
- **Membership** (Added 2/4/19)
	- On defining "success": Hours or exchanges made could be high while membership is low. Some timebanks become less active due to members developing a strong network and no longer relying on the timebank to connect. Number of new members within a given period could be an alternate way of thinking about success, considering long-term sustainability.

## <a name="resources"></a>Resources / References
### General	
- Forbes: [Time Banking Helps Build Individuals, Organizations And Communities](https://www.forbes.com/sites/devinthorpe/2018/03/21/time-banking-helps-build-individuals-organizations-and-communities/) (Added 2/7/19)
	- "In Chicago, 127 schools nearly eliminated special ed by having fifth graders help the third graders learn the alphabet. The kids earned credits that allowed them to receive a recycled computer from the system."
	- "In another example Cahn shared, teens serving on a youth court jury in Washington, D.C., helped reduce recidivism. The program reduced rearrests from 34 to 6%, he says."
- [Solidarity Mass](https://www.solidaritymass.com/profiles) (sponsor of Ujima timebank) (Added 2/3/19) 
- People Fixing the World podcast: [The Banks That Run on Time Instead of Money](https://www.bbc.co.uk/programmes/p06s83lp) (Added 2/4/19)
- Smithsonian mag: [“Time Banking” Is Catching On In the Digital World](https://www.smithsonianmag.com/innovation/time-banking-is-catching-on-in-digital-world-180969437) (Added 2/4/19)
- Other "timebanks": (Added 2/4/19)
	- [TimeRepublik](https://timerepublik.com/)
	- [Ying app](https://www.yingme.co/)

### Bystander effect
- [How to Get People to Help Each Other, Online and Off](https://medium.com/behavior-design/how-to-get-people-to-help-each-other-online-and-off-22e1004391ea): In terms of timebanks, do more people with a given talent actually mean more exchanges will happen or requests needing those talents will be filled? Or will people with those talents see a request and think "someone else can do it"? This could help explain why some timebanks have a lot of members/talents yet also have requests that go unanswered. (Added 2/9/19)
		
### Cryptocurrency
- Coindesk: [Time is Money as Alternative Banking Moves to the Blockchain](https://www.coindesk.com/time-money-alternative-banking-moves-blockchain) (Added 2/7/19)
- Shareable: [The English City With Its Own Cryptocurrency: Q&A With the Founders of HullCoin](https://www.shareable.net/blog/the-english-city-with-its-own-cryptocurrency-a-qa-with-the-hullcoin-team) (Added 2/7/19)
- Founder Institute: [Seva Exchange is the Blockchain and A.I. Platform Reinventing Timebanking for Volunteerism in the Digital Economy](https://fi.co/insight/seva-exchange-is-the-blockchain-and-a-i-platform-reinventing-timebanking-for-volunteerism-in-the-digital-economy) (Added 2/7/19
	- "Today, Seva Exchange is an official for-profit Benefit Corporation and subsidiary of TimeBanks USA. Dr. Cahn serves as the Chairman of the Board for Seva Exchange, while Anitha Beberg serves as its CEO."

### Recession / unemployment
- PBS: [In Maine, Service Time Swapped to Help Stretch Dollars in Recession](https://www.pbs.org/newshour/show/in-maine-service-time-swapped-to-help-stretch-dollars-in-recession) (Added 2/7/19)
- Shareable: [Just in Time](https://www.shareable.net/blog/just-in-time) (Added 2/7/19)
	- "As the recession and Occupy movement encourage people to reimagine work and how they get their needs met in the new economy, Timebanks are catching fire."
- Motley Fool: [What Is Time Banking? Should You Join a Time Bank?](https://www.fool.com/investing/general/2012/09/07/what-is-time-banking-should-you-join-a-time-bank.aspx) (Added 2/7/19)
	- "The front page of last Monday's Wall Street Journal featured a story on how time banks, and even alternate currencies sponsored by banks, are flourishing in Spain in an environment of 24.6% national unemployment -- and youth unemployment of 50% and up!"
		
### Social context / cohesion
- [No Wonder America Is Divided. We Can't Even Agree on What Our Values Mean](http://time.com/5435825/divided-america-values-language-meaning/) (Added 2/7/19)
- [Dying Alone - An interview with Eric Klinenberg
author of Heat Wave: A Social Autopsy of Disaster in Chicago](https://www.press.uchicago.edu/Misc/Chicago/443213in.html) (Added 2/7/19)
	- "Many Chicagoans attributed the disparate death patterns to the ethnic differences among blacks, Latinos, and whites—and local experts made much of the purported Latino “family values.” But there’s a social and spatial context that makes close family ties possible. Chicago’s Latinos tend to live in neighborhoods with high population density, busy commercial life in the streets, and vibrant public spaces. Most of the African American neighborhoods with high heat wave death rates had been abandoned—by employers, stores, and residents—in recent decades. The social ecology of abandonment, dispersion, and decay makes systems of social support exceedingly difficult to sustain."

### Volunteerism, mutual aid, etc.
- [Mexican Solidarity: Citizen Participation and Volunteerism](https://books.google.com/books?id=mhMZL31NWL8C&pg=PA14&lpg=PA14&dq=mexican+solidarity+mutual+aid+volunteerism&source=bl&ots=jTMrFc4MG_&sig=ACfU3U0QRhlUUj7QhKChxJm3CXzwG0qjLw&hl=en&sa=X&ved=2ahUKEwj8gO3EgqrgAhUitlkKHXBLANoQ6AEwBHoECAYQAQ#v=onepage&q=mexican%20solidarity%20mutual%20aid%20volunteerism&f=false) (Added 2/7/19)
	-  Identifies and defines four categories of volunteer activity:
		-  Mutual aid or self-help
		-  Philanthropy or service to others
		-  Participation (i.e. social commitment motivated by the idea of active citizenry)
		-  Advocacy or campaigning
-  Wikipedia: [Mutual aid (organization theory)](https://en.wikipedia.org/wiki/Mutual_aid_(organization_theory)) (Added 2/7/19)
	- "Examples of mutual-aid organizations include unions, the Friendly Societies that were common throughout Europe in the eighteenth and nineteenth centuries, medieval craft guilds, the American "fraternity societies" that existed during the Great Depression providing their members with health and life insurance and funeral benefits, and the English "workers clubs" of the 1930s that also provided health insurance."
- [Seniors benefiting from ‘Time Bank’ helpers](http://www.nationmultimedia.com/detail/breakingnews/30358782?fbclid=IwAR1IXFHKyIkGbFLYydUsuy3wwItTooDIKSqM7CIN7Us1MOvIF7Waq0iRoso) (Added 2/3/19)
- [Solidarity economy (wikipedia)](https://en.wikipedia.org/wiki/Solidarity_economy) (Added 2/3/19)


### Technical Resources
- **Mapping** (Added 2/4/19)
	- [Kepler example of county unemployment](http://kepler.gl/demo/county_unemployment) and [article](https://medium.com/vis-gl/visualizing-u-s-county-unemployment-with-kepler-gl-c5f2ed31c71).