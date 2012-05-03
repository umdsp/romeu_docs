#################
Server management
#################

These procedures will help to ensure that your Romeu install is operating smoothly.

**********************
Search engine indexing
**********************

The search system in Romeu will only update its index manually. The following command will update
the search index::

    manage.py update_index

To completely rebuild the search index from scratch, use `rebuild_index` instead::

    manage.py rebuild_index

It is recommended that you run one of these commands on a nightly basis via cron or similar
scheduling tool.

*********************
Updating translations
*********************

Romeu uses the Rosetta third-party app to manage translations in a way that is simple for
translators to access (available on the web at your site's URL plus `/rosetta`). Changes made to 
the translations database will take effect once the server is restarted. However, new translation
strings (e.g., when you add a new `{% trans %}` block to a template) will not show up in Rosetta
until the `makemessages` command is issued::

    manage.py makemessages

This will scan all files in the project looking for translatable strings and build a gettext catalog
file (django.po) for Rosetta to operate on, preserving any translations which have already been
entered. As with search indexing, it is recommended that you schedule `makemessages` to run on a 
regular basis.

********************
Thumbnail management
********************

Sorl-thumbnail, the thumbnailing solution used by Romeu, has its own set of `manage.py` commands. To
"clean" the thumbnail cache (removing thumbnails for items that no longer exist), run::

    manage.py thumbnail cleanup

If incorrect thumbnails appear on the site, it may be preferable to completely clear the cache of
thumbnails and re-build it from scratch. This uses the `clear` command::

    manage.py thumbnail clear

.. note::
   Clearing the thumbnail cache will cause all thumbnails to be regenerated from scratch as needed.
   While this generally will not tax the system, attempts to access list pages with large numbers of
   images, like the digital objects list views, may use a large amount of server processing power
   and may result in timeout errors while the cache is rebuilt. It is best to clear the thumbnail
   cache during non-peak hours and to run some form of crawler to force-rebuild the cache.

.. todo::
   Write a script that rebuilds the cache, either by crawling web pages or by using the thumbnail
   API in a Python shell.

*************************
Documentation with Sphinx
*************************

This document is written with Sphinx, a documentation system that makes use of reStructuredText
(.rst) files to generate a variety of outputs. To update or extend this documentation, use the
following procedure.

1. Clone the documentation repository to your computer using Git.::

    git clone 
