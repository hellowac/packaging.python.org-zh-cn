================================
创建和发现插件
================================

**Creating and discovering plugins**

.. tab:: 中文

    在创建 Python 应用程序或库时，通常你会希望能够通过 **插件** 提供自定义功能或额外的特性。由于 Python 包可以单独分发，你的应用程序或库可能希望自动 **发现(discover)** 所有可用的插件。

    自动插件发现有三种主要方法：

    #. `使用命名约定`_。
    #. `使用命名空间包`_。
    #. `使用包元数据`_。

.. tab:: 英文

    Often when creating a Python application or library you'll want the ability to
    provide customizations or extra features via **plugins**. Because Python
    packages can be separately distributed, your application or library may want to
    automatically **discover** all of the plugins available.

    There are three major approaches to doing automatic plugin discovery:

    #. `Using naming convention`_.
    #. `Using namespace packages`_.
    #. `Using package metadata`_.


.. _Using naming convention:

使用命名约定
=======================

**Using naming convention**

.. tab:: 中文

    如果你应用程序的所有插件都遵循相同的命名约定，你可以使用 :func:`pkgutil.iter_modules` 来发现所有符合命名约定的顶级模块。例如， `Flask`_ 使用命名约定 ``flask_{plugin_name}``。如果你想自动发现所有已安装的 Flask 插件，可以这样做：

    .. code-block:: python

        import importlib
        import pkgutil

        discovered_plugins = {
            name: importlib.import_module(name)
            for finder, name, ispkg
            in pkgutil.iter_modules()
            if name.startswith('flask_')
        }

    如果你安装了 `Flask-SQLAlchemy`_ 和 `Flask-Talisman`_ 插件，那么 ``discovered_plugins`` 将会是：

    .. code-block:: python

        {
            'flask_sqlalchemy': <module: 'flask_sqlalchemy'>,
            'flask_talisman': <module: 'flask_talisman'>,
        }

    使用命名约定来处理插件还可以让你查询 Python 包索引的 :ref:`simple repository API <simple-repository-api>`，查找所有符合命名约定的包。

.. tab:: 英文

    If all of the plugins for your application follow the same naming convention,
    you can use :func:`pkgutil.iter_modules` to discover all of the top-level
    modules that match the naming convention. For example, `Flask`_ uses the
    naming convention ``flask_{plugin_name}``. If you wanted to automatically
    discover all of the Flask plugins installed:

    .. code-block:: python

        import importlib
        import pkgutil

        discovered_plugins = {
            name: importlib.import_module(name)
            for finder, name, ispkg
            in pkgutil.iter_modules()
            if name.startswith('flask_')
        }

    If you had both the `Flask-SQLAlchemy`_ and `Flask-Talisman`_ plugins installed
    then ``discovered_plugins`` would be:

    .. code-block:: python

        {
            'flask_sqlalchemy': <module: 'flask_sqlalchemy'>,
            'flask_talisman': <module: 'flask_talisman'>,
        }

    Using naming convention for plugins also allows you to query
    the Python Package Index's :ref:`simple repository API <simple-repository-api>`
    for all packages that conform to your naming convention.

.. _Flask: https://pypi.org/project/Flask/
.. _Flask-SQLAlchemy: https://pypi.org/project/Flask-SQLAlchemy/
.. _Flask-Talisman: https://pypi.org/project/flask-talisman


.. _Using namespace packages:

使用命名空间包
========================

**Using namespace packages**

.. tab:: 中文

    :doc:`命名空间包 <packaging-namespace-packages>` 可以用来提供插件的放置约定，并且提供了一种执行发现的方法。例如，如果你将子包 ``myapp.plugins`` 设为命名空间包，那么其他 :term:`分发包 <Distribution Package>` 可以将模块和包提供给该命名空间。一旦安装，你可以使用 :func:`pkgutil.iter_modules` 来发现所有安装在该命名空间下的模块和包：

    .. code-block:: python

        import importlib
        import pkgutil

        import myapp.plugins

        def iter_namespace(ns_pkg):
            # 在 iter_modules 中指定第二个参数 (prefix) 使得返回的名称是绝对名称，而不是相对名称。
            # 这样可以让 import_module 正常工作，而无需对名称进行额外的修改。
            return pkgutil.iter_modules(ns_pkg.__path__, ns_pkg.__name__ + ".")

        discovered_plugins = {
            name: importlib.import_module(name)
            for finder, name, ispkg
            in iter_namespace(myapp.plugins)
        }

    指定 ``myapp.plugins.__path__`` 给 :func:`~pkgutil.iter_modules`，使其仅查找该命名空间下的模块。例如，如果你安装了提供模块 ``myapp.plugins.a`` 和 ``myapp.plugins.b`` 的分发包，那么此时的 ``discovered_plugins`` 将是：

    .. code-block:: python

        {
            'a': <module: 'myapp.plugins.a'>,
            'b': <module: 'myapp.plugins.b'>,
        }

    这个示例使用了子包作为命名空间包（``myapp.plugins``），但也可以使用顶级包来实现这一目的（如 ``myapp_plugins``）。选择使用哪个命名空间包是个人偏好的问题，但不建议将项目的主顶级包（在本例中是 ``myapp``）作为插件的命名空间包，因为一个坏的插件可能会导致整个命名空间崩溃，从而使得你的项目无法导入。为了使“命名空间子包”方法正常工作，插件包必须省略顶级包目录（在本例中是 ``myapp``）的 :file:`__init__.py`，并在命名空间子包目录（ ``myapp/plugins``）中包含命名空间包风格的 :file:`__init__.py`。这也意味着插件需要明确地将包列表传递给 :func:`setup` 的 ``packages`` 参数，而不是使用 :func:`setuptools.find_packages`。

    .. warning:: 
        
        命名空间包是一个复杂的特性，有多种不同的方法来创建它们。强烈建议阅读 :doc:`packaging-namespace-packages` 文档，并清楚地记录你希望为项目插件采用的优选方法。

.. tab:: 英文

    :doc:`Namespace packages <packaging-namespace-packages>` can be used to provide
    a convention for where to place plugins and also provides a way to perform
    discovery. For example, if you make the sub-package ``myapp.plugins`` a
    namespace package then other :term:`distributions <Distribution Package>` can
    provide modules and packages to that namespace. Once installed, you can use
    :func:`pkgutil.iter_modules` to discover all modules and packages installed
    under that namespace:

    .. code-block:: python

        import importlib
        import pkgutil

        import myapp.plugins

        def iter_namespace(ns_pkg):
            # Specifying the second argument (prefix) to iter_modules makes the
            # returned name an absolute name instead of a relative one. This allows
            # import_module to work without having to do additional modification to
            # the name.
            return pkgutil.iter_modules(ns_pkg.__path__, ns_pkg.__name__ + ".")

        discovered_plugins = {
            name: importlib.import_module(name)
            for finder, name, ispkg
            in iter_namespace(myapp.plugins)
        }

    Specifying ``myapp.plugins.__path__`` to :func:`~pkgutil.iter_modules` causes
    it to only look for the modules directly under that namespace. For example,
    if you have installed distributions that provide the modules ``myapp.plugins.a``
    and ``myapp.plugins.b`` then ``discovered_plugins`` in this case would be:

    .. code-block:: python

        {
            'a': <module: 'myapp.plugins.a'>,
            'b': <module: 'myapp.plugins.b'>,
        }

    This sample uses a sub-package as the namespace package (``myapp.plugins``), but
    it's also possible to use a top-level package for this purpose (such as
    ``myapp_plugins``). How to pick the namespace to use is a matter of preference,
    but it's not recommended to make your project's main top-level package
    (``myapp`` in this case) a namespace package for the purpose of plugins, as one
    bad plugin could cause the entire namespace to break which would in turn make
    your project unimportable. For the "namespace sub-package" approach to work,
    the plugin packages must omit the :file:`__init__.py` for your top-level
    package directory (``myapp`` in this case) and include the namespace-package
    style :file:`__init__.py` in the namespace sub-package directory
    (``myapp/plugins``).  This also means that plugins will need to explicitly pass
    a list of packages to :func:`setup`'s ``packages`` argument instead of using
    :func:`setuptools.find_packages`.

    .. warning:: Namespace packages are a complex feature and there are several
        different ways to create them. It's highly recommended to read the
        :doc:`packaging-namespace-packages` documentation and clearly document
        which approach is preferred for plugins to your project.

.. _plugin-entry-points:

.. _Using package metadata:

使用包元数据
======================

**Using package metadata**

.. tab:: 中文

    包可以在 :ref:`入口点 <entry-points>` 中描述插件的元数据。通过指定这些元数据，包声明它包含某种特定类型的插件。其他支持这种插件类型的包可以使用这些元数据来发现该插件。

    例如，如果你有一个名为 ``myapp-plugin-a`` 的包，并且在它的 ``pyproject.toml`` 中包含以下内容：

    .. code-block:: toml

        [project.entry-points.'myapp.plugins']
        a = 'myapp_plugin_a'

    那么你可以通过使用 :func:`importlib.metadata.entry_points` （对于 Python 3.6-3.9，使用 backport_ 的 ``importlib_metadata >= 3.6`` ）来发现并加载所有注册的入口点：

    .. code-block:: python

        import sys
        if sys.version_info < (3, 10):
            from importlib_metadata import entry_points
        else:
            from importlib.metadata import entry_points

        discovered_plugins = entry_points(group='myapp.plugins')

    在这个示例中， ``discovered_plugins`` 将是一个类型为 :class:`importlib.metadata.EntryPoint` 的集合：

    .. code-block:: python

        (
            EntryPoint(name='a', value='myapp_plugin_a', group='myapp.plugins'),
            ...
        )

    现在，你可以通过执行 ``discovered_plugins['a'].load()`` 来导入你选择的模块。

    .. note:: 
        
        :file:`setup.py` 中的 ``entry_point`` 规范非常灵活，具有很多选项。建议阅读 :doc:`entry points <setuptools:userguide/entry_point>` 部分的全部内容。

    .. note:: 
        
        由于这个规范是 :doc:`标准库 <python:library/importlib.metadata>` 的一部分，除了 setuptools 之外，大多数打包工具也提供了对定义入口点的支持。

.. tab:: 英文

    Packages can have metadata for plugins described in the :ref:`entry-points`.
    By specifying them, a package announces that it contains a specific kind of plugin.
    Another package supporting this kind of plugin can use the metadata to discover that plugin.

    For example if you have a package named ``myapp-plugin-a`` and it includes
    the following in its ``pyproject.toml``:

    .. code-block:: toml

        [project.entry-points.'myapp.plugins']
        a = 'myapp_plugin_a'

    Then you can discover and load all of the registered entry points by using
    :func:`importlib.metadata.entry_points` (or the backport_
    ``importlib_metadata >= 3.6`` for Python 3.6-3.9):

    .. code-block:: python

        import sys
        if sys.version_info < (3, 10):
            from importlib_metadata import entry_points
        else:
            from importlib.metadata import entry_points

        discovered_plugins = entry_points(group='myapp.plugins')


    In this example, ``discovered_plugins`` would be a collection of type :class:`importlib.metadata.EntryPoint`:

    .. code-block:: python

        (
            EntryPoint(name='a', value='myapp_plugin_a', group='myapp.plugins'),
            ...
        )

    Now the module of your choice can be imported by executing
    ``discovered_plugins['a'].load()``.

    .. note:: The ``entry_point`` specification in :file:`setup.py` is fairly
        flexible and has a lot of options. It's recommended to read over the entire
        section on :doc:`entry points <setuptools:userguide/entry_point>` .

    .. note:: Since this specification is part of the :doc:`standard library
        <python:library/importlib.metadata>`, most packaging tools other than setuptools
        provide support for defining entry points.

.. _backport: https://importlib-metadata.readthedocs.io/en/latest/
