# scholarly-trends


## Step 1: 
First, run the programs which produce intermediate files which are used to get the final statistics.

1. Run the programs to create the json files under scholarly-trends/Intermediate Programs/Create JSON files/:
For __noun phrases__, run:
```
 groupcount_noun_phrases.py
 month_to_year_json.py
```
For __entity mentions__: The other 2 'wiki' programs are used to create json files for entity mentions.

2. Run the programs to generate 'pickle' files for phrases frequencies year-wise. This also creates sets of negative and trending phrases by linear regression as a byproduct:
For noun phrases: yearly_trends_and_yearly_pickle.py
For entity mentions: yearly_wiki_trends_pickle.py

Run pickle_phrase_doc_counts.py to produce total phrase counts (and unique occurrences) of noun phrases (over all years) as pickles. The foll. are the files produced
total_phrase_count_dataframe.pickle
total_doc_count_dataframe.pickle

Run pickle_wikiurl_doc_counts.py to produce total entity mention counts and unique occurrences of entity mentions (over all phrases) as pickles. The foll. are the files produced.
wiki_phrase_count_dataframe.pickle
wiki_doc_count_dataframe.pickle


## Step 2: 
The pickle and JSON files are expected to have been created already. The programs to calculate the Mann Kendall and Theil Sen statistics for noun phrases are the following (with description): 
```
Mann-Kendall Theil-Sen/Noun Phrases/mannkendall_theilsen_total.py
```
This module is used to create a data frame with all phrases which occur at least 100 times in the whole corpus (the
common words which occur 50000 times or more are later discarded), gets the percentage of total occurrences they occur in each
year from 2007 to 2017, and if they occur in at least 3 of these years, calculates the Mann Kendall and Theil Sen
statistics. These statistics calculate the strength of a trend, and 5 files are written to which have: 1. Positive
Mann-Kendall Z, 2. Negative Mann Kendall Z, 3. Non-Trending phrases by Mann-Kendall, 4. positive Theil Sen
slope, 5. negative Theil Sen slope
```
Mann-Kendall Theil-Sen/Noun Phrases/mannkendall_theilsen_docs.py
```
This module is used to create a data frame with all phrases which occur at least 100 times in the whole corpus (the
common words which occur 50000 times or more are later discarded), gets the percentage of docs they occur in each
year from 2007 to 2017, and if they occur in at least 3 of these years, calculates the Mann Kendall and Theil Sen
statistics. These statistics calculate the strength of a trend, and 5 files are written to which have: 1. Positive
Mann-Kendall Z, 2. Negative Mann Kendall Z, 3. Non-Trending phrases by Mann-Kendall, 4. positive Theil Sen
slope, 5. negative Theil Sen slope

The corresponding programs for entity mentions are: 
```
scholarly-trends/Mann-Kendall Theil-Sen/Entity Mentions/mannkendall_theilsen_total_wiki.py
scholarly-trends/Mann-Kendall Theil-Sen/Entity Mentions/mannkendall_theilsen_docs_wiki.py
```
