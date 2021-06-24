..  _about:

#############
About Plugins
#############

CMS Plugins are reusable content publishers that can be inserted into django
CMS pages (or indeed into any content that uses django CMS placeholders *need link to placeholders*). They
enable the publishing of information automatically, without further
intervention.

A plugin is the most convenient way to integrate content from another Django
application into a django CMS page.

This means that your published web content, whatever it is, is kept
up-to-date at all times.

Unless you're lucky enough to discover that your needs can be met by the
built-in plugins, or by the many available third-party plugins, you'll have to
write your own custom CMS Plugin. *need link to create article*

**************
When to use it
**************

Suppose you're developing a site for a record company in django
CMS. You might like to have a "Latest releases" box on your site's home page.

Of course, you could every so often edit that page and update the information.
However, a sensible record company will manage its catalogue in Django too,
which means Django already knows what this week's new releases are.

This is an excellent opportunity to make use of that information to make your
life easier - all you need to do is create a django CMS plugin that you can
insert into your home page, and leave it to do the work of publishing information
about the latest releases for you.

Plugins are **reusable**. Perhaps your record company is producing a series of
reissues of seminal Swiss punk records; on your site's page about the series,
you could insert the same plugin, configured a little differently, that will
publish information about recent new releases in that series.

************
How it works
************

Components of a plugin
======================

A django CMS plugin is fundamentally composed of three components, that correspond to Django's
familiar Model-View-Template scheme:

===================  ================================  ========================================================
What                 Function                          Subclass of
===================  ================================  ========================================================
model (if required)  plugin instance configuration     :class:`CMSPlugin <cms.models.pluginmodel.CMSPlugin>`
view                 display logic                     :class:`CMSPluginBase <cms.plugin_base.CMSPluginBase>`
template             rendering                         --
===================  ================================  ========================================================


:class:`CMSPluginBase <cms.plugin_base.CMSPluginBase>`
======================================================

The plugin **model**, the sub-class of :class:`cms.models.pluginmodel.CMSPlugin`,
is optional.

You could have a plugin that doesn't need to be configured, because it only
ever does one thing.

For example, you could have a plugin that only publishes information
about the top-selling record of the past seven days. Obviously, this wouldn't
be very flexible - you wouldn't be able to use the same plugin for the
best-selling release of the last *month* instead.

Usually, you find that it is useful to be able to configure your plugin, and this
will require a model.


:class:`CMSPlugin <cms.models.pluginmodel.CMSPlugin>`
==================================================================

:class:`cms.plugin_base.CMSPluginBase` is actually a sub-class of
:class:`django:django.contrib.admin.ModelAdmin`.

Because :class:`~cms.plugin_base.CMSPluginBase` sub-classes ``ModelAdmin`` several important
``ModelAdmin`` options are also available to CMS plugin developers. These
options are often used:

* ``exclude``
* ``fields``
* ``fieldsets``
* ``form``
* ``formfield_overrides``
* ``inlines``
* ``radio_fields``
* ``raw_id_fields``
* ``readonly_fields``

Please note, however, that not all ``ModelAdmin`` options are effective in a CMS
plugin. In particular, any options that are used exclusively by the
``ModelAdmin``'s ``changelist`` will have no effect. These and other notable options
that are ignored by the CMS are:

* ``actions``
* ``actions_on_top``
* ``actions_on_bottom``
* ``actions_selection_counter``
* ``date_hierarchy``
* ``list_display``
* ``list_display_links``
* ``list_editable``
* ``list_filter``
* ``list_max_show_all``
* ``list_per_page``
* ``ordering``
* ``paginator``
* ``prepopulated_fields``
* ``preserve_fields``
* ``save_as``
* ``save_on_top``
* ``search_fields``
* ``show_full_result_count``
* ``view_on_site``


..  _common-plugins:

**************************
Some commonly-used plugins
**************************

.. warning::
    In version 3 of the CMS we removed all the plugins from the main repository
    into separate repositories to continue their development there.
    you are upgrading from a previous version. Please refer to
    :ref:`Upgrading from previous versions <upgrade-to-3.0>`


Please note that dozens if not hundreds of different django CMS plugins have been made available
under open-source licences. Some, like the ones on this page, are likely to be of general interest,
while others are highly specialised.

This page only lists those that fall under the responsibility of the django CMS project. Please see
the `Django Packages <https://djangopackages.org/search/?q=django+cms>`_ site for some more, or
just do a web search for the functionality you seek - you'll be surprised at the range of plugins
that has been created.



django CMS Core Addons
======================

We maintain a set of *Core Addons* for django CMS.

You don't need to use them, and for many of them alternatives exist, but they represent a good way
to get started with a reliable project set-up. We recommend them for new users of django CMS in
particular. For example, if you start a project on `Divio Cloud <https://divio.com/>`_ or using the
`django CMS installer <https://github.com/nephila/djangocms-installer>`_, this is the set of addons
you'll have installed by default.

The django CMS Core Addons are:

* `Django Filer <http://github.com/divio/django-filer>`_ - a file management application for
  images and other documents.
* `django CMS Admin Style <https://github.com/django-cms/djangocms-admin-style>`_ - a CSS theme for the
  Django admin
* `django CMS Text CKEditor <https://github.com/django-cms/djangocms-text-ckeditor>`_ - our default rich
  text WYSIYG editor
* `django CMS Link <https://github.com/django-cms/djangocms-link>`_ - add links to content
* `django CMS Picture <https://github.com/django-cms/djangocms-picture>`_ - add images to your site
  (Filer-compatible)
* `django CMS File <https://github.com/django-cms/djangocms-file>`_ - add files or an entire folder to
  your pages (Filer-compatible)
* `django CMS Style <https://github.com/django-cms/djangocms-style>`_ - create HTML containers with
  classes, styles, ids and other attributes
* `django CMS Snippet <https://github.com/django-cms/djangocms-snippet>`_ - insert arbitrary HTML content
* `django CMS Audio <https://github.com/django-cms/djangocms-audio>`_ - publish audio files
  (Filer-compatible)
* `django CMS Video <https://github.com/django-cms/djangocms-video>`_ - embed videos from YouTube, Vimeo
  and other services, or use uploaded videos (Filer-compatible)
* `django CMS GoogleMap <http://github.com/django-cms/djangocms-googlemap>`_ - displays a map of an
  address on your page. Supports addresses and co-ordinates. Zoom level and route planner options
  can also be set.

We welcome feedback, documentation, patches and any other help to maintain and improve these
valuable components.


Other addons of note
====================

These packages are no longer officially guaranteed support by the django CMS project, but they have
good community support.

* `django CMS Inherit <https://github.com/divio/djangocms-inherit>`_ - renders the plugins from a
  specified page (and language) in its place
* `django CMS Column <https://github.com/divio/djangocms-column>`_ - layout page content in columns
* `django CMS Teaser <http://github.com/divio/djangocms-teaser>`_ - displays a teaser box for
  another page or a URL, complete with picture and a description

..  _deprecated:

**********
Deprecated
**********

Some older plugins that you may have encountered are now deprecated and we advise against
incorporating them into new projects.

These are:

* `cmsplugin-filer <https://github.com/divio/cmsplugin-filer>`_
* `Aldryn Style <https://github.com/aldryn/aldryn-style>`_
* `Aldryn Locations <https://github.com/aldryn/aldryn-locations>`_
* `Aldryn Snippet <https://github.com/aldryn/aldryn-snippet>`_


