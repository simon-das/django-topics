# Custom Template Tags

Custom template tags allow you to create your own template tags that extend the functionality of Django's template system. Template tags are used within templates to perform more complex operations 
or to output dynamic content.

Custom template tags provide a way to encapsulate complex logic or reusable functionality and make it available within your templates. They can be used to generate dynamic content, manipulate data, render custom HTML, and more. By creating custom template tags, 
you can extend the capabilities of Django's template system to suit your specific requirements.

### The process to create a custom template tag

1. Create a Python module for your custom template tags. Let's say we create a file called `my_tags.py` in your app's `templatetags` directory.
2. Import the necessary modules:

   ```python
   from django import template
   ```
3. Create a library instance:

   ```python
   register = template.Library()
   ```
4. Define your custom template tag as a Python class or function decorated with `register.tag` or `register.simple_tag`:
   - If you're creating a simple template tag as a function, use `register.simple_tag`:

     ```python
     @register.simple_tag
     def my_tag(arg1, arg2):
         # Perform desired operations
         result = arg1 + arg2
         return result
     ```

   - If you're creating a more complex template tag as a class, use `register.tag`:

     ```python
     @register.tag
     def my_tag(parser, token):
         # Perform desired operations
         # Return a Node object that handles rendering
         return MyTagNode()
     ```

   In both cases, the custom template tag performs some operations and returns the desired result.
5. Load your custom tag library in your template:

   ```django
   {% load my_tags %}
   ```
6. Use your custom template tag in the template:

   ```django
   {% my_tag arg1 arg2 %}
   ```

   The custom template tag is invoked with the given arguments, and the tag's logic is executed to produce the desired output.

<br>

## Reference
1. https://docs.djangoproject.com/en/4.2/howto/custom-template-tags
2. https://realpython.com/django-template-custom-tags-filters/
