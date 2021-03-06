==================
Django Blog Zinnia
==================

Simple yet powerful application for managing a blog within your Django website.

Zinnia has been made for publishing weblog entries and is designed to do it well.

Basically any feature that can be provided by another reusable app has been left out. 
Why should we re-implement something that is already done and reviewed by others and tested.

.. contents::

Features
========

Main features :

  * Comments
  * Sitemaps
  * RSS Feeds
  * Search engine
  * Archives views
  * Related entries
  * Tags and categories views
  * Prepublication and expiration
  * Widgets (Popular entries, Similar entries, ...)
  * Spam protection with Akismet
  * Ping Directories
  * Bit.ly support
  * Twitter support
  * Gravatar support
  * WYMeditor support
  * Django-cms plugins
  * Collaborative work
  * Tags autocompletion
  * Ready to use and extendables templates

Dependancies
============

Make sure to install these packages prior to installation :

 * akismet
 * django-tagging

Installation
============

You could retrieve the last sources from http://github.com/Fantomas42/django-blog-zinnia and run the installation script ::

  $> python setup.py install

or use pip ::

  $> pip install -e git://github.com/Fantomas42/django-blog-zinnia.git#egg=django-blog-zinnia

For the latest stable version use easy_install ::

  $> easy_install django-blog-zinnia

Applications
------------

Then register **zinnia**, and these following applications in the INSTALLED_APPS section of your project's settings. ::

  >>> INSTALLED_APPS = (
  ...   # Your favorites apps
  ...   'django.contrib.contenttypes',
  ...   'django.contrib.comments',
  ...   'django.contrib.sites',
  ...   'django.contrib.admin',
  ...   'tagging',
  ...   'zinnia',)

Template Context Processors
---------------------------

Add these following template context processors if not already present. ::

  >>> TEMPLATE_CONTEXT_PROCESSORS = (
  ...      'django.core.context_processors.auth',
  ...      'django.core.context_processors.i18n',
  ...      'django.core.context_processors.request',
  ...      'django.core.context_processors.media',
  ...      'zinnia.context_processors.media',
  ...	)

Media Files
-----------

You have to make a symbolic link from zinnia/media/zinnia directory to your media directory or make a copy named **zinnia**,
but if want to change this value, define ZINNIA_MEDIA_URL in the settings.py as appropriate.

And don't forget to serve this url.

Urls
----

Add the following lines to your project's urls.py in order to display the blog. ::

  >>> url(r'^weblog/', include('zinnia.urls')),
  >>> url(r'^comments/', include('django.contrib.comments.urls')),


Note that the default zinnia urlset is provided for convenient usage, but you can customize your urls if you want. Here's how : ::

  >>> url(r'^weblog/feeds/', include('zinnia.urls.feeds')),
  >>> url(r'^weblog/authors/', include('zinnia.urls.authors')),
  >>> url(r'^weblog/categories/', include('zinnia.urls.categories')),
  >>> url(r'^weblog/search/', include('zinnia.urls.search')),
  >>> url(r'^weblog/', include('zinnia.urls.entries')),
  >>> url(r'^comments/', include('django.contrib.comments.urls')),

Advanced Configuration
======================

Sitemap
-------

One of the cool features of Django is the sitemap application,
so if you want to fill your website's sitemap with the entries of your blog, follow these steps.

  * Register **django.contrib.sitemaps** in the INSTALLED_APPS section.
  * Edit your project's urls and add this code :

::

  >>> from zinnia.sitemaps import TagSitemap
  >>> from zinnia.sitemaps import EntrySitemap
  >>> from zinnia.sitemaps import CategorySitemap
  >>> from zinnia.sitemaps import AuthorSitemap
  >>>
  >>> sitemaps = {'tags': TagSitemap,
  ...             'blog': EntrySitemap,
  ...             'authors': AuthorSitemap,
  ...             'categories': CategorySitemap,}
  ...
  >>> urlpatterns += patterns('django.contrib.sitemaps.views',
  ... 	                      (r'^sitemap.xml$', 'index',
  ...                          {'sitemaps': sitemaps}),
  ...                         (r'^sitemap-(?P<section>.+)\.xml$', 'sitemap',
  ...                          {'sitemaps': sitemaps}),
  ...			      )


Akismet
-------

By default the Akismet spam protection is enabled when anyone leaves a comment.

IMPORTANT : you need an API key. If you don't have any, get one for free at http://akismet.com/personal/ then set it in your project's settings like this : ::

  >>> AKISMET_API_KEY = 'your key'

If you don't want spam protection for comments, you can disable it with this setting. ::

  >>> ZINNIA_AKISMET_COMMENT = False

Bit.ly
------

You find `Bit.ly
<http://bit.ly>`_ useful and want to use it for your blog entries ?

It's simple, install `django_bitly
<http://bitbucket.org/discovery/django-bitly/>`_ in your project's settings and add these settings. ::

  >>> BITLY_LOGIN = 'your bit.ly login'
  >>> BITLY_API_KEY = 'your bit.ly api key'

Zinnia will do the rest.

Twitter
-------

When you post a new entry on your blog you might want to tweet it as well.

In order to do that, you first need to activate the Bit.ly support like described above.

Then install `python-twitter
<http://code.google.com/p/python-twitter/>`_ and add these settings. ::

  >>> TWITTER_USER = 'your twitter username'
  >>> TWITTER_PASSWORD = 'your twitter password'

Now in admin, you have the possibilty to post an update containing your entry's title and
the shortened url of your entry.

Django-CMS
----------

If you use `django-cms
<http://www.django-cms.org/>`_, Zinnia can be integrated into your pages, thanks to the plugin system.

Simply register **zinnia.plugins** in the INSTALLED_APPS section of your project's settings.

Templatetags
============

Zinnia provides several templatetags to create some **widgets** in your website's templates.

* get_recent_entries [number]

Display the latest entries.

* get_random_entries [number]

Display random entries.

* get_popular_entries [number]

Display popular entries.

* get_similar_entries [number]

Display entries similar to an existing entry.

* get_categories

Display all the categories available.

* get_archives_entries

Display the archives by month.

* get_link_archives_entries

Display link markups for listing the archives.

Development
===========

A `Buildout 
<http://pypi.python.org/pypi/zc.buildout>`_ script is provided to properly initialize the project 
for anybody who wants to contribute to the project.

First of all, please use `VirtualEnv
<http://pypi.python.org/pypi/virtualenv>`_ to protect your system.

Follow these steps to start the development : ::

  $> git clone git://github.com/Fantomas42/django-blog-zinnia.git
  $> virtualenv --no-site-packages django-blog-zinnia
  $> cd django-blog-zinnia
  $> source ./bin/activate
  $> python bootstrap.py
  $> ./bin/buildout

The buildout script will resolve all the dependancies needed to develop the application.

Once these operations are done, you are ready to develop the zinnia project.

Run this command to launch the tests. ::

  $> ./bin/test

Pretty easy no ?

Translations
============

If you want to contribute by updating a translation or adding a translation in your language,
it's simple, create a account on Transifex.net and you will have the possibility to edit the translations at this url :

http://www.transifex.net/projects/p/django-blog-zinnia/c/master/


Examples
========

  * `Fantomas' side
    <http://fantomas.willbreak.it>`_.

If you are a proud user of Zinnia, send me the url of your website and I will add it to the list.
