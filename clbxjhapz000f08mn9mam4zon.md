# FastAPI: The High-Performance Python Framework for Building APIs

[**FastAPI**](https://fastapi.tiangolo.com/) is a modern, fast (high-performance), web framework for building APIs with Python 3.7+ based on standard Python type hints. One of the key features of **FastAPI** is its automatic OpenAPI validation and documentation, which allows you to automatically generate interactive documentation for your API using Swagger UI. **FastAPI** is built on top of Starlette, which is a lightweight ASGI framework, and is designed to be easy to use and to scale.

To get started with **FastAPI**, you will need to install the **FastAPI** package using `pip`. You can then create a new FastAPI application by defining a Python function that takes in an `FastAPI` object and uses it to define your API endpoints.

Here is an example of a simple **FastAPI** application:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}
```

This example defines a single endpoint at the root path (`/`) that returns a JSON object containing the message "Hello World".

To run this application, you can use the `uvicorn` ASGI server, which is automatically installed with **FastAPI**. Simply run the following command:

```perl
uvicorn main:app --reload
```

This will start the server and you should be able to access the endpoint at `http://localhost:8000/`.

**FastAPI** also includes support for dependency injection, which allows you to easily manage the dependencies of your application and make them available to your API functions. This can be particularly useful for managing database connections or other resources that you need to use in multiple places in your application.

I hope this gives you a good overview of **FastAPI** and how to get started with it.

Enjoy!