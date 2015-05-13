#About wercker
Wercker is a Docker-based platform for developing, building and
delivering your applications. Wercker provides you with a toolchain and 
cloud platform to speed up your development process. It allows you 
to build apps for the 21st century by containerizing your code and 
automating your development pipelines.

#Why wercker
At wercker, we believe that micro-services and Service Oriented
Architectures are the future. However, with great power comes great
responsibility and micro-services naturally introduce a lot of extra moving
parts in your application. To fully leverage the advantages of micro-services,
they should be managed carefully.

Wercker lets you do just that by setting up automated development pipelines.
Very much in the spirit of "_release early, release often_" you can
build and deploy your services with just a `git push` and let wercker compile
and run any other _steps_ that are necessary to build your project before it
gets deployed to a [target](http://devcenter.wercker.com/docs/deploy/deploy-steps.html)
of your choosing. 

Under the hood wercker leverages Docker containers to not only allow
automated pipelines, but also isolated environments to run code in. 
Wercker runs your code in a container and takes it through what we call 
[_steps_](http://devcenter.wercker.com/docs/steps/about-steps.html) 
and saves the output as a container.

Wercker also works for developers working on websites, blogs or apps that are not
neccesarily as complex. Because wercker is also a **workflow** and not only a
platform, you can start using wercker from the get-go with 
[`wercker dev`](http://blog.wercker.com/2015/05/15/Introducing-local-development.html). 
This sets you up with a container locally and allows you to start developing in that
container, without changing your existing workflow. 

