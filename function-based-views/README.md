# Function-based Views

In Django, function-based views (FBVs) are a way to define views using Python functions. They are the traditional and simplest approach to define views in Django.

A function-based view is a Python function that takes a `HttpRequest` object as its first parameter and returns an `HttpResponse` object as the response. You can define the logic for processing the request 
and generating the response within the function.

Function-based views are simple and straightforward, making them suitable for handling basic view logic. However, they can become lengthy and less reusable as the complexity of the view increases. In such cases, 
you might consider using class-based views, which provide a more structured and reusable approach.

### Example

```python
from django.http import HttpResponse

def my_view(request):
    # Logic to handle the request
    if request.method == 'GET':
        return HttpResponse("Hello, GET request!")
    elif request.method == 'POST':
        return HttpResponse("Hello, POST request!")
    else:
        return HttpResponse("Invalid request method.")
```

To use this function-based view in a URL pattern, you can include it in your `urls.py` file like this:

```python
from django.urls import path
from .views import my_view

urlpatterns = [
    path('my-view/', my_view, name='my-view'),
]
```

When a request matches this URL pattern, Django will call the `my_view` function and pass the `HttpRequest` object as the argument.
