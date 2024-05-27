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