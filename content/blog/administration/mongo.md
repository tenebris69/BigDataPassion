+++
author = ""
categories = []
date = "2017-06-01T21:43:17+02:00"
description = ""
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Docker wprowadzenie"
type = "post"

draft = true
+++

~~~
docker run --name some-mongo -d mongo
docker run --name some-app --link some-mongo:mongo -d application-that-uses-mongo


docker cp przyklady_json/profile_json.txt some-mongo:/tmp

mongoimport --jsonArray --db test --collection docs --file example2.json
mongoimport --jsonArray --db test --collection docs --file /tmp/profile_json.txt

docker exec -it some-mongo bash



show dbs
use test
show collections

db.docs.find()
db.docs.findOne()

db.docs.find(query, projection)
db.docs.findOne({"status" : "initialized"})
db.docs.findOne({"status" : "initialized"},{"customer":1,"organization.otherNames":1})
~~~