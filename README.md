# DATA512 Assignment 1: Reproducibility

### Purpose

In the popular and well-known public information website, Wikipedia, several articles exist that describe or tangentially discuss different rare diseases. For each of these pages, as with all its others, the site maintains a record of the number of times that page was viewed and the platform that it was viewed from. The purpose of this repository is to provide a brief and reproducible analysis of some of the pageview trends for Wikipedia's articles on rare diseases.

### Sources and Licencing

In this analysis, all raw data will be obtained through the Wikipedia Analytics Pageviews API, an API that provides access to Wikipedia's analytics according to its terms of use. By the [Wikimedia Foundation's terms of use](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use), site and API users are free to read, print, share, and reuse the contents of their sites under the condition that the content in question is used responsibly, with civility, lawfully, without intent to harm the Foundation or other human beings, with full disclosure, and while being held fully responsible for the consequences of how this content is used or interpretted. As the data from this API will be used for a strictly educational exploration of copyright free material with no intent to monetize, this notebook's dataset falls under the Foundation's terms of use.

In addition to this, an API template provided by David McDonald's [sample wikimedia notebook](wp_article_views_example.ipynb) was used to help perform API calls to the Analytics API. This code was used under its CC-BY licence, with no substantial alterations whatsoever. Both the API call constants and the API call function utilized in the analysis notebook are the work of David McDonald.

### Methods

From this API, a dataset will be obtained consisting of monthly data from July 2015 to September 2024. This dataset contains only user pageview counts for 1773 specific Wikipedia articles that describe or or associated with rare diseases, which are all named in the [rare-disease_cleaned.AUG.2024](rare-disease_cleaned.AUG.2024.csv) csv file. This dataset will come in three parts. Firstly, a dataset of pageview counts for each article by desktop users. Secondly, a dataset of pageview counts for each article by mobile users (both mobile web and mobile app users), and finally a cumulative dataset containing views from both user groups. The documentation for the API used to obtain these requests is made publicly available by the Wikimedia Foundation at [this link](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/reference/page-views.html).

Once obtained, these three datasets are cleaned to ensure that pageview counts are represented numerically and are saved into newly generated JSON files for ease of use.
* Desktop data will be stored in `rare-disease_monthly_desktop_start201507-end202409.json`
* Mobile data will be stored in `rare-disease_monthly_mobile_start201507-end202409.json`
* Cumulative data will be stored in `rare-disease_monthly_cumulative_start201507-end202409.json`

For each of these JSON files, the contents will be a list of dictionary entries corresponding to the pageview information for a specific article on a specific month. Each of these entries will be of the format:

```
{
    "project": "en.wikipedia",
    "article": <Article Name>,
    "granularity": "monthly",
    "timestamp": <Pageview Timestamp String>,
    "access": <desktop or mobile or all-access>,
    "agent": "user",
    "views": <Pageview Count For the Month>
}
```

Of note to any individuals seeking to utilize this dataset: The Wikimedia Analytics Pageview API does not properly process article titles with special characters such as `/` by default. As such, three articles in this dataset contain `N/A` as their pagecount values due to containing `/` in their titles. Care should be taken to remove these entries if attempting to work with the numeric pagecount values rather than with the article title values of this dataset.

Once obtained, these datasets will be used to answer the followin three quetions about how pagecounts may differ between desktop and mobile users:
* How greatly does the maximum and minimum average view count for the top 10 and bottom 10 most viewed rare disease pages differ between desktop users and mobile users?
* Do the pageview counts of the rare disease pages with the highest peak views differ consistently between desktop and mobile users?
* And do the rare disease pages with the fewest months of data avilable high consistently higher pageview counts for either desktop or mobile users?

To accomplish this, three visualizations will be created by the analysis notebook code that highlight the time series view count trends of desktop and mobile users side-by-side.

### Outputs

One fully run, the analysis notebook will produce three plots in addition to the three JSONs generated earlier.

The first plot generated is `MostAndLeastViewedPagecounts.png`. A plot that shows the average time series pageview counts for the 10 articles on desktop annd on mobile that see either the highest or the lowest average view counts. This plot aggregates these and displays four average pagecount lines for the four groups being considered. Looking at the plot, it can be determined that the answer to the first question is that pagecounts for mobile users are noticably higher for the 10 most viewed articles, with the difference becoming particularly pronounced during the pandemic.

The second plot generated is `HighestPeakPagecounts.png`. A plot that shows the pageview counts over time for the 10 articles that have had the highest peak single-month pageview counts in either dataset. This plot will show that the average pagecounts are roughly even between both desktop and mobile users but that sudden spikes in view counts are larger on mobile. From this, the second question be answered by concluding that no consistent difference can be found between the two groups' typical pageviews.

The final plot generated by the analysis notebook is `FewestMonthsOfDataPagecounts.png`. A plot that shows the pageview counts over time for the articles with the fewest months of data on record (excluding the three articles with invalid names). This plot will show that the pageview counts for low-data disease articles is consistenly even or higher for mobile users than for desktop users, thereby answering the third and final question.