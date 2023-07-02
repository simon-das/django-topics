# Messages Framework

The messages framework in Django provides a way to store simple messages for the user and display them in the next response. It's commonly used to show success messages, error messages, or informational messages to the user after certain actions, such as form submissions or updates.

### Storage Backends
1. The SessionStorage class is responsible for storing all messages within the request's session. It relies on Django's contrib.sessions application to function properly.
2. The CookieStorage class stores message data in a cookie, which is signed with a secret hash to ensure its integrity. This allows the messages to persist across multiple requests. If the size of the cookie data exceeds 2048 bytes, older messages will be removed.
3. The FallbackStorage class first attempts to use the CookieStorage for storing messages. If the message data cannot fit within a single cookie, it falls back to using the SessionStorage. Like the SessionStorage, it also requires Django's contrib.sessions application to be installed and configured.

### Message levels
1. DEBUG: Messages related to development that will be disregarded (or eliminated) in a production environment.
2. INFO: Messages containing information for the user.
3. SUCCESS: Indication that an action has been successfully completed, such as "Your profile was updated successfully".
4. WARNING: Notification that a failure has not occurred yet but could potentially happen.
5. ERROR: Indication that an action has not been successful or that some other failure has occurred.

### Message tags
1. DEBUG:	debug
2. INFO:	info
3. SUCCESS:	success
4. WARNING:	warning
5. ERROR:	error

#### The process of using messages framework:

1. Add the message storage backend in settings.py:
   ```python
   # settings.py

   MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'
   ```

2. Import the message functions and use them in your views:
   ```python
   # views.py

   from django.contrib import messages
   from django.shortcuts import redirect

   def my_view(request):
       # Some logic here
       ...

       # Add messages
       messages.success(request, 'Success message.')
       messages.error(request, 'Error message.')
       messages.info(request, 'Informational message.')
       messages.warning(request, 'Warning message.')

       # Redirect to another page
       return redirect('some_view')
   ```

3. Display messages in templates:
   ```html
   <!-- template.html -->

   {% if messages %}
   <ul class="messages">
       {% for message in messages %}
       <li{% if message.tags %} class="{{ message.tags }}"{% endif %}>{{ message }}</li>
       {% endfor %}
   </ul>
   {% endif %}
   ```

<br>

## Custom Messaging Level
Message levels in Django are represented by integers, allowing you to define your own level constants for creating personalized user feedback. For instance, you can define a custom level constant called CRITICAL with a value of 50. To add a message with your custom level, you can use the messages framework like this:

```python
CRITICAL = 50

def my_view(request):
    messages.add_message(request, CRITICAL, "A serious error occurred.")
```

When defining custom message levels, it is important to ensure that they do not conflict with the existing levels. The default values for the built-in levels are as follows:

- DEBUG: 10
- INFO: 20
- SUCCESS: 25
- WARNING: 30
- ERROR: 40

<br>

## Reference
https://docs.djangoproject.com/en/4.2/ref/contrib/messages/
