# CSRF Token

CSRF (Cross-Site Request Forgery) protection is a security feature in Django that helps prevent malicious websites from making unauthorized requests on behalf of a user. It guards against CSRF attacks by including a CSRF token in forms and AJAX requests, which is validated on the server side.

When a user visits a Django site, the server generates a unique CSRF token and includes it in the response. The token is typically stored in a cookie (by default, with the name 'csrftoken') and also included in the HTML form as a hidden input field or as a custom header in AJAX requests.

When the user submits a form or sends an AJAX request, the CSRF token is included. The server then checks if the token is valid and matches the one stored in the user's session. If the token is valid, the request is processed; otherwise, a CSRF verification failure error is raised.

To enable CSRF protection in Django, you need to make sure that the `django.middleware.csrf.CsrfViewMiddleware` middleware is included in the `MIDDLEWARE` setting in your project's `settings.py` file. This middleware takes care of generating and validating the CSRF token.

Here's an example of how to use CSRF protection in a Django form template:

```html
<form method="POST" action="{% url 'my_view' %}">
  {% csrf_token %}
  <!-- other form fields -->
  <input type="submit" value="Submit">
</form>
```

In this example, the `{% csrf_token %}` template tag inserts the CSRF token as a hidden input field within the form. When the form is submitted, the token will be included in the request, and Django will validate it.

For AJAX requests, you need to include the CSRF token in the request headers **manually**. You can obtain the CSRF token value from the cookie or from the `csrftoken` cookie value if using JavaScript.

<br>

## Reference
https://docs.djangoproject.com/en/4.2/ref/csrf/
