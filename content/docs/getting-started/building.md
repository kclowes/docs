### Building your app

Now that we've done some development, we want to build and test our app 
- preferably in an automated fashion. Luckily, wercker does just that. 

Let's review our `werkcer.yml` file. 


```yaml
box: python:2.7-slim
dev:
  steps:
    - pip-install
    - internal-watch:
        code: python app.py
        reload: true
```

Currently we haven't told wercker anything about how we want our build _pipeline_ 
to look like. Let's change that by adding the `build` section:

```yaml
box: python:2.7-slim
dev:
  steps:
    - pip-install
    - internal-watch:
        code: python app.py
        reload: true
build:
  steps:
    - pip-install
    - script:
        name: python unit test
        code: |
          python app_test.py
```

### Wercker build
Let's take a closer look at our **wercker.yml** file again. 

`- pip-install`

This is another step which will install all the dependencies that are specified in our requirements.txt.

```
- script:
    name: python unit test
    code: |
      python app_test.py
```

This is a simple inline **bash** script. You can create these steps directly in
your **wercker.yml** or share them with the community by [submitting a step to
our repository](http://devcenter.wercker.com/docs/steps/creating-steps.html).
This script simply runs the unit tests using python. 

Now that we have a better understanding of our **wercker.yml** let's go ahead
and let wercker build our project. In your project folder, run

```
$ wercker build
--> Executing pipeline
--> Running step: setup environment
Pulling repository python
Pulling image (2.7-slim) from python: 8320d6315882
Pulling image (2.7-slim) from python, endpoint: https://registry-1.docker.io/v1/: 8320d6315882
Pulling dependent layers: 8320d6315882
Download complete: 8320d6315882
(...)
Download complete: 8320d6315882
Status: Image is up to date for python:2.7-slim
--> Running step: wercker-init
--> Running step: pip-install
No virtual environment detected. Sudo will be used for pip install
Running pip install with sudo: sudo pip install  -r requirements.txt
Collecting Flask==0.9 (from -r requirements.txt (line 1))
  Downloading Flask-0.9.tar.gz (481kB)
Collecting Werkzeug>=0.7 (from Flask==0.9->-r requirements.txt (line 1))
  Downloading Werkzeug-0.10.4-py2.py3-none-any.whl (293kB)
Collecting Jinja2>=2.4 (from Flask==0.9->-r requirements.txt (line 1))
  Downloading Jinja2-2.7.3.tar.gz (378kB)
Collecting markupsafe (from Jinja2>=2.4->Flask==0.9->-r requirements.txt (line 1))
  Downloading MarkupSafe-0.23.tar.gz
Installing collected packages: Werkzeug, markupsafe, Jinja2, Flask
  Running setup.py install for markupsafe
  Running setup.py install for Jinja2
  Running setup.py install for Flask
Successfully installed Flask-0.9 Jinja2-2.7.3 Werkzeug-0.10.4 markupsafe-0.23
pip install succeeded
--> Running step: python unit test
F
======================================================================
FAIL: test_index (__main__.CitiesTestCase)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "app_test.py", line 12, in test_index
    self.assertEqual(response.data, json.dumps(['Amsterdam', 'San Francisco', 'Berlin', 'New York']))
AssertionError: '["Amsterdam", "San Francisco", "Berlin", "New York", "Tokyo"]' != '["Amsterdam", "San Francisco", "Berlin", "New York"]'

----------------------------------------------------------------------
Ran 1 test in 0.008s

FAILED (failures=1)
--> Step failed: python unit test
--> Pipeline failed
```

It seems like our test is failing because we added a new city that isn't
reflected in the tests!  Go ahead ahead and add our new city to `app_test.py`
and run the `wercker build` again.

It should give you the following output:

```
pip install succeeded
--> Running step: python unit test
.
----------------------------------------------------------------------
Ran 1 test in 0.007s

OK
--> Steps passed
--> Pipeline finished
```

Building locally is very useful when you're sure your code won't run because of
some changes you made and as such you don't want to push these changes to your
Git provider.

That's not always the case however, which brings us to the next step in our
work flow: building & deploying in the cloud!

[next]()

