# Throttling

Throttling in Django REST Framework (DRF) is a mechanism that limits the number of requests a user or client can make within a certain time period. It helps prevent abuse, protect the server from excessive load, and ensure fair usage of the API.

DRF provides built-in throttling classes that you can configure in your project. Throttling can be applied globally to all views or applied to specific views or viewsets.

#### Key Components

1. Throttling Classes:
   Throttling classes define the actual throttling behavior. DRF provides several built-in throttling classes, such as:
   - `AnonRateThrottle`: Limits requests per anonymous user.
   - `UserRateThrottle`: Limits requests per authenticated user.
   - `ScopedRateThrottle`: Limits requests based on a custom scope (e.g., per endpoint or per action).

2. Throttling Configuration:
   Throttling classes are configured in the Django settings.py file under the `DEFAULT_THROTTLE_CLASSES` setting. For example:
   ```python
   REST_FRAMEWORK = {
       'DEFAULT_THROTTLE_CLASSES': [
           'rest_framework.throttling.AnonRateThrottle',
           'rest_framework.throttling.UserRateThrottle',
       ],
       'DEFAULT_THROTTLE_RATES': {
           'anon': '100/hour',
           'user': '1000/day',
       }
   }
   ```

3. Rate Configuration:
   The `DEFAULT_THROTTLE_RATES` setting defines the rate limits for each throttle class. It uses a dictionary where the keys correspond to the throttle class names or custom scope names, and the values define the rate limit. Rate limits can be specified in various formats, such as requests per second, minute, hour, or day.

4. Applying Throttling:
   Throttling can be applied at different levels:
   - Global Level: By setting `DEFAULT_THROTTLE_CLASSES` in the settings, throttling will be applied to all views by default.
   - View Level: You can specify throttling classes for individual views or viewsets using the `throttle_classes` attribute.
   - Action Level: For viewsets, you can set throttling classes for specific actions using the `throttle_classes` attribute in the action decorator.

<br>

## Examples
1. Applying Throttling at View Level:
```python
# myapp/views.py

from rest_framework.views import APIView
from rest_framework.throttling import AnonRateThrottle, UserRateThrottle

class MyView(APIView):
    throttle_classes = [AnonRateThrottle, UserRateThrottle]
    
    def get(self, request):
        # Handle GET request logic
        ...
```

2. Applying Throttling at Action Level (Viewset):
```python
# myapp/views.py

from rest_framework.viewsets import ModelViewSet
from rest_framework.decorators import action

class MyViewSet(ModelViewSet):    
    @action(detail=True, methods=['post'], throttle_classes=[UserRateThrottle])
    def custom_action(self, request, pk=None):
        # Handle custom action logic
        ...
```

<br>


## Reference
https://www.django-rest-framework.org/api-guide/throttling/
