# What would be the solution to use for an application utilizing a CouchDB or other Document store?

If I picked dinamodb, that is the document store solution from AWS, I would use the pynamodb library, that allows to use a declarative syntax to define models (it is a basic ORM)

Something like:

```
from pynamodb.models import Model
from pynamodb.attributes import UnicodeAttribute

class UserModel(Model):
    """
    A DynamoDB User
    """
    class Meta:
        table_name = "dynamodb-user"
    email = UnicodeAttribute(null=True)
    first_name = UnicodeAttribute(range_key=True)
    last_name = UnicodeAttribute(hash_key=True)

```

pynamodb used boto underneath, so the configuration is standard. Credentials can be setup as an env var or using IAM, that is the more secure and preferred mechanism.

If I picked couchdb, first we would need a couch db server. In development we could use the couchdb docker image, but in production/staging I would rather use a server installation from the AWS marketplace. The reason for that is ready to use, you can put your app in production right away, and tune it later when the necessity arrives. 

To use couchdb from the django app, I would use the library couchdbkit. The syntax and usage is similar to pynamodb:

```
 from couchdbkit import Document

 class Greeting(Document):
      author = StringProperty()
      content = StringProperty()
      date = DateTimeProperty()
```

The settings necessary to connect django and couchdb are:

```
 COUCHDB_DATABASES = (
     ('djapp.db1’, 'http://127.0.0.1:5984/db1’),
 )

 INSTALLED_APPS = (
     ....
     'couchdbkit.ext.django’,
     ....
```
