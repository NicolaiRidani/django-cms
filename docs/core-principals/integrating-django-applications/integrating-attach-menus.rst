.. _core-principals-integrating-django-applications-integrating-attach-menus:

************************
Integrating Attach Menus
************************

Classes that extend from :class:`menus.base.Menu` always get attached to the
root. But if you want the menu to be attached to a CMS Page you can do that as
well.

Instead of extending from :class:`~menus.base.Menu` you need to extend from
:class:`cms.menu_bases.CMSAttachMenu` and you need to define a name.

We will do that with the example from above::

    from menus.base import NavigationNode
    from menus.menu_pool import menu_pool
    from django.utils.translation import gettext_lazy as _
    from cms.menu_bases import CMSAttachMenu

    class TestMenu(CMSAttachMenu):

        name = _("test menu")

        def get_nodes(self, request):
            nodes = []
            n = NavigationNode(_('sample root page'), "/", 1)
            n2 = NavigationNode(_('sample settings page'), "/bye/", 2)
            n3 = NavigationNode(_('sample account page'), "/hello/", 3)
            n4 = NavigationNode(_('sample my profile page'), "/hello/world/", 4, 3)
            nodes.append(n)
            nodes.append(n2)
            nodes.append(n3)
            nodes.append(n4)
            return nodes

    menu_pool.register_menu(TestMenu)

Now you can link this Menu to a page in the *Advanced* tab of the page
settings under attached menu.
