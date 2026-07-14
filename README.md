
### Climate Hearings Over Time ###

In this project, I want to explore the topics of Congressional hearings in the top environment committees over time. Public Congressional hearings, especially the House, are an especially great proxy for overall political messaging; I wanted to see how that differed across party over time.

### Data ###

CONGRESSIONAL COMMITTEE MEETINGS

I have obtained this data using the congress.gov API. The relevent documentation is available here: https://github.com/LibraryOfCongress/api.congress.gov/blob/main/Documentation/CommitteeMeetingEndpoint.md

CONGRESSIONAL COMMITTEE PARTY SPLIT ACROSS DIFFERENT CONGRESSES

I obtained the information about which party is in control during which Congress here: https://www.congress.gov/crs-product/R40478

### METHODOLOGY ###

1) Scraping 

I used the congress.gov API to make a dataframe of all the congressional meetings in each Congress for those Congresses with good available data: House Congresses 113-119, and Senate Congresses 116-119.

The relevent Congress.gov endpont is url = f'https://api.congress.gov/v3/committee-meeting/{congress_number}/{chamber}?offset={i*250}&limit=250&format=json&api_key={API_key}'

In this initial scraping, I created a column in the dataframe called committees. I then filtered the data for only those hearings from my four environment committees (House Energy and Commerce; House Natural Resources; Senate Environment and Public Works; Senate Energy and Natural Resources Committees).

In some cases, this information was available in the committees column that I had made, but in others it was not. A for loop was necessary to look for additional information about which committee(s) held the meeting, which involved opening additional endpoints in the API as needed.

2) Phrase Frequency Analysis

I conducted a word frequency analysis using Spacy.

Specifically, I asked spacy to pull out the top phrases from all of the titles of Congressional hearings across the House Energy and Commerce, House Natural Resources, Senate Environment and Public Works and Senate Energy and Natural Resources Committees.

I asked it to include a list of custom phrases in its phrase list, in addition to its automaticly detected phrases. Those included:

I also filtered out a list of custom excluded words. Those included:
- words focused on process rather than substance, like days of the week, "an oversight hearing" and "a discussion draft"
- healthcare words having to do with non-environment aspects of these committees' jurisdiction (like "health care costs" and "agriculture")
- broad actors like "the secretary" and "the committee"

More information about the custom include and exclude can be found in the word_frequency_analysis notebook.

3) I then further cleaned the data by hand sorting it into categories that served to make a smaller catgeory of the thematic words and phrases I'm most interested in this analysis.

The categories are:
- "not interesting", with words that do not gove me information about D vs. R trends like "congress," "a member" or "legislative hearing". I also filter out filler words like "that" and "all" that I noticed in the top phrases list that spacy returned, as well as words having to do with other areas of these committees jurisdiction, such as healthcare (although I chose to leave in the words "health" and "public health" because of their clear tie to pollution/the likelihood that it is at least sometimes being used in an environmental context).
- "legislation_and_initiatives" that include the names of specific bills/acts
- "places" which revealed interesting trends about what kind of places Rs vs Ds spend time discussing -- but that seems like a whole seperate project!
- "federal_actors" including lots of names of the various agency and department heads who frequently testify before these committees
- and "lawmakers" that include the names of lawmakers, likely being used in the context of who is hosting or announcing the meetings, or who has introduced/sponsored legislation is being discussed.

### Notebooks/Instructions ###

1) The "getting_data.ipynb" is a notebook scraping all of the meetings during each of the relevent Congresses, then pulling out only those hosted by the environment committees, and then making dataframes out of them. 

    Steps to use:

    1) You will need to set up your own API key to use this notebook, and create a .env that contains a variable called API_key_.gov with your API key.

    2) Input the chamber you want ('house' or 'senate') and the number of Congress

    3) Run the notebook. Make sure you look at the lists of items to handcheck at 2 points in this notebook:
        one when it is scraping to make the full list of meetings under the "## Grab all meetings on remaining pages, running through each page. ## header, and
        one when it is filtering that first list for just environment, under the "## Make a new dataframe with just the environment committee meetings. ##" header.

2) "word_frequency_analysis.ipynb" uses spacy to do a word frequency analysis on each of four lists:
    the aggregate words/phrases in titles in 1) R-controlled House meetings 2) D-controlled House meetings 3) R-controlled Senate meetings, and 4) D-controlled Senate meetings.

    It exports the findings into .csv files.

3) The "frequency_data_cleaning.ipynb" notebook cleans the .csvs from the word frequency analysis.

