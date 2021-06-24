..  _create_new:

###############################
Create a new Django CMS project
###############################


Installing django CMS
=====================

..  note::
    Here we assume that you have Django 3.0, and Python 3.8. If you need info about support old versions of Django or Python you can find it here: `<https://docs.django-cms.org/en/latest/index.html#software-version-requirements-and-release-notes>`_


Step 1 - venv
-------------

Linux
::
    python3.8 -m venv env
    source env/bin/activate

Windows
::
    py -m venv env
    env\Scripts\activate

First line creates python virtual environment, second activates it. Than you need to update pip
::
    pip install --upgrade pip

`More info about venv <https://docs.python.org/3.8/library/venv.html?highlight=venv#module-venv>`_

Use the django CMS installer
============================

The `django CMS installer <https://github.com/nephila/djangocms-installer>`_ is
a helpful script that takes care of setting up a new project.

Install it::

    pip install djangocms-installer

This provides you with a new command, ``djangocms``.

Create a new directory to work in, and ``cd`` into it::

    mkdir tutorial-project
    cd tutorial-project

Run it to create a new Django project called ``mysite``::

    djangocms mysite

This means:

* run the django CMS installer
* call the new project directory ``mysite``


.. warning::
   djangocms-installer expects current directory to be empty at this stage, and will check for this,
   and will warn if it's not. You can get it to skip the check and go ahead anyway using the ``-s``
   flag; **note that this may overwrite existing files**.


Windows users may need to do a little extra to make sure Python files are associated correctly if that doesn't work right away::

    assoc .py=Python.file
    ftype Python.File="C:\Users\Username\workspace\demo\env\Scripts\python.exe" "%1" %*

By default, the installer runs in `Batch mode
<https://djangocms-installer.readthedocs.io/en/latest/usage.html#batch-mode-default>`_, and sets up your new project
with some default values.

Later, you may wish to manage some of these by yourself, in which case you need to run it in `Wizard mode
<https://djangocms-installer.readthedocs.io/en/latest/usage.html#wizard-mode>`_. The default in *Batch mode* is to set
up an English-only project, which will be sufficient for the purposes of this tutorial. You can of course simply edit
the new project's ``settings.py`` file at any time to change or add site languages or amend other settings.

The installer creates an admin user for you, with username/password ``admin``/``admin``.


Start up the runserver
======================

Go to mysite directory

::

    cd mysite

Start the server using following command

::

    cd mysite
    python manage.py runserver

Open http://localhost:8000/ in your browser, where you should be invited to login, and then create
a new page.

.. image:: /introduction/images/welcome.png
   :alt: a django CMS home page
   :width: 400
   :align: center

Congratulations, you now have installed a fully functional CMS.

If you need to log in at any time, append ``?edit`` to the URL and hit Return. This will enable the
toolbar, from where you can log in and manage your website.

If you are not already familiar with django CMS, you can take a few minutes to run through the
basics of the :ref:`django CMS tutorial for users <user-tutorial>`.
