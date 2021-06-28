.. _core-principals-plugins-introduction-when-to-use-it:

##############
When To Use It
##############

..  seealso::

    * :ref:`Articles about custom plugins <core-principals-plugins-custom-plugins>`

CMS Plugins are reusable content publishers that can be inserted into django
CMS pages (or indeed into any content that uses django CMS placeholders). They
enable the publishing of information automatically, without further
intervention.

This means that your published web content, whatever it is, is kept
up-to-date at all times.

It's like magic, but quicker.

Unless you're lucky enough to discover that your needs can be met by the
built-in plugins, or by the many available third-party plugins, you'll have to
write your own custom CMS Plugin.


***************************************
*Why* would you need to write a plugin?
***************************************

A plugin is the most convenient way to integrate content from another Django
application into a django CMS page.

For example, suppose you're developing a site for a record company in django
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
