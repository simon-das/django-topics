# Logging

Logging in Django is the process of recording events, messages, or errors that occur during the execution of your application. It helps you monitor and troubleshoot your application by 
providing valuable information about its behavior and any issues that may arise.

By properly configuring and utilizing logging in your Django application, you can gain valuable insights into its behavior, track down issues, and monitor its performance and stability. It's an essential tool for debugging, troubleshooting, and maintaining your application effectively.

Django provides a built-in logging module that you can use to configure and manage logging in your application. Here's how it works:

1. **Logging Levels**: Logging in Django is based on different levels that indicate the severity of a logged message. The available levels, in increasing order of severity, are `DEBUG`, `INFO`, `WARNING`, `ERROR`, and `CRITICAL`. You can choose the appropriate level based on the type of information you want to log.

2. **Configuration**: Django's logging can be configured in your project's settings file (`settings.py`). You can define various settings such as the logging level, log format, log handlers, and loggers.

3. **Loggers**: Loggers are objects that you use to emit log messages. They allow you to specify different sources or areas within your application where logs can originate from. Django provides a default logger named `'django'`, which logs messages related to the Django framework itself. You can also create custom loggers to separate and categorize logs from different parts of your application.

4. **Handlers**: Handlers determine what happens to log messages once they are emitted by loggers. Django supports different types of handlers, such as `ConsoleHandler`, `FileHandler`, `EmailHandler`, etc. Each handler can be configured to control aspects like the log output destination, formatting, and filtering.

5. **Filters**: Filters provide a way to selectively process log records based on certain criteria. You can create custom filter classes and attach them to handlers or loggers to control which log records get processed. Filters can be useful for applying additional conditions or fine-grained filtering logic before handling a log record. 

6. **Formatters**: Formatters control the formatting of log records before they are output. They define the structure and content of the logged messages, including timestamps, log levels, module names, etc. Django provides several built-in formatter classes, such as `logging.Formatter`, and `logging.JSONFormatter`.

8. **Logging in Code**: You can add logging statements in your code using the `logging` module. For example, you can use `logger.debug()`, `logger.info()`, `logger.warning()`, `logger.error()`, or `logger.critical()` to log messages at different levels. These messages will be emitted by the corresponding logger and handled according to the configured handlers.

Here's a simplified example of configuring logging in Django's settings file:

```python
# settings.py
import logging

class MyCustomFilter(logging.Filter):
    def filter(self, record):
        # Add your filtering logic here
        # Return True to process the log record, False to ignore it
        return True

LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'filters': {
        'my_filter': {
            '()': MyCustomFilter,
        },
    },
    'formatters': {
        'my_formatter': {
            'format': '%(asctime)s [%(levelname)s] %(message)s',
            'datefmt': '%Y-%m-%d %H:%M:%S',
        },
    },
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
            'filters': ['my_filter'],
            'formatter': 'my_formatter',
        },
    },
    'loggers': {
        'my_app': {
            'handlers': ['console'],
            'level': 'DEBUG',
        },
    },
}
```

You can then use logging in your code like this:

```python
# myapp/views.py
import logging

logger = logging.getLogger(__name__)

def my_view(request):
    logger.debug('This is a debug message')
    logger.info('This is an info message')
    logger.warning('This is a warning message')
    logger.error('This is an error message')
    logger.critical('This is a critical message')
    # ...
```

When your application runs, the log messages will be emitted by the logger and processed by the configured handlers. In this case, they will be displayed in the console.

<br>


## Reference
https://docs.djangoproject.com/en/4.2/topics/logging/
