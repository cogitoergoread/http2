

# HTTP2 tests

Forrás: https://pgjones.dev/blog/http-1-2-3-2019/

Requirements:
requirements.txt:
``` text
quart
hypercorn
aioquic
```

``` shell
pip install -r requirements.txt
```

Minta program, run.py:
``` python
from quart import Quart

app = Quart(__name__)

@app.route("/")
async def index():
    return "<b>Hello World!</b>"
```

Futtatás, HTTP 1.1:

``` shell
hypercorn --bind localhost:8000 run:app
```

Teszt, HTTP 1.1:

``` shell
curl http://127.0.0.1:8000
```

Keygen, HTTPS:
``` shell
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes -subj "/CN=127.0.0.1/C=HU/L=Budapest"
```

Futtatás, HTTP2:
``` shell
hypercorn --certfile cert.pem --keyfile key.pem --bind localhost:8000 run:ap
```

Teszt mindkettőre, HTTP1.1 / HTTP2

``` shell
curl --cacert cert.pem  --http2 -s https://127.0.0.1:8000
curl --cacert cert.pem -I --http1.1 -s https://127.0.0.1:8000
```
