## garoon-google-zoom-rooms

First, follow the directions from Cybozu below (and on their website in Japanese) to see how syncing your personal Garoon schedule to a G-suite calendar works.
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

Comment that line out and add this: 

scheduleGetEvents.setMember(MemberType.FACILITY, 7);

(make sure to replace "7" with the facility ID you got from step 1)

#### 2.2 Go to line 372 of GGsync.java

You'll see a line that looks like this:

scheduleLocation = garoonSchedular.getFacilitiesInfo(facilitiesElement);

Comment that out, and change it to this:

scheduleLocation = "https://zoom.us/j/1234567890";

(but change the "1234567890" portion to the Meeting ID for your Zoom Room. You can find the Zoom Room Meeting ID by tapping "settings" on the Zoom Room controller tablet)

#### 2.3 Save GGsync.java



### 3. Install gradle

You have to install gradle in order to build the executable file. If you're on mac and using brew, use this command to install gradle:

```sh
brew install gradle
```
  
Otherwise, refer to Gradle's website for installation instructions.


### 4. Rebuild the project

Open up a terminal, and run this command in the directory that contains the "build.gradle" file:

```sh
gradle build
```

You should see something like "BUILD SUCCESSFUL in ##s" and there will be a new "build" folder in your current directory. There should be a file at /build/libs called "GGsync.jar". Rename that file to something unique like "GGsync-trestles.jar"


### 5. Set up your properties file and run the program

Now you just need to follow the original Cybozu instructions to enter in your garoon username and password, and provide your G-suite service account and calendar resource info. After that, run the GGsync-trestles.jar (or whatever you named it) file that you just made, and it should sync all events that are scheduled in the Garoon facility that you selected to your Google calendar. 


### 6. Integrate your google calendar with your Zoom Room

Follow the instructions on Zoom's website to integrate your Google Calendar with your Zoom Room:
https://support.zoom.us/hc/en-us/articles/206905656-Setting-Up-Zoom-Rooms-with-Google-Calendar


### Rinse and Repeat

Repeat steps 2, 4, 5, and 6 for each Garoon facility you want to sync with Zoom. You'll need to keep each new "GGsync-FACILITY.jar" file in its own directory, with its own GGsync.properties file and google auth files. Once you have created and tested all the java executables you need, just set up the programs to run automatically every 10 minutes or so (using task scheduler / cron / etc.) and voila! Your Garoon facility schedules are now automatically synced to your Zoom Rooms schedules. 

## License

MIT

## Copyright

Copyright(c) 2015 Cybozu, Inc.
