# Timebanks: Process Log
> Author: [Dawn Graham](https://dawngraham.github.io/)

The purpose of this log is to document the process as I go through this project, including challenges, questions, and ideas.

## January 23, 2019
### Updates
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

## January 22, 2019
### Updates
- **Timebank name error**
	- Caught issue with extracting timebank shortname from `url` where `t`'s were dropped if first letter. Need to update daily scraper.

## January 21, 2019

### Notes
- **Capturing activity**
	- It could make sense to get updates on overall numbers (members, exchanges, hours, etc.) and request/offer descriptions daily. This could provide insight into what requests/offers are exchanged and what goes unmet.
- **Natural language processing**
	- Can do NLP to take a deeper dive into requests/offers. Curious to see how they change and what patterns there are.
	- Can also look at connection with language/descriptiveness of mission statements and about pages.
- **Mapping**
	- There are some timebank "hot spots". Should investigate if areas with more in closer proximity also have more activity.
	- Want to look at demographic info in relation to timebank locations.

### Challenges
- **Facebook / Twitter followers**
	- I had planned to get the follower count for each (if available) from each timebank's main page. However, the follower numbers are hidden within the widgets. I could scrape the Facebook page link, but would not be allowed by Facebook TOS to scrape the number from there.

- **Other timebank directories**
	- `hourworld.org` has 44,262 members, 542 communities, and 2,284,276 hours of service received (as of a few days ago), but does not allow scraping.
	- `community-exchange.org` (worldwide), `timebanking.org` (UK), and other regional sites include listings of timebanks, but with varying amounts of information and platforms. Many timebanks have independent sites and would have to be visited individually.