### Developing the app

Now that we've setup our CLI we can start developing.  Our list of cities is a
bit short, so why not add another one in there?  Before we do that however, let's
take a closer look at the **wercker.yml** file included in your project
folder.


### Werkcer.yml
The wercker.yml is the only config file you need for using wercker.  In it, you
will define all your steps needed to successfully develop, build and deploy
your application.

To get started however, we're only interested in developing our app, so our
wercker.yml defines only this development step right now 

```yaml
box: python:2.7-slim
dev:
  steps:
    - pip-install
    - internal-watch:
        code: python app.py
        reload: true
```
Let's take a closer look at what's happening. 

`box: python:2.7-slim`

This line specifies which container image you want to use for your project.
Since we're developing for Python, we've already specified a python image for
you.  These images are retrieved from
[dockerhub](https://registry.hub.docker.com/u/library/python/) if no other
registry is specified. 

```
dev:
  steps:
    - pip-install
    - internal-watch:
        code: python app.py
        reload:true
```

Here we define what should happen if we want to start developing our app,
which in this case are two steps: `pip-install` and `internal-watch`.
These steps are simply pre written bash scripts written by either wercker or
the community. Steps will be explained more in depth later. 

`internal-watch` watches your files for changes, and if `reload` is set to
`true` it restarts your app so your changes are reflected immediately. This is
especially useful for when you're developing webapps, as we're doing now.

But enough talking, let's see some action!

### wercker dev

In your project folder, run `wercker dev --publish 5000`. You should see
something similar to the following output:

```
--> Executing pipeline
--> Running step: setup environment
Pulling repository python
Pulling image (2.7-slim) from python: 8320d6315882
Pulling image (2.7-slim) from python, endpoint: https://registry-1.docker.io/v1/: 8320d6315882
Pulling dependent layers: 8320d6315882
Download complete: 41b730702607
Download complete: fc00109c41f7
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
--> Running step: watch
--> Reloading on file changes
--> Reloading
--> Forwarding 192.168.59.103:5000 to 5000 on the container.
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
 * Restarting with stat
```

The IP displayed here could be different for you, as this tutorial was written
using boot2docker. If you navigate to [DOCKER_HOST_IP]:5000/cities.json you
should see the same output as before. That's not very exciting, so let's see
our live reloading in action!

In **app.py** add a new city to the array

```
@app.route('/cities.json')
def cities():
    data = ['Amsterdam', 'San Francisco', 'Berlin', 'New York', 'Tokyo']
    resp = Response(json.dumps(data), status=200, mimetype='application/json')
    return resp
```

Your app should automatically reload, so if you refresh your page, our new city
should be there! Hurray!

Next up, building your app!
