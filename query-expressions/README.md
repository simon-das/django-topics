# Query Expressions

Query expressions allow you to perform complex database operations and calculations using Python expressions. They provide a way to generate complex SQL queries using Django's ORM.

Query expressions are typically used in situations where simple field lookups or annotations are not sufficient to express the desired query logic. They can be used in various parts of a query, such as filtering, ordering, annotations, and updates.

Query expressions provide a powerful way to perform advanced database operations with Django's ORM, making it easier to express complex logic and calculations directly in Python code.

### Key Points
1. Syntax: Query expressions are created using Python expressions and are represented by instances of `F()` objects or other expression classes provided by Django.
2. Field Lookups: Query expressions can be used with field lookups to perform comparisons and calculations involving fields. For example, `F('quantity') * F('price')` represents the product of the 'quantity' and 'price' fields.
3. Aggregations: Query expressions can be used with aggregations to perform complex calculations on groups of records. For example, `Sum(F('quantity') * F('price'))` calculates the total value by summing the product of 'quantity' and 'price' for all records.
4. Updates: Query expressions can be used in update queries to perform calculations on fields during the update operation. For example, `Model.objects.filter(...).update(quantity=F('quantity') + 1)` increments the 'quantity' field by 1 for selected records.
5. Database Functions: Django provides various database functions that can be used in query expressions to perform specialized operations. These functions represent SQL functions and can be used for string operations, date/time manipulations, mathematical calculations, etc.

<br>

## Example
```python
from django.db.models import F, Q, Case, When, Value, IntegerField
from myapp.models import Product

# Filter products where quantity is greater than 10 and price is less than 100
filtered_products = Product.objects.filter(quantity__gt=10, price__lt=100)

# Annotate products with a new field 'total_value' that represents the product of quantity and price
annotated_products = filtered_products.annotate(total_value=F('quantity') * F('price'))

# Order the products by total_value in descending order
ordered_products = annotated_products.order_by('-total_value')

# Perform a complex conditional update: set the price to 0 for products with quantity less than 5,
# otherwise reduce the price by 10%
Product.objects.update(price=Case(
    When(quantity__lt=5, then=Value(0)),
    default=F('price') * 0.9
))

# Perform a complex filter using Q objects: retrieve products where quantity is less than 5 or price is greater than 100
complex_filtered_products = Product.objects.filter(Q(quantity__lt=5) | Q(price__gt=100))

# Use conditional expressions to annotate products with a new field 'status' based on price:
# - If price is less than 50, set status as 'Low'
# - If price is between 50 and 100, set status as 'Medium'
# - Otherwise, set status as 'High'
annotated_products = complex_filtered_products.annotate(
    status=Case(
        When(price__lt=50, then=Value('Low')),
        When(price__range=(50, 100), then=Value('Medium')),
        default=Value('High'),
        output_field=CharField()
    )
)

# Retrieve the count of products in each status category
status_counts = annotated_products.values('status').annotate(count=Count('id'))

# Use F expressions to perform arithmetic operations on annotated fields
discounted_products = annotated_products.annotate(discounted_price=F('price') * 0.8)

# Use F expressions to update the quantity of all products by adding 5
Product.objects.all().update(quantity=F('quantity') + 5)
```

<br>

## Reference
https://docs.djangoproject.com/en/4.2/ref/models/expressions/
