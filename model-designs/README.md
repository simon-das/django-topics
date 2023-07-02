# Model Designs

Model design refers to how you structure and organize your database tables and relationships using Django's ORM (Object-Relational Mapping). Two common approaches to model design are normalized design and fat model design.

## Normalized Design
   1. In normalized design, the focus is on reducing data redundancy and maintaining data integrity.
   2. Tables are organized to minimize data duplication by breaking them into smaller, related tables.
   3. Relationships between tables are established using foreign keys.
   4. Normalization follows specific rules (normal forms) to ensure data consistency and eliminate anomalies.
   5. This approach is recommended for complex data structures or when dealing with large datasets.
   6. It helps maintain data integrity, reduces redundancy, and improves database efficiency.

### Example
Suppose you have an e-commerce application with two main entities: "Product" and "Category". In a normalized design, you would have separate tables for each entity. The "Product" table would contain product-related 
information (e.g., name, price, description), while the "Category" table would store category-related details (e.g., name, description). The relationship between these two tables is established using a foreign key in the "Product" table to reference the corresponding "Category" table.

``` python
from django.db import models

class Category(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()

class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=8, decimal_places=2)
    description = models.TextField()
    category = models.ForeignKey(Category, on_delete=models.CASCADE)
```

## Fat Model Design
   1. In fat model design, the focus is on moving logic and behavior closer to the data model.
   2. Models are enriched with methods and properties that encapsulate business logic and perform various operations.
   3. This approach follows the principle of "skinny views, fat models" to keep views and controllers lightweight.
   4. Fat model design can make the codebase more organized and modular by centralizing logic within the models.

### Example
Continuing with the e-commerce application example, in a fat model design, you might include methods within the "Product" model such as `calculate_discounted_price()` or `get_related_products()`. These methods can perform calculations, retrieve related data, or implement specific business rules associated with the "Product" entity.

``` python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(max_digits=8, decimal_places=2)
    description = models.TextField()
    category = models.ForeignKey('Category', on_delete=models.CASCADE)

    def calculate_discounted_price(self, discount_percentage):
        return self.price * (1 - discount_percentage)

    def get_related_products(self):
        return Product.objects.filter(category=self.category)

class Category(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()
```

Both normalized and fat model designs have their advantages and considerations. The choice between them depends on the specific requirements of your application, the complexity of your data, and the trade-offs you are willing to make in terms of performance, maintainability, and development speed. It's important to strike a balance that best suits your application's needs.
