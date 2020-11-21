[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Django Framework

When we worked with JavaScript, we used the Express framework to build routes and other functionality into our backend applications. In Python there are similar frameworks we can use. Django is one of the most popular backend Python frameworks, supporting lots of built-in features like an admin content management system, browsable API, and more!

## Prerequisites

- Experience with Python
- Familiarity with APIs

## Objectives

By the end of this, developers should be able to:

- Explain the purpose of Django as a framework
- Identify the pieces of an MVC framework
- Create a Django project and apps

## Preparation

1.  Fork and clone this repository.
 [FAQ](https://git.generalassemb.ly/ga-wdi-boston/meta/wiki/ForkAndClone)
1.  Create a new branch, `training`, for your work.
1.  Checkout to the `training` branch.

## What is Django

Django is a Python framework to build backend and full-stack applications.
We will be using it to make APIs with which our client applications can communicate.

Read through this [history of Django](https://django-book.readthedocs.io/en/latest/chapter01.html#django-s-history) before we get started.

> "At its core, Django is simply a collection of libraries written in the Python programming language. To develop a site using Django, you write Python code that uses these libraries. Learning Django, then, is a matter of learning how to program in Python and understanding how the Django libraries work."
> - [ReadTheDocs](https://django-book.readthedocs.io/en/latest/chapter01.html#required-python-knowledge)

When we worked with JavaScript, Express allowed us to add routes and more to our applications, but it didn't really care **how** we went about doing that. That is known as an "unopinionated" framework. Django is a little different. It's not quite "opinionated", but it has **some** opinions. Django is all about customization, so we will be able to pick from many built-in options as well as scale those to meet our needs.

### MVC Frameworks

MVC stands for model-view-controller. Simply put, this is a way of
developing applications so that the code for defining and using our data
(Model), any request-routing logic (Controller), and the user interface (View)
are all separate from one another. These pieces work together, but are what we
call "loosely coupled", so they can be changed independently without affecting
other pieces.

![mvc](https://media.git.generalassemb.ly/user/16103/files/6c809e80-6db8-11ea-8774-9fce7a0598fb)

We will be using Django to make an API, so we won't be taking advantage of
Django views (feel free to look into that on your own). Instead, we will use
the views to return JSON data from our Django API and have our front-end update
the UI based on the data it recieves from requests.

> Note: _Technically_, Django is known as an [MVT](https://www.javatpoint.com/django-mvt) (Model-View-Template)
> framework since Django actually does the "controller" part for us in the
> background. For now though, it's good to have an understanding of what MVC
> means when working in Django.

## Pipenv and Virtual Environments

We haven't talked about them yet, but virtual environments are essential when
working with Python applications. When you install packages with `pip`, you are
installing them globally. That means every project you create will use the same
packages and versions! This is generally not a good thing, since you may have
projects that use an older version of Django and will break if you suddenly
install the newest version.

We will **always** use virtual environments when working with Django, and only
left them out until now because we weren't working with any packages while
looking at Python on it's own.

Pipenv is a tool that can help you manage your virtual environments as you move
from project to project. That way, your projects will end up looking more like
this:

![pipenv diagram](https://media.git.generalassemb.ly/user/16103/files/ab165900-6db8-11ea-8f8d-e3f410d9c2b7)

### Code-Along: Setting Up a Virtual Environment

1. We should have `pipenv` installed from installfest. Confirm this by running `pipenv --version`. If you get an error, flag down an instructor.

2. Run `pipenv shell` to start our virtual environment. You should see an
indication of this to the left of the file system location:

`(django-intro) ~/sei/trainings/django-intro`

We are now in a virtual environment only for the files in the
`django-intro` folder. We can safely install packages like Django and know that
it will only exist in this project without affecting any others.

3. To get pipenv going on this project, run `pipenv install` inside of this directory.

> Note: Normally we could run this with a package name (and we will in a
> minute), but for now we can run it without anything to initialize pipenv.

After these two steps, we should have some new files: `Pipfile` and `Pipfile.lock`.

**`Pipfile`:** This holds references to the packages we have installed and what
version they should be. This is similar to the `package.json` we used in Node,
in that it will control what is installed when our project is set up. Right
now, our `Pipfile` is mostly empty, but after it's filled with packages it will
be used to guide package setup.

**`Pipfile.lock`:** This is generated when someone runs `pipenv install`, and
holds lots of detail on the specific versions of each pacakage that was
installed. This is important, especially if our `Pipfile` doesn't specify a
version for a package, and is similar to `package-lock.json` in Node.

4. Run `pipenv install django` to install the Django framework. You should see
it appear in the `Pipfile`:

```
[packages]
django = "*"
```

The `*` indicates that if we try to install from this `Pipfile`, any Django
version could be installed! That's not great.

5. Re-do! Run `pipenv install django==3.0` to have pipenv install a specific
version (3.0) of the Django framework.

Your `Pipfile` will override the old reference:

```
[packages]
django = "==3.0"
```

## Code-Along: First Django Project

Django is made up of projects and apps. Projects will hold apps, and apps could
be used across multiple projects.

For us, we will be using the `django-admin` package to spin up our first
project and app inside that project.

### Project: `firstproject`

From inside this repo, run `django-admin startproject firstproject .` to create
our first project!

> **PAUSE**
>
> Look back at the command above. We have added a `.` at the end. This is
> actually pretty important! Without that `.` (denoting the current directory),
> Django will create a folder called `firstproject` with another folder called
> `firstproject` inside of it. That's redundant, and will mean later on we will
> be `cd`ing back and forth to run commands.
>
> TLDR: Make sure you don't forget the `.`

This will create a folder called `firstproject` filled up with some of the
things we will need for this project. Let's look at what we have so far:

- `firstproject/settings.py`: Holds the settings for our project. Things like
our database connection information and other Python modules we want to use
will go in here.
- `firstproject/urls.py`: This is the main routing file for our project,
telling our endpoints what methods to run when they're hit.
- `manage.py`: This will give us common actions we will perform on our
application, like running the server, applying database changes, and more.

We can test out our `firstproject` by running our server. Run
`python3 manage.py runserver` to spin up the server. This will throw some
"migration" errors that we can ignore for now - we will come back to those.

You should see a message like `Starting development server at http://127.0.0.1:8000/`.
If we navigate to that URL in the browser, we should see a message from Django
telling us our installation worked and we are good to go!

> Note: The `127.0.0.1` location is the same as `localhost`, so we should be
> able to go to `localhost:8000` and see the same thing.

### App: `api`

Our project isn't complete without some functionality! In order to add
things like routes, resources, etc. we will need to make an application
inside of our project. In the root of this repository, run `django-admin startapp api`
to create our app, `api`.

> Note: We are choosing to call this app `api` because it will hold our api! We
> talked about how Django is an MVC framework, which means it can serve Views
> for users. However, we want to use it as an API that will send back JSON
> responses to the client server, so we will call it `api`.

This command created another folder for us, this time called `api`. Let's dig
into what we have access to now:

- `models.py`: This file is where we will define all of our data models.
- `views.py`: This is where we will write the logic that happens when someone
accesses one of our URLs.
- `tests.py`: This is where we would write tests. We will come back to this
later.
- `admin.py`: This is how we set up the admin side of our Django application.
- `apps.py`: This is where we can configure our `api` app.

In order to use our app, we need to tell the `firstproject` project about it.
Open up `firstproject/settings.py` so we can add our `api` app to the project's
settings.

## Django Views

Views are really just functions that take in web requests and return web
responses. Often, they are used for displaying information in the form of the
front-end's user interface.

When we used Express, we were able to make the choice between responding to
requests with JSON data or other things, such as HTML files! If we had served
HTML, our backend would have been in control of the front-end "views". Instead,
we chose to respond with JSON like an API, allowing our client-side server to
decide what to do with that data and how to display things for our user. This
is a purposeful separation, however the MVC framework is very popular and is
another way of doing web development.

For our Django application, we will be using our "views" in the same way as
Express, so we will have them respond with JSON instead of display some UI.

Don't get confused as we talk about some of these new concepts like MVC and
Views, though. Let's think about it like this:

> **If our function/class will return a response to a client, it's a view and
> should go in our `views.py` file.** If it doesn't, it's just regular app
> logic and should go somewhere else.

Keep in mind as well that in Express, the URL and the route function are
usually referred to collectively as the route, while it is possible to separate
those out. This is exactly what Django is doing: separating the declaration of
the URL from the function that runs when the URL is requested.

### Code-Along: Our First View

Navigate to the `api/views.py` file. This is where we will add our method
to handle a "view" of our API.

```py
# api/views.py
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
  return HttpResponse('<h1>Hello World! /ᐠ｡‸｡ᐟ\ﾉ</h1>')
```

We've named this method `index` because that's a common name for the "root" or
"home" route of an application. This is our only view so far, so it makes sense
for us to have it be the "home" for our testing purposes.

Currently, our view is just returning an `HttpResponse` with some HTML in it to
the client who is requesting our index view. This is the simplest way to send
back information in response to a request. However, in order to use it we need
to make sure we import it from the `django.http` module into this file.

### Code-Along: Mapping URLs

Our view doesn't do anything without being connected to a URL, however. When we
connect these pieces, we talk about "mapping" our view to a URL endpoint. Our
URL will point to a certain view method that will be called when we reach the
URL.

Our URLs will be app-specific, not project-specific, which means this
functionality should go inside of our `api` folder. There's no file there yet
for handling our URLs, so let's make one. Create a file called `urls.py` inside
of `api`.

In here, we will import the `path` method from Django's modules to setup our
URL as well as our own custom views from our app. Then, we will create our
`urlpatterns` that will reference all of our available endpoints and what view
they map to.

```py
# api/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

For our index view, we are giving the `path` method `''` as the endpoint, so
this will be what we hit when we go to that "home" page of our app. In Express,
we may have used `'/'` for our "home" or "root" route, but in Django we just
put an empty string. If we tried to put `'/'`, we would get a "Page not found"
error when trying to reload the browser.

We are pointing this "home" path to `views.index` which is the method we want
run when we hit the endpoint. Finally, we give it a name just to identify it
for other parts of our Django application.

This is great, but our URL is only set up for our app, `api`. It's not
connected to our project yet, though! We have a `urls.py` file for our project
too. Open up `firstproject/urls.py` and we will update it so it knows about the
URLs in our `api` app.

First, we need to import the `include` method from the `django.urls` module.
Then, we can add a path to the URLs of our project that points to the URLs in
our `api` app.

```py
# firstproject/firstproject/urls.py
from django.urls import path, include
from django.contrib import admin

urlpatterns = [
  path('admin/', admin.site.urls),
  # Add the line below...
  path('', include('api.urls'))
]
```

Once we add that to our project, we should be able to refresh our browser page
and see our "Hello World" message! Django will restart the server for us when
it sees a file change, so we don't need to close and restart the server
ourselves.

## Sidenote: The Admin Route

You may have noticed that Django came with a URL already included in the
`firstproject/urls.py` file: `path('admin/', admin.site.urls)`.

> "One of the most powerful parts of Django is its automatic admin interface.
> It reads metadata in your models to provide a powerful and production-ready
> interface that content producers can immediately use to start managing
> content on your site. It’s easy to set up and provides many hooks for
> customization."
>
> - [Django Project Docs](https://docs.djangoproject.com/en/3.0/ref/contrib/admin/)

This route will bring us to an admin interface where we can CRUD on resources
on our API without having to make actual requests to it! This is one of the
nice things built-in to Django.

Navigate to `localhost:8000/admin` to see the admin page. You'll see a form
asking for you to sign in with an email and password, which we don't have yet!
We will create one a little later when we have resources to look at.

For now, we can use this url setup as an example for how to create a route that
isn't at our "home" endpoint of `''`.

## Lab: Adding More Routes

Now that we know how to add a basic route to our project, let's add more!

Create routes for two pages: `about` and `faq`. These pages should display some
text. You can pick what text to display, but we should see something other than
Django's default page when we go to `localhost:8000/about` and `localhost:8000/
faq` in the browser.

1. Add a URL pattern to your application's urls.py file.
2. Add a view function that the URL will call in your application's views.py file.

While you work on this, you may run into errors with the endpoint. Play around
with how you are defining the endpoint, and look at how Django automatically
defines the "admin" route as a reference.

## Additional Resources

### Try More:

- [Django Introduction](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Introduction)
- [Getting Started with Django](https://www.djangoproject.com/start/)

### Dive Deeper:

- [Pipenv](https://pypi.org/project/pipenv/)
- [MVC and MTC in Django](https://data-flair.training/blogs/django-architecture/)

## [License](LICENSE)

1. All content is licensed under a CC­BY­NC­SA 4.0 license.
1. All software code is licensed under GNU GPLv3. For commercial use or
    alternative licensing, please contact legal@ga.co.
