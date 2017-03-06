[![Build Status](https://travis-ci.org/scorelab/Bassa.svg?branch=master)](https://travis-ci.org/scorelab/Bassa)
[![PyPI](https://img.shields.io/pypi/dm/Bassa.svg)]()
[![PyPI](https://img.shields.io/pypi/v/Bassa.svg)]()
![logo](http://gdurl.com/7XYK)

Automated Download Queue for Enterprise to take the best use of Internet bandwidth

### About 
Bassa solves the problem of wasting internet bandwidth by queuing a download if it is larger than a given threshold value in high traffic and when the traffic is low, it completes the download of the files. After the files are downloaded, the users can get their files from the local servers which do not require external internet bandwidth.

## Installation
```
  $ ./setup.sh
  $ cd components/core/
  $ sudo python setup.py develop
```

## Test Server
```
  $ cd components/core/
  $ python Main.py
```

### Main functionalities
* Provides an interface for users to add their downloads as links or torrent magnet links
* Provide users  an interface to view and download the files in local server
* Provide a rating system to users to rate the files residing in local server
* Automatically start and stop downloading in given time frame
* Automatically clean the disks and make room for new downloads
* Notify user when his/her download is completed
* Mark inappropriate downloads
* Provides admins an interface to deal with inappropriate files

### How to Use Bassa
* After Setting up Bassa, Login/Register.There are two types of users in Bassa- (1) The Admin and (2) The Normal Users.
* A user can add a link through the webapp and Bassa stores it in the local server right away. This way multiple users can add various links, but the downloads won’t start right away. 
* The organisation admin can start the downloads at a time of his/her liking. 
* Then the users who had added links for certain files can download them from the local servers at a much higher speed.

## URL endpoints

**http://localhost:5000/api/login**

Form data: user_name, password
Returns auth token in response header for successful login

**http://localhost:5000/api/user**
###### POST
Headers: Content-type : Application/JSON, token: <auth token>
JSON: ```{"user_name":"<username>", "password":"<password>", "auth":<authleval>, "email":"<email>"}  ```

*Auth levels*
* 0:ADMIN
* 1:STUDENT
* 2:ACADEMIC
* 3:NONACADEMIC

###### GET
Headers: token: <auth token>
Returns JSON of all the users
###### DELETE
```http://localhost:5000/api/user/<username>  ```
Deletes the user from system. Adviced not to use.

###### PUT
```http://localhost:5000/api/user/<username>  ```
Headers: Content-type : Application/JSON, token: <auth token>
JSON: ```{"user_name":"<username>", "password":"<password>", "auth":<authleval>, "email":"<email>"}  ```
Update the given user

**http://localhost:5000/api/regularuser**
###### POST
Headers: Content-type : Application/JSON
JSON: ```{"user_name":"<username>", "password":"<password>", "email":"<email>"}  ```

**http://localhost:5000/api/user/blocked**
###### GET
Headers: token: <auth token>
Returns JSON of all the blocked users
###### POST
```http://localhost:5000/api/user/blocked/<username>  ```
Headers: token: <auth token>
Block the given user

**http://localhost:5000/api/download**
###### POST
Headers: Content-type : Application/JSON, token: <auth token>
JSON: ```{"link":"<download link>"}  ```
###### GET
```http://localhost:5000/api/download/<page> ```
Headers: token: <auth token>
Returns JSON of all the completed downloads. Page contains 15 records ordered by added time. Page number is a int.

*Status*
* 0:DEFAULT- not started
* 1:STARTED - started ot finished yet
* 2:DELETED - download completed but deleted from disk
* 3:COMPLETED - download completed
* 4:ERROR - download error

###### DELETE
```http://localhost:5000/api/download/<id>  ```
Deletes the download only if it is not starteed

**http://localhost:5000/api/download/rate/<id>**
###### POST
Headers: Content-type : Application/JSON, token: <auth token>
JSON: ```{"rate":"<rating between 0-5>"}  ```
Adds the rating to download. If exists, update.

**http://localhost:5000/api/user/downloads/\<page\>**
###### GET
Headers: token: <auth token>
Returns JSON of all the downloads of current user. Page contains 15 records ordered by added time. Page number is a int.

**http://localhost:5000/api/download/\<id\>**
###### GET
Headers: token: <auth token>
Returns file as multipart form data. Does not return a new auth token header

**http://localhost:5000/api/user/requests**
###### GET
Headers: token: <auth token>
Returns a JSON of all the users who has signed up and not been approved yet

**http://localhost:5000/api/user/approve/\<username\>**
###### POST
Headers: Content-type : Application/JSON, token: <auth token>
Approve the user with given username

# Bassa UI

### Install dependencies with


```
  $ cd ui/
  $ npm install
```


### To start
run `gulp serve`

### Make sure you have aria2 installed.
run `aria2c --enable-rpc`

