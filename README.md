# Hadoop-Twitter-Sentiment-Analysis
Analyze huge amounts of real-time tweets using Hadoop

The application extracts raw Twitter data, stores in HDFS, assigns positive, negative and neutral sentiment to each tweet and how to analyze and visualizes this sentiment data.

####Project Details
1. Project Files
* `flume.conf` will be used for Flume configuration.
* `dictionary.tsv` is a reference file to map words to sentiment i.e. whether a word is of positive sentiment, negative sentiment or neutral sentiment.
* `json-serde-1.3.8-SNAPSHOT-jar-with-dependencies.jar` is a compiled jar file used to store JSON data into HDFS.
* `time_zone_map.tsv` maps time zone with country.
* `tweets.sql` contains definition of all tables and views created on Hive.

####Create a Twitter application

Once you have created an application on Twitter, you need the following:

* 	Consumer Key (API Key)
* 	Consumer Secret (API Secret)
* 	Access Token
* 	Access Token Secret

	These will be used when we setup the Flume agent.

####Install Flume
####In `flume.conf`, substitute `CONSUMER_KEY`, `CONSUMER_SECRET`, `ACCESS_TOKEN` and `ACCESS_TOKEN_SECRET` with the values copied from Twitter app console. 

####Now type below commands to create folders in HDFS:

* `hadoop fs –mkdir data`
* `hadoop fs –mkdir data/dictionary`
* `hadoop fs –mkdir data/time_zone_map`
* `hadoop fs –mkdir data/tweets_raw`
* `hadoop fs –put data/dictionary/dictionary.tsv data/dictionary`
* `hadoop fs –put data/time_zone_map/time_zone_map.tsv data/time_zone_map`

`dictionary` file has all the Positive, Negative and Neutral sentiment words list.
`time_zone_map` has mapping with time zones to countries. This will help us identify the country from a tweet.
All tweets which we are going to extract will be stored in `tweets_raw` folder. 


####Run Twitter Extraction Program using Flume

We can start the flume agent with below command:

`/usr/lib/flume/bin/flume-ng agent –conf ./conf/ -f /etc/flume/conf/flume.conf -Dflume.root.logger=DEBUG,console -n TwitterAgent`

This will start loading Twitter data into HDFS. Wait for about 10 minutes so that sufficient number of tweets are stored into HDFS for better analysis.

Now you can view the files in the location folder by using -ls and –cat commands.

####Run Hive script and create tables and view on extracted data files 

At the command prompt, type the following command, then press the Enter: `hive –f tweets.sql`

Script will start running and a series of MapReduce jobs will be executed on behalf of this script. It will take few minutes for the script to finish.

The tweets.sql script will create tables & views which will help in transforming the raw twitter data and performing the following steps:

* Convert the raw Twitter data into a tabular format.
* Use the dictionary file to score the sentiment of each Tweet by the number of positive words compared to the number of negative words, and then assigned a positive, negative, or neutral sentiment value to each Tweet.
* Create a new table that includes the sentiment value for each Tweet.

The command `show tables` will show you all the tables. You can browse the data using the `select * from <table name>;` command.
