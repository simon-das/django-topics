# Middleware

Middleware in Django is a powerful mechanism that allows you to process requests and responses globally across your Django project. Middleware sits between the web server and the view functions, allowing you to perform operations such as modifying request/response objects, authentication, logging, error handling, and more. It provides a flexible way to handle common functionality across your entire project without having to repeat code in every view function.

Middleware works as a series of hooks that intercept the incoming requests and outgoing responses. Each middleware component can process the request before it reaches the view function and can modify the response before it's sent back to the client. You can have multiple middleware classes defined and specify their order of execution. The order of middleware classes is important as they are executed in the order they are defined.

To create a middleware class in Django, you need to define a class that implements the following methods:

- `__init__(self, get_response)`: This method is called when the middleware is instantiated. It receives a `get_response` parameter, which is a callable that represents the next middleware or the view function.

- `__call__(self, request)`: This method is called for each request. It receives the `request` object and should return a response object. This is where you can perform operations before and after the view function is executed.

Here's an example of a simple middleware class that adds a custom header to the response:

```python
class CustomHeaderMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        # Process the request before the view function is executed

        # Get response
        response = self.get_response(request)

        # Process the response before it's sent back to the client
        response['X-Custom-Header'] = 'Hello from middleware'

        return response
```

In the example above, we define a middleware class `CustomHeaderMiddleware` that adds a custom header `'X-Custom-Header'` to the response. The `__init__` method stores the `get_response` callable, which represents the next middleware or the view function. The `__call__` method is called for each request, allowing us to process the request and modify the response.

To enable the middleware, you need to add it to the `MIDDLEWARE` setting in your Django project's settings.py file:

```python
MIDDLEWARE = [
    # Other middleware classes...
    'myapp.middleware.CustomHeaderMiddleware',
]
```

<br>

## Reference
https://docs.djangoproject.com/en/4.2/topics/http/middleware/
