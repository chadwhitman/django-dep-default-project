# DEP XXX: Django Default Project Enhancements

* DEP: XXX
* Author: Timothy Allen
* Implementation Team: Timothy Allen, TBA
* Shepherd: TBA
* Status: Draft
* Type: Feature
* Created: 2017-02-20
* Last-Modified: 2017-02-20

## Abstract

This DEP describes enhancements which could be made to the default Django Project created when the `startproject` command is run. The goal is to maintain the simplicity of the initial project, while removing pain points for newcomers and taking advantage of several opportunities to give direction for learning.

This new defined structure would become the default project created with the `startproject` command.

## Background and Motivation

Over the past few years, I have had the opportunity to introduce Django to over a hundred people. Some of these people have been experienced developers, some have been brand new to coding, with the full spectrum of experience levels in between. Patterns have emerged from this experience, exposing several pain points that the default Django Project could be modified to remove. The end goal would be making Django more accessible to the newcomer.

There are three main pain points which have been revealed:

* Confusion on the location of project settings, and how this directory differs from Django apps
* Lack of console feedback from the `djangoadmin startproject` command
* Answering the question after "It Worked!": "What's Next?"

This proposal also seeks to leverage opportunities for providing useful feedback to the learning process. The feature would also include rewriting the documenation where necessary, and notifying (and working with) maintainers of third-party tutorials. It would not include [a rewrite of the URLs syntax](https://gist.github.com/tomchristie/cb388f0f6a0dec931c611775f32c5f98), another pain point for beginners, which is already being addressed.

## Specification

Here are functional specifications for what we would recommend changing.

### Feedback From the 'startproject' Command

The `django-admin startproject` command currently only gives console feedback if there is an error. This is an opportunity to provide some friendly text for new Djangonauts, which can easily be ignored by those with more experience:

    $ django-admin startproject myproject
    Your project 'myproject' has been created successfully! You can try to run your site with these commands:
        $ cd myproject
        $ python manage.py runserver
    Then, visit http://localhost:8000/ in your browser. If you are having issues (or are using Vagrant), you may want to try:
        $ python manage.py runserver 0:8000

This extra piece of handholding can be especially helpful to newcomers just starting to get comfortable with the command line. The 'startproject' command should be geared towards the newcomer in particular, since experienced Djangonauts can use some sort of starting template system for their preferred project layout (copy & paste, the '--template' argument, or Cookie Cutter, for example).

### Configuration Directory Convention

Currently, Django creates a configuration sub-directory with the same name given to the project. For example:

```
myproject/
    manage.py
    myproject/
        settings.py
        urls.py
        wsgi.py
    app1/
    app2/
```

This is a source of confusion for the majority of new Djangonauts. Let's pick a convention and stick with it, a `config` directory:

```
myproject/
    manage.py
    requirements.txt
    config/
        settings.py
        urls.py
        wsgi.py
    app1/
    app2/
```

This has several advantages. First, configuration files will always be in a `config` subdirectory. Explaining to newcomers that the `config.urls` is the root of your URLs has made a lot more sense to newcomers since I have started using this project layout.

While a requirements file isn't necessarily part of a first time experience, it gives an opportunity for learning by introspection as well. Let's create a `requirements.txt` file with a single line, `Django==X.Y.Z`, so when a newcomer sees references to a requirements file, there is something to find.

### Valuable Real Estate: The "It Worked!" page

After successfully starting a Django project and bringing it up in the browser, the new Django user is greeted with this page:

![Django's 'It Worked!' Page](img/itworked.jpg "Django's 'It Worked!' Page")

While it gives a nice instruction on starting a Django app, this seems like a huge missed opportunity to point a new Djangonaut towards one of the many vibrant Django tutorials available. Imagine if we provided the newcomer with something like this instead:

![Proposal 'Congratulations!' Page](img/congrats.jpg "Proposal 'Congratulations!' Page")

This would allow us to feature several styles of next steps, for different kinds of newcomers. An idea for the Google search link would be [django tutorials modified within the past year](https://www.google.com/search?q=django+tutorials&tbs=qdr:y).

## Documentation

The documentation will have to be updated to refelect these changes, most heavily in the tutorial.

* The [Django Tutorial](https://docs.djangoproject.com/en/1.11/intro/) contains many references to `settings.py`.
* Finding other locations that specifically reference a `settings.py` file: https://docs.djangoproject.com/en/1.10/search/?q=settings.py

## Implementation tasks

The following independant tasks can be identified:

* Convert the project created with the `djangoadmin startproject` command to reflect the new default project structure.
    * Create a `config` directory instead of a nested directory with the project name.
    * Modify the "It Worked!" browser confirmation page.
    * Add a `requirements.txt` file with current Django version.
* Update the Django documentation.
* Identify affected third party tutorials and notify their maintainers.
* Choose sites to highlight in "What's Next" section of the "Congratulations!" page (formerly titled, "It Worked!").
