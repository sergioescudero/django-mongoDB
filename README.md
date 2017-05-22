## Django + MongoDB

This is an example of how to use MongoDB as database in a Django app.

#### Software versions
- Python 3.6.0
- Django 1.11
- MongoDB 3.2.10


#### Modules	
- appdirs	1.4.3	
- mongoengine	0.9.0	
- packaging	16.8	
- pip	9.0.1	
- pymongo	2.8.1	
- pyparsing	2.2.0	
- pytz	2017.2	
- setuptools	35.0.2	
- six	1.10.0	

#### Setting up Django to work with MongoDB, settings.py
The current database engine is replaceed with with a engine. That will instruct Django to stop using its built-in database engines:


```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.dummy'
    }
}
```

And the connection to the new Mongo database is set:

```python
connect('my_db', username='my_user', password='MY_PASSWORD')
```


If it is wanted that MongoDB hands the session tracking, the following class has to be added to *MIDDLEWARE_CLASSES* if is not added,

```python
'django.contrib.sessions.middleware.SessionMiddleware'
```

make sure that the following is in *INSTALLED_APPS*

```python
'django.contrib.sessions'
```


and the following code:

```python
SESSION_ENGINE = 'mongoengine.django.sessions'
SESSION_SERIALIZER = 'mongoengine.django.sessions.BSONSerializer'
```

#### How to persist an object in MongoDB
