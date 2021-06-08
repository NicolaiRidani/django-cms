
.. _apphooks:

########
Apphooks
########

Right now, our Django Polls application is statically hooked into the project's
``urls.py``. This is all right, but we can do more, by attaching applications to
django CMS pages.


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

.. image:: /introduction/images/select-application.png
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




Application hooks ("apphooks")
==============================

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

The *Advanced settings* of a CMS page provides an *Application* field. :ref:`Adding an apphook class <apphooks_how_to>` to the
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

An :ref:`apphook configuration <apphook_configurations>` class allows the site editors to create multiple configuration
instances that specify the behaviour. The kind of configuration available is presented in an admin form, and determined by the
application developer.

..  important::

    It's important to understand that an apphook (and therefore also an apphook configuration)
    serves no function until it is attached to a page - and until the page is **published**, the
    application will be unable to fulfil any publishing function.

    Also note that the apphook "swallows" all URLs below that of the page, handing them over to the
    attached application. If you have any child pages of the apphooked page, django CMS will not be
    able to serve them reliably.




