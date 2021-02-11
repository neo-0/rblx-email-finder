# rblx-email-finder 
 
 HOW TO USE  
 NEEDS PYTHON 3.6  
 WHEN INSTALLED USE THIS CODE  
 import requests
2
from base64 import b64decode
3

4
def get_email_from_cookie(cookie):
5
    user_agent = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36"
6

7
    ## Custom request function to avoid bandwith waste caused by requesting
8
    ## error pages and what not
9
    def request(url):
10
        resp = requests.get(
11
            url=url,
12
            headers={"User-Agent": user_agent},
13
            cookies={".ROBLOSECURITY": cookie},
14
            allow_redirects=False)
15
        new_url = resp.headers.get("location")
16


Update emailgrabber.py
9 months ago
17
        ## Under-age accounts cannot access the devforum SSO

Create emailgrabber.py
9 months ago
18
        if new_url and "web." in new_url:

Update emailgrabber.py
9 months ago
19
            return None, None

Create emailgrabber.py
9 months ago
20

21
        return resp, new_url
22

23
    ## Devforum -> Roblox SSO
24
    resp, new_url = request("https://devforum.roblox.com/session/sso?return_path=%2F")
25
    if not new_url or not "/discourse/dev-forum?sso=" in new_url:
26
        return
27

28
    ## Devforum <- Roblox SSO
29
    resp, new_url = request(new_url)
30
    if not new_url or not "/session/sso_login?sso=" in new_url:
31
        return
32

33
    ## Grab base64-encoded SSO data and decode it

Update emailgrabber.py
8 months ago
34
    data = new_url.split("/session/sso_login?sso=")[1] \
35
        .split("&")[0] \
36
        .replace("%3D", "=")

Create emailgrabber.py
9 months ago
37
    data = b64decode(data.encode("utf-8")).decode("utf-8")
38

39
    ## Grab email param and URL-decode it
40
    email = data.split("email=")[1].split("&")[0]

Update emailgrabber.py
8 months ago
41
    email = email \
42
        .replace("%40", "@") \
43
        .replace("%2b", "+")

Create emailgrabber.py
9 months ago
44

45
    return email
46

47

48
if __name__ == "__main__":

Update emailgrabber.py
9 months ago
49
    cookie = input("Enter your cookie >> ").strip()

Create emailgrabber.py
9 months ago
50
    email = get_email_from_cookie(cookie)
51

52
    if not email:

Update emailgrabber.py
9 months ago
53
        print("No email found, possibly unverified/underage/banned?")

Create emailgrabber.py
9 months ago
54
        input()

Update emailgrabber.py
9 months ago
55
        exit()

Create emailgrabber.py
9 months ago
56

57
    print("Email:", email)

Update emailgrabber.py
9 months ago
58
    input()
