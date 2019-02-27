I made this fork as a quick and dirty way to sync all Garoon facility events to corresponding Zoom Rooms.
This readme pertains specifically to the hosted cloud version of Garoon.

First follow the directions from Cybozu below to see how syncing your personal Garoon schedule to a G-suite calendar works.
Then take a look at the "How to sync to Zoom Rooms" section that I appended below. 

I realize this method is extremely inefficient and could be a lot cleaner, but in my case I just needed something that worked, and this does the job.
I might clean this up at a later date and make it a little more user friendly if there is a need. (2019-02-26 time of writing)

==========BEGIN ORIGINAL CYBOZU INSTRUCTIONS===========

To synchronize the Garoon schedule to Google Calendar.

## Requirements

- Java 1.8

## Usage

### 1.Build
```sh
$ ./gradlew clean build copy
```

### 2.Setting
```sh
$ cp ./src/main/resources/GGsync.properties .
```

and rewrite GGsync.properties.

See also https://cybozudev.zendesk.com/hc/ja/articles/204426680

### 3.Synchronize
```sh
$ java -jar GGsync.jar .
```

with secure access
```sh
$ java -Djavax.net.ssl.keyStore=xxxx.pfx -Djavax.net.ssl.keyStorePassword=xxxx -Djavax.net.ssl.keyStoreType=PKCS12 -jar GGsync.jar .
```

with proxy
```sh
$ java -Dhttp.proxyHost=ホスト名 -Dhttp.proxyPort=ポート番号 -Dhttps.proxyHost=ホスト名 -Dhttps.proxyPort=ポート番号 -jar GGsync.jar .
```

==============END ORIGINAL CYBOZU INSTRUCTIONS============

## How to sync to Zoom Rooms

### 1. Find your Garoon facility IDs

You'll need these ID numbers in step 2. To get the facility ID numbers, we'll be using Garoon's REST API and the Postman chrome app.

#### 1.1 Install the Postman Chrome app from here:
   
   https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en

#### 1.2 Open Postman (Chrome > new tab > click "apps" in top left > Postman).

#### 1.3 Make a test schedule event in each of your Garoon facilities for today. 
  
#### 1.4 Send a GET request from Postman to your Garoon (See: screenshots below)
  
  ![alt text](https://github.com/adamkendall1/garoon-google-zoom-rooms/blob/master/postman-auth.png)
  
  ![alt text](https://github.com/adamkendall1/garoon-google-zoom-rooms/blob/master/postman-GET.png)

### 2. Modify ggsync.java

Ok, so if you followed the instructions above and got the GGsync.jar program running correctly, you'll notice that only events which contain the user you specified in the GGsync.properties file as a participant are synced to the Google calendar. In order to 



## License

MIT

## Copyright

Copyright(c) 2015 Cybozu, Inc.
