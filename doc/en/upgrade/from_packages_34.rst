===========================
Upgrading from Centreon 3.4
===========================

This chapter describes how to upgrade your platform to version Centreon 19.10.

.. warning::
    Upon completing the upgrade procedure, Centreon EMS users will have to request a new
    license from `Centreon support <https://centreon.force.com>`_.

.. warning::
    This procedure only applies to Centreon platforms installed from Centreon 3.4
    packages on **Red Hat / CentOS version 7** distributions.

    If this is not the case, refer to the procedure in :ref:`migration<upgradecentreon1904>`.

To upgrade your Centreon MAP server, refer to the `related documentation
<https://documentation.centreon.com/docs/centreon-map-4/en/latest/upgrade/index.html>`_.

To upgrade your Centreon MBI server, refer to the `related documentation
<https://documentation.centreon.com/docs/centreon-bi-2/en/latest/update/index.html>`_.

*******************
Performing a backup
*******************

Be sure that you have fully backed up your environment for the following servers:

* Central server
* Database server

*************************************
Upgrading the Centreon Central Server
*************************************

Upgrading the repository
========================

To install Centreon you will need to set up the official software collections
repository supported by Redhat.

.. note::
    *Software collections* are required in order to install PHP 7 and the associated
    libraries (Centreon requirement).

Run the following command: ::

    # yum install centos-release-scl

Upgrading the Centreon Repository
=================================

Run the following commands: ::

    # yum install -y http://yum.centreon.com/standard/19.10/el7/stable/noarch/RPMS/centreon-release-19.10-1.el7.centos.noarch.rpm

Updating the Centreon solution
==============================

Clean yum cache: ::

    # yum clean all

Upgrade all the components with the following command: ::

    # yum update centreon\*

.. note::
    Accept new GPG keys from the repositories as needed.

Additional actions
==================

Updating the PHP version
------------------------

Centreon 19.10 uses a new version of PHP.

The PHP timezone should be set. Run the command: ::

    # echo "date.timezone = Europe/Paris" > /etc/opt/rh/rh-php72/php.d/php-timezone.ini

.. note::
    Change **Europe/Paris** to your timezone.

Run the following commands: ::

    # systemctl start rh-php72-php-fpm
    # systemctl enable rh-php72-php-fpm

Updating the Apache web server
------------------------------

Centreon 19.10 uses a new version of Apache web server.

.. note::
    If you made manual configuration, please report it into
    **/opt/rh/httpd24/root/etc/httpd/conf.d/**.
		    
    If SSL mode was enabled, execute command: ::
				    
    # yum install httpd24-mod_ssl

Then, run the following commands: ::

    # systemctl disable httpd
    # systemctl stop httpd
    # systemctl enable httpd24-httpd
    # systemctl start httpd24-httpd
    # systemctl enable centreon
    # systemctl restart centreon

Finalizing the upgrade
======================

Log on to the Centreon web interface to continue the upgrade process:

Click on **Next**:

.. image:: /_static/images/upgrade/web_update_1.png
    :align: center

Click on **Next**:

.. image:: /_static/images/upgrade/web_update_2.png
    :align: center

The release notes describe the main changes. Click on **Next**:

.. image:: /_static/images/upgrade/web_update_3.png
    :align: center

This process performs the various upgrades. Click on **Next**:

.. image:: /_static/images/upgrade/web_update_4.png
    :align: center

Your Centreon server is now up to date. Click on **Finish** to access the login
page:

.. image:: /_static/images/upgrade/web_update_5.png
    :align: center

To upgrade your Centreon BAM module, refer to the `related documentation
<https://documentation.centreon.com/docs/centreon-bam/en/latest/update/index.html>`_.

*********************
Upgrading the Pollers
*********************

Upgrading the repository
========================

Run the following command: ::

    # yum install -y http://yum.centreon.com/standard/19.10/el7/stable/noarch/RPMS/centreon-release-19.10-1.el7.centos.noarch.rpm

Upgrading the Centreon solution
===============================

Upgrade all the components with the following command: ::

    # yum update centreon\*

.. note::
    Accept new GPG keys from the repositories as needed.

Additional actions
==================

Restart the services by executing the following commands: ::

    # systemctl restart cbd
    # systemctl restart centengine

*************************************
Upgrading the Centreon Poller Display
*************************************

Refer to the :ref:`migration procedure for Poller Display to Remote Server 19.10 <migratefrompollerdisplay>`.
