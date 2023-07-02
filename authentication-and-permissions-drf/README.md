# Authentication and Permissions in Django Rest Framework (DRF)

Django Rest Framework (DRF) provides powerful tools for implementing authentication and permissions in your API.

<br>

## Authentication
Authentication in DRF is the process of identifying the user making a request to your API. It ensures that the user is who they claim to be. DRF offers various authentication methods out of the box, including:

1. **TokenAuthentication**: This authentication method involves sending an API token with each request. The token is typically obtained by authenticating the user using their username and password. The `TokenAuthentication` class provided by DRF handles the authentication process.
2. **SessionAuthentication**: This authentication method uses Django's session framework. It requires clients to include a session cookie with each request. The server verifies the session cookie to authenticate the user.
3. **BasicAuthentication**: This authentication method requires clients to include a username and password in the request headers. The server verifies the provided credentials.
4. **JSONWebTokenAuthentication**: This authentication method uses JSON Web Tokens (JWT) to authenticate clients. JWT is a self-contained token that includes the user's identity and additional claims. The token is typically passed in the `Authorization` header of the request.

<br>

### Custom Authentication
Custom authentication classes allow you to define your own authentication logic for securing your API endpoints. Custom authentication classes provide flexibility to authenticate requests using different methods or external systems.


By using custom authentication classes in DRF, you can implement various authentication mechanisms, such as token-based authentication, OAuth, JWT, or integrating with external authentication providers. You have the flexibility to define your own authentication logic and combine multiple authentication classes as needed for your API endpoints.

To create a custom authentication class in DRF, you need to define a class that subclasses `rest_framework.authentication.BaseAuthentication` and implement the `authenticate()` method. The `authenticate()` method performs the authentication process and returns a tuple of `(user, token)` if successful or `None` if authentication fails. The `user` represents the authenticated user object, and the `token` can be used for additional authentication information or for identification purposes.

#### Example 

```python
from rest_framework.authentication import BaseAuthentication

class MyCustomAuthentication(BaseAuthentication):
    def authenticate(self, request):
        # Custom authentication logic
        # Perform necessary checks and validations
        # Return a tuple of (user, token) if authentication is successful
        # Return None if authentication fails
        pass
```

In the example above, we define a custom authentication class `MyCustomAuthentication` by subclassing `BaseAuthentication`. We need to implement the `authenticate()` method with our custom logic. In the `authenticate()` method, you would perform the necessary authentication checks, such as validating the request headers, tokens, or credentials against your custom criteria or external systems. If the authentication is successful, you would return a tuple of `(user, token)`, where `user` represents the authenticated user object and `token` provides additional authentication information if needed. If the authentication fails, you would return `None`.

Once you've created your custom authentication class, you need to add it to the `DEFAULT_AUTHENTICATION_CLASSES` setting in your Django project's settings.py file:

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'myapp.authentication.MyCustomAuthentication',
        # Other authentication classes...
    ],
}
```

In the example above, we've added our custom authentication class `myapp.authentication.MyCustomAuthentication` to the `DEFAULT_AUTHENTICATION_CLASSES` setting. You can have multiple authentication classes defined and specify the order in which they should be evaluated.

<br>

## Permissions
Permissions in DRF determine whether a user is allowed to perform a certain action on a particular resource. Permissions can be applied at the view level or the object level. DRF provides several built-in permission classes, including:

1. **IsAuthenticated**: Ensures that the user making the request is authenticated. If the user is not authenticated, they will receive a `401 Unauthorized` response.
2. **AllowAny**: Allows unrestricted access to the view or resource. This permission class is useful for publicly accessible endpoints.
3. **IsAdminUser**: Requires that the user is authenticated as an admin user. Admin users have additional privileges compared to regular users.
4. **DjangoModelPermissions**: Provides object-level permissions based on the model's default permissions (`view`, `add`, `change`, `delete`). Requires the user to be authenticated.

<br>

### Custom Permission Classes
Custom permission classes allow you to define your own authorization logic to control access to views or API endpoints. Custom permission classes provide fine-grained control over who can access specific resources in your API.

Custom permission classes in DRF provide a powerful way to implement complex authorization logic and enforce specific access controls in your API. You can create multiple custom permission classes and combine them to achieve the desired behavior for different views or objects.

To create a custom permission class in DRF, you need to define a class that subclasses `rest_framework.permissions.BasePermission`. This class should implement the `has_permission(self, request, view)` method for view-level permissions and the `has_object_permission(self, request, view, obj)` method for object-level permissions.

The `has_permission(self, request, view)` method is called to determine if the user has permission to access the entire view or API endpoint. It takes the `request` object representing the current HTTP request and the `view` object representing the view being accessed. This method should return `True` if the user has permission or `False` otherwise.

The `has_object_permission(self, request, view, obj)` method is called to determine if the user has permission to perform an action on a specific object. It takes the same parameters as `has_permission()` along with the `obj` parameter representing the object being accessed. This method should return `True` if the user has permission or `False` otherwise.

#### Example

```python
from rest_framework.permissions import BasePermission

class IsOwnerOrReadOnly(BasePermission):
    def has_permission(self, request, view):
        if request.method in ['GET', 'HEAD', 'OPTIONS']:
            return True
        return request.user.is_authenticated
    
    def has_object_permission(self, request, view, obj):
        if request.method in ['GET', 'HEAD', 'OPTIONS']:
            return True
        return obj.owner == request.user
```

In the example above, we define a custom permission class `IsOwnerOrReadOnly`. The `has_permission()` method allows read-only access (`GET`, `HEAD`, `OPTIONS`) to all API endpoints for authenticated users. The `has_object_permission()` method allows read-only access to objects but restricts modification (`PUT`, `PATCH`, `DELETE`) to only the owner of the object.

Once you've created your custom permission class, you can use it in your views or viewsets by setting the `permission_classes` attribute to include your custom permission class or by specifying it in the `permission_classes` decorator.

## Example
To apply authentication and permissions to your views or viewsets in DRF, you can use the `authentication_classes` and `permission_classes` attributes, respectively.

```python
from rest_framework.authentication import TokenAuthentication
from rest_framework.permissions import IsAuthenticated
from rest_framework.views import APIView

class MyView(APIView):
    authentication_classes = [TokenAuthentication]
    permission_classes = [IsAuthenticated]

    def get(self, request):
        # Access restricted to authenticated users
        # ...

    def post(self, request):
        # Access restricted to authenticated users
        # ...

    def delete(self, request, pk):
        # Access restricted to authenticated users and admin users
        # ...

    # ...
```

In the example above, we define a class-based view `MyView` that requires token-based authentication for all requests (`TokenAuthentication`) and allows access only to authenticated users (`IsAuthenticated` permission). You can modify the authentication and permission classes according to your requirements.

By utilizing authentication and permissions in DRF, you can secure your API endpoints and control access to resources based on user authentication and authorization requirements.

<br>

## Reference
1. https://www.django-rest-framework.org/tutorial/4-authentication-and-permissions/
2. https://www.django-rest-framework.org/api-guide/authentication/
