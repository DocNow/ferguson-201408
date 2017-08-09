This is a view of the most retweeted tweets that mention Ferguson, that still exist, and were sent August 8-10, 2014. The dataset of tweet ids and a description of how it was collected can be found at the [Internet Archive](https://archive.org/details/ferguson-201408). Of the original 32,056 tweets collected 14% have been deleted or their accounts have been protected as of August 9, 2017.

Here are the steps I took to generate the top 500 retweeted tweets from the list
of ids. You'll need to install [twarc](https://github.com/docnow/twarc) and
[csvkit](https://csvkit.readthedocs.io/) to do follow along. One thing you'll
notice is that there is an effort to remove tweets about Alex Ferguson, the
football player, that were happening at the same time. 

    wget https://archive.org/download/ferguson-201408/ids.txt.gz
    gunzip ids.txt.gz
    twarc hydrate ids.txt > tweets.json
    twarc/utils/json2csv.py tweets.json > tweets.csv
    csvsql --query "SELECT DISTINCT(id) FROM tweets WHERE text NOT LIKE '% Alex %' ORDER BY retweet_count DESC LIMIT 500" tweets.csv > ids.txt
    # massage ids.txt into js/ids.js
