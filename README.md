# scholarly-trends
This repository contains the code for determining positively and negatively trending scientific concepts.

## Step 1: Obtain Scientific Concepts
We assume that the scientific concepts to be analyzed have already been obtained. See [here](http://dbis.informatik.uni-freiburg.de/content/team/faerber/data/LREC2018/arxiv-filtered-nps.tar.bz2) for the filtered noun phrase data set based on the [arXiv CS dataset](http://citation-recommendation.org/publications/#A_High-Quality_Gold_Standard_for_Citation-based_Tasks).

## Step 2: Generate intermediate files
Then, we run the programs that produce intermediate files to be used to obtain the final statistics.

1. Run the indexing programs after creating the Apache Solr indices. The following three indices are required (one for noun phrases, one for entity mentions, and one for getting the published date from arXiv's metadata):
```
arxiv_metadata
nounphrases
nounphrases_wikipedia
```
and the corresponding indexing programs are:
```
pysolr_xml_arxiv.py
index_noun_phrases.py
index_noun_phrases_wiki.py
```
The indexing programs and Solr configuration files are located at `scholarly-trends/Intermediate Files/`.

2. Run the programs to create the json files under `scholarly-trends/Intermediate Programs/Create JSON files/`:
For __noun phrases__, run:
```
 groupcount_noun_phrases.py
 month_to_year_json.py
```
For __entity mentions__: The other 2 'wiki' programs below are used to create json files for entity mentions: 
```
groupcount_noun_phrases_wiki.py
month_to_year_json_wiki.py
```

3. Generate PICKLE files
Run `pickle_phrase_doc_counts.py` to produce total phrase counts (and unique occurrences) of noun phrases (over all years) as pickles. The foll. are the files produced
```
total_phrase_count_dataframe.pickle
total_doc_count_dataframe.pickle
```

Run `pickle_wikiurl_doc_counts.py` to produce total entity mention counts and unique occurrences of entity mentions (over all phrases) as pickles. The following files are produced:
```
wiki_phrase_count_dataframe.pickle
wiki_doc_count_dataframe.pickle
```
Both the above programs are located at: `scholarly-trends/Intermediate Programs/Create PICKLE files/`

## Step 2: Calculate linear regression and further intermediate files
Run the programs to calculate the linear regression statistics while also creating more intermediate pickles which are used in the Theil-Sen and Mann-Kendall calculations.

The two programs which should be run in this step are: 
For noun phrases: `yearly_trends_and_yearly_pickle.py`
For entity mentions: `yearly_wiki_trends_pickle.py`

Both programs are located at: `scholarly-trends/Linear Regression`. These programs also create two sets of 11 pickle files (one for each year from 2007 to 2017) which are used in Step 3. 

## Step 3: Run Mann-Kendall and Theil Sen statistics
The pickle and JSON files are expected to have been created already. The programs to calculate the Mann Kendall and Theil Sen statistics for noun phrases are the following (with description): 
```
scholarly-trends/Mann-Kendall Theil-Sen/Noun Phrases/mannkendall_theilsen_total.py
```
This module is used to create a data frame with all phrases which occur at least 100 times in the whole corpus (the
common words which occur 50000 times or more are later discarded), gets the percentage of total occurrences they occur in each
year from 2007 to 2017, and if they occur in at least 3 of these years, calculates the Mann Kendall and Theil Sen
statistics. These statistics calculate the strength of a trend, and 5 files are written to which have: 1. Positive
Mann-Kendall Z, 2. Negative Mann Kendall Z, 3. Non-Trending phrases by Mann-Kendall, 4. positive Theil Sen
slope, 5. negative Theil Sen slope
```
scholarly-trends/Mann-Kendall Theil-Sen/Noun Phrases/mannkendall_theilsen_docs.py
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

## Additional programs
We provide a set of further programs that can be used for other kinds of analysis.

1. The programs under `scholarly-trends/Diff between Time Points` produce a list of noun phrases which occur < 50000 times along with the percentage of docs in which they occur in each year/month (everything is sorted by num_documents). 
2. `scholarly-trends/Diff between Time Points` is a linear regression program, but takes into account 4 months in 2007 and 4 months in 2017 only. 
3. `scholarly-trends/Sparsity Statistics` contains programs which are used to produce files with sparsity statistics. For noun phrases, the program used is `sparsity_stats.py` and the following calcuations are made: 
*(i) how many noun phrases occur in less than 3 years, and how many occur in more than 3 years
*(ii) how many noun phrases occur below 100 times or above 50000 times, and how many occur between 101 and 49999 times
*(iii) how many noun phrases occur in less than 3 years AND have a frequency above 50000 or below 100, and how many occur in more than 3 years and have a frequency between 101 and 49999. 

A similar set of statistics is computed for entity mentions as well using the program `sparsity_stats_wiki.py`.

## Demo 
Associated to this work is the demo available at http://scholarsight.org/. See also https://github.com/michaelfaerber/scholarsight.


## Contact
Feel free to reach out to us in case of questions or suggestions:

[Michael Färber](https://sites.google.com/view/michaelfaerber), michael.faerber@cs&#46;uni-freiburg&#46;de

## How to Cite
Please cite our work as follows:
```
@inproceedings{Faerber2019BIR,
  author    = {Michael F{\"{a}}rber and
               Adam Jatowt},
  title     = "{Finding Temporal Trends of Scientific Concepts}",
  booktitle = "{Proceedings of the 8th International Workshop on Bibliometric-enhanced Information Retrieval}",
  series    = "{BIR'19}",
  location  = "{Cologne, Germany}",
  pages     = {},
  year      = {2019}
}
```
