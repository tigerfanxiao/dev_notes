备注: 
1. 需要看一下同一个作者的pydantic的视频, 有2个深度视频
https://www.youtube.com/watch?v=M81pfi64eeM


# 第一天
视频链接 https://www.youtube.com/watch?v=iukOehU5aF4
github: https://github.com/CoreyMSchafer/FastAPI-Full-Course
初始化环境
```shell
uv init fastapi_blog
cd fastapi_blog

uv add "fastapi[standard]" # 使用uv安装包
```
在目录下已经存在uv 为我们创建的main.py
```python
from fastapi import FastAPI
from fastapi import HTMLResponse

app = FastAPI()

posts: list[dict] = [
	{
		"id": 1,	
		"author": "Corey Schafer",
		"title": "FastAPI is Awesome",
		"content": "This framework is really easy to use and super fast.",
		"date_posted": "April 20, 2025",
	},
	{
		"id": 2,
		"author": "Jane Doe",
		"title": "Python is Great for Web Development",
		"content": "Python is a great language for web development, and FastAPI makes it even better.",
		"date_posted": "April 21, 2025",
	},
]

@app.get("/", response_class=HTMLResponse, include_in_schema=False)
@app.get("/posts", response_class=HTMLResponse, include_in_schema=False)
def home():
	return f"<h1>{posts[0]['title']}</h1>"
	
@app.get("/api/posts")
def get_posts():
	return posts # 返回一个序列字典, fastweb会自动转换为json
	
```

在开发环境通过uv运行FastAPI的服务器
```shell
# 使用uv 在虚拟环境下运行fastapi的命令
uv run fastapi dev main.py # 注意这里的fastapi dev 的跑法, 是带有自动重载的服务器的
```

服务器运行后用浏览器打开, 可以看到json返回值. chrome支持prettier 表示
FastAPI 自带两种自动生成的api文档
```shell

127.0.0.1:8000/docs
127.0.0.1:8000/redocs 
```

# 第二天



```python
import json
from datetime import UTC, datetime
from functools import partial # partial 用户生成一个函数但是不执行
from typing import Annotated, Literal # Literal 类似于下来菜单
from uuid import UUID, uuid4 # 用于生成uuid

from pydantic import (
    BaseModel,
    ConfigDict,
    EmailStr, # 自动校验email
    Field, # 用于生成空的list等工厂方法
    HttpUrl, # 自动校验url, 需要从http或者https开始
    SecretStr, # 自动校验密码
    ValidationError,
    ValidationInfo,
    computed_field,
    field_validator, # 对单个field进行校验
    model_validator, # 对多个field进行计算校验
)


class User(BaseModel): # 所有的模型都要基于BaseModel
    model_config = ConfigDict(
        populate_by_name=True, # 允许模型中的 field使用别名
        strict=True,
        extra="allow",
        frozen=True,
    )

    uid: UUID = Field(alias="id", default_factory=uuid4) # 这里定义了别名
    username: Annotated[str, Field(min_length=3, max_length=20)]
    email: EmailStr
    password: SecretStr
    website: HttpUrl | None = None
    age: Annotated[int, Field(ge=13, le=130)] # 大于 13小于130的整数
    verified_at: datetime | None = None
    bio: str = ""
    is_active: bool = True

    first_name: str = ""
    last_name: str = ""
    follower_count: int = 0

    @field_validator("username")
    @classmethod
    def validate_username(cls, v: str) -> str:
        if not v.replace("_", "").isalnum():
            raise ValueError("Username must be alphanumeric (underscores allowed)")
        return v.lower()

    @field_validator("website", mode="before")
    @classmethod
    def add_https(cls, v: str | None) -> str | None:
        if v and not v.startswith(("http://", "https://")):
            return f"https://{v}"
        return v

    @computed_field
    @property
    def display_name(self) -> str:
        if self.first_name and self.last_name:
            return f"{self.first_name} {self.last_name}"
        return self.username

    @computed_field
    @property
    def is_influencer(self) -> bool:
        return self.follower_count >= 10000


class Comment(BaseModel):
    content: str
    author_email: EmailStr
    likes: int = 0


class BlogPost(BaseModel):
    title: Annotated[str, Field(min_length=1, max_length=200)]
    content: Annotated[str, Field(min_length=10)]
    author: User
    view_count: int = 0
    is_published: bool = False
    tags: list[str] = Field(default_factory=list)
    create_at: datetime = Field(default_factory=partial(datetime.now, tz=UTC))
    status: Literal["draft", "published", "archived"] = "draft"
    slug: Annotated[str, Field(pattern=r"^[a-z0-9-]+$")]

    comments: list[Comment] = Field(default_factory=list)


class UserRegistration(BaseModel):
    email: EmailStr
    password: str
    confirm_password: str

    @model_validator(mode="after") # 在实例创建后才开始校验, 默认是在创建实例时就开始校验
    def passwords_match(self) -> "UserRegistration": # 返回模型实例
        if self.password != self.confirm_password:
            raise ValueError("Passwords do not match")
        return self


user_data = {
    "id": "3bc4bf25-1b73-44da-9078-f2bb310c7374",
    "username": "Corey_Schafer",
    "email": "CoreyMSchafer@gmail.com",
    "age": 39,
    "password": "secret123",
    "notes": "Kind've a Karen...",
}
user = User.model_validate_json(json.dumps(user_data))

user.email = "CoreyMSchafer2@gmail.com"

print(user.model_dump_json(indent=2))
```


### FastAPI和其他框架的对比
1. 原生支持async
2. 支持 documentation swagger

# FastAPI Components
FastAPI 由两个组件组成
- Fastapi 包
- Uvicorn 是一个服务器
使用pip 安装fastapi
```shell
pip install fastapi # 安装包
pip install uvicorn # 安装服务器
```

### 最基本的fastapi 应用
1. 构建入口的 `main.py` 文件
2. 初始化 FastAPI 对象 app
3. 用`@app.method(path)`定义路由, 并修饰返回的函数.
应用文件
``` python 
# main.py 文件
from fastapi import FastAPI

app = FastAPI()

# route
@app.get("/")
def home():
	return {"Data": "TEST"}
```
启动应用
```shell
# app是文件夹名, main 是 py 文件名 :app FastAPI 实例名
unicorn app.main:app
# 自动 reload
uvicorn app.main:app --reload
```

# Documentation
FastAPI 会自动使用swagger 生成docs
```
127.0.0.1/docs 
```
RAW json
```python
http://localhost:8000/openapi.json
```


# 第五天

```shell
uv add sqlalchemy

```