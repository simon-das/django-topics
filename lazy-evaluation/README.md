# Lazy Evaluation

In Django, lazy evaluation refers to the mechanism by which certain operations or computations are deferred until their results are actually needed. It allows for more efficient processing by delaying the execution of code 
until the last possible moment.

Lazy evaluation is used in database queries with Django's QuerySet API. When you define a QuerySet, the database query is not executed immediately. Instead, the query is lazily evaluated, 
and the database is queried only when the results are accessed or iterated over. This allows you to chain multiple filters, annotations, and other query operations together without hitting the database until necessary.

### Example

```python
# Assuming we have a model called 'Product' with fields 'name', 'price', and 'category'

# Define a QuerySet to retrieve all products with a price greater than 50
products_query = Product.objects.filter(price__gt=50)

# Apply additional filters and ordering
filtered_products = products_query.filter(category='Electronics').order_by('name')

# Retrieve the first 10 products from the filtered set
top_products = filtered_products[:10]

# At this point, the actual database query is executed and the results are fetched
for product in top_products:
    print(product.name, product.price)
```

<br>

## Reference
https://docs.djangoproject.com/en/4.2/topics/db/queries/#querysets-are-lazy
