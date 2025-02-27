.. eql:section-intro-page:: datamodel

.. _ref_datamodel_index:

======
Schema
======

.. toctree::
    :maxdepth: 3
    :hidden:

    primitives
    objects
    properties
    links
    computeds
    indexes
    constraints
    aliases
    annotations
    globals
    access_policies
    functions
    inheritance
    extensions
    comparison


EdgeDB schemas are declared using **SDL** (EdgeDB's Schema Definition
Language).

SDL
---

Your schema is defined inside ``.esdl`` files. Its common to define your
entire schema in a single file called ``default.esdl``, but you can split it
across multiple files if you wish.

By convention, your schema
files should live in a directory called ``dbschema`` in the root of your
project.

.. code-block:: sdl

  # dbschema/default.esdl

  type Movie {
    required property title -> str;
    required link director -> Person;
  }

  type Person {
    required property name -> str;
  }

.. important::

  Syntax highlighter packages/extensions for ``.esdl`` files are available for
  `Visual Studio Code <https://marketplace.visualstudio.com/
  itemdetails?itemName=magicstack.edgedb>`_,
  `Sublime Text <https://packagecontrol.io/packages/EdgeDB>`_,
  `Atom <https://atom.io/packages/edgedb>`_, and `Vim <https://github.com/
  edgedb/edgedb-vim>`_.

Migrations
----------

EdgeDB's baked-in migration system lets you painlessly evolve your schema over
time. Just update the contents of your ``.esdl`` file(s) and use the EdgeDB CLI
to *create* and *apply* migrations.

.. code-block:: bash

  $ edgedb migration create
  Created dbschema/migrations/00001.esdl
  $ edgedb migrate
  Applied dbschema/migrations/00001.esdl.

For a full guide on migrations, refer to the :ref:`Creating and applying
migrations <ref_intro_migrations>` guide.

.. important::

  A migration consists of a sequence of *imperative* schema-modifying commands
  like ``create type``, ``alter property``, etc. Collectively these commands
  are known as :ref:`DDL <ref_eql_ddl>` (*data definition language*). We
  recommend using SDL and the migration system when building applications,
  however you're free to use DDL directly if you prefer.

.. _ref_datamodel_terminology:

Terminology
-----------

.. _ref_datamodel_instances:

.. rubric:: Instance

An EdgeDB **instance** is a collection of databases that store their data in
a shared directory, listen for queries on a particular port, and are managed
by a running EdgeDB process. Instances can be created, started, stopped, and
destroyed locally with the :ref:`EdgeDB CLI <ref_cli_overview>`.

.. _ref_datamodel_databases:

.. rubric:: Database

Each instance can contain several **databases**, each with a unique name. At
the time of creation, all instances contain a single default database called
``edgedb``. All incoming queries are executed
against it unless otherwise specified.

.. _ref_datamodel_modules:

.. rubric:: Module

Each database has a schema consisting of several **modules**, each with a
unique name. Modules can be used to organize large schemas into logical units.
In practice, though, most users put their entire schema inside a single module
called ``default``.

.. code-block:: sdl

  module default {
    # declare types here
  }

.. _ref_name_resolution:

.. note:: Name resolution

  When referencing schema objects from another module, you must use
  a *fully-qualified* name in the form ``module_name::object_name``.

The following module names are reserved by EdgeDB and contain pre-defined
types, utility functions, and operators.

* ``std``: standard types, functions, and operators in the :ref:`standard
  library <ref_std>`
* ``math``: algebraic and statistical :ref:`functions <ref_std_math>`
* ``cal``: local (non-timezone-aware) and relative date/time :ref:`types and
  functions <ref_std_datetime>`
* ``schema``: types describing the :ref:`introspection <ref_eql_introspection>`
  schema
* ``sys``: system-wide entities, such as user roles and
  :ref:`databases <ref_datamodel_databases>`
* ``cfg``: configuration and settings
