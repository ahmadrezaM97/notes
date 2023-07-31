# car-knowledge-sharing-0

Tags: #ipython #kuber #divar
____

#### IPYTHON

```python


#output
_,__,___
_<cellnumber>


# document

import random
random?

# display source code
from brand_model import models
models??


from brand_model.models import Car
Car.*b*?


#history
%history
%pastebin 1-5
%whos


%timetit
%%timeit


%edit
%edit -p


def test():
	a = []
	print(a[2])
%debug


%paste


%lsmagic


```
[more](https://ep2019.europython.eu/media/conference/slides/cBeHNyZ-wait-ipython-can-do-that.pdf)


### Kubectl

``` bash

kubectl port-forward queue-car-price-check-1 15672:15672

kubectl scale deployment canary-cardetails-grpc-deployment --replicas=1

kubectl cp cardetails-grpc-deployment-5b98b4f96b-dh7bl:1.csv ~/Desktop/1.csv

kubectl get events --sort-by=.metadata.creationTimestamp

kubectl top pod cardetails-grpc-deployment-5b98b4f96b-6vg4v

```
[cheat-sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)


### DIVAR

#### curl -> python/postman

``` bash
curl 'https://api.divar.ir/v6/ongoingposts?city=1' \
  -H 'authority: api.divar.ir' \
  -H 'accept: application/json, text/plain, */*' \
  -H 'accept-language: en-GB,en;q=0.9,fa-IR;q=0.8,fa;q=0.7,en-US;q=0.6' \
  -H 'authorization: Basic eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiMDk5NTAwMDAwMDAiLCJpc3MiOiJhdXRoIiwiaWF0IjoxNjUyNTExMzQ1LCJleHAiOjE2NTM4MDczNDUsInZlcmlmaWVkX3RpbWUiOjE2NTI1MTEzNDQsInVzZXItdHlwZSI6Im1hcmtldHBsYWNlLWJ1c2luZXNzIiwidXNlci10eXBlLWZhIjoiXHUwNjdlXHUwNjQ2XHUwNjQ0IFx1MDY0MVx1MDYzMVx1MDY0OFx1MDYzNFx1MDZhZlx1MDYyN1x1MDY0NyIsInNpZCI6Ijc5NjAzMTQzLTczNDMtNGQ2Mi1hYWEyLTYwODQ0Y2FhYzMwNSJ9.h-xxEaodk_uVVqy1NKXgDlg0hoQQgeovLzokHRAF-2k' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'cookie: _gcl_au=1.1.321321557.1650804943; _ga=GA1.2.421803717.1650804944; did=d162b804-2d6d-401c-b4e6-31668bed6806; multi-city=tehran%7C; analytics_campaign={%22source%22:%22direct%22%2C%22medium%22:null}; city=tehran; _gid=GA1.2.380404704.1652511337; _gat_gtag_UA_32884252_2=1; _gat_UA-32884252-2=1; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiMDk5NTAwMDAwMDAiLCJpc3MiOiJhdXRoIiwiaWF0IjoxNjUyNTExMzQ1LCJleHAiOjE2NTM4MDczNDUsInZlcmlmaWVkX3RpbWUiOjE2NTI1MTEzNDQsInVzZXItdHlwZSI6Im1hcmtldHBsYWNlLWJ1c2luZXNzIiwidXNlci10eXBlLWZhIjoiXHUwNjdlXHUwNjQ2XHUwNjQ0IFx1MDY0MVx1MDYzMVx1MDY0OFx1MDYzNFx1MDZhZlx1MDYyN1x1MDY0NyIsInNpZCI6Ijc5NjAzMTQzLTczNDMtNGQ2Mi1hYWEyLTYwODQ0Y2FhYzMwNSJ9.h-xxEaodk_uVVqy1NKXgDlg0hoQQgeovLzokHRAF-2k; _gat=1' \
  -H 'origin: https://divar.ir' \
  -H 'pragma: no-cache' \
  -H 'referer: https://divar.ir/' \
  -H 'sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "Linux"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-site' \
  -H 'user-agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36' \
  --data-raw '{"post":{"contact":{"chat_enabled":true,"phone":"09950000000"},"location":{"neighborhood":907,"city":1,"districtError":false},"post_type":"فروشی","colored_parts":["roof-piles","front-driver-door"],"body_chassis_status":{},"category":"light","images":[],"brand_model":"Alfa Romeo 4C","year":"۱۳۹۵","color":"آبی","usage":0,"body_status":"paintless-dent-removal","motor_status":"healthy","title":"عنوان آگهیعنوان آگهی","description":"عنوان آگهی\nعنوان آگهی\nعنوان آگهی\nعنوان آگهی\n"}}' \
  --compressed
```

``` python
import requests


headers = {
    'authority': 'api.divar.ir',
    'accept': 'application/json, text/plain, */*',
    'accept-language': 'en-GB,en;q=0.9,fa-IR;q=0.8,fa;q=0.7,en-US;q=0.6',
    'authorization': 'Basic eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiMDk5NTAwMDAwMDAiLCJpc3MiOiJhdXRoIiwiaWF0IjoxNjUyNTExMzQ1LCJleHAiOjE2NTM4MDczNDUsInZlcmlmaWVkX3RpbWUiOjE2NTI1MTEzNDQsInVzZXItdHlwZSI6Im1hcmtldHBsYWNlLWJ1c2luZXNzIiwidXNlci10eXBlLWZhIjoiXHUwNjdlXHUwNjQ2XHUwNjQ0IFx1MDY0MVx1MDYzMVx1MDY0OFx1MDYzNFx1MDZhZlx1MDYyN1x1MDY0NyIsInNpZCI6Ijc5NjAzMTQzLTczNDMtNGQ2Mi1hYWEyLTYwODQ0Y2FhYzMwNSJ9.h-xxEaodk_uVVqy1NKXgDlg0hoQQgeovLzokHRAF-2k',
    'cache-control': 'no-cache',
    'origin': 'https://divar.ir',
    'pragma': 'no-cache',
    'referer': 'https://divar.ir/',
    'sec-ch-ua': '" Not A;Brand";v="99", "Chromium";v="101", "Google Chrome";v="101"',

    'user-agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36',
}

params = {
    'city': '1',
}

json_data = {
    'post': {
        'contact': {
            'chat_enabled': True,
            'phone': '09950000000',
        },
        'location': {
            'neighborhood': 907,
            'city': 1,
            'districtError': False,
        },
        'post_type': 'فروشی',
        'colored_parts': [
            'roof-piles',
            'front-driver-door',
        ],
        'body_chassis_status': {},
        'category': 'light',
        'images': [],
        'brand_model': 'Alfa Romeo 4C',
        'year': '۱۳۹۵',
        'color': 'آبی',
        'usage': 0,
        'body_status': 'paintless-dent-removal',
        'motor_status': 'healthy',
        'title': 'عنوان آگهیعنوان آگهی',
        'description': 'عنوان آگهی\nعنوان آگهی\nعنوان آگهی\nعنوان آگهی\n',
    },
}

response = requests.post('https://api.divar.ir/v6/ongoingposts', params=params,  headers=headers, json=json_data)
```

#### method monitor
[method monitor](https://rasad.divar.ir/d/6FCha6jGk/method-monitor?orgId=1&refresh=5s)

#### change rpc dest
``` yaml
    - name: GLOBAL_CONFIG
      value: '{"services_config": {"servicers": {"car_price_estimator": {"grpc":
           { "endpoint": "canary-priceestimator-grpc-service.divar-car-business:80"
              },"services": ["CAR_PRICE_ESTIMATION"]}}}}'

```

``` json
	{
		"services_config": {
			"servicers": {
				"car_price_estimator": {
					"grpc": { 
						"endpoint": "canary-priceestimator-grpc-service.divar-car-business:80"
	              },
	            "services": ["CAR_PRICE_ESTIMATION"]
	            }
	        }
	    }
	}
```


### IDEAS
-> discuss how we could improve development ease, speed and quality
	-> code smells
	-> Divar architecture 
	-> solutions
	-> clean coder
-> What happens under the hood(knowledge) 
	-> Docker ( + best practice)
	-> Django ORM 
	-> Python
		-> debuger
		-> gc
		-> tricks
		-> concurrency
	-> Go
	-> Why grpc?
	-> HTTP evolution 
	-> Network ( dns, tcp, udp , ...)
	
