## garoon-google-zoom-rooms

I made this fork as a quick and dirty way to sync all Garoon facility events to corresponding Zoom Rooms.
This readme pertains specifically to the hosted cloud version of Garoon.

First, follow the directions from Cybozu below to see how syncing your personal Garoon schedule to a G-suite calendar works.
Then take a look at the "How to sync to Zoom Rooms" section that I appended below. 




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

It's quick and dirty, not efficient. Might come back later and clean this up.



### 1. Find your Garoon facility IDs

You'll need these ID numbers in step 2. To get the facility ID numbers, we'll be using Garoon's REST API and the Postman chrome app.

#### 1.1 Install the Postman Chrome app from here:
   
   https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en

#### 1.2 Open Postman (Chrome > new tab > click "apps" in top left > Postman).

#### 1.3 Make a test schedule event in each of your Garoon facilities for today. 
  
#### 1.4 Send a GET request from Postman to your Garoon (See: screenshots below)
  
  ![alt text](https://github.com/adamkendall1/garoon-google-zoom-rooms/blob/master/postman-auth.png)
  
  ![alt text](https://github.com/adamkendall1/garoon-google-zoom-rooms/blob/master/postman-GET.png)
  
In this example, the facility name is "Trestles" and the facility ID is 7. Yours will be different. Write down the ID numbers for each of your facilities that you want to sync.


### 2. Modify ggsync.java

Ok, so if you followed the instructions above and got the GGsync.jar program running correctly, you'll notice that only events which contain the user you specified in the GGsync.properties file as a participant are synced to the Google calendar. In order to change this behavior and make ggsync sync ALL events for a given facility, we'll modify the source code a little.

#### 2.1 Open GGsync.java in a text editor and go to line 178.

You'll see a line that looks like this:
scheduleGetEvents.setMember(MemberType.USER, garoonUid);

Comment that line out, and add a line underneath it like this (replace 7 with the facility ID you got from step 1):
scheduleGetEvents.setMember(MemberType.FACILITY, 7);

#### 2.2 Save GGsync.java


### 3. Install gradle

Now we need to build the program. Cybozu included the gradle build with the source code so this is fairly simple.
Just install gradle ("brew install gradle" if you're using brew on MacOS)


### 4. Rebuild the project

Open up a terminal, and run this command in the directory that contains the "build.gradle" file:

  gradle build

You should see something like "BUILD SUCCESSFUL in 22s" and there will be a new "build" folder in your current directory.

....... wip


## License

MIT

## Copyright

Copyright(c) 2015 Cybozu, Inc.
