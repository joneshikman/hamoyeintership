The first step used in implementation of the nosql using mongo db is firstly installing the neccessary package for execution of the nosql into my scripting enviroment using th foollowing code
MongoDB download and installation
!wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-debian71-3.0.15.tgz  # Downloads MongoDB from official repository
!tar xfv mongodb-linux-x86_64-debian71-3.0.15.tgz     # Unpack compressed file
!rm mongodb-linux-x86_64-debian71-3.0.15.tgz          # Removes downloaded file

# Default location of database is "/data/db" folder  
!mkdir /data                                          # data folder creation 
!mkdir /data/db                                       # db folder creation inside data
the second step is installation of the sever and i make use of the following code to excute that 
# Runs mongoDB server
!mongodb-linux-x86_64-debian71-3.0.15/bin/mongod --nojournal --dbpath GDrive/My\ Drive/data/db
!mongodb-linux-x86_64-debian71-3.0.15/bin/mongod --nojournal --dbpath /data/db

!mongodb-linux-x86_64-debian71-3.0.15/bin/mongod --nojournal --port 27017 --dbpath=/data/db --fork --logpath=/data/db/mongodb.log

#  --nojournal : disables journal and allows to run Mongo in Colab enviroment (reducing memory usage ) (Warning!!! Journal prevents incompleted data writes and data corruption)
#  --port      : defines the port where mongoDB will run
#  --dbpath    : defines the location of database folder, by default : /data/db
#  --fork      : runs mongoDB in background
#  --logpath   : defines the location and name of log file, by default : /data/db/mongodb.log
#  --directoryperdb : mongodb will store databases in folder structure
#  --wiredTigerDirectoryForIndexes : mongodb will store collections and indexes in folder structure 
#                                    (allows to create simbolic links and store collections and indexes in independent disks)
the next step is importation of neccessary libary and creating of database connection which the following code is been used 
# Import the python libraries
from pymongo import MongoClient
from pprint import pprint

# Choose the appropriate client
client = MongoClient()

# Connect to the test db 
db=client.test

location = db.location # the db id that was been created 
the id that as been created is location so that to make data modelling ease 
after creation of the of the id then the data is been model into the enviroment using the following code
location_details = {
    'SiteID': '209',
    'Address': 'IKEA M32',
}
the type of data modlling that was used is embedded model that is the key and value are in the same location and we have the second type which is normalization model which enables the key and values to be in different model but the one choose here is embedded model to make it easy for the querying of the data
the next step is querying of the data from respctive database that the station data that was model into the database with the foolowing code 
# Use the insert method
result = location.insert_one(location_details)
from the above code we can see that the location_details was been query to view the neccessary key and value 
and the next step is querying of the database which was done by the following database 
# Query for the inserted document.
#Queryresult = location.find_one({'SiteID': '209'})
#pprint(Queryresult)
this particular code allows the query of a particular station  that was model into th database 
those are the step use and method used in implementation of  modelling of data and into nosql database using mongo db 
now give the explanation on the type of data model used which is embedded 
Embedded Data Model
In this model, you can have (embed) all the related data in a single document, it is also known as de-normalized data model.

For example, assume we are getting the details of Location  in three different documents namely, SiteID, Locataion and, DateTime, you can embed all the three documents in a single one as shown below
{
	_id: ,
	SiteID: "10025AE336"
	locatioon_details:{
		First_Name: "IKEA ",
		Last_Name: "M29",
		DateTime : "1995-09-26"
	},
	Nox: {
		NOx Value: "245.78",
		NI: "9848022338"
	},
	: {
		city: "west london",
		Area: "united kingdom",
		State: "uniteed kingdom"
	}
Considerations use while designing Schema in my  MongoDB
Designng  my schema according to user requirements.

Combining  objects into one document if i will use them together. Otherwise separate them (but i make sure there should not be need of joins).

Duplicating  the data (but limited) because disk space is cheap as compare to compute time.

I Do joins while write, not on read.

I Optimize my schema for most frequent use cases.

I Do complex aggregation in the schema.

OTHER EXAMPLE 
Suppose a client needs a database design for his blog/website and see the differences between RDBMS and MongoDB schema design. Website has the following requirements.

Every post has the unique title, description and url.

Every post can have one or more tags.

Every post has the name of its publisher and total number of likes.

Every post has comments given by users along with their name, message, data-time and likes.

On each post, there can be zero or more comments.

In RDBMS schema, design for above requirements will have minimum three tables.

RDBMS Schema Design
While in MongoDB schema, design will have one collection post and the following structure −

{
   _id: POST_ID
   title: TITLE_OF_POST, 
   description: POST_DESCRIPTION,
   by: POST_BY,
   url: URL_OF_POST,
   tags: [TAG1, TAG2, TAG3],
   likes: TOTAL_LIKES, 
   comments: [	
      {
         user:'COMMENT_BY',
         message: TEXT,
         dateCreated: DATE_TIME,
         like: LIKES 
      },
      {
         user:'COMMENT_BY',
         message: TEXT,
         dateCreated: DATE_TIME,
         like: LIKES
      }
   ]
}
So while showing the data, in RDBMS I need to join three tables and in MongoDB, data will be shown from one collection only.

The use Command
MongoDB use DATABASE_NAME is used to create database. The command will create a new database if it doesn't exist, otherwise it will return the existing database.

Syntax
Basic syntax of use DATABASE statement is as follows −

use DATABASE_NAME
Example
If you want to use a database with name <mydb>, then use DATABASE statement would be as follows −

>use mydb
switched to db mydb
To check my  currently selected database,i used the command db

>db
mydb
If i want to check my databases list, use the command show dbs.

>show dbs
local     0.78125GB
test      0.23012GB
my  created database (mydb) is not present in list. To display database, i need to insert at least one document into it.

>db.location.insert({"name":"stationID"})
>show dbs
local      0.78125GB
mydb       0.23012GB
test       0.23012GB
In MongoDB default database is test. If I didn't create any database, then collections will be stored in test database.

The dropDatabase() Method
MongoDB db.dropDatabase() command is used to drop a existing database.

Syntax
Basic syntax of dropDatabase() command is as follows −

db.dropDatabase()
This will delete the selected database. If i have not selected any database, then it will delete default 'test' database.

Example
First,  i check the list of available databases by using the command, show dbs.

>show dbs
local      0.78125GB
mydb       0.23012GB
test       0.23012GB
>
If i  to delete new database <mydb>, then dropDatabase() command would be as follows −

>use mydb
switched to db mydb
>db.dropDatabase()
>{ "dropped" : "mydb", "ok" : 1 }
>
Now checking  list of databases.

>show dbs
local      0.78125GB
test       0.23012GB
>

The createCollection() Method
MongoDB db.createCollection(name, options) is used to create collection.

Syntax
Basic syntax of createCollection() command is as follows −

db.createCollection(name, options)
In the command, name is name of collection to be created. Options is a document and is used to specify configuration of collection.

Parameter	Type	Description
Name	String	Name of the collection to be created
Options	Document	(Optional) Specify options about memory size and indexing
Options parameter is optional, so you need to specify only the name of the collection. Following is the list of options you can use −

Field	Type	Description
capped	Boolean	(Optional) If true, enables a capped collection. Capped collection is a fixed size collection that automatically overwrites its oldest entries when it reaches its maximum size. If you specify true, you need to specify size parameter also.
autoIndexId	Boolean	(Optional) If true, automatically create index on _id field.s Default value is false.
size	number	(Optional) Specifies a maximum size in bytes for a capped collection. If capped is true, then you need to specify this field also.
max	number	(Optional) Specifies the maximum number of documents allowed in the capped collection.
While inserting the document, MongoDB first checks size field of capped collection, then it checks max field.

Examples
Basic syntax of createCollection() method without options is as follows −

>use test
switched to db test
>db.createCollection("mycollection")
{ "ok" : 1 }
>
i check the created collection by using the command show collections.

>show collections
mycollection
system.indexes
The following example shows the syntax of createCollection() method with few important options −

> db.createCollection("mycol", { capped : true, autoIndexID : true, size : 6142800, max : 10000 } ){
"ok" : 0,
"errmsg" : "BSON field 'create.autoIndexID' is an unknown field.",
"code" : 40415,
"codeName" : "Location40415"
}
>
In MongoDB, i don't need to create collection. MongoDB creates collection automatically, when i insert some document.

>db.SiteID.insert({"name" : "location"}),
WriteResult({ "nInserted" : 1 })
>show collections
mycol
mycollection
system.indexes
location

The drop() Method
MongoDB's db.collection.drop() is used to drop a collection from the database.

Syntax
Basic syntax of drop() command is as follows −

db.COLLECTION_NAME.drop()
Example
Firstly ,  i check the available collections into your database mydb.

>use mydb
switched to db mydb
>show collections
mycol
mycollection
system.indexes
Location
>
Now drop the collection with the name mycollection.

>db.mycollection.drop()
true
>
Again check the list of collections into database.

>show collections
mycol
system.indexes
Location
>
drop() method will return true, if the selected collection is dropped successfully, otherwise it will return false.

MongoDB supports many datatypes. Some of them are −

String − This is the most commonly used datatype to store the data. String in MongoDB must be UTF-8 valid.

Integer − This type is used to store a numerical value. Integer can be 32 bit or 64 bit depending upon your server.

Boolean − This type is used to store a boolean (true/ false) value.

Double − This type is used to store floating point values.

Min/ Max keys − This type is used to compare a value against the lowest and highest BSON elements.

Arrays − This type is used to store arrays or list or multiple values into one key.

Timestamp − ctimestamp. This can be handy for recording when a document has been modified or added.

Object − This datatype is used for embedded documents.

Null − This type is used to store a Null value.

Symbol − This datatype is used identically to a string; however, it's generally reserved for languages that use a specific symbol type.

Date − This datatype is used to store the current date or time in UNIX time format. You can specify your own date time by creating object of Date and passing day, month, year into it.

Object ID − This datatype is used to store the document’s ID.

Binary data − This datatype is used to store binary data.

Code − This datatype is used to store JavaScript code into the document.

Regular expression − This datatype is used to store regular expression.

 




