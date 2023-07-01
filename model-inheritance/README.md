# Model Inheritance
In Django, model inheritance allows you to create new models by deriving them from existing models. It provides a way to reuse fields and behaviors from a base model while adding additional fields and methods specific to the derived model. There are three types of model inheritance in Django: abstract base classes, multi-table inheritance, and proxy models.

<br>

## Abstract Base Classes
   1. An abstract base class is a base model that doesn't create its own database table.
   2. It serves as a base for other models to inherit from and provides common fields and methods.
   3. Abstract base classes are defined using the `abstract = True` option in the model's Meta class.

  ### Example
   ```python
   class BaseModel(models.Model):
       created_at = models.DateTimeField(auto_now_add=True)
       updated_at = models.DateTimeField(auto_now=True)

       class Meta:
           abstract = True

   class MyModel(BaseModel):
       name = models.CharField(max_length=100)
   ```

<br>

## Multi-table Inheritance
   1. Multi-table inheritance creates a separate database table for each model in the inheritance hierarchy.
   2. Each model contains its own fields and inherits all the fields from its parent model(s).

  ### Example
   ```python
   class BaseModel(models.Model):
       created_at = models.DateTimeField(auto_now_add=True)
       updated_at = models.DateTimeField(auto_now=True)

   class MyModel(BaseModel):
       name = models.CharField(max_length=100)

   class MyChildModel(MyModel):
       age = models.IntegerField()
   ```

<br>

## Proxy Models
   1. Proxy models are based on an existing model and don't create their own database table.
   2. They allow you to change the behavior of the original model by adding new methods or modifying existing ones.
   3. Proxy models are created by setting the `proxy = True` option in the model's Meta class.

  ### Example

   ```python
   class MyModel(models.Model):
       name = models.CharField(max_length=100)

   class MyModelProxy(MyModel):
       class Meta:
           proxy = True

       def greet(self):
           return f"Hello, {self.name}!"
   ```

<br>

## Reference
https://docs.djangoproject.com/en/3.2/topics/db/models/#model-inheritance
