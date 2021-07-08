.. _core-principals-integrating-django-applications-apphooks:

########
Apphooks
########

Right now, our Django Polls application is statically hooked into the project's
``urls.py``. This is all right, but we can do more, by attaching applications to
django CMS pages.


.. _core-principals-integrating-django-application-apphooks-create-an-apphook:

*****************
Create an apphook
*****************

We do this with an **apphook**, created using a :class:`CMSApp
<cms.app_base.CMSApp>` sub-class, which tells the CMS how to include that application.


Create the apphook class
========================

Apphooks live in a file called ``cms_apps.py``, so create one in your Polls/CMS Integration
application, i.e. in ``polls_cms_integration``.

This is a very basic example of an apphook for a django CMS application:

.. code-block:: python

    from cms.app_base import CMSApp
    from cms.apphook_pool import apphook_pool


    @apphook_pool.register  # register the application
    class PollsApphook(CMSApp):
        app_name = "polls"
        name = "Polls Application"

        def get_urls(self, page=None, language=None, **kwargs):
            return ["polls.urls"]


In this ``PollsApphook`` class, we have done several key things:

* ``app_name`` attribute gives the system a unique way to refer to the apphook. You can see from
  `Django Polls <https://github.com/divio/django-polls/blob/master/polls/urls.py#L6>`_ that the
  application namespace ``polls`` is hard-coded into the application, so this attribute **must**
  also be ``polls``.
* ``name`` is a human-readable name, and will be displayed to the admin user.
* ``get_urls()`` method is what actually hooks the application in, returning a
  list of URL configurations that will be made active wherever the apphook is used - in this case,
  it will use the ``urls.py`` from ``polls``.


Remove the old ``polls`` entry from the project's ``urls.py``
=============================================================

You must now remove the entry for the Polls application::

    re_path(r'^polls/', include('polls.urls', namespace='polls'))

from your project's ``urls.py``.

Not only is it not required there, because we reach the polls via the apphook
instead, but if you leave it there, it will conflict with the apphook's URL handling. You'll
receive a warning in the logs::

    URL namespace 'polls' isn't unique. You may not be able to reverse all URLs in this namespace.


Restart the runserver
=====================

**Restart the runserver**. This is necessary because we have created a new file containing Python
code that won't be loaded until the server restarts. You only have to do this the first time the
new file has been created.


.. _apply_apphook:

***************************
Apply the apphook to a page
***************************

Now we need to create a new page, and attach the Polls application to it through this apphook.

Create and save a new page, then publish it.

..  note:: Your apphook won't work until the page has been published.

In its *Advanced settings* (from the toolbar, select *Page > Advanced settings...*) choose "Polls
Application" from the *Application* pop-up menu, and save once more.

.. image:: /gettingstarted/images/select-application.png
   :alt: select the 'Polls' application
   :width: 400
   :align: center

Refresh the page, and you'll find that the Polls application is now available
directly from the new django CMS page.

..  important::

    Don't add child pages to a page with an apphook.

    The apphook "swallows" all URLs below that of the page, handing them over to the attached
    application. If you have any child pages of the apphooked page, django CMS will not be
    able to serve them reliably.

.. _about_apphooks:

##############################
Application hooks ("apphooks")
##############################

An Application Hook, usually simply referred to as an apphook, is a way of attaching
the functionality of some other application to a django CMS page. It's a convenient way
of integrating other applications into a django CMS site.

For example, suppose you have an application that maintains and publishes information
about Olympic records. You could add this application to your site's ``urls.py`` (before
the django CMS URLs pattern), so that users will find it at ``/records``.

However, although it would thus be integrated into your *project*, it would not be
fully integrated into django CMS, for example:

* django CMS would not be aware of it, and - for example - would allow your users to create a CMS page with the same
  ``/records`` slug, that could never be reached.
* The application's pages won't automatically appear in your site's menus.
* The application's pages won't be able to take advantage of the CMS's publishing
  workflow, permissions or other functionality.

Apphooks offer a more complete way of integrating other applications, by attaching them
to a CMS page. In this case, the attached application takes over the page and its URL
(and all the URLs below it, such as ``/records/1984``).

The application can be served at a URL defined by the content managers, and easily moved
around in the site structure.

The *Advanced settings* of a CMS page provides an *Application* field. :ref:`apphooks_how_to` to the
application will allow it to be selected in this field.


*********************************
Multiple apphooks per application
*********************************

It's possible for an application to be added multiple times, to different pages. See :ref:`multi_apphook` for more.

Also possible to provide **multiple apphook configurations**:


**********************
Apphook configurations
**********************

You may require the same application to behave differently in different locations on your site. For example, the Olympic
Records application may be required to publish athletics results at one location, but cycling results at another, and so on.

An :ref:`apphook_configurations` class allows the site editors to create multiple configuration
instances that specify the behaviour. The kind of configuration available is presented in an admin form, and determined by the
application developer.

..  important::

    It's important to understand that an apphook (and therefore also an apphook configuration)
    serves no function until it is attached to a page - and until the page is **published**, the
    application will be unable to fulfil any publishing function.

    Also note that the apphook "swallows" all URLs below that of the page, handing them over to the
    attached application. If you have any child pages of the apphooked page, django CMS will not be
    able to serve them reliably.

.. _apphooks_how_to:

######################
How to create apphooks
######################

An **apphook** allows you to attach a Django application to a page. For example,
you might have a news application that you'd like integrated with django CMS. In
this case, you can create a normal django CMS page without any content of its
own, and attach the news application to the page; the news application's content
will be delivered at the page's URL.

All URLs in that URL path will be passed to the attached application's URL configs.

The gettingstarted section contains a basic guide to :ref:`getting started with
apphooks <core-principals-integrating-django-application-apphooks-create-an-apphook>`. This document assumes more familiarity with the CMS generally.


******************************
The basics of apphook creation
******************************

To create an apphook, create a ``cms_apps.py`` file in your application.

The file needs to contain a :class:`CMSApp <cms.app_base.CMSApp>` sub-class. For example::

    from cms.app_base import CMSApp
    from cms.apphook_pool import apphook_pool

    @apphook_pool.register
    class MyApphook(CMSApp):
        app_name = "myapp"  # must match the application namespace
        name = "My Apphook"

        def get_urls(self, page=None, language=None, **kwargs):
            return ["myapp.urls"] # replace this with the path to your application's URLs module

.. versionchanged:: 3.3
    ``CMSApp.get_urls()`` replaces ``CMSApp.urls``. ``urls`` was removed
    in version 3.5.


Apphooks for namespaced applications
====================================

Your application should use :ref:`namespaced URLs <django:topics-http-defining-url-namespaces>`.

In the example above, the application uses the ``myapp`` namespace. Your ``CMSApp``
sub-class **must reflect the application's namespace** in the ``app_name`` attribute.

The application may specify a namespace by supplying an ``app_name`` in its ``urls.py``, or its
documentation might advise that you when include its URLs, you do it thus:

..  code-block:: python

    re_path(r'^myapp/', include('myapp.urls', app_name='myapp'))

If you fail to do this, then any templates in the application that invoke URLs using the form ``{% url
'myapp:index' %}`` or views that call (for example) ``reverse('myapp:index')`` will throw a
``NoReverseMatch`` error.


Apphooks for non-namespaced applications
----------------------------------------

If you are writing apphooks for third-party applications, you may find one that in fact does
not have an application namespace for its URLs. Such an application is liable to tun into namespace
conflicts, and doesn't represent good practice.

However if you *do* encounter such an application, your own apphook for it will need in turn to forgo the
``app_name`` attribute.

Note that unlike apphooks without ``app_name`` attributes can be attached only to one page at a
time; attempting to apply them a second time will cause an error. Only one instance of these
apphooks can exist.

See :ref:`multi_apphook` for more on having multiple apphook instances.


Returning apphook URLs manually
===============================

Instead of defining the URL patterns in another file ``myapp/urls.py``, it also is possible
to return them manually, for example if you need to override the set provided. An example:

..  code-block:: python

    from django.urls import re_path
    from myapp.views import SomeListView, SomeDetailView

    class MyApphook(CMSApp):
        # ...
        def get_urls(self, page=None, language=None, **kwargs):
            return [
                re_path(r'^$', SomeListView.as_view()),
                re_path(r'^(?P<slug>[\w-]+)/?$', SomeDetailView.as_view()),
            ]

However, it's much neater to keep them in the application's ``urls.py``, where they can easily be
reused.


.. _reloading_apphooks:

Loading new and re-configured apphooks
======================================

Certain apphook-related changes require server restarts in order to be loaded.

Whenever you:

* add or remove an apphook
* change the slug of a page containing an apphook or the slug of a page which has a descendant with
  an apphook

the URL caches must be reloaded.

If you have the :ref:`ApphookReloadMiddleware` installed, which is recommended, the server will do
it for you by re-initialising the URL patterns automatically.

Otherwise, you will need to restart the server manually.


****************
Using an apphook
****************

Once your apphook has been set up and loaded, you'll now be able to select the *Application* that's
hooked into that page from its *Advanced settings*.

.. note::

    An apphook won't actually do anything until the page it belongs to is published. Take note that
    this also means all parent pages must also be published.

The apphook attaches all of the apphooked application's URLs to the page; its root URL will be the
page's own URL, and any lower-level URLs will be on the same URL path.

So, given an application with the ``urls.py`` for the views ``index_view`` and ``archive_view``::

    urlpatterns = [
        re_path(r'^$', index_view),
        re_path(r'^archive/$', archive_view),
    ]

attached to a page whose URL path is ``/hello/world/``, the views will be exposed as follows:

* ``index_view`` at ``/hello/world/``
* ``archive_view`` at ``/hello/world/archive/``


Sub-pages of an apphooked page
==============================

..  important::

    Don't add child pages to a page with an apphook.

    The apphook "swallows" all URLs below that of the page, handing them over to the attached
    application. If you have any child pages of the apphooked page, django CMS will not be
    able to serve them reliably.


*****************
Managing apphooks
*****************

Uninstalling an apphook with applied instances
==============================================

If you remove an apphook class from your system (in effect uninstalling it) that still has
instances applied to pages, django CMS tries to handle this as gracefully as possible:

* Affected pages still maintain a record of the applied apphook; if the apphook class is
  subsequently reinstated, it will work as before.
* The page list will show apphook indicators where appropriate.
* The page will otherwise behave like a normal django CMS page, and display its placeholders in the
  usual way.
* If you save the page's *Advanced settings*, the apphook will be removed.


Management commands
===================

You can clear uninstalled apphook instances using the CMS management command ``uninstall apphooks``. For example::

    manage.py cms uninstall apphooks MyApphook MyOtherApphook

You can get a list of installed apphooks using the :ref:`cms-list-command`; in this case::

    manage.py cms list apphooks

See the :ref:`Management commands reference <management_commands>` for more information.


.. _apphook_menus:

************************
Adding menus to apphooks
************************

Generally, it is recommended to allow the user to control whether a menu is attached to a page (See
:ref:`core-principals-integrating-django-applications-integrating-attach-menus` for more on these menus). However, an apphook can be made to do
this automatically if required. It will behave just as if the menu had been attached to the page
using its *Advanced settings*).

Menus can be added to an apphook using the ``get_menus()`` method. On the basis of the example above::

    # [...]
    from myapp.cms_menus import MyAppMenu

    class MyApphook(CMSApp):
        # [...]
        def get_menus(self, page=None, language=None, **kwargs):
            return [MyAppMenu]

.. versionchanged:: 3.3
    ``CMSApp.get_menus()`` replaces ``CMSApp.menus``. The ``menus`` attribute is now deprecated and
    has been removed in version 3.5.


The menus returned in the ``get_menus()`` method need to return a list of nodes, in their
``get_nodes()`` methods. :ref:`core-principals-integrating-django-applications-integrating-attach-menus` has more information on creating menu
classes that generate nodes.

You can return multiple menu classes; all will be attached to the same page::

    def get_menus(self, page=None, language=None, **kwargs):
        return [MyAppMenu, CategoryMenu]


.. _apphook_permissions:

********************************
Managing permissions on apphooks
********************************

By default the content represented by an apphook has the same permissions set as the page it is
assigned to. So if for example a page requires the user to be logged in, then the attached apphook
and all its URLs will have the same requirements.

To disable this behaviour set ``permissions = False`` on your apphook::

    class MyApphook(CMSApp):
        [...]
        permissions = False

If you still want some of your views to use the CMS's permission checks you can enable them via a decorator, ``cms.utils.decorators.cms_perms``

Here is a simple example::

    from cms.utils.decorators import cms_perms

    @cms_perms
    def my_view(request, **kw):
        ...

If you make your own permission checks in your application, then use the ``exclude_permissions`` property of the apphook::

    class MyApphook(CMSApp):
        [...]
        permissions = True
        exclude_permissions = ["some_nested_app"]

where you provide the name of the application in question


***********************************************
Automatically restart server on apphook changes
***********************************************

As mentioned above, whenever you:

* add or remove an apphook
* change the slug of a page containing an apphook
* change the slug of a page with a descendant with an apphook

The CMS the server will reload its URL caches. It does this by listening for
the signal ``cms.signals.urls_need_reloading``.

.. warning::

    This signal does not actually do anything itself. For automated server
    restarting you need to implement logic in your project that gets executed
    whenever this signal is fired. Because there are many ways of deploying
    Django applications, there is no way we can provide a generic solution for
    this problem that will always work.

    The signal is fired **after** a request - for example, upon saving a page's settings. If you
    change and apphook's setting via an API the signal won't fire until a subsequent request.


**************************************
Apphooks and placeholder template tags
**************************************

It's important to understand that while an apphooked application takes over the CMS page at that
location completely, depending on how the application's templates extend other templates, a
django CMS ``{% placeholder %}`` template tag may be invoked - **but will not work**.

``{% static_placeholder %}`` tags on the other hand are *not* page-specific and *will* function
normally.

.. _complex_apphooks_how_to:

###########################################
How to manage complex apphook configuration
###########################################

In :ref:`apphooks_how_to` we discuss some basic points of using apphooks. In this document we will cover some more
complex implementation possibilities.


.. _multi_apphook:

***************************************
Attaching an application multiple times
***************************************

Define a namespace at class-level
=================================

If you want to attach an application multiple times to different pages, then the class defining the apphook *must*
have an ``app_name`` attribute::

    class MyApphook(CMSApp):
        name = _("My Apphook")
        app_name = "myapp"

        def get_urls(self, page=None, language=None, **kwargs):
            return ["myapp.urls"]

The ``app_name`` does three key things:

* It provides the *fallback namespace* for views and templates that reverse URLs.
* It exposes the *Application instance name* field in the page admin when applying an apphook.
* It sets the *default apphook instance name* (which you'll see in the *Application instance name* field).

We'll explain these with an example. Let's suppose that your application's views or templates use
``reverse('myapp:index')`` or ``{% url 'myapp:index' %}``.

In this case the namespace of any apphooks you apply must match ``myapp``. If they don't, your pages using them will
throw up a ``NoReverseMatch`` error.

You can set the namespace for the instance of the apphook in the *Application instance name* field. However, you'll
need to set that to something *different* if an instance with that value already exists. In this case, as long as
``app_name = "myapp"`` it doesn't matter; even if the system doesn't find a match with the name of the instance it will
fall back to the one hard-wired into the class.

In other words setting ``app_name`` correctly guarantees that URL-reversing will work, because it sets the fallback
namespace appropriately.


Set a namespace at instance-level
=================================

On the other hand, the *Application instance name* will override the ``app_name`` *if* a match is found.

This arrangement allows you to use multiple application instances and namespaces if that flexibility is required, while
guaranteeing a simple way to make it work when it's not.

Django's :ref:`django:topics-http-reversing-url-namespaces` documentation provides more information on how this works,
but the simplified version is:

1. First, it'll try to find a match for the *Application instance name*.
2. If it fails, it will try to find a match for the ``app_name``.


.. _apphook_configurations:

**********************
Apphook configurations
**********************

Namespacing your apphooks also makes it possible to manage additional database-stored apphook configuration, on an
instance-by-instance basis.


Basic concepts
==============

To capture the configuration that different instances of an apphook can take, a Django model needs to be created - each
apphook instance will be an instance of that model, and administered through the Django admin in the usual way.

Once set up, an apphook configuration can be applied to to an apphook instance, in the *Advanced settings* of the page
the apphook instance belongs to:

.. image:: /images/select_apphook_configuration.png
   :alt: selecting an apphook configuration application
   :width: 400
   :align: center

The configuration is then loaded in the application's views for that namespace, and will be used to determined how it
behaves.

Creating an application configuration in fact creates an apphook instance namespace. Once created, the namespace of a
configuration cannot be changed - if a different namespace is required, a new configuration will also need to be
created.


********************************
An example apphook configuration
********************************

In order to illustrate how this all works, we'll create a new FAQ application, that provides a simple list
of questions and answers, together with an apphook class and an apphook configuration model that allows it to
exist in multiple places on the site in multiple configurations.

We'll assume that you have a working django CMS project running already.

Using helper applications
=========================

We'll use a couple of simple helper applications for this example, just to make our work easier.


Aldryn Apphooks Config
----------------------

`Aldryn Apphooks Config <https://github.com/aldryn/aldryn-apphooks-config>`_ is a helper application that makes it
easier to develop configurable apphooks. For example, it provides an ``AppHookConfig`` for you to subclass, and other
useful components to save you time.

In this example, we'll use Aldryn Apphooks Config, as we recommend it. However, you don't have to use it in your own
projects; if you prefer to can build the code you require by hand.

Use ``pip install aldryn-apphooks-config`` to install it.

Aldryn Apphooks Config in turn installs `Django AppData <https://github.com/ella/django-appdata>`_, which provides an
elegant way for an application to extend another; we'll make use of this too.


Create the new FAQ application
==============================

.. code-block:: shell

    python manage.py startapp faq


Create the FAQ ``Entry`` model
------------------------------

``models.py``:

.. code-block:: python

    from aldryn_apphooks_config.fields import AppHookConfigField
    from aldryn_apphooks_config.managers import AppHookConfigManager
    from django.db import models
    from faq.cms_appconfig import FaqConfig


    class Entry(models.Model):
        app_config = AppHookConfigField(FaqConfig)
        question = models.TextField(blank=True, default='')
        answer = models.TextField()

        objects = AppHookConfigManager()

        def __unicode__(self):
            return self.question

        class Meta:
            verbose_name_plural = 'entries'

The ``app_config`` field is a ``ForeignKey`` to an apphook configuration model; we'll create it in a moment. This model
will hold the specific namespace configuration, and makes it possible to assign each FAQ Entry to a namespace.

The custom ``AppHookConfigManager`` is there to make it easy to filter the queryset of ``Entries`` using a convenient
shortcut: ``Entry.objects.namespace('foobar')``.


Define the AppHookConfig subclass
---------------------------------

In a new file ``cms_appconfig.py`` in the FAQ application:

.. code-block:: python

    from aldryn_apphooks_config.models import AppHookConfig
    from aldryn_apphooks_config.utils import setup_config
    from app_data import AppDataForm
    from django.db import models
    from django import forms
    from django.utils.translation import gettext_lazy as _


    class FaqConfig(AppHookConfig):
        paginate_by = models.PositiveIntegerField(
            _('Paginate size'),
            blank=False,
            default=5,
        )


    class FaqConfigForm(AppDataForm):
        title = forms.CharField()
    setup_config(FaqConfigForm, FaqConfig)

The implementation *can* be left completely empty, as the minimal schema is already defined in
the abstract parent model provided by Aldryn Apphooks Config.

Here though we're defining an extra field on model, ``paginate_by``. We'll use it later
to control how many entries should be displayed per page.

We also set up a ``FaqConfigForm``, which uses ``AppDataForm`` to add a field to ``FaqConfig`` without actually
touching its model.

The title field could also just be a model field, like ``paginate_by``. But we're using the AppDataForm to demonstrate
this capability.


Define its admin properties
---------------------------

In ``admin.py`` we need to define all fields we'd like to display:

.. code-block:: python

    from django.contrib import admin
    from .cms_appconfig import FaqConfig
    from .models import Entry
    from aldryn_apphooks_config.admin import ModelAppHookConfig, BaseAppHookConfig


    class EntryAdmin(ModelAppHookConfig, admin.ModelAdmin):
        list_display = (
            'question',
            'answer',
            'app_config',
        )
        list_filter = (
            'app_config',
        )
    admin.site.register(Entry, EntryAdmin)


    class FaqConfigAdmin(BaseAppHookConfig, admin.ModelAdmin):
        def get_config_fields(self):
            return (
                'paginate_by',
                'config.title',
            )
    admin.site.register(FaqConfig, FaqConfigAdmin)

``get_config_fields`` defines the fields that should be displayed. Any fields
using the AppData forms need to be prefixed by ``config.``.


Define the apphook itself
-------------------------

Now let's create the apphook, and set it up with support for multiple instances. In ``cms_apps.py``:

.. code-block:: python

    from aldryn_apphooks_config.app_base import CMSConfigApp
    from cms.apphook_pool import apphook_pool
    from django.utils.translation import gettext_lazy as _
    from .cms_appconfig import FaqConfig


    @apphook_pool.register
    class FaqApp(CMSConfigApp):
        name = _("Faq App")
        app_name = "faq"
        app_config = FaqConfig

        def get_urls(self, page=None, language=None, **kwargs):
            return ["faq.urls"]


Define a list view for FAQ entries
----------------------------------

We have all the basics in place. Now we'll add a list view for the FAQ entries
that only displays entries for the currently used namespace. In ``views.py``:

.. code-block:: python

    from aldryn_apphooks_config.mixins import AppConfigMixin
    from django.views import generic
    from .models import Entry


    class IndexView(AppConfigMixin, generic.ListView):
        model = Entry
        template_name = 'faq/index.html'

        def get_queryset(self):
            qs = super().get_queryset()
            return qs.namespace(self.namespace)

        def get_paginate_by(self, queryset):
            try:
                return self.config.paginate_by
            except AttributeError:
                return 10


``AppConfigMixin`` saves you the work of setting any attributes in your view - it automatically sets, for the view
class instance:

* current namespace in ``self.namespace``
* namespace configuration (the instance of FaqConfig) in ``self.config``
* current application in the ``current_app parameter`` passed to the
  ``Response`` class

In this case we're filtering to only show entries assigned to the current
namespace in ``get_queryset``. ``qs.namespace``, thanks to the model manager we defined earlier, is the equivalent of
``qs.filter(app_config__namespace=self.namespace)``.

In ``get_paginate_by`` we use the value from our appconfig model.


Define a template
^^^^^^^^^^^^^^^^^

In ``faq/templates/faq/index.html``:

.. code-block:: html+django

    {% extends 'base.html' %}

    {% block content %}
        <h1>{{ view.config.title }}</h1>
        <p>Namespace: {{ view.namespace }}</p>
        <dl>
            {% for entry in object_list %}
                <dt>{{ entry.question }}</dt>
                <dd>{{ entry.answer }}</dd>
            {% endfor %}
        </dl>

        {% if is_paginated %}
            <div class="pagination">
                <span class="step-links">
                    {% if page_obj.has_previous %}
                        <a href="?page={{ page_obj.previous_page_number }}">previous</a>
                    {% else %}
                        previous
                    {% endif %}

                    <span class="current">
                        Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}.
                    </span>

                    {% if page_obj.has_next %}
                        <a href="?page={{ page_obj.next_page_number }}">next</a>
                    {% else %}
                        next
                    {% endif %}
                </span>
            </div>
        {% endif %}
    {% endblock %}


URLconf
^^^^^^^

``urls.py``:

.. code-block:: python

    from django.urls import re_path
    from . import views


    urlpatterns = [
        re_path(r'^$', views.IndexView.as_view(), name='index'),
    ]


Put it all together
===================

Finally, we add ``faq`` to ``INSTALLED_APPS``, then create and run migrations:

.. code-block:: shell

    python manage.py makemigrations faq
    python manage.py migrate faq

Now we should be all set.

Create two pages with the ``faq`` apphook (don't forget to publish them), with different namespaces and different
configurations. Also create some entries assigned to the two namespaces.

You can experiment with the different configured behaviours (in this case, only pagination is available), and the way
that different ``Entry`` instances can be associated with a specific apphook.
