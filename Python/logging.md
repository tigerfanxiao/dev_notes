
# Thinking

1. Never think your log is secure, never log password, credit card in log
2. Think of deleting log

原本的例子


video 
https://www.youtube.com/watch?v=pxuXaaT1u3k&t=45s





```python
import logging

def main() -> None:
	logging.basicConfig(level=logging.WARNING)
	logging.debug("This is a debug message.")
	logging.info("This is an info message")
	logging.warning("This is a warning message")
	logging.error("This is an error message")
	logging.critical("This is a critical message")

if __name__ == "__main__":
	main()
```

format logging
```python
logging.basicConfig(
	level = logging.DEBUG,
	format = "%(asctime)s %(levelname)s %(message)s",
	datefmt = "%Y-%m-%d %H:%M:%S",
	filename = "basic.log",
)
```

logging server

```python
PAPERTRAIL_HOST = "logs.papertrailapp.com"
PAPERTRAIL_PORT = 37554

def main() -> None:
	# you can it with your app name, backend nane, front-end name
	logger = logging.getLogger("drjancodes")  
	logger.setLevel(logging.DEBUG)
	handler = SysLogHandler(address=(PAPERTRAIL_HOST, PAPERTRAIL_PORT))
	logger.addHandler(handler)
	logger.debug("This is a debug message.")
```