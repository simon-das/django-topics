# Task Queue

Task queues are used to handle time-consuming or resource-intensive tasks asynchronously. They allow you to offload tasks from the main request-response cycle and process them separately in the background.

A task queue system consists of the following components:

1. **Task Queue**: It is a system or framework that manages the queuing and execution of tasks. In Django, popular task queue systems include Celery, RQ (Redis Queue), and Dramatiq.

2. **Tasks**: Tasks are units of work that need to be executed asynchronously. They can be any time-consuming or resource-intensive operations, such as sending emails, processing large datasets, or performing complex calculations.

3. **Workers**: Workers are processes or threads responsible for executing the tasks in the task queue. They listen to the queue (broker), pick up tasks as they become available, and process them.

4. **Message Broker**: A message broker is a middleware that facilitates communication between the Django application and the task queue system. It acts as a buffer, allowing tasks to be published by the application and consumed by the workers. Popular message brokers include RabbitMQ, Redis, and AWS SQS (Simple Queue Service).

To use task queues in Django, you typically follow these steps:

1. Set up a task queue system like Celery or RQ by installing the required dependencies and configuring the message broker.

2. Define your tasks as separate functions or methods within your Django project. These tasks should encapsulate the specific work that needs to be done asynchronously.

3. In your Django views or other parts of your code, instead of performing the time-consuming operations directly, enqueue the tasks to the task queue system for asynchronous processing.

4. Start the worker processes or threads that will consume tasks from the queue and execute them in the background. The workers should be configured to connect to the message broker.

### Example

1. Install Celery and configure the message broker (e.g., RabbitMQ) in your Django project.

2. Define a task function:

```python
# tasks.py
from celery import shared_task

@shared_task
def send_email_task(recipient, subject, message):
    # Code to send an email asynchronously
    # ...
    return 'Email sent successfully'
```

3. Enqueue the task in your Django view:

```python
# views.py
from .tasks import send_email_task

def my_view(request):
    # Code to handle the request
    # ...

    # Enqueue the task for sending an email
    send_email_task.delay('recipient@example.com', 'Hello', 'This is the email message')

    # Return the response
    # ...
```

4. Start the Celery worker to process the tasks:

```
$ celery -A your_project_name worker --loglevel=info
```

The worker will listen to the task queue and execute the tasks as they are enqueued.

<br>

## Reference
https://agrahariyash.medium.com/asynchronous-task-queues-in-the-django-world-a5d9be407e18
