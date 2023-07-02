# Signals

Signals provide a way to decouple various components of an application by allowing certain senders to notify a set of receivers when a particular action or event occurs. Signals are a key part of the publish-subscribe pattern and allow you to respond to events or trigger certain actions based on specific conditions.

An overview of how signals work:

1. Signal Registration:
   - You define signal receivers by creating functions or methods that will be triggered when a signal is sent.
   - The `@receiver` decorator or the `connect()` method is used to register the receiver functions to a specific signal.

2. Signal Sending:
   - When a specific action or event occurs in your application, you can send a signal using the `send()` function.
   - The sender of the signal and any necessary data can be passed as arguments to the `send()` function.

3. Signal Receivers:
   - Signal receivers are functions or methods that are decorated or connected to a specific signal.
   - When a signal is sent, all connected receivers are invoked and executed.
   - Receivers can perform any desired actions or operations in response to the signal, such as updating models, sending notifications, or triggering other processes.

Signals are commonly used in Django for various purposes, such as:

- Pre and post-save signals: Allows performing actions before or after saving a model instance.
- User authentication signals: Triggers actions when a user logs in, logs out, or when a user's password is reset.
- Database-related signals: Provides hooks for performing actions when certain database operations occur, such as object creation, deletion, or modification.

### Example
```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from myapp.models import MyModel

@receiver(post_save, sender=MyModel)
def my_model_saved(sender, instance, created, **kwargs):
    if created:
        # Perform actions when a new MyModel instance is created
        pass
    else:
        # Perform actions when an existing MyModel instance is updated
        pass
```

In the example above, the `my_model_saved` function is registered as a receiver for the `post_save` signal of the `MyModel` model. Whenever a `MyModel` instance is saved, the receiver function will be called. It can then perform specific actions based on whether the instance is newly created or updated.

<br>

## Custom Signals
In addition to the built-in signals provided by Django, you can also create custom signals to further decouple and extend the functionality of your Django applications. Custom signals allow you to define and send signals specific to your application's needs.

1. Custom signals are typically defined as instances of the `django.dispatch.Signal` class.
2. You can define custom signals in a separate module or within a relevant Django app's `signals.py` file.
3. Custom signals should have a meaningful name that reflects the event or action they represent.

### Example
```python
from django.dispatch import Signal

# Define a custom signal
my_signal = Signal(providing_args=["arg1", "arg2"])

# Define a receiver function
def my_signal_receiver(sender, arg1, arg2, **kwargs):
    # Perform actions based on the signal
    print(f"Received signal from {sender}: arg1={arg1}, arg2={arg2}")

# Connect the receiver function to the custom signal
my_signal.connect(my_signal_receiver)

# Send the custom signal
my_signal.send(sender="my_sender", arg1="value1", arg2="value2")
```

<br>

## Reference
https://docs.djangoproject.com/en/4.2/topics/signals/
