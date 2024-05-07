# Backend documentation 📚

> [!NOTE]
> For this guide, I'll assume your directories look like this:

Backend/    /
├─ Routes/  /
│  ├─ __init__.py   /
│  ├─ ExampleRouter.py /
├─ App.py   /

<hr/>

## The `App.py` file
Although it's pretty self-explanatory, but I'll still explain what it does.
Also look for comments in the code! They're very useful :)
I'll be renaming some stuff so that it's more generic...

```Python
my_api = FastAPI(lifespan = lifespan, root_path = '/api')
```
> This is your API instance. It takes in the `lifespan` function above, which
> asynchronously calls the `register_routes` function. You'll learn about it
> in the next paragraph. In this case, your API will start in
> `{host}:{port}/api`.

<hr/>

```Python
origins = [
    "http://localhost:8001",
    "https://localhost:8001",
    "http://192.168.178.101:8001",
]
```
> Simply put, these are the IP addresses from where the API accepts requests.
> You can also put "*" to allow requests from every address, but it's
> highly discouraged.

<hr/>

```Python
my_api.add_middleware(
    CORSMiddleware,
    allow_origins = ['*'],
    allow_credentials = True,
    allow_methods = ['*'],
    allow_headers = ['*'],
)
```
> This middleware is necessary in order to be able to host the backend and
> frontend on the same machine, as it enables CORS.

<hr/>

```
@my_api.get("/")
async def root():
    return {'message': 'Hello World'}
```
> This is what your first route. It's kinda useless, but it's also useful
> for when you need to "ping" your API. See how the argument of the decorator
> is just `"/"`? That's where your route is located. In this case
> `{host}:{port}/api/`.
> 
<hr/>

## Routes 🔁

### The initializer
> [!NOTE]
> In order to make the routes modular and not fill the main file with routing,
> we chose to instead have a [secondary file](https://github.com/Biblioteca-Midossi/Backend/tree/Alessandro/Routes/__init__.py)
> filled with routers! It might not be the best way to do this, but it works,
> and it's convenient :)

Here you can add routes, error handlers, and whatever extra you might need
that is not a route, for example your favicon.

To add routes you need to do import the router's file from and
include its router in your `app` using the `include_router()` method in the
`register_router()` function, like this:

```Python
from fastapi import FastAPI
from Routes import ExampleRouter

async def register_routes(app: FastAPI):
    app.include_router(ExampleRouter.router)
```

Don't panic, I'll also show you how to create a router and add some routes!

### How to create new routes?
- The simple way:
```Python
biblioteca
```

