# Custom Template Filters

Custom template filters allow you to create your own template filters that modify or format data within a template. Custom filters are useful when you need to perform specific operations on data before 
displaying it in your templates.


Custom filters can be used to perform various operations such as string manipulation, date formatting, mathematical calculations, and more. They provide a flexible way to modify data within Django templates according to your specific needs.

### Process to create a custom filter

1. Create a Python module for your custom filters. Let's say we create a file called `my_filters.py` in your app's `templatetags` directory.
2. Import the necessary modules:

   ```python
   from django import template
   ```
3. Create a library instance:

   ```python
   register = template.Library()
   ```
4. Define your custom filter as a Python function decorated with `register.filter`:

   ```python
   @register.filter
   def my_filter(value, arg1):
       # Perform the desired operations on the value
       result = value + arg1
       return result
   ```

   In this example, the `my_filter` function takes a `value` argument (the data to be filtered) and one additional argument `arg1`. It performs some operations on the value and returns the filtered result.

5. Load your custom filter library in your template:

   ```django
   {% load my_filters %}
   ```

6. Use your custom filter in the template:

   ```django
   {{ my_variable|my_filter:"arg1" }}
   ```

   The `my_variable` will be passed as the `value` argument to the `my_filter` function, and the filtered result will be displayed.

<br>

## Reference
1. https://docs.djangoproject.com/en/4.2/howto/custom-template-tags
2. https://realpython.com/django-template-custom-tags-filters/
