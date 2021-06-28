.. _core-principals-plugins-introduction-how-it-works:

############
How It Works
############

**********************
Components of a plugin
**********************

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
