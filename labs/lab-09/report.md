# MongoDB Licensing

[Blog Post](https://rcos.io/projects/natsr4/exalendar/blog)

# Installing MongoDB

Successful connection through `mongo`:

![Successful Connection](./images/mongodb-connection-accepted.png)

"Connection Accepted" can be seen in the second line of the output image above.

# Importing Data

Successful import using `mongoimport`:

![Successful Import](./images/mongo-create-collection.png)

# Basic Queries

Successful finds after insert and update:

![Successful Insert/Update](./images/mongo-insert-update.png)

git diff:

![git diff](./images/mongo-git-diff.png)

# Driving Queries

checkpoint4.py:

```python
from pymongo import MongoClient
import pprint
client = MongoClient()

if __name__ == '__main__':
    db = client.mongo_db_lab

    # Fetch all records
    for definition in db.definitions.find():
        pprint.pprint(definition)

    # Fetch one record
    pprint.pprint(db.definitions.find_one())

    # Fetch a specific record
    word = db.definitions.find_one({"word": "B-Vector"})
    pprint.pprint(word)

    # Fetch a record by object id
    # Uses the id of the word in the previous query
    pprint.pprint(db.definitions.find_one({"_id": word['_id']}))

    # Inserting a new record
    new_word = {"word": "Commons Dining Hall",
                "definition": " n. (RPI) The best place to get grilled cheese sandwiches (if you're lucky)"}
    db.definitions.insert_one(new_word)
    pprint.pprint(db.definitions.find_one({"word": "Commons Dining Hall"}))
```

pprint output:

![pprint output](./images/mongo-pprint.png)

# Random Word Requester

checkpoint5.py:

```python
from pymongo import MongoClient
import datetime
client = MongoClient()


def random_word_requester():
    '''
    This function should return a random word and its definition and also
    log in the MongoDB database the timestamp that it was accessed.
    '''
    db = client.mongo_db_lab
    # Grab a random word using aggregate
    word = db.definitions.aggregate([{"$sample": {"size": 1}}]).next()

    db.definitions.update_one(
            {"_id": word['_id']},
            {"$push": {"dates": datetime.datetime.isoformat(datetime.datetime.utcnow())}})
    
    return word

if __name__ == '__main__':
    print(random_word_requester())
```

Find after running until a duplicate:

![Find User](./images/mongo-find-user.png)
