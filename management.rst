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

To completely rebuild the search index from scratch, use ``rebuild_index`` instead::

    manage.py rebuild_index

It is recommended that you run one of these commands on a nightly basis via cron or similar
scheduling tool.

*********************
Updating translations
*********************

Romeu uses the Rosetta third-party app to manage translations in a way that is simple for
translators to access (available on the web at your site's URL plus ``/rosetta``). Changes made to 
the translations database will take effect once the server is restarted. However, new translation
strings (e.g., when you add a new ``{% trans %}`` block to a template) will not show up in Rosetta
until the ``makemessages`` command is issued::

    manage.py makemessages

This will scan all files in the project looking for translatable strings and build a gettext catalog
file (django.po) for Rosetta to operate on, preserving any translations which have already been
entered. As with search indexing, it is recommended that you schedule ``makemessages`` to run on a 
regular basis.

********************
Thumbnail management
********************

Sorl-thumbnail, the thumbnailing solution used by Romeu, has its own set of ``manage.py`` commands. To
"clean" the thumbnail cache (removing thumbnails for items that no longer exist), run::

    manage.py thumbnail cleanup

If incorrect thumbnails appear on the site, it may be preferable to completely clear the cache of
thumbnails and re-build it from scratch. This uses the ``clear`` command::

    manage.py thumbnail clear

.. warning::
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

1. Install Sphinx. Assuming you already have ``pip`` set up, run::

    pip install Sphinx

2. Clone the documentation repository to your computer using Git::

    git clone git://github.com/umdsp/romeu_docs.git

3. Make changes as needed. If you are using this project as a basis for your own documentation
   project, be sure to update the settings in ``conf.py`` to match your organization's information.

4. Build the documentation. To create the HTML version of the docs in the ``docs`` subdirectory of your
   home directory, use the following::

    sphinx-build -b html . ~/docs

   .. tip::
      If you are compiling the documentation on the same server which will be serving the HTML
      documentation, you can substitute ``~/docs`` with the directory where your served files are stored.
   
   Sphinx will only write changed files to disk, leaving the rest as-is; to override this behavior
   and re-compile all documentation from scratch, add the ``-a`` option to ``sphinx-build``. Note that
   you will need to run ``sphinx-build`` every time you modify a ``.rst`` file.

For more information on Sphinx, including a detailed description of the reStructuredText format,
please visit the `Sphinx documentation site <http://sphinx.pocoo.org/index.html>`_.
