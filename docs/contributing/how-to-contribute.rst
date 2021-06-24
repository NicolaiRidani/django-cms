.. _how-to-contribute:

#################
How to contribute
#################

There are various ways to contribute to the django CMS project. For developers the

* :ref:`contributing_code`
* :ref:`contributing-documentation`
* :ref:`contributing-translations`
* :ref:`contributing-other`

.. _contributing_code:

*****************
Contributing code
*****************

Like every open-source project, django CMS is always looking for motivated
individuals to contribute to its source code.

In a nutshell
=============

Here's what the contribution process looks like in brief:

#. Fork our `GitHub`_ repository, https://github.com/django-cms/django-cms
#. Work locally and push your changes to your repository.
#. When you feel your code is good enough for inclusion, send us a pull request.
#. After that, please join the `Slack Channel`_ of our Pull Request Review work group (#workgroup-pr-review). This group of friendly community members is dedicated to reviewing pull requests. Report your PR and find a “pr review buddy” who is going to review your pull request.
#. Get acknowledged by the django CMS community for your contribution

See the :ref:`contributing_patch` how-to document for a walk-through of this process.

********************************
Basic requirements and standards
********************************


If you're interested in developing a new feature for the CMS, it is recommended
that you first discuss it on the `Discourse forum <https://discourse.django-cms.org>`_ so as
not to do any work that will not get merged in anyway.

- Code will be reviewed and tested by at least one core developer, preferably
  by several. Other community members are welcome to give feedback.
- Code *must* be tested. Your pull request should include unit-tests (that cover
  the piece of code you're submitting, obviously)
- Documentation should reflect your changes if relevant. There is nothing worse
  than invalid documentation.
- Usually, if unit tests are written, pass, and your change is relevant, then
  it'll be merged.

Since we're hosted on GitHub, django CMS uses `git`_ as a version control system.

The `GitHub help`_ is very well written and will get you started on using git
and GitHub in a jiffy. It is an invaluable resource for newbies and old timers
alike.


Syntax and conventions
======================

Python
------

We try to conform to `PEP8`_ as much as possible. A few highlights:

- Indentation should be exactly 4 spaces. Not 2, not 6, not 8. **4**. Also, tabs
  are evil.
- We try (loosely) to keep the line length at 79 characters. Generally the rule
  is "it should look good in a terminal-base editor" (eg vim), but we try not be
  too inflexible about it.


HTML, CSS and JavaScript
------------------------

As of django CMS 3.2, we are using the same guidelines as described in `Aldryn
Boilerplate`_

Frontend code should be formatted for readability. If in doubt, follow existing
examples, or ask.

.. _js_linting:

JS Linting
----------

JavaScript is linted using `ESLint <http://eslint.org>`_. In order to run the
linter you need to do this:

.. code-block:: sh

    gulp lint

Or you can also run the watcher by just running ``gulp``.


Process
=======

This is how you fix a bug or add a feature:

#. `fork`_ us on GitHub.
#. Checkout your fork.
#. *Hack hack hack*, *test test test*, *commit commit commit*, test again.
#. Push to your fork.
#. Open a pull request.

And at any point in that process, you can add: *discuss discuss discuss*,
because it's always useful for everyone to pass ideas around and look at things
together.

:ref:`testing` is really important: a pull request that lowers our testing
coverage will only be accepted with a very good reason; bug-fixing patches
**must** demonstrate the bug with a test to avoid regressions and to check
that the fix works.

We have a `Slack Channel`_, a `Discourse forum
<https://discourse.django-cms.org>`_, and of course the code reviews mechanism on GitHub - do use them.


.. _contributing_frontend:


Frontend
========

..  important::

    When we refer to the *frontend* here, we **only** mean the frontend of django CMS's admin/editor interface.

    The frontend of a django CMS website, as seen by its visitors (i.e. the published site), is *wholly independent of
    this*. django CMS places almost no restrictions at all on the frontend - if a site can be described in
    HTML/CSS/JavaScript, it can be developed in django CMS.

In order to be able to work with the frontend tooling contributing to the
django CMS you need to have the following dependencies installed:

    1. `Node <https://nodejs.org/>`_ version 6.10.1 (will install npm 3.10.10 as well).
       We recommend using `NVM <https://github.com/creationix/nvm>`_ to get
       the correct version of Node.
    2. gulp - see `Gulp's Getting Started notes <https://github.com/gulpjs/gulp/tree/master/docs/getting-started>`_
    3. Local dependencies ``npm install``

Styles
======

We use `Sass <http://sass-lang.com/>`_ for our styles. The files
are located within ``cms/static/cms/sass`` and can be compiled using the
`libsass <https://sass-lang.com/libsass>`_ implementation of Sass compiler through
`gulp <http://gulpjs.com/>`_.

In order to compile the stylesheets you need to run this command from the repo
root::

    gulp sass

While developing it is also possible to run a watcher that compiles Sass files
on change::

    gulp

By default, source maps are not included in the compiled files. In order to turn
them on while developing just add the ``--debug`` option::

    gulp --debug

Icons
=====

We are using `gulp-iconfont <https://github.com/backflip/gulp-iconfont>`_ to
generate icon web fonts into ``cms/static/cms/fonts/``. This also creates
``_iconography.scss`` within ``cms/static/cms/sass/components`` which adds all
the icon classes and ultimately compiles to CSS.

In order to compile the web font you need to run::

    gulp icons

This simply takes all SVGs within ``cms/static/cms/fonts/src`` and embeds them
into the web font. All classes will be automatically added to
``_iconography.scss`` as previously mentioned.

Additionally we created an SVG template within
``cms/static/cms/font/src/_template.svgz`` that you should use when converting
or creating additional icons. It is named *svgz* so it doesn't get compiled
into the font. When using *Adobe Illustrator* please mind the
`following settings <images/svg_settings.png>`_.


JS Bundling
===========

JavaScript files are split up for easier development, but in the end they are
bundled together and minified to decrease amount of requests made and improve
performance. In order to do that we use the ``gulp`` task runner, where ``bundle``
command is available. We use `Webpack <https://github.com/webpack/webpack>`_ for
bundling JavaScript files. Configuration for each bundle are stored inside the
``webpack.config.js`` and their respective entry points. CMS exposes only one
global variable, named ``CMS``. If you want to use JavaScript code provided by
CMS in external applications, you can only use bundles distributed by CMS, not
the source modules.

.. _contributing-documentation:

**************************
Contributing documentation
**************************

Perhaps considered "boring" by hard-core coders, documentation is sometimes even
more important than code! This is what brings fresh blood to a project, and
serves as a reference for old timers. On top of this, documentation is the one
area where less technical people can help most - you just need to write
simple, unfussy English. Elegance of style is a secondary consideration, and
your prose can be improved later if necessary.

Contributions to the documentation earn the greatest respect from the
core developers and the django CMS community.

Documentation should be:

- written using valid `Sphinx`_/`restructuredText`_ syntax (see below for
  specifics); the file extension should be ``.rst``
- wrapped at 100 characters per line
- written in English, using British English spelling and punctuation
- accessible - you should assume the reader to be moderately familiar with
  Python and Django, but not anything else. Link to documentation of libraries
  you use, for example, even if they are "obvious" to you

Merging documentation is pretty fast and painless.

Except for the tiniest of change, we recommend that you test them before
submitting.

Building the documentation
==========================

Follow the same steps above to fork and clone the project locally. Next, ``cd`` into the
``django-cms/docs`` and install the requirements::

    make install

Now you can test and run the documentation locally using::

    make run

This allows you to review your changes in your local browser using ``http://localhost:8001/``.

.. note:: **What this does**

    ``make install`` is roughly the equivalent of::

        virtualenv env
        source env/bin/activate
        pip install -r requirements.txt
        cd docs
        make html

    ``make run`` runs ``make html``, and serves the built documentation on port 8001 (that is, at
    ``http://localhost:8001/``.

    It then watches the ``docs`` directory; when it spots changes, it will automatically rebuild
    the documentation, and refresh the page in your browser.


.. _spelling:


Spelling
========

We use `sphinxcontrib-spelling <https://pypi.python.org/pypi/sphinxcontrib-spelling/>`_, which in
turn uses `pyenchant <https://pypi.python.org/pypi/pyenchant/>`_ and `enchant
<http://www.abisource.com/projects/enchant/>`_ to check the spelling of the documentation.

You need to check your spelling before submitting documentation.

.. important::

    We use British English rather than US English spellings. This means that we use *colour*
    rather than *color*, *emphasise* rather than *emphasize* and so on.


Install the spelling software
=============================

``sphinxcontrib-spelling`` and ``pyenchant`` are Python packages that will be installed in the
virtualenv ``docs/env`` when you run ``make install`` (see above).

You will need to have ``enchant`` installed too, if it is not already. The easy way to check is to
run ``make spelling`` from the ``docs`` directory. If it runs successfully, you don't need to do
anything, but if not you will have to install ``enchant`` for your system. For example, on OS X::

    brew install enchant

or Debian Linux::

    apt-get install enchant


Check spelling
==============

Run::

    make spelling

in the ``docs`` directory to conduct the checks.

.. note::

    This script expects to find a virtualenv at ``docs/env``, as installed by ``make install`` (see
    above).

If no spelling errors have been detected, ``make spelling`` will report::

    build succeeded.

Otherwise::

    build finished with problems.
    make: *** [spelling] Error 1

It will list any errors in your shell. Misspelt words will be also be listed in
``build/spelling/output.txt``

Words that are not in the built-in dictionary can be added to ``docs/spelling_wordlist``. **If**
you are certain that a word is incorrectly flagged as misspelt, add it to the ``spelling_wordlist``
document, in alphabetical order. **Please do not add new words unless you are sure they should be
in there.**

If you find technical terms are being flagged, please check that you have capitalised them
correctly - ``javascript`` and ``css`` are **incorrect** spellings for example. Commands and
special names (of classes, modules, etc) in double backticks - `````` - will mean that they are not
caught by the spelling checker.

.. important::

    You may well find that some words that pass the spelling test on one system but not on another.
    Dictionaries on different systems contain different words and even behave differently. The
    important thing is that the spelling tests pass on `Github Actions
    <https://github.com/django-cms/django-cms/actions>`_ when you submit a pull request.


Making a pull request
=====================

Before you commit any changes, you need to check spellings with ``make spelling`` and rebuild the
docs using ``make html``. If everything looks good, then it's time to push your changes to GitHub
and open a pull request in the usual way.



Documentation structure
=======================

Our documentation is divided into the following main sections:

* :doc:`/introduction/index` (``introduction``): step-by-step, beginning-to-end tutorials to get
  you up and running
* :doc:`/reference/index` (``reference``): technical reference for APIs, key
  models
  and so on
* :doc:`/contributing/index` (``contributing``)
* :doc:`/upgrade/index` (``upgrade``)
* :doc:`/user/index` (``user``): guides for *using* rather than setting up or developing for the
  CMS



Documentation markup
====================

Sections
--------

We mostly follow the Python documentation conventions for section marking::

    ##########
    Page title
    ##########

    *******
    heading
    *******

    sub-heading
    ===========

    sub-sub-heading
    ---------------

    sub-sub-sub-heading
    ^^^^^^^^^^^^^^^^^^^

    sub-sub-sub-sub-heading
    """""""""""""""""""""""


Inline markup
-------------

* use backticks - `````` - for:
    * literals::

        The ``cms.models.pagemodel`` contains several important methods.

    * filenames::

        Before you start, edit ``settings.py``.

    * names of fields and other specific items in the Admin interface::

        Edit the ``Redirect`` field.

* use emphasis - ``*Home*`` - around:
    * the names of available options in or parts of the Admin::

        To hide and show the *Toolbar*, use the...

    * the names of important modes or states::

        ... in order to switch to *Edit mode*.

    * values in or of fields::

        Enter *Home* in the field.

* use strong emphasis - ``**`` - around:
    * buttons that perform an action::

        Hit **View published** or **Save as draft**.



Rules for using technical words
===============================

There should be one consistent way of rendering any technical word, depending on its context.
Please follow these rules:

* in general use, simply use the word as if it were any ordinary word, with no capitalisation or
  highlighting: "Your placeholder can now be used."
* at the start of sentences or titles, capitalise in the usual way: "Placeholder management guide"
* when introducing the term for the the first time, or for the first time in a document, you may
  highlight it to draw attention to it: "**Placeholders** are special model fields".
* when the word refers specifically to an object in the code, highlight it as a literal:
  "``Placeholder`` methods can be overwritten as required" - when appropriate, link the term to
  further reference documentation as well as simply highlighting it.


References
==========

Create::

    .. _testing:

and use::

     :ref:`testing`

internal cross-references liberally.


Use absolute links to other documentation pages - ``:doc:`/how_to/toolbar``` -
rather than relative links - ``:doc:`/../toolbar```. This makes it easier to
run search-and-replaces when items are moved in the structure.

.. _contributing-translations:

*************************
Contributing translations
*************************

For translators we have a `Transifex account
<https://www.transifex.com/divio/django-cms/>`_ where you can translate
the ``.po`` files and don't need to install git or mercurial to be able to
contribute. All changes there will be automatically sent to the project.


.. _contributing-other:

**************************************************************
Contributing to other areas like (product) marketing or events
**************************************************************

As you can imagine, as an open source project we need help on all fronts.
Whether it's content creation or user support. Any help is welcome. Would you like
to present your django CMS project? Then create a case study together with us.
Are you an expert in a field and would like to share your knowledge with others?
Then start a workshop series and teach other people. Or do you like writing blog
articles and would like to draw the attention of a large readership to a topic related
to django CMS? Then publish your article on our website.

Learn more about our community tasks `here <https://www.django-cms.org/en/community-tasks/>`_


.. _restructuredText: http://docutils.sourceforge.net/docs/ref/rst/introduction.html
.. _Sphinx: http://sphinx-doc.org//
.. _Slack Channel: https://django-cmsworkspace.slack.com/
.. _GitHub: http://www.github.com
.. _fork: https://github.com/django-cms/django-cms
.. _PEP8: http://www.python.org/dev/peps/pep-0008/
.. _Aldryn Boilerplate: https://aldryn-boilerplate-bootstrap3.readthedocs.io/en/latest/guidelines/index.html
.. _django-cms-developers: https://groups.google.com/group/django-cms-developers
.. _GitHub help: http://help.github.com
.. _freenode: http://freenode.net/
.. _pull request: http://help.github.com/send-pull-requests/
.. _git: http://git-scm.com/

