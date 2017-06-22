# nplcors
基于npl express的跨域请求中间件

```
local cors = NPL.load('cors');
local app = express:new();

-- ........

-- 所有的请求都支持跨域，并对所有域都是开放的
app:use(cors());
```

`cors()` 可以接受一个参数，也可以是两个参数，也可以一个参数都不传。

如果不传任何参数，表示对所有域的所有请求都支持跨域。

如果只传一个参数，并且是一个function，则由此function来决定请求是否支持跨域。若返回true，则表示支持跨域，否则不支持。

```
-- 当请求的URL为“/user/add”时，支持跨域
app:use(cors(function(req, res)
	local url = req.url;
	return url == '/user/add';
end));
```

如果只传一个参数，并且是一个table，则是一个对跨域的相关配置，可支持的配置有：

* **origin:** 域，可以是一个string，也可以是一个table（array）。默认为 “\*” 。
* **credentials:** 如果为 true，则会在 http 响应头中加入 Access-Control-Allow-Credentials=true
* **methods:** HTTP request method，只有匹配的 method 才支持跨域，默认为 “GET,HEAD,PUT,PATCH,POST,DELETE”

```
-- 对所有请求都支持跨域，并且只支持对来自域“google.com”的请求的跨域
app:use(cors({ origin = 'google.com' }));
```

如果传两个参数，则第一个参数是决定是否跨域的function，第二个参数是相关配置。

```
-- 当请求的URL为“/user/add”时，支持跨域，并且只支持对来自域“google.com”的请求的跨域
app:use(cors(function(req, res)
	local url = req.url;
	return url == '/user/add';
end, { origin = 'google.com' }));
```
