Python 2.6.8 fails to build on Ubuntu 13.04, so use this ppa::

   $ sudo add-apt-repository ppa:fkrull/deadsnakes
   $ sudo apt-get update
   $ sudo apt-get install python2.6 python2.6-dev

Make a virtual environment for your plone dev::

   $ mkvirtualenv -p /usr/bin/python2.6 plone-python

Install PIL into the environment::

   (plone-python)$ wget http://effbot.org/downloads/Imaging-1.1.7.tar.gz
   (plone-python)$ tar -vzxf Imaging-1.1.7.tar.gz
   (plone-python)$ cd Imaging-1.1.7
   (plone-python)$ sudo aptitude install libjpeg62-dev zlib1g-dev libfreetype6-dev liblcms1-dev
   (plone-python)$ python setup.py install

Make a directory for your website::

   (plone-python)$ mkdir site
   (plone-python)$ cd site

Now get the buildout bootstrap script::

   (plone-python)$ wget https://raw.github.com/plone/buildout.coredev/4.3/bootstrap.py

Run bootstrap to setup the buildout commands in bin::

   (plone-python)$ python bootstrap.py --distribute

Create a buildout.cfg::

   (plone-python)$ vim buildout.cfg

Setup a spot in your home directory where you can use buildout junk for all
your plone sites::

  (plone-python)$ mkdir ~/.buildout
  (plone-python)$ vim ~/.buildout/default.cfg
  (plone-python)$ mkdir /home/moorepants/.buildout/eggs
  (plone-python)$ mkdir /home/moorepants/.buildout/extends
  (plone-python)$ mkdir /home/moorepants/.buildout/downloads

The default.cfg file should look like::

  [buildout]
  eggs-directory = /home/moorepants/.buildout/eggs
  download-cache = /home/moorepants/.buildout/downloads
  extends-cache = /home/moorepants/.buildout/extends

Now download and install plone/zope with buildout::

  (plone-python)$ bin/buildout

Buildout failed because I was missing these::

  (plone-python)$ sudo aptitude install libxml2-dev libxslt-dev

Get cython too for the xml python pacakge speedups::

  (plone-python)$ pip install cython

Now buildout works::

  (plone-python)$ bin/buildout

Create a package. Ideally this should be under its on git repo but for this it
will be monolithic::

  (plone-python)$ cd src
  (plone-python)$ ../bin/zopeskel plone optilux.policy

Edit the setup.py, configure.zcml, packages.cfg, and buildout.cfg

bin/buildout
bin/zopepy
