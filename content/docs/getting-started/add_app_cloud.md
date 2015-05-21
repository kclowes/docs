### Building in the cloud

Wercker allows you to build in the cloud with every `git push`.  Let's see how
that works exactly! 

### Adding your app to wercker 
For the purpose of this guide, we assume you have already [set up your
wercker account](https://app.wercker.com/users/new) and added your GitHub or Bitbucket connection (settings -> git connections -> connect).

The next step is to create a new application on wercker. Head over to
[https://app.wercker.com/](https://app.wercker.com/) and select create ->
application.

First select your Git provider, after which a list of your existing
repositories on either GitHub or BitBucket is presented. Select our python
example you forked earlier from the list and click on **Use selected repo**. 

Now we have to choose who owns the app. For this tutorial, go ahead and select
yourself. If you like, you can also select an organization you created on
wercker. Click on **Use selected owner** once you're ready.

The next step is about configuring access, and the first option - **checkout
the code without using an SSH key** - is fine for the purpose of this tutorial,
because it's an open source and public application. So go ahead and click
**Next step**

At this point wercker will try to detect if you have a **wercker.yml** file in
your repository and if not, try to make one for you. However, we already have a
**wercker.yml** file so let's change that and click **I already have a
wercker.yml**. 

Finally, once you've verified all the settings you can click "Finish" to
complete setting up our app!  When done, you will be redirected to your very
own app page, which looks empty now, so let's go ahead and change that. 

### Triggering your first build

Wercker will automatically trigger a build every time you push new code
to your Git provider. Let's see that in action:

```
$ git commit -am 'wercker build time!'
$ git push origin master
```

Next, navigate to your app page and you should see a new build has been
triggered! This build will do the exact same as the one you triggered locally
but now everyone in your team can see and comment on the build. 

The main advantage however, is that you can now let wercker (automatically)
deploy your code to your infrastructure!
