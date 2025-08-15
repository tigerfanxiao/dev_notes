
# Closure


```python
def outer_func():
# here is the local scope where the inner func has been defined
	message = 'HI'
	def inner_func():
		# variable message is defined outside of inner_func
		print(mesage)
	return inner_func
	
my_func = outer_func() # my_func is a function object
my_func.__name__ # inner_func

print(my_func())
```

所以 closure 可以实现向一个没有参数的函数进行传参, 并改变函数的行为
Closure in Python
```python
def outer_func(msg):
	message = msg
	def inner_func():
		print(mesage)  # variable message is defined outside of inner_func
	return inner_func
	
hi_func = outer_func('hi')
hello_func = outer_func('hello')

hi_func()
hello_func()
```

Closure in JS
```js
function html_tag(tag) {
	function wrap_text(text) {
		console.log(`<{tag}>{text}</{tag}>`)
	}
	return wrap_text;
}

print_h1 = html_tag('h1');
print_h1('hello world')
print_p = html_tag('p');
print_p('hello world')
```

```python
import logging
logging.basicConfig(filename='example.log', level=logging.INFO)

def logger(func):
	def log_func(*args):
		logging.info(f'Running {func.__name__} with arguments {args}')
		print(func(*args))
	return log_func

def add(x, y):
	return x + y

def sub(x, y):
	return x - y

add_logger = logger(add)
sub_logger = logger(sub)

add_logger(3, 3)
sub_logger(3, 3)

```
## First-Class function
Treat function as object


常常用于增加通用的额外功能. 比如日志, 性能测试, 事务处理, 缓存

`@wraps` 保持 `func.__name__`, `func.__doc__`

```python
from typing import Callable, Any
from functools import wraps


# 性能测试函数: 测试执行函数需要多少时间.
def benchmark(func: Callable[..., Any]) -> Callable[..., Any]:  
	@wraps(func) # 如果不这么写，在多个装饰器叠加时，在引用func名字时会错误
    def wrapper(*args: Any, **kwargs: Any) -> Any:  
        start_time = perf_counter()  
        value = func(*args, **kwargs)   # 调用函数
        end_time = perf_counter()  
        run_time = end_time - start_time  
        logging.info(  
            f"Execution of {func.__name__} took {run_time}"  
        )  
        return value  
    return wrapper

def with_logging(func: Callable[..., Any]) -> Callable[..., Any]:  
    @wraps(func)  
    def wrapper(*args: Any, **kwargs: Any) -> Any:  
        logging.info(f"Calling {func.__name__}")  
        value = func(*args, **kwargs)  
        logging.info(f"Finished calling {func.__name__}")  
        return value  
    return wrapper
    

def is_prime(number: int) -> bool:  
    if number < 2:  
        return False  
    for element in range(2, int(sqrt(number) + 1)):  
        if number % element == 0:  
            return False  
    return True
    
@with_logging
@benchmark
def count_prime_numbers(upper_bound: int) -> int:  
    count = 0  
    for number in range(upper_bound):  
        if is_prime(number):  
            count += 1  
    return count


```

带参数的装饰器
```python

# 把结果写到日志中
def with_logginer(logger: logging.Logger)
	def decorator(func: Callable[..., Any]) -> Callable[..., Any]:  
	    @wraps(func)  
	    def wrapper(*args: Any, **kwargs: Any) -> Any:  
	        logging.info(f"Calling {func.__name__}")  
	        value = func(*args, **kwargs)   # 调用函数
	        logging.info(f"Finished calling {func.__name__}")  
	        return value  
	    return wrapper
	return decorator


# 使用装饰器
import logging
logger = logging.getLogger("my_app")

@with_logging(logger) #传入 logger 对象
@benchmark
def count_prime_numbers(upper_bound: int) -> int:  
    count = 0  
    for number in range(upper_bound):  
        if is_prime(number):  
            count += 1  
    return count
```


用functools再优化一下三层变两层

```python

def with_logging(func: Callable[..., Any], logger:logging.Logger) -> Callable[..., Any]:  
    @functools.wraps(func)  
    def wrapper(*args: Any, **kwargs: Any) -> Any:  
        logger.info(f"Calling {func.__name__}")  
        value = func(*args, **kwargs)  
        logger.info(f"Finished calling {func.__name__}")  
        return value  
    return wrapper  
  
# 把logger参数直接灌到with_logging中
with_default_logging = functools.partial(with_logging, logger=logger)

@with_default_logging  
@benchmark  
def count_prime_numbers(upper_bound: int) -> int:  
    count = 0  
    for number in range(upper_bound):  
        if is_prime(number):  
            count += 1  
    return count
	
```