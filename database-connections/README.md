# Database connections

Database connections are established to interact with the underlying database system. Django supports both single and multiple database connections.

1. Single Database Connection:
   By default, Django uses a single database connection specified in the `DATABASES` setting. This connection is used for all database operations throughout the project. It simplifies the configuration and is suitable for most applications.

   Example:
   ```python
   # settings.py
   
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.postgresql',
           'NAME': 'mydatabase',
           'USER': 'myuser',
           'PASSWORD': 'mypassword',
           'HOST': 'localhost',
           'PORT': '5432',
       }
   }
   ```

2. Multiple Database Connections:
   Django also allows configuring and using multiple database connections for applications that need to work with multiple databases simultaneously. Each database connection is given a unique alias, and the models can specify the database alias they should use.

   Example:
   ```python
   # settings.py
   
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.postgresql',
           'NAME': 'mydatabase',
           'USER': 'myuser',
           'PASSWORD': 'mypassword',
           'HOST': 'localhost',
           'PORT': '5432',
       },
       'second_db': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'otherdatabase',
           'USER': 'otheruser',
           'PASSWORD': 'otherpassword',
           'HOST': 'localhost',
           'PORT': '3306',
       }
   }
   ```

   Models can specify the database alias using the `using` attribute:
   ```python
   # models.py
   
   class MyModel(models.Model):
       # Uses the 'default' database connection
       field1 = models.CharField(max_length=100)
       
       # Uses the 'second_db' database connection
       field2 = models.CharField(max_length=100, using='second_db')
   ```

   You can perform database operations on different connections using the `using` parameter of model queries, such as `MyModel.objects.using('second_db').filter(...)`.


   #### Example of performing database operations on different connections
  
    ```python
    # views.py
    
    from django.shortcuts import render
    from .models import MyModel
    
    def my_view(request):
        # Save a record using the 'default' database connection
        model1 = MyModel(field1='Value 1')
        model1.save()
    
        # Save a record using the 'second_db' database connection
        model2 = MyModel(field1='Value 2', using='second_db')
        model2.save()
    
        return render(request, 'my_template.html')
    ```






<br>

## Reference
https://docs.djangoproject.com/en/4.2/topics/db/multi-db/

