# Class-based Views

In Django, class-based views (CBVs) are an alternative way to define views using Python classes rather than function-based views (FBVs). CBVs provide a more structured and reusable approach to define 
views by encapsulating related functionality within class methods.

Class-based views offer several advantages over function-based views, including code reusability, inheritance, and the ability to override specific methods to customize behavior. 
They also provide a more object-oriented approach to view development.

### Example

```python
from django.views import View
from django.http import HttpResponse

class MyView(View):
    def get(self, request):
        # Logic to handle GET request
        return HttpResponse("Hello, GET request!")

    def post(self, request):
        # Logic to handle POST request
        return HttpResponse("Hello, POST request!")
```

To use this class-based view in a URL pattern, it should be included in the `urls.py` file like this:

```python
from django.urls import path
from .views import MyView

urlpatterns = [
    path('my-view/', MyView.as_view(), name='my-view'),
]
```

The `as_view()` method converts the class into a callable view function that Django can understand.

Class-based views provide additional methods that you can override to customize their behavior, such as `put()`, `delete()`, and `dispatch()`. 
They also allow for inheritance, so you can create subclasses to reuse common functionality and override specific methods as needed.

<br>

## Reference
https://docs.djangoproject.com/en/4.2/topics/class-based-views/
