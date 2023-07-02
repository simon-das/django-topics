# Database Functions

Database functions allow you to execute database-specific functions within your Django queries. You can use these functions to perform calculations, transformations, or extract information from the database.

#### A few commonly used database functions
1. `Concat`: Concatenates multiple string values into a single string.
2. `Lower`: Converts a string value to lowercase.
3. `Upper`: Converts a string value to uppercase.
4. `Length`: Returns the length of a string.
5. `Substr`: Extracts a substring from a string based on the specified start and end positions.
6. `ExtractYear`: Extracts the year from a date or datetime field.
7. `ExtractMonth`: Extracts the month from a date or datetime field.
8. `ExtractDay`: Extracts the day from a date or datetime field.
9. `Coalesce`: Returns the first non-null value from a list of arguments.
10. `Sum`: Calculates the sum of numeric values in a column.
11. `Avg`: Calculates the average of numeric values in a column.
12. `Count`: Counts the number of rows in a table or the number of occurrences of a value in a column.

<br>

### Example

``` python
from django.db.models import Concat, F, Sum
from django.db.models.functions import Lower, ExtractYear, ExtractMonth, ExtractDay

# Concatenating strings using `Concat`
users = User.objects.annotate(full_name=Concat('first_name', 'last_name'))
for user in users:
    print(user.full_name)

# Converting string to lowercase using `Lower`
users = User.objects.annotate(lowercase_username=Lower('username'))
for user in users:
    print(user.lowercase_username)

# Calculating the sum of numeric values using `Sum`
total_price = Product.objects.aggregate(total_price=Sum('price'))
print(total_price['total_price'])

# Extracting year, month, and day from a date field using `ExtractYear`, `ExtractMonth`, and `ExtractDay`
posts = Post.objects.annotate(
    year=ExtractYear('created_at'),
    month=ExtractMonth('created_at'),
    day=ExtractDay('created_at')
)
for post in posts:
    print(post.year, post.month, post.day)
```

<br>

## Reference
https://docs.djangoproject.com/en/4.2/ref/models/database-functions/
