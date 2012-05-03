#######################
Installation and setup
#######################

To install Romeu, you will need the following:

* Python (minimum version 2.6; 2.7 recommended; 3.x *not supported*)
* Django and a number of third-party Django apps
* A database (SQLite, MySQL, PostgreSQL, or Oracle)
* A WSGI-compliant web server. In development, you may use the built-in "runserver" environment.

.. note::
   For more information on installing Django in general, please consult the official Django
   documentation at https://docs.djangoproject.com/en/1.4/intro/install/.

**************************
Preparing your environment
**************************

Installing Django
=================

Most Linux distributions can install Django through their package management tools, allowing for
easy maintenance and upgrades. (As of this writing, the current version of Django is 1.4.) If you are not using Linux, or you do not wish to use a package
management tool like apt or rpm, the easiest way to install Django is using ``pip``::

    pip install Django

If your system does not have pip installed, you can get it using setuptools (bundled by default with
most Python installations)::

    easy_install pip

.. note::
   On Windows, you will need to install setuptools yourself. Visit
   http://pypi.python.org/pypi/setuptools/0.6c11 for more information.

   On Mac / Linux, if pip complains about lacking write access to certain directories, simply run
   the command with sudo: ``sudo pip install Django``. You will be prompted for your password.

   If Django is already installed on your system and you wish to upgrade to the latest version, add
   the ``--upgrade`` flag to the install command: ``pip install Django --upgrade``. Note that this
   will remove older versions of the package you are upgrading. The ``--upgrade`` flag can be used
   with any of the packages listed below as well.

Installing third-party apps
===========================

Romeu makes use of a number of third-party Django apps, all of which are available through `PyPI
<http://pypi.python.org>`_. As with Django itself, it is best to install these packages either
through your system's package management facilities or by using pip. Romeu uses the following apps:

* ``South``: Database migrations
* ``sorl-thumbnail``: Image manipulation and resizing
* ``django-dajax`` and ``django-dajaxice``: Interface to get admin data via AJAX/JSON
* ``django-smart-selects``: A tool to limit input choices based on another field. Used in the admin
  site.
* ``django-rosetta``: Simplified way to set up interface translations
* ``django-reversion``: Keeps a restorable history of all changes made through the admin site
* ``django-taggit``: Tagging system that can be applied to any model
* ``django-tinymce``: "WYSIWYG" editor for use in the admin site
* ``django-haystack``: Search middleware; CTDA uses Whoosh as its search engine backend
* ``Unidecode``: Translates between Unicode and ASCII text equivalents. Mainly used for searching.
* ``Whoosh``: A pure-Python, serverless search engine. May be replaced with Solr if needed.

You will also need to install Python bindings for your database of choice. SQLite is bundled as part
of the Python standard library since version 2.5, so it does not require any additional packages.
For PostgreSQL, install the ``psycopg2`` package, and for MySQL, install the ``MySQL-python``
package. For all other database types, consult the official Django documentation.

****************
Installing Romeu
****************

Get the source code
===================

Now that you have a Python/Django environment ready to go, it's time to install and set up Romeu. If
you have Git installed, installing Romeu is as easy as navigating to the parent directory you want
to install it in and typing::

    git clone git://github.com/umdsp/romeu.git

This will create a new directory, ``romeu``, containing the Romeu source code. If you do not have 
Git installed, you can alternatively download a .zip or .tar.gz archive of the
project's source code by visiting https://github.com/umdsp/romeu/downloads.

Configure Romeu
===============

After downloading Romeu, you will need to configure it for your server environment (this includes
using Romeu on your development machine). All server-specific configuration for Romeu is contained
in a ``local_settings.py`` file at the root Romeu directory. To begin configuring Romeu, rename the
``local_settings.py.example`` file to ``local_settings.py``.

.. todo::
   Clean up file paths (using ``os``) so people have less config to do, will get rid of most "must"
   commands below.

Within ``local_settings.py``, you will need to adjust the following entries to match your server
environment:

* ``ADMINS``: Enter your name and email address. This is where error reports, etc. will be sent. You
  may also enter more than one admin here; each admin will be notified when an error occurs.
* ``DEBUG`` / ``THUMBNAIL_DEBUG`` / ``TEMPLATE_DEBUG``: ``DEBUG`` is set to ``True``, and the rest 
  are set to ``False``, by default. Change these to ``True`` if you would like to see more detailed 
  debug information when an error occurs.

.. warning::
   Turning any of the ``DEBUG`` options to ``True`` will present technical details of your
   installation to end users and is likely to reduce site performance. Only use debug information on
   a private development server. In production deployments, set all ``DEBUG`` options to ``False``.

* ``DATABASES``: Under ``default``, enter the connection details for your database. Django supports
  PostgreSQL, MySQL, SQLite and Oracle without additional plugins. The included
  ``local_settings.py.example`` sets up an SQLite database; just change the database listed under
  ``NAME`` to an absolute path.

.. tip::
   If you are using SQLite, be sure to create a blank file matching the filename you entered under ``NAME``.
   Django will not create the database file for you. A simple ``touch`` command is all you need.

* ``MEDIA_ROOT`` / ``STATIC_ROOT``: These are directories on disk which will contain your media
  (uploaded files) and static (CSS / JavaScript) files. You **must** replace the values in
  ``local_settings.py.example`` with absolute paths.

.. note::
   If you wish to add additional directories of static content, use the ``STATICFILES_DIRS`` field.

* ``MEDIA_URL`` / ``STATIC_URL``: These represent the URLs at which media and static files,
  respectively, will be available on the Web. The default URLs are ``/media/`` and ``/static/``,
  which should work well unless you have a custom setup. If you change these values, be sure to end 
  each URL with a forward slash (``/``).
* ``SECRET_KEY``: Set this to a long string of random characters. The secret key will be used when
  storing user passwords in the database, and access to your site's secret key could potentially
  allow an attacker free access to the entire site.
* ``TEMPLATE_DIRS``: This is a list of directories in which Django can find templates for your site.
  Every time a request is made for a template, Django will search these directories in order and
  return the first match it finds. You **must** replace the relative path listed in
  ``local_settings.py.example`` with an absolute path, based on where you installed Romeu.
* ``HAYSTACK`` settings: These settings control the search system of Romeu. The defaults should
  generally work well, but **be sure to change** ``HAYSTACK_WHOOSH_PATH`` to an absolute path.

Once you are done making adjustments, save your new ``local_settings.py``.

Syncing the database
====================

Before you can begin to use Romeu, you must perform an initial database sync. To do this, navigate
to the main Romeu directory and type::

    python manage.py syncdb

This will analyze your models (located in the ``archive`` and ``workflow`` apps, as well as in some
of the third-party apps that Romeu uses) and create tables in your database as needed. It will also
prompt you to set up an admin user, who will have free access to all site functions in the admin
site.

.. note::
   If the above command does not work, make sure that ``python`` is the correct version of Python.
   If you have multiple installed versions of Python on your system, you may need to substitute
   something like ``python2.7`` for ``python``.

With the basic database structure in place, we will load some initial data which is required for the
site to operate::

    python manage.py loaddata startup.json

Database migrations
-------------------

When the installation is finished, you will see the following::

    Synced:
    > django.contrib.auth
    > django.contrib.contenttypes
    > django.contrib.sessions
    > django.contrib.sites
    > django.contrib.messages
    > django.contrib.flatpages
    > sorl.thumbnail
    > archive
    > workflow
    > modeltranslation
    > django.contrib.admin
    > ajax_select
    > selectable
    > dajaxice
    > dajax
    > rosetta
    > taggit
    > tinymce
    > haystack
    > south

    Not synced (use migrations):
    - 
    (use ./manage.py migrate to migrate these)

None of the models are currently being managed by South; we will change that by creating "initial"
migrations for the ``archive`` and ``workflow`` apps, to enable you to easily alter them later::

    python manage.py schemamigration archive --initial
    python manage.py schemamigration workflow --initial

    python manage.py migrate archive --fake
    python manage.py migrate workflow --fake

Whenever you make changes to the ``archive`` or ``workflow`` models, you can update your database
schema to match by using the following commands (using the ``archive`` app here as an example)::

    python manage.py schemamigration archive --auto
    python manage.py migrate archive

Installing the admin media
==========================

Before testing your Romeu install, we'll need to make sure that it can access Django's default
styles for the admin site. In ``local_settings.py``, you set a ``STATIC_ROOT`` and ``MEDIA_ROOT``
for your site. Make sure that these directories actually exist, and then copy Django's default admin
media into the directory you listed as ``STATIC_ROOT`` like so::

    python manage.py collectstatic

This will copy all static files from the various apps your site is using into the ``STATIC_ROOT``
directory. This includes the admin site's media, files for TinyMCE, and more.

*************
Testing Romeu
*************

Running the development server
==============================

With the system installed and configured, you are ready to begin using your new site. In the main
Romeu directory, start the development server by running::

    python manage.py runserver

This will start a development server at http://127.0.0.1:8000. Since there is no data in the
database yet, the home page will present you with ``Testing``. To change this,
log in to the admin site by going to http://127.0.0.1:8000/admin and entering the username and
password you created while performing ``syncdb``. Click on ``Static pages`` and select the page with
URL ``/``. Change the text here, save the page, and reload the home page to see your changes.

.. note::
   The ``runserver`` command will only serve static files (CSS, JavaScript, etc.) when ``DEBUG`` is
   set to ``True``. If ``DEBUG`` is not being used, you **must** provide your own means of serving
   static files (through a web server such as Apache, Nginx, etc.).

.. todo::
   In the initial fixture, add properly-configured flatpages and a Home Page Settings object.
   Also import the list of countries and cities - might be useful.
   Create a set of default templates with accompanying CSS/JS, so new users don't get CTDA branding
   with no static files.
