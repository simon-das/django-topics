# Stored Procedure

A stored procedure is a set of precompiled SQL statements stored in the database and can be executed on demand. Django supports calling stored procedures using the `connection.cursor()` method.

### Example
Suppose you have a stored procedure named `get_people` in your database that returns a list of people. You can call this stored procedure using Django's database connection:

```python
from django.db import connection

def execute_stored_procedure():
    with connection.cursor() as cursor:
        # Execute the stored procedure
        cursor.execute("CALL your_stored_procedure()")
        
        # Fetch the results, if any
        results = cursor.fetchall()
        
        # Process the results
        for row in results:
            # Access individual columns in the row
            col1 = row[0]
            col2 = row[1]
            
            # Perform further processing on the data
            # ...

execute_stored_procedure()
```

<br>

## Reference
https://dev.to/adii9/stored-procedures-and-django-a-match-made-in-performance-heaven-1fi9#:~:text=Using%20Stored%20Procedures%20in%20Django&text=When%20you%20execute%20a%20stored,as%20a%20list%20of%20tuples
