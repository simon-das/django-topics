# Caching

The cache framework in Django provides a flexible and powerful way to cache data and improve the performance of your Django applications. It allows you to store the results of expensive or frequently accessed operations in memory or other storage backends for faster retrieval.

## Key components

1. Cache Backends:
   Django supports various cache backends, which are responsible for storing and retrieving cached data. Some commonly used cache backends are:

    - Memcached is a distributed in-memory cache system. It allows you to store key-value pairs in memory for fast retrieval. Think of it as a big cache that can be shared across multiple servers or processes. It's commonly used for caching data in web applications to improve performance.
    - Redis is an in-memory data structure store that can be used as a cache, database, or message broker. It provides advanced data types and features such as persistence and pub/sub messaging. Redis cache is known for its speed, flexibility, and the ability to handle complex data structures.
    - FileCache is a cache backend that stores cached data on the file system. It saves the cached data in files on your server's disk. This cache backend is suitable for smaller applications or when you want a simple caching solution without the need for external dependencies.
    - DatabaseCache uses a database table to store cached data. It stores the cached data in a dedicated table within your application's database. This cache backend is useful when you already have a database available and want a persistent storage option for your cache.
  
     You can configure the cache backend(s) in the `CACHES` setting in your Django project's `settings.py` file.
  
     ```python
     CACHES = {
         'default': {
             'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
             'LOCATION': '127.0.0.1:11211',
         }
     }
     ```

2. Cache Keys:
   Cache keys are used to uniquely identify cached data. They are typically strings that represent the data being cached. Django provides a cache key generation algorithm that combines the cache prefix (if specified) with the cache key itself.

   It's important to choose cache keys that are unique and descriptive to avoid collisions and ensure proper cache management.

   ```python
   from django.core.cache import cache
   
   def get_user_data(user_id):
       cache_key = f'user_data_{user_id}'
       data = cache.get(cache_key)
       if data is None:
           data = fetch_user_data_from_database(user_id)
           cache.set(cache_key, data, timeout=3600)
       return data
   ```

3. Caching API:
   Django provides a set of functions and methods to interact with the cache, allowing you to perform operations such as setting, retrieving, and deleting cached data. Some commonly used cache API methods include:

   - `cache.get(key)`: Retrieves the cached data associated with the specified key.
   - `cache.set(key, value, timeout)`: Sets the value for the given key in the cache with an optional timeout value.
   - `cache.add(key, value, timeout)`: Adds the key-value pair to the cache if the key doesn't already exist.
   - `cache.delete(key)`: Deletes the cached data associated with the specified key.
   - `cache.clear()`: Clears the entire cache, removing all cached data.

<br>

   ```python
   from django.core.cache import cache
   
   def expensive_function():
       result = cache.get('expensive_result')
       if result is None:
           result = perform_expensive_operation()
           cache.set('expensive_result', result, timeout=3600)
       return result
   ```

4. Cache Middleware:
   Django provides cache middleware that can automatically cache the output of views based on certain conditions. By enabling cache middleware, you can cache the responses from your views and serve them directly from the cache, reducing the processing time.

   The cache middleware allows you to specify cache control rules based on factors such as the request method, URL patterns, or custom logic.

   ```python
   MIDDLEWARE = [
       # Other middleware classes...
       'django.middleware.cache.UpdateCacheMiddleware',
       'django.middleware.common.CommonMiddleware',
       'django.middleware.cache.FetchFromCacheMiddleware',
   ]
   ```

5. Template Fragment Caching:
   Django's template system includes template fragment caching, which allows you to cache specific sections of your templates. By caching template fragments, you can avoid redundant computations and database queries.

   Template fragment caching is achieved using the `{% cache %}` template tag, which wraps the content to be cached.

   ```django
   {% load cache %}
   {% cache 3600 "my_template_fragment" %}
       <!-- Cached content here -->
   {% endcache %}
   ```

<br>

## Reference
https://docs.djangoproject.com/en/4.2/topics/cache/
