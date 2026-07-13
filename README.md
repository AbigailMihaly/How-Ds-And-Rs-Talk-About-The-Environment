
### Climate Hearings Over Time ###

In this project, I want to explore the topics of Congressional hearings in the top environment committees over time. Public Congressional hearings, especially the House, are an especially great proxy for overall political messaging; I wanted to see how that differed across party over time.

### Data ###

CONGRESSIONAL COMMITTEE MEETINGS

I have obtained this data using the congress.gov API. The relevent documentation is available here: https://github.com/LibraryOfCongress/api.congress.gov/blob/main/Documentation/CommitteeMeetingEndpoint.md 

CONGRESSIONAL COMMITTEE PARTY SPLIT ACROSS DIFFERENT CONGRESSES

I scraped this data from this website here: https://www.congress.gov/crs-product/R40478
That process is in the notebook committee-party-split-scraping.ipynb

### Notebooks ###

1) "getting_data.ipynb" is a notebook scraping all of the meetings during each of the relevent Congresses, then pulling out only those hosted by the environment committees, and then making dataframes out of them. 

    Steps to use:

    1) You will need to set up your own API key to use this notebook, and create a .env that contains a variable called API_key_.gov with your API key.

    2) Input the chamber you want ('house' or 'senate') and the number of Congress

    3) Run the notebook. Make sure you look at the lists of items to handcheck at 2 points in this notebook:
        one when it is scraping to make the full list of meetings under the "## Grab all meetings on remaining pages, running through each page. ## header, and
        one when it is filtering that first list for just environment, under the "## Make a new dataframe with just the environment committee meetings. ##" header.

2) "word_frequency_analysis.ipynb" uses spacy to do a word frequency analysis on each of four lists:
    the aggregate words/phrases in titles in 1) R-controlled House meetings 2) D-controlled House meetings 3) R-controlled Senate meetings, and 4) D-controlled Senate meetings. 

    It exports the findings into .csv files

3) I then impoted the .csvs from the word frequency analysis into a "frequency_data_cleaning.ipynb" notebook to clean the data
