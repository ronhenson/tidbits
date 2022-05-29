---
title: "Twitter Access Using Bearer Token"
created: 2022-05-19 05:21:26
tags: howto
keywords: twitter, example, curl
---

# Twitter Access Using Bearer Token

```bash
# using curl to access twitter
curl "https://api.twitter.com/2/users/10185562/tweets" -H "Authorization: Bearer $BEARER_TOKEN"
# gettng id or author_id
curl "https://api.twitter.com/2/users/by/username/drchuck" -H "Authorization: Bearer $BEARER_TOKEN"
# getting id or author_id with optional fields
curl "https://api.twitter.com/2/users/by/username/TwitterDev?expansions=pinned_tweet_id&user.fields=created_at&tweet.fields=created_at" -H "Authorization: Bearer $BEARER_TOKEN"
```

## python script to request twitter 

script location as 2022-05-19: `/home/ronh/data/workspace/sdt/fcc/scientific_computing/py4e/ex_15/twfriends/twurl2.py`

```python
import json
# Get aid from url "https://api.twitter.com/2/users/by/username/drchuck"
author_id or id = response["data"]["id"]
```
## Following is twurl2 as of 2022-05-19

```python
# import os  If you use os.environ to read environment variables

import requests
import json
import hidden_tw

# Used the example provided by : https://towardsdatascience.com/an-extensive-guide-to-collecting-tweets-from-twitter-api-v2-for-academic-research-using-python-3-518fcb71df2a

# To set your enviornment variables in your terminal run the following line:
# export 'BEARER_TOKEN'='<your_bearer_token>'

def create_headers(bearer_token):
    return {"Authorization" : f"Bearer {bearer_token}"}

def connect_to_endpoint(url, headers, params, next_token = None):
    params['next_token'] = next_token   #params object received from create_url function
    response = requests.request("GET", url, headers = headers, params = params)
    print("Endpoint Response Code: " + str(response.status_code))
    if response.status_code != 200:
        raise Exception(response.status_code, response.text)
    return response.json()

# https://apps.twitter.com/
# Create App and get the strings, put them in hidden.py

def get_twitter_data(url, params, next_token = None):
    params["next_token"] = next_token
    secrets = hidden_tw.oauth_keys()
    headers = create_headers(secrets["bearer_token"])
    return connect_to_endpoint(url, headers, params, next_token)

def test_me():
    print('* Calling Twitter...')
    url = 'https://api.twitter.com/2/users/10185562/tweets'
    query_params = {
        "tweet.fields":"id,author_id,text",
        "expansions":"author_id",
        "max_results":"5"
    }
    data = get_twitter_data(url, query_params)
    print(data)
    
if __name__ == "__main__":
    test_me()
```
