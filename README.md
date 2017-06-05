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

#### Admin module
Admin Django module does not work. The following error arises when the hotel model is registered:

```python
admin.site.register(Hotel)
```


```
  File "/../Benchmark/hotels/admin.py", line 4, in <module>
    admin.site.register(Hotel)
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/contrib/admin/sites.py", line 103, in register
    for model in model_or_iterable:
TypeError: 'TopLevelDocumentMetaclass' object is not iterable
```

There is a third party module that replace Admin Django Module https://github.com/jschrewe/django-mongoadmin.

But the following error arises

```
/Users/sergio/Desarrollo/python/virtualenv/benchmark/bin/python3 /Users/sergio/Desarrollo/workspaces/.../manage.py runserver
Unhandled exception in thread started by <function check_errors.<locals>.wrapper at 0x105d51048>
Traceback (most recent call last):
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/utils/autoreload.py", line 227, in wrapper
    fn(*args, **kwargs)
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/core/management/commands/runserver.py", line 117, in inner_run
    autoreload.raise_last_exception()
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/utils/autoreload.py", line 250, in raise_last_exception
    six.reraise(*_exception)
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/utils/six.py", line 685, in reraise
    raise value.with_traceback(tb)
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/utils/autoreload.py", line 227, in wrapper
    fn(*args, **kwargs)
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/__init__.py", line 27, in setup
    apps.populate(settings.INSTALLED_APPS)
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/apps/registry.py", line 85, in populate
    app_config = AppConfig.create(entry)
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/apps/config.py", line 94, in create
    module = import_module(entry)
  File "/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python3.6/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 978, in _gcd_import
  File "<frozen importlib._bootstrap>", line 961, in _find_and_load
  File "<frozen importlib._bootstrap>", line 950, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 655, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 678, in exec_module
  File "<frozen importlib._bootstrap>", line 205, in _call_with_frames_removed
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/mongoadmin/__init__.py", line 1, in <module>
    from .options import *
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/mongoadmin/options.py", line 6, in <module>
    from mongoadmin.contenttypes.models import ContentType
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/mongoadmin/contenttypes/models.py", line 1, in <module>
    from .utils import has_rel_db, get_model_or_document
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/mongoadmin/contenttypes/utils.py", line 5, in <module>
    from django.contrib.contenttypes.models import ContentType
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/contrib/contenttypes/models.py", line 139, in <module>
    class ContentType(models.Model):
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/db/models/base.py", line 110, in __new__
    app_config = apps.get_containing_app_config(module)
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/apps/registry.py", line 247, in get_containing_app_config
    self.check_apps_ready()
  File "/Users/sergio/Desarrollo/python/virtualenv/benchmark/lib/python3.6/site-packages/django/apps/registry.py", line 125, in check_apps_ready
    raise AppRegistryNotReady("Apps aren't loaded yet.")
django.core.exceptions.AppRegistryNotReady: Apps aren't loaded yet.
```

#### How to persist an object in MongoDB
