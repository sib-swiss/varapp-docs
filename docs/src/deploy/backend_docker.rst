
.. Non-breaking white space, to fill empty divs
.. |nbsp| unicode:: 0xA0
   :trim:

Installation with Docker
........................

Since version 2.0.0, the backend comes with Docker support.
Warning: this version is not compatible with 1.x.x and previously generated databases.

Requirements
++++++++++++

* A running Docker client.

Pull the Docker image

  docker pull sibswiss/varapp-backend

Deployment
++++++++++

Clone the repository for the backend:

  git clone git@github.com:sib-swiss/varapp-backend-py.git

Run the docker-compose at the root of the repository:

  docker-compose up

By default it assumes that Gemini databases are in ``./resources/db``, see Settings section below.

This will

* start an Apache web server with mod_wsgi.
* start an instance of Redis
* start an instance of Mysql
* create a named volume that saves the Mysql data on shutdown (located somewhere in Docker internals)
* create the mysql db schema
* install demo data

How to:

* start the service: ``docker-compose up``
* stop the service: ``docker-compose down``
* stop and clear users db (msyql): `docker-compose down -v`

Settings
++++++++

Data
----

The location of Gemini databases is configured via the environment variable ``GEMINI_DB_PATH``,
defaults to ``./resources/db``, and is mounted to ``/db`` inside the container.

It allows you to add/remove databases inside ``$GEMINI_DB_PATH`` dymanically.

General settings
----------------

The location of Django settings is configured via the environment variable ``SETTINGS_PATH``,
defaults to ``./varmed/settings``, and is mounted to ``/app/varmed/settings`` inside the container.

It allows you to modify the settings file ``settings_docker.py`` inside ``$SETTINGS_PATH`` dynamically
(you will still need to restart the service). **Not** ``settings.py``, which is used for local
development and manual installations.
