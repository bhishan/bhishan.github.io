---
title: 'Going serverless with Chalice and AWS lambda'
date: 2018-12-05
permalink: /posts/2018/12/chalice-aws-lambda/
tags:
  - Python
  - Tutorials
---

The intentions of this post is to host a simple example of chalice from AWS that allows serverless API creation with the use of AWS lambda. You also get auto-generation of IAM policy making it faster to deploy web applications. Chalice expects to pick the AWS credentials from ~/.aws/config

Prerequisites for chalice[AWS Credentials]
======
If you've used AWS API or boto/boto3 for python, you've probably already added the credentials in place. Otherwise you could do it as below:

```
mkdir ~/.aws
touch ~/.aws/config
```

Contents of the config file

```
[default]
aws_access_key_id = YOUR_ACCESS_KEY_HERE
aws_secret_access_key = YOUR_SECRET_ACCESS_KEY
region = YOUR_REGION
```

Installing Chalice
======
As a good development practice, we should segregate dependencies between various projects and therefore is advised to use virtual environment or some sort.

```
$ python3 -m venv serverlessenv
$ source serverlessenv/bin/activate

(serverlessenv)$ pip install chalice
```

Creating simple API with chalice
======
Chalice comes with command line tool. See what commands are available

```
(serverlessenv)$ chalice

Usage: chalice [OPTIONS] COMMAND [ARGS]...

Options:
--version Show the version and exit.
--project-dir TEXT The project directory. Defaults to CWD
--debug / --no-debug Print debug logs to stderr.
--help Show this message and exit.

Commands:
delete
deploy
gen-policy
generate-pipeline Generate a cloudformation template for a...
generate-sdk
invoke Invoke the deployed lambda function NAME.
local
logs
new-project
package
url

(serverlessenv)$ chalice new-project

___ _ _ _ _ ___ ___ ___
/ __|| || | /_\ | | |_ _|/ __|| __|
| (__ | __ | / _ \ | |__ | || (__ | _|
\___||_||_|/_/ \_\|____||___|\___||___|

The python serverless microframework for AWS allows
you to quickly create and deploy applications using
Amazon API Gateway and AWS Lambda.

Please enter the project name: basichelloworld

(serverlessenv)$ cd basichelloworld/

(serverlessenv)~/basichelloworld$ ls -a

app.py .chalice .gitignore requirements.txt
```

Chalice is very simple, somewhat similar to Flask if you come from there.

Auto-generated app.py
======
```python
from chalice import Chalice

app = Chalice(app_name='basichelloworld')


@app.route('/')
def index():
    return {'hello': 'world'}


# The view function above will return {"hello": "world"}
# whenever you make an HTTP GET request to '/'.
#
# Here are a few more examples:
#
# @app.route('/hello/{name}')
# def hello_name(name):
#    # '/hello/james' -> {"hello": "james"}
#    return {'hello': name}
#
# @app.route('/users', methods=['POST'])
# def create_user():
#     # This is the JSON body the user sent in their POST request.
#     user_as_json = app.current_request.json_body
#     # We'll echo the json body back to the user in a 'user' key.
#     return {'user': user_as_json}
#
# See the README documentation for more examples.
#
```

Running the API locally
======
```
(serverlessenv)~/basichelloworld$ chalice local
Serving on 127.0.0.1:8000

(serverlessenv)~/basichelloworld$ curl -X GET http://127.0.0.1:8000
{"hello": "world"}
```

Going serverless with chalice through AWS lambda
======
```
(serverlessenv)~/basichelloworld$ chalice deploy
Creating deployment package.
Updating policy for IAM role: basichelloworld
Creating lambda function: basichelloworld
Creating Rest API
Resources deployed:
- Lambda ARN: arn:aws:lambda:us-east-1:9582857991:function:basichelloworld
- Rest API URL: https://fxcdyzuitc.execute-api.us-east-1.amazonaws.com/api/

(serverlessenv)~/basichelloworld$ curl -X GET https://fxcdyzuitc.execute-api.us-east-1.amazonaws.com/api/
{"hello": "world"}
```