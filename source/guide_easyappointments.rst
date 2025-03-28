.. author:: Lukas Herzog <hallo@lukasherzog.de>

.. spelling:word-list::
    Tselegidis
    english
    french
    german

.. tag:: lang-php
.. tag:: web
.. tag:: calendar
.. tag:: scheduler

.. highlight:: console

.. sidebar:: Logo

  .. image:: _static/images/easyappointments.png
      :align: center

######################
Easy Appointments
######################

.. tag_list::

`Easy Appointments`_ is an open source web application for booking / scheduling appointments. It allows for many customization options and use-cases and provides optional sync to Google Calendar.

Easy Appointments was first released 2016 and is maintained by Alex Tselegidis. It is distributed under GPL 3.0.



----

.. note:: For this guide you should be familiar with the basic concepts of

  * PHP_
  * :manual:`MySQL <database-mysql>`
  * :manual:`domains <web-domains>`

Prerequisites
=============

.. include:: includes/my-print-defaults.rst

We suggest using an :manual_anchor:`additional <database-mysql.html#additional-databases>` database. Create one with:

.. code-block:: console

 [isabell@stardust ~]$ mysql -e "CREATE DATABASE ${USER}_ea"

We’re using :manual:`PHP <lang-php>` in version 8.1:

.. code-block:: console

 [isabell@stardust ~]$ uberspace tools version show php
 Using 'PHP' version: '8.1'
 [isabell@stardust ~]$

Your domain needs to be setup:

.. include:: includes/web-domain-list.rst


Installation
============

``cd`` into your :manual:`document root <web-documentroot>`, download the latest build, unzip and remove the zip file. Make sure to replace ``x.y.z`` with the latest version number.

.. code-block:: console

 [isabell@stardust ~]$ version=x.y.z
 [isabell@stardust ~]$ cd /var/www/virtual/$USER/html/
 [isabell@stardust html]$ wget https://github.com/alextselegidis/easyappointments/releases/download/$version/easyappointments-$version.zip
 [isabell@stardust html]$ unzip easyappointments-$version.zip
 [isabell@stardust html]$ rm easyappointments-$version.zip
 [isabell@stardust html]$ rm nocontent.html
 [isabell@stardust ~]$

Configuration
=============

To get started set some basic configuration variables. First, copy the sample configuration:

.. code-block:: console

 [isabell@stardust ~]$ cd /var/www/virtual/$USER/html/
 [isabell@stardust html]$ cp config-sample.php config.php
 [isabell@stardust html]$

Then, edit ``config.php``. In here you need to enter your base-URL, :manual_anchor:`MySQL credentials <database-mysql.html#login-credentials>` database connection parameters and the name of your database (e.g. ``isabell_ea``).

.. code-block:: ini
 :emphasize-lines: 4,12,13,14,15

 // GENERAL SETTINGS
 // ------------------------------------------------------------------------

 const BASE_URL      = 'https://isabell.uber.space';
 const LANGUAGE      = 'english';
 const DEBUG_MODE    = FALSE;

 // ------------------------------------------------------------------------
 // DATABASE SETTINGS
 // ------------------------------------------------------------------------

 const DB_HOST       = 'localhost';
 const DB_NAME       = 'isabell_ea';
 const DB_USERNAME   = 'isabell';
 const DB_PASSWORD   = 'MySuperSecretPassword';

Finishing installation
======================

.. code-block:: console

 [isabell@stardust ~]$ cd /var/www/virtual/$USER/html/
 [isabell@stardust html]$ php index.php console install
 [isabell@stardust ~]$

Alternatively, point your browser to your domain, ``https://isabell.uber.space/`` in this example, where you can set up admin credentials and basic settings for your intended usage.

Easy Appointments is now ready to use.

Tuning
======

Translations
------------

Easy Appointments' inbuilt language translations are not always perfect and you might want to change some wordings to better fit your needs. You can remove unneeded translations by removing them from the array in ``/var/www/virtual/$USER/html/application/config/config.php``, for instance:

.. code-block:: ini
 :emphasize-lines: 12,13,14

 /*
 |--------------------------------------------------------------------------
 | Available Languages
 |--------------------------------------------------------------------------
 |
 | Each item of this array must be a directory with the translation files in
 | the /application/language directory. The users will be able to select one
 | of these languages.
 |
 */
 $config['available_languages'] = [
  'english',
  'french',
  'german'
 ];

You can also add a new translation by copying and renaming a translation-folder in ``/var/www/virtual/$USER/html/application/language/`` and adding its name to the array above.

You can modify an existing translation by editing the ``translation_lang.php`` file in the desired language folder. For English this would be ``/var/www/virtual/$USER/html/application/language/english/translation_lang.php``

.. note:: The `official documentation`_ contains additional information on translation and other topics.

Updates
=======

.. note:: Check the update feed_ regularly to stay informed about the newest version.

.. warning:: Backup all your files and the database before updating!

Download the new version, unzip it and overwrite all the files in your :manual:`document root <web-documentroot>` except ``config.php``.

.. code-block:: console

 [isabell@stardust ~]$ version=x.y.z
 [isabell@stardust ~]$ cd /var/www/virtual/$USER/html/
 [isabell@stardust html]$ wget https://github.com/alextselegidis/easyappointments/releases/download/$version/easyappointments-$version.zip
 [isabell@stardust html]$ unzip easyappointments-$version.zip
 [isabell@stardust html]$ rm easyappointments-$version.zip
 [isabell@stardust ~]$

If updating from version ``1.3.x`` also remove ``/system`` and ``/autoload.php``

.. code-block:: console

 [isabell@stardust html]$ rm -r system/
 [isabell@stardust html]$ rm autoload.php

Run a database update

.. code-block:: console

 [isabell@stardust ~]$ cd /var/www/virtual/$USER/html/
 [isabell@stardust html]$ php index.php console migrate
 [isabell@stardust ~]$





.. _`Easy Appointments`: https://easyappointments.org/
.. _PHP: http://www.php.net/
.. _feed: https://github.com/alextselegidis/easyappointments/releases/
.. _`official documentation`: https://easyappointments.org/docs.html#1.4.3/readme.md


----

Tested with Easy Appointments 1.4.3, Uberspace 7.15.6, PHP 8.1

.. author_list::
