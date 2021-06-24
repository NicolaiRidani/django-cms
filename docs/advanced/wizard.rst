.. _wizards:

#######
Wizards
#######

.. _content-wizard:

************************
Content creation wizards
************************

Content creation wizards allow you to make use of the toolbar's **Create** button in your own
applications. It opens up a simple dialog box with the basic fields required to create a new item.

django CMS uses it for creating Pages, but you can add your own models to it.

In the ``polls_cms_integration`` application, add a ``forms.py`` file::

    from django import forms

    from polls.models import Poll


    class PollWizardForm(forms.ModelForm):
        class Meta:
            model = Poll
            exclude = []

Then add a ``cms_wizards.py`` file, containing::

    from cms.wizards.wizard_base import Wizard
    from cms.wizards.wizard_pool import wizard_pool

    from polls_cms_integration.forms import PollWizardForm


    class PollWizard(Wizard):
        pass

    poll_wizard = PollWizard(
        title="Poll",
        weight=200,  # determines the ordering of wizards in the Create dialog
        form=PollWizardForm,
        description="Create a new Poll",
    )

    wizard_pool.register(poll_wizard)

Refresh the Polls page, hit the **Create** button in the toolbar - and the wizard dialog will open,
offering you a new wizard for creating Polls.

.. note::

    Once again, this particular example is for illustration only. In the case of a Poll, with
    its multiple Questions associated with it via foreign keys, we really want to be able to
    edit those questions at the same time too.

    That would require a much more sophisticated form and processing than is possible within the
    scope of this tutorial.

.. _create-wizard:

.. versionadded:: 3.2


*****************************************
How to implement content creation wizards
*****************************************

django CMS offers a framework for creating 'wizards' - helpers - for content editors.

They provide a simplified workflow for common tasks.

A django CMS Page wizard already exists, but you can create your own for other content types very easily.



Create a content-creation wizard
================================

Creating a CMS content creation wizard for your own module is fairly easy.

To begin, create a file in the root level of your module called ``forms.py``
to create your form(s)::

    # my_apps/forms.py

    from django import forms

    class MyAppWizardForm(forms.ModelForm):
        class Meta:
            model = MyApp
            exclude = []

Now create another file in the root level called ``cms_wizards.py``.
In this file, import ``Wizard`` as follows::

    from cms.wizards.wizard_base import Wizard
    from cms.wizards.wizard_pool import wizard_pool

Then, simply subclass ``Wizard``, instantiate it, then register it. If you were to
do this for ``MyApp``, it might look like this::


    # my_apps/cms_wizards.py

    from cms.wizards.wizard_base import Wizard
    from cms.wizards.wizard_pool import wizard_pool

    from .forms import MyAppWizardForm

    class MyAppWizard(Wizard):
        pass

    my_app_wizard = MyAppWizard(
        title="New MyApp",
        weight=200,
        form=MyAppWizardForm,
        description="Create a new MyApp instance",
    )

    wizard_pool.register(my_app_wizard)

.. note::

    If your model doesn't define a ``get_absolute_url`` function then your wizard
    will require a :ref:`get_success_url` method.

    ..  code-block:: python

        class MyAppWizard(Wizard):

            def get_success_url(self, obj, **kwargs):
                """
                This should return the URL of the created object, «obj».
                """
                if 'language' in kwargs:
                    with force_language(kwargs['language']):
                        url = obj.get_absolute_url()
                else:
                    url = obj.get_absolute_url()

                return url

That's it!

.. note::

    The module name ``cms_wizards`` is special, in that any such-named modules in
    your project's Python path will automatically be loaded, triggering the
    registration of any wizards found in them. Wizards may be declared and
    registered in other modules, but they might not be automatically loaded.

The above example is using a ``ModelForm``, but you can also use ``forms.Form``.
In this case, you **must** provide the model class as another keyword argument
when you instantiate the Wizard object.

For example::

    # my_apps/forms.py

    from django import forms

    class MyAppWizardForm(forms.Form):
        name = forms.CharField()


    # my_apps/cms_wizards.py

    from cms.wizards.wizard_base import Wizard
    from cms.wizards.wizard_pool import wizard_pool

    from .forms import MyAppWizardForm
    from .models import MyApp

    class MyAppWizard(Wizard):
        pass

    my_app_wizard = MyAppWizard(
        title="New MyApp",
        weight=200,
        form=MyAppWizardForm,
        model=MyApp,
        description="Create a new MyApp instance",
    )

    wizard_pool.register(my_app_wizard)

You must subclass ``cms.wizards.wizard_base.Wizard`` to use it. This is because
each wizard's uniqueness is determined by its class and module name.

See the :ref:`Reference section on wizards <wizard_reference>` for technical details of the wizards
API.


