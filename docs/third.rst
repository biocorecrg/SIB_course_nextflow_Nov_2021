.. _third-page:

*******************
Third Day
*******************

Modify Pages to allow uploading an image as decoration (like News Items do).

.. container:: toggle

    .. container:: header

        **Show/Hide Code**

    .. code-block:: xml
       :linenos:

       from plone import api
       ...


```{admonition} Solution
:class: toggle

- Go to the dexterity control panel (<http://localhost:8080/Plone/@@dexterity-types>)
- Click on *Page* (<http://127.0.0.1:8080/Plone/dexterity-types/Document>)
- Select the tab *Behaviors* (<http://127.0.0.1:8080/Plone/dexterity-types/Document/@@behaviors>)
- Check the box next to {guilabel}`Lead Image` and save.

The images are displayed above the title.
```

..  admonition:: Solution
        :class: toggle
        * Go to the dexterity-controlpanel (http://localhost:8080/Plone/@@dexterity-types)
        * Click on *Page* (http://127.0.0.1:8080/Plone/dexterity-types/Document)
        * Select the tab *Behaviors* (http://127.0.0.1:8080/Plone/dexterity-types/Document/@@behaviors)
        * Check the box next to *Lead Image* and save.
