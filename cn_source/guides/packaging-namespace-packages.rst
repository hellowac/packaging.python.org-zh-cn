.. _packaging-namespace-packages:

============================
打包命名空间包
============================

**Packaging namespace packages**

.. tab:: 中文

    命名空间包允许你将单个 :term:`包 <Import Package>` 中的子包和模块拆分到多个独立的 :term:`发行包 <Distribution Package>` 中（为避免歧义，本文件中称之为 **distributions**）。例如，如果你有以下包结构：

    .. code-block:: text

        mynamespace/
            __init__.py
            subpackage_a/
                __init__.py
                ...
            subpackage_b/
                __init__.py
                ...
            module_b.py
        pyproject.toml

    并且你在代码中像这样使用这个包::

        from mynamespace import subpackage_a
        from mynamespace import subpackage_b

    那么你可以将这些子包拆分成两个独立的发行包：

    .. code-block:: text

        mynamespace-subpackage-a/
            pyproject.toml
            src/
                mynamespace/
                    subpackage_a/
                        __init__.py

        mynamespace-subpackage-b/
            pyproject.toml
            src/
                mynamespace/
                    subpackage_b/
                        __init__.py
                    module_b.py

    现在，每个子包可以单独安装、使用和版本控制。

    命名空间包对于一大批松散相关的包非常有用（例如，一个公司为多个产品提供的大量客户端库）。然而，命名空间包有几个注意事项，并不是在所有情况下都适用。一个简单的替代方案是为所有发行包使用一个前缀，例如 ``import mynamespace_subpackage_a`` （你甚至可以使用 ``import mynamespace_subpackage_a as subpackage_a`` 来保持导入对象简短）。

.. tab:: 英文

    Namespace packages allow you to split the sub-packages and modules within a
    single :term:`package <Import Package>` across multiple, separate
    :term:`distribution packages <Distribution Package>` (referred to as
    **distributions** in this document to avoid ambiguity). For example, if you
    have the following package structure:

    .. code-block:: text

        mynamespace/
            __init__.py
            subpackage_a/
                __init__.py
                ...
            subpackage_b/
                __init__.py
                ...
            module_b.py
        pyproject.toml

    And you use this package in your code like so::

        from mynamespace import subpackage_a
        from mynamespace import subpackage_b

    Then you can break these sub-packages into two separate distributions:

    .. code-block:: text

        mynamespace-subpackage-a/
            pyproject.toml
            src/
                mynamespace/
                    subpackage_a/
                        __init__.py

        mynamespace-subpackage-b/
            pyproject.toml
            src/
                mynamespace/
                    subpackage_b/
                        __init__.py
                    module_b.py

    Each sub-package can now be separately installed, used, and versioned.

    Namespace packages can be useful for a large collection of loosely-related
    packages (such as a large corpus of client libraries for multiple products from
    a single company). However, namespace packages come with several caveats and
    are not appropriate in all cases. A simple alternative is to use a prefix on
    all of your distributions such as ``import mynamespace_subpackage_a`` (you
    could even use ``import mynamespace_subpackage_a as subpackage_a`` to keep the
    import object short).


创建命名空间包
============================

**Creating a namespace package**

.. tab:: 中文

    目前有两种不同的方法来创建命名空间包，其中后一种方法不推荐使用：

    1. 使用 `本机命名空间包 <native namespace packages_>`_ 。这种类型的命名空间包在 :pep:`420` 中定义，并且在 Python 3.3 及更高版本中可用。如果你的命名空间中的包只需要支持 Python 3 且通过 ``pip`` 安装，推荐使用这种方法。
    2. 使用 `传统命名空间包 <legacy namespace packages_>`_ 。这包括 `pkgutil 风格的命名空间包 <pkgutil-style namespace packages_>`_ 和 `pkg_resources 风格的命名空间包 <pkg_resources-style namespace packages_>`_。

.. tab:: 英文

    There are currently two different approaches to creating namespace packages,
    from which the latter is discouraged:

    #. Use `native namespace packages`_. This type of namespace package is defined in :pep:`420` and is available in Python 3.3 and later. This is recommended if packages in your namespace only ever need to support Python 3 and installation via ``pip``.
    #. Use `legacy namespace packages`_. This comprises `pkgutil-style namespace packages`_ and `pkg_resources-style namespace packages`_.

.. _native namespace packages:

本机命名空间包
-------------------------

**Native namespace packages**

.. tab:: 中文

    Python 3.3 引入了 **隐式** 命名空间包，来自 :pep:`420`。创建原生命名空间包所需的唯一条件是，在命名空间包目录中省略 :file:`__init__.py` 文件。以下是一个示例文件结构（遵循 :ref:`src-layout <setuptools:src-layout>`）：

    .. code-block:: text

        mynamespace-subpackage-a/
            pyproject.toml # 和/或 setup.py, setup.cfg
            src/
                mynamespace/ # 命名空间包
                    # 这里没有 __init__.py 文件。
                    subpackage_a/
                        # 常规导入包有一个 __init__.py 文件。
                        __init__.py
                        module.py

    非常重要的一点是，使用命名空间包的每个发行包都必须省略 :file:`__init__.py` 或使用 pkgutil 风格的 :file:`__init__.py` 文件。如果任何发行包没有这样做，就会导致命名空间逻辑失败，其他子包将无法导入。

    ``src-layout`` 目录结构允许大多数 :term:`构建后端 <Build Backend>` 自动发现包。有关更多信息，请参阅 :ref:`src-layout-vs-flat-layout`。如果你希望自己管理包的排除或包含，也可以在顶级 :file:`pyproject.toml` 文件中进行配置：

    .. code-block:: toml

        [build-system]
        ...

        [tool.setuptools.packages.find]
        where = ["src/"]
        include = ["mynamespace.subpackage_a"]

        [project]
        name = "mynamespace-subpackage-a"
        ...

    同样的配置也可以通过 :file:`setup.cfg` 实现：

    .. code-block:: ini

        [options]
        package_dir =
            =src
        packages = find_namespace:

        [options.packages.find]
        where = src

    或者 :file:`setup.py`：

    .. code-block:: python

        from setuptools import setup, find_namespace_packages

        setup(
            name='mynamespace-subpackage-a',
            ...
            packages=find_namespace_packages(where='src/', include=['mynamespace.subpackage_a']),
            package_dir={'': 'src'},
        )

    默认情况下，:ref:`setuptools` 会搜索目录结构中的隐式命名空间包。

    可以在 `原生命名空间包示例项目`_ 中找到两个原生命名空间包的完整工作示例。

    .. _原生命名空间包示例项目:
        https://github.com/pypa/sample-namespace-packages/tree/master/native

    .. 注意:: 由于原生命名空间包和 pkgutil 风格的命名空间包在很大程度上是兼容的，因此你可以在只支持 Python 3 的发行包中使用原生命名空间包，在需要支持 Python 2 和 3 的发行包中使用 pkgutil 风格的命名空间包。

.. tab:: 英文

    Python 3.3 added **implicit** namespace packages from :pep:`420`. All that is
    required to create a native namespace package is that you just omit
    :file:`__init__.py` from the namespace package directory. An example file
    structure (following :ref:`src-layout <setuptools:src-layout>`):

    .. code-block:: text

        mynamespace-subpackage-a/
            pyproject.toml # AND/OR setup.py, setup.cfg
            src/
                mynamespace/ # namespace package
                    # No __init__.py here.
                    subpackage_a/
                        # Regular import packages have an __init__.py.
                        __init__.py
                        module.py

    It is extremely important that every distribution that uses the namespace
    package omits the :file:`__init__.py` or uses a pkgutil-style
    :file:`__init__.py`. If any distribution does not, it will cause the namespace
    logic to fail and the other sub-packages will not be importable.

    The ``src-layout`` directory structure allows automatic discovery of packages
    by most :term:`build backends <Build Backend>`. See :ref:`src-layout-vs-flat-layout`
    for more information. If however you want to manage exclusions or inclusions of packages
    yourself, this is possible to be configured in the top-level :file:`pyproject.toml`:

    .. code-block:: toml

        [build-system]
        ...

        [tool.setuptools.packages.find]
        where = ["src/"]
        include = ["mynamespace.subpackage_a"]

        [project]
        name = "mynamespace-subpackage-a"
        ...

    The same can be accomplished with a :file:`setup.cfg`:

    .. code-block:: ini

        [options]
        package_dir =
            =src
        packages = find_namespace:

        [options.packages.find]
        where = src

    Or :file:`setup.py`:

    .. code-block:: python

        from setuptools import setup, find_namespace_packages

        setup(
            name='mynamespace-subpackage-a',
            ...
            packages=find_namespace_packages(where='src/', include=['mynamespace.subpackage_a']),
            package_dir={'': 'src'},
        )

    :ref:`setuptools` will search the directory structure for implicit namespace
    packages by default.

    A complete working example of two native namespace packages can be found in
    the `native namespace package example project`_.

    .. _native namespace package example project:
        https://github.com/pypa/sample-namespace-packages/tree/master/native

    .. note:: Because native and pkgutil-style namespace packages are largely
        compatible, you can use native namespace packages in the distributions that
        only support Python 3 and pkgutil-style namespace packages in the
        distributions that need to support Python 2 and 3.

.. _legacy namespace packages:

旧式命名空间包
-------------------------

**Legacy namespace packages**

.. tab:: 中文

    在 :pep:`420` 之前使用的这两种方法，现在被认为是过时的，除非你需要与已经使用这种方法的包保持兼容，否则不应使用。此外，:doc:`pkg_resources <setuptools:pkg_resources>` 已经被弃用。

    要迁移现有的包，所有共享命名空间的包必须同时进行迁移。

    .. warning:: 
        
        虽然原生命名空间包和 pkgutil 风格的命名空间包在很大程度上是兼容的，但 pkg_resources 风格的命名空间包与其他方法不兼容。因此，不建议在提供同一命名空间包的不同发行包中使用不同的方法。

.. tab:: 英文

    These two methods, that were used to create namespace packages prior to :pep:`420`,
    are now considered to be obsolete and should not be used unless you need compatibility
    with packages already using this method. Also, :doc:`pkg_resources <setuptools:pkg_resources>`
    has been deprecated.

    To migrate an existing package, all packages sharing the namespace must be migrated simultaneously.

    .. warning:: While native namespace packages and pkgutil-style namespace
        packages are largely compatible, pkg_resources-style namespace packages
        are not compatible with the other methods. It's inadvisable to use
        different methods in different distributions that provide packages to the
        same namespace.

.. _pkgutil-style namespace packages:

pkgutil 样式命名空间包
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**pkgutil-style namespace packages**

.. tab:: 中文

    Python 2.3 引入了 :doc:`pkgutil <python:library/pkgutil>` 模块和 :py:func:`python:pkgutil.extend_path` 函数。这可以用来声明需要与 Python 2.3+ 和 Python 3 兼容的命名空间包。这是推荐的做法，以确保最高级别的兼容性。

    要创建一个 pkgutil 风格的命名空间包，你需要为命名空间包提供一个 :file:`__init__.py` 文件：

    .. code-block:: text

        mynamespace-subpackage-a/
            src/
                pyproject.toml # 和/或 setup.cfg, setup.py
                mynamespace/
                    __init__.py  # 命名空间包的 __init__.py
                    subpackage_a/
                        __init__.py  # 常规包的 __init__.py
                        module.py

    命名空间包的 :file:`__init__.py` 文件需要包含以下内容：

    .. code-block:: python

        __path__ = __import__('pkgutil').extend_path(__path__, __name__)

    **每个** 使用该命名空间包的发行包必须包含这样的 :file:`__init__.py` 文件。如果任何发行包没有这样做，就会导致命名空间逻辑失败，其他子包将无法导入。 :file:`__init__.py` 中的任何额外代码将无法访问。

    一个完整的 pkgutil 风格命名空间包的工作示例可以在 `pkgutil 命名空间示例项目 <pkgutil namespace example project_>`_ 中找到。

.. tab:: 英文

    Python 2.3 introduced the :doc:`pkgutil <python:library/pkgutil>` module and the
    :py:func:`python:pkgutil.extend_path` function. This can be used to declare namespace
    packages that need to be compatible with both Python 2.3+ and Python 3. This
    is the recommended approach for the highest level of compatibility.

    To create a pkgutil-style namespace package, you need to provide an
    :file:`__init__.py` file for the namespace package:

    .. code-block:: text

        mynamespace-subpackage-a/
            src/
                pyproject.toml # AND/OR setup.cfg, setup.py
                mynamespace/
                    __init__.py  # Namespace package __init__.py
                    subpackage_a/
                        __init__.py  # Regular package __init__.py
                        module.py

    The :file:`__init__.py` file for the namespace package needs to contain
    the following:

    .. code-block:: python

        __path__ = __import__('pkgutil').extend_path(__path__, __name__)

    **Every** distribution that uses the namespace package must include such
    an :file:`__init__.py`. If any distribution does not, it will cause the
    namespace logic to fail and the other sub-packages will not be importable.  Any
    additional code in :file:`__init__.py` will be inaccessible.

    A complete working example of two pkgutil-style namespace packages can be found
    in the `pkgutil namespace example project`_.

.. _extend_path:
    https://docs.python.org/3/library/pkgutil.html#pkgutil.extend_path
.. _pkgutil namespace example project:
    https://github.com/pypa/sample-namespace-packages/tree/master/pkgutil


.. _pkg_resources-style namespace packages:

pkg_resources 样式命名空间包
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**pkg_resources-style namespace packages**

.. tab:: 中文

    :doc:`Setuptools <setuptools:index>` 提供了 `pkg_resources.declare_namespace`_ 函数和
    ``namespace_packages`` 参数，供 :func:`~setuptools.setup` 使用。这两者可以一起用来声明命名空间包。虽然这种方法现在不再推荐，但它在大多数现有的命名空间包中仍然广泛存在。如果你正在创建一个新的分发包，且该分发包位于一个使用此方法的现有命名空间包内，那么建议继续使用这种方法，因为不同的方法之间并不兼容，而且不建议尝试迁移现有包。

    要创建一个 pkg_resources 风格的命名空间包，你需要为该命名空间包提供一个 :file:`__init__.py` 文件：

    .. code-block:: text

        mynamespace-subpackage-a/
            src/
                pyproject.toml # 和/或 setup.cfg, setup.py
                mynamespace/
                    __init__.py  # 命名空间包 __init__.py
                    subpackage_a/
                        __init__.py  # 常规包 __init__.py
                        module.py

    命名空间包的 :file:`__init__.py` 文件需要包含以下内容：

    .. code-block:: python

        __import__('pkg_resources').declare_namespace(__name__)

    **每个** 使用命名空间包的分发包都必须包含这样的 :file:`__init__.py` 文件。如果任何分发包没有包含该文件，将导致命名空间逻辑失败，其他子包将无法导入。 :file:`__init__.py` 文件中的任何额外代码将无法访问。

    .. note:: 
        
        一些较旧的推荐方法建议在命名空间包的 :file:`__init__.py` 文件中使用以下代码：

        .. code-block:: python

            try:
                __import__('pkg_resources').declare_namespace(__name__)
            except ImportError:
                __path__ = __import__('pkgutil').extend_path(__path__, __name__)

        这种做法的想法是在少数情况下，如果 setuptools 不可用，包将回退到 pkgutil 风格的包。但这并不推荐，因为 pkgutil 和 pkg_resources 风格的命名空间包并不兼容。如果担心 setuptools 的存在问题，应该通过 ``install_requires`` 明确地让包依赖 setuptools。

    最后，每个分发包都必须在 :file:`setup.py` 文件中的 :func:`~setuptools.setup` 调用中提供 ``namespace_packages`` 参数。例如：

    .. code-block:: python

        from setuptools import find_packages, setup

        setup(
            name='mynamespace-subpackage-a',
            ...
            packages=find_packages()
            namespace_packages=['mynamespace']
        )

    一个完整的 pkg_resources 风格命名空间包示例可以在 `pkg_resources 命名空间示例项目 <pkg_resources namespace example project_>`_ 中找到。

.. tab:: 英文

    :doc:`Setuptools <setuptools:index>` provides the `pkg_resources.declare_namespace`_ function and
    the ``namespace_packages`` argument to :func:`~setuptools.setup`. Together
    these can be used to declare namespace packages. While this approach is no
    longer recommended, it is widely present in most existing namespace packages.
    If you are creating a new distribution within an existing namespace package that
    uses this method then it's recommended to continue using this as the different
    methods are not cross-compatible and it's not advisable to try to migrate an
    existing package.

    To create a pkg_resources-style namespace package, you need to provide an
    :file:`__init__.py` file for the namespace package:

    .. code-block:: text

        mynamespace-subpackage-a/
            src/
                pyproject.toml # AND/OR setup.cfg, setup.py
                mynamespace/
                    __init__.py  # Namespace package __init__.py
                    subpackage_a/
                        __init__.py  # Regular package __init__.py
                        module.py

    The :file:`__init__.py` file for the namespace package needs to contain
    the following:

    .. code-block:: python

        __import__('pkg_resources').declare_namespace(__name__)

    **Every** distribution that uses the namespace package must include such an
    :file:`__init__.py`. If any distribution does not, it will cause the
    namespace logic to fail and the other sub-packages will not be importable.  Any
    additional code in :file:`__init__.py` will be inaccessible.

    .. note:: Some older recommendations advise the following in the namespace
        package :file:`__init__.py`:

        .. code-block:: python

            try:
                __import__('pkg_resources').declare_namespace(__name__)
            except ImportError:
                __path__ = __import__('pkgutil').extend_path(__path__, __name__)

        The idea behind this was that in the rare case that setuptools isn't
        available packages would fall-back to the pkgutil-style packages. This
        isn't advisable because pkgutil and pkg_resources-style namespace packages
        are not cross-compatible. If the presence of setuptools is a concern
        then the package should just explicitly depend on setuptools via
        ``install_requires``.

    Finally, every distribution must provide the ``namespace_packages`` argument
    to :func:`~setuptools.setup` in :file:`setup.py`. For example:

    .. code-block:: python

        from setuptools import find_packages, setup

        setup(
            name='mynamespace-subpackage-a',
            ...
            packages=find_packages()
            namespace_packages=['mynamespace']
        )

    A complete working example of two pkg_resources-style namespace packages can be found
    in the `pkg_resources namespace example project`_.

.. _pkg_resources.declare_namespace:
    https://setuptools.readthedocs.io/en/latest/pkg_resources.html#namespace-package-support
.. _pkg_resources namespace example project:
    https://github.com/pypa/sample-namespace-packages/tree/master/pkg_resources
