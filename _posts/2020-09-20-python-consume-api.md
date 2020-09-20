---
layout: post
title:  "Consume APIs using Python and requests"
date:   2020-09-20 17:30:00 -0500
categories: tutorial python
tags: python api requests
---

Hi Everyone!. In this post, I want to share with you a little guide that will show you how to consume an APIs using Python and [requests](https://requests.readthedocs.io/en/master/).

We will use the following API, [https://jsonplaceholder.typicode.com/](https://jsonplaceholder.typicode.com/). That is a simple API for posts.

## Requirements

- [Python](https://www.python.org/) (I use Python 3.8.2)
- Pip (I use Pip 20.1.1)

## Setup Project

To create our project we are going to use [virtualenv](https://pypi.org/project/virtualenv/), to create an isolated python environment for this project. However, you can also use [pyenv](https://github.com/pyenv/pyenv) or [venv](https://docs.python.org/3/library/venv.html).

So, first, we have to install `virtualenv`.

```bash
pip3 install virtualenv
```

Now, we have to create the project folder and set up the virtualenv.

```bash
# Creating a project folder
mkdir apis-example
cd apis-example

# Creating the virtual environment
virtualenv env

# Activate the virtual environment
source env/bin/activate

# Create our main file
touch main.py
```

**NOTE:** To exit the environment you just have to write `deactivate`.

## Install dependencies

To make HTTP requests we will use the `requests` module. We can install this module using the following command.

```bash
pip install requests
```

We can see the installed dependencies with the following command.

```bash
# Installed dependencies
pip freeze

# The above mentioned command will list something like the following
certifi==2020.6.20
chardet==3.0.4
idna==2.10
requests==2.24.0
urllib3==1.25.10
```

We can also export our dependencies.

```bash
pip freeze > requirements.txt
```

And install our dependencies from a `requirements.txt` file.

```bash
pip install -r requirements.txt
```

## List posts

Now that we have the necessary modules installed, we can start writing code. First, we must import the requests module, which will help us to consume the API.

```python
# main.py
# import section ....
import requests            # import python module
import json
# ....
```

Now, we will define the base url.

```python
# main.py

# Constants section ....
# API base URL
BASE_URL = "https://jsonplaceholder.typicode.com"
```

Ok, we will create a function  called `get_all` that will show all posts.

```python
# main.py

# Methods section ....
def get_all():
    """
    Make a GET request to the API rest to get all the items
    """
    url = BASE_URL + "/posts"
    response = requests.get(url)

    json_result = response.json()
    print(json_result)
```

Our `main` function will be the following.

```python
# main.py
# import section ....
import sys

# Main section ...
if __name__ == "__main__":
    if len(sys.argv)> 1 :
        if sys.argv[1] == '--help':
            print('Info: ')
            print('--help List the options to send an email')
            print('--list Show all posts')
            print('--show Show a specific post')
            print('--create Create a post')
            print('--update Update a post')
            print('--delete Delete a post')
        elif sys.argv[1] == '--list':
            print("Listing all posts")
            get_all()
    else:
        print("Please give the type of message to send.")
        print("For help execute `python main.py --help`")
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --list

# Listing all posts
# [{'userId': x, 'id': x, 'title': 'xxxxxxx', 'body': 'xxxxxxxxxx'}, ....]
```

## Create post

Here, we will create a post using a POST request. First, we will create a method called `create_post`.

```python
# main.py

# Methods section ....
def create_post():
    """
    Make a post request to the API rest to create an item
    """
    url = BASE_URL + "/posts"
    
    # Request headers
    headers = { "Content-type": "application/json; charset=UTF-8" }

    # Request body
    body = {
      "title": 'foo',
      "body": 'bar',
      "userId": 1
    }
    
    data = json.dumps(body) # parse a dictionary to json string format

    response = requests.post(url, headers=headers, data=data)
    
    json_result = response.json()
    
    print(json_result)
```

Add the option to our main function.

```python
# main.py

# Main section ...
if __name__ == "__main__":
	# ....
    
    if len(sys.argv)> 1 :
        # if ....
        elif sys.argv[1] == '--create':
            print("Create a post")
            create_post()
    else:
        # ....
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --create

# Create a post
# {'title': 'foo', 'body': 'bar', 'userId': x, 'id': x}
```

## Show a post

Here, we will show a specific post using a GET request. We will create a method called `show_post`.

```python
# main.py

# Methods section ....
def show_post(id=1):
    url = BASE_URL + "/posts/" + str(id)
    response = requests.get(url)

    json_result = response.json()
    print(json_result)
```

Add the option to our main function.

```python
# main.py

# Main section ...
if __name__ == "__main__":
	# ....
    
    if len(sys.argv)> 1 :
        # if ....
        elif sys.argv[1] == '--show':
            print("Show a post")
            show_post(1)
    else:
        # ....
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --show

# Show a post
# {'userId': x, 'id': x, 'title': 'xxxxxxxxx', 'body': 'xxxxxxxxxx'}
```

## Update a post

Here, we will update a post using a PUT request. We will create a method called `update_post`.

```python
# main.py

# Methods section ....
def update_post(id = 1):
    """
    Make a post request to the API rest to create an item
    """
    url = BASE_URL + "/posts/" + str(id)
    
    # Request headers
    headers = { "Content-type": "application/json; charset=UTF-8" }

    # Request body
    body = {
      "title": 'fooz',
      "body": 'barz',
      "userId": 1,
      "id": id
    }
    
    data = json.dumps(body) # parse a dictionary to json string format

    response = requests.put(url, headers=headers, data=data)
    
    json_result = response.json()
    
    print(json_result)
```

Add the option to our main function.

```python
# main.py

# Main section ...
if __name__ == "__main__":
	# ....
    
    if len(sys.argv)> 1 :
        # if ....
        elif sys.argv[1] == '--update':
            print("Update a post")
            show_post(1)
    else:
        # ....
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --update

# Update a post
# {'userId': x, 'id': x, 'title': 'xxxxxxxx', 'body': 'xxxxxxxx'}
```

## Delete a post

Here, we will update a post using a DELETE request. We will create a method called `delete_post`.

```python
# main.py

# Methods section ....
def delete_post(id = 1):
    url = BASE_URL + "/posts/" + str(id)
    response = requests.delete(url)

    json_result = response.json()
    print(json_result)
```

Add the option to our main function.

```python
# main.py

# Main section ...
if __name__ == "__main__":
	# ....
    
    if len(sys.argv)> 1 :
        # if ....
        elif sys.argv[1] == '--delete':
            print("Delete a post")
            show_post(1)
    else:
        # ....
```

We can test our function by executing the following command in our terminal.

```bash
python main.py --delete

# Delete a post
# {'userId': x, 'id': x, 'title': 'xxxxxxxxxxx', 'body': 'xxxxxxxxxxx'}
```

## Final Words

Thanks for reading this post and you can find the code of this guide [here](https://gitlab.com/-/snippets/2017078).

