### Deploying your app

The last step in our development cycle would be deploying the app to 
infrastructure of your choosing. For this tutorial, we will be deploying
to a **Digital Ocean** droplet.


### Setting up the deploy

First we have to let wercker now that we want a new deploy target. Head on over
to your application's settings page to the **SSH KEYS** section and click on
**Generate new key pair**. I called my SSH key pair **digitalocean**, but you
can of course name it whatever you like. 

##### Creating the deploy target
After you've generated the key head over to the **DEPLOY TARGETS** section and
click on **Add deploy target**.  From there, click on **custom deploy** and
fill out the forms as you wish. Also check the **auto deploy successful builds
to branch(es)** and in the text box below type in **master**. 

This option will automatically trigger a deploy to Digital Ocean when code is
pushed to master. Very handy indeed (but not necessarily advisable in
production environments). 

Then, click on "Add new variable" below and select **SSH Key pair**.  Give the
variable a name - I called mine **DIGITALOCEAN_SSH** - and select the key pair
you generated just now and click OK. The public key should be automatically
copied into the text field. Click "OK" and "Save" if everything is filled out.
These keys will now be exposed in your deployment pipeline as environment
variables named **DIGITALOCEAN_SSH_PUBLIC** and **DIGITALOCEAN_SSH_PRIVATE**.

##### Update your infra stack
The last thing we need to do, is update our **Infra stack** to **Ewok**.  This
setting can be found at the bottom of the setting page. Go ahead and change
that from **Andorian (1)**.

Now your deploy target is set up! Next up, configuring your droplet.

### Creating the Digital Ocean Droplet
Log into your Digital Ocean dashboard and add a new droplet. In the SSH section
click **add new SSH key** and paste in the public key you created earlier. Name
it however you like; I called mine **wercker-tutorial**. 

If you want to use this key with an **existing** droplet you will want to add
the previously created key to the **authorized_keys** on your droplet. See
Digital Ocean's [SSH
article](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets)
to find out how.

Next, login to your server and provision it with the python environment:

```
$ apt-get install python-pip -y 
$ mkdir /var/local/www
```

We also create the /var/local/www directory which we'll be deploying to later.

### Updating our wercker.yml for deployment
We now have to update our **wercker.yml** file to let wercker now how our
deploy should look like. Go ahead and add the following **deploy** section:

```
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
deploy:
  # we use a different box here which provides a more complete linux distro
  # which is needed for some of the deploy steps
  box: python:2.7
  steps:
    - add-to-known_hosts:
        hostname:your.digital.ocean.ip
    - mktemp:
        envvar: PRIVATEKEY_PATH
    - create-file:
        name: write key
        filename: $PRIVATEKEY_PATH
        # this is the env var you added to your deploy pipeline earlier
        content: $DIGITALOCEAN_SSH_PRIVATE
        overwrite: true
    - script:
        name: transfer application
        code: |
          pwd
          ls -la
          scp -i $PRIVATEKEY_PATH app.py requirements.txt root@<your_digitalocean_ip>:/var/local/www
    - script:
        name: pip install server
        code: ssh -i $PRIVATEKEY_PATH -l root <your.digital.ocean.ip> cd /var/www/local && pip install -r requirements.txt
    - script:
        name: start application
        # mind the '&' at the end of the command so that the app gets started in the background
        code: ssh -i $PRIVATEKEY_PATH -l root <your_digitalocean_ip> python /var/local/www/ app.py &       
```
There's a lot of stuff going on in the deploy section. Let's go over it
step-by-step.

First we need to add the server we want to deploy to, to the known hosts file.
We can do this using the
[add-to-know_hosts](https://app.wercker.com/#applications/521764dde36a64ff110022f2/tab/details)
step.

Second, we create a temporary file and make it available as an environment
variable using the [mktemp
step](https://app.wercker.com/#applications/52167277e9fa619606001064/tab/details).
This filepointer will contain our private key that we’ve previously created.

Next, we write the contents of our private key ($DIGITALOCEAN_SSH_PRIVATE) to
the temporary file we created in the previous step using **mktemp**. We use the
[create-file
step](https://app.wercker.com/#applications/51c829dd3179be4478002113/tab/details)
to do so. Now we are ready to communicate with our droplet!

We need to transfer our application, consisting of **app.py** and
**requirements.txt** using `scp` to our remote /var/local/wwww folder.

Then we need to `pip install` on the server so **flask** gets installed and
finally we can run our app using `python /var/local/www/app.py &`.

### Deploying the app

Now that we've specified how our deploy should look like using the 
**deploy** section in our **wercker.yml**, we can go ahead and build & deploy
our app!

```
$ git commit -am "deploying our app!"
$ git push origin master
```

Head over to your app page and you should see a build getting triggered.  Once
the build finishes, you can switch over to the **deploys** page and watch your
app get deployed. If all goes well you should be able to see your app in action
on http://your.digitalocean.ip:5000/cities.json !
