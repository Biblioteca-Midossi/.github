# Documentazione del Backend 📚

> [!NOTE]
> Per questa guida, supponiamo che le directory siano strutturate in questo modo:

Backend <br/>
├─ Routes <br/>
│ ├─ init.py <br/>
│ ├─ ExampleRouter.py <br/>
├─ App.py <br/>

<hr/>

## Il file `App.py`
Anche se è abbastanza autoesplicativo, spiegherò comunque cosa fa.
Guarda anche i commenti nel codice! Sono molto utili :)
Rinominerò alcune cose per renderle più generiche...

``` Python
my_api = FastAPI(lifespan = lifespan, root_path = '/api')
```

> Questa è l'istanza della tua API. Prende come input la funzione `lifespan` sopra, che
> chiama asincronamente la funzione `register_routes`. Ne imparerai di più
> nel prossimo paragrafo. In questo caso, la tua API partirà da
> `{host}:{port}/api`.

<hr/>

``` Python
origins = [
    "http://localhost:8001",
    "https://localhost:8001",
    "http://192.168.178.101:8001",
]
```
> In parole semplici, questi sono gli indirizzi IP da cui l'API accetta richieste.
> Puoi anche mettere "*" per consentire richieste da qualsiasi indirizzo, ma è
> altamente sconsigliato.

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
> Questo middleware è necessario per poter ospitare il backend e
> il frontend sulla stessa macchina, poiché abilita il CORS.

<hr/>

```Python
@my_api.get("/")
async def root():
    return {'message': 'Hello World'}
```
> Questo è il tuo primo percorso. È un po' inutile, ma è anche utile
> quando hai bisogno di "pingare" la tua API. Vedi come l'argomento del decoratore
> è solo `"/"`? È lì che si trova il tuo percorso. In questo caso
> `{host}:{port}/api/`.

<hr/>

## Routes 🔁
### L'inizializzatore

> [!NOTE]
> Per rendere le rotte modulari e non riempire il file principale con il routing,
> abbiamo scelto di avere invece un file secondario
> pieno di router! Potrebbe non essere il modo migliore per farlo, ma funziona,
> ed è conveniente :)

Qui puoi aggiungere rotte, gestori di errori e qualsiasi altra cosa extra di cui potresti aver bisogno
che non sia una rotta, ad esempio il tuo favicon.

Per aggiungere rotte, devi importare il file del router e
includere il suo router nella tua `app` utilizzando il metodo `include_router()` nella
funzione `register_router()`, in questo modo:

```Python
from fastapi import FastAPI
from Routes import ExampleRouter

async def register_routes(app: FastAPI):
    app.include_router(ExampleRouter.router)
```
Non preoccuparti, ti mostrerò anche come creare un router e aggiungere alcune rotte!

### Come creare nuove rotte?
- Il modo semplice:
```Python
# App.py
from fastapi import FastAPI
biblioteca = FastAPI(root_path = '/api')

@biblioteca.get("/")
async def root():
    return {'message': 'Hello World'}
```

- Il modo migliore:
```Python
# Routes/NewRoute.py
from fastapi import APIRouter
from fastapi.responses import JSONResponse

my_router = APIRouter(
    responses = {
        404: {
            "description": "Not found"
        }
    }
)

@my_router('/test')
async def test_route():
    return JSONResponse({'message': ''})
```

```Python
# Routes/__init__.py
from fastapi import FastAPI
from Routes import NewRoute

async def register_routes(app: FastAPI):
    app.include_router(Test.router)
```

```Python
# App.py
from contextlib import asynccontextmanager
from fastapi import FastAPI
from Routes import register_router
@asynccontextmanager
  async def lifespan(app: FastAPI):
  # Run at startup
  await asyncio.create_task(register_routes(app))
  yield


  biblioteca = FastAPI(lifespan = lifespan, root_path = '/api')
```

> Anche se quest'ultimo richiede più codice, una volta che avrai molti router/rotte, tutto sarà più gestibile, piuttosto
> che avere tutte le rotte in un file unico.
> Puoi comunque utilizzare il primo metodo se hai bisogno di una rotta di test per iniziare, o per la root, come facciamo noi.