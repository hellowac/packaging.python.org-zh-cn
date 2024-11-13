.. _modernize-setup-py-project:


==============================================
如何使基于 ``setup.py`` 的项目现代化？
==============================================

**How to modernize a** ``setup.py`` **based project?**

是否应添加 ``pyproject.toml``？
===================================

**Should** ``pyproject.toml`` **be added?**

.. tab:: 中文

    强烈推荐使用 :term:`pyproject.toml` 文件。  
    文件本身的存在并没有太大意义。[#]_  
    真正强烈推荐的是在 :file:`pyproject.toml` 中的 ``[build-system]`` 表。

.. tab:: 英文

    A :term:`pyproject.toml` file is strongly recommended.
    The presence of a :file:`pyproject.toml` file itself does not bring much. [#]_
    What is actually strongly recommended is the ``[build-system]`` table in :file:`pyproject.toml`.

.. [#] 请注意，它会影响 pip 的构建隔离功能，见下文。
.. [#] Note that it has influence on the build isolation feature of pip, see below.


是否应删除 ``setup.py``？
===============================

**Should ``setup.py`` be deleted?**

.. tab:: 中文

    不用删除，现代基于 :ref:`setuptools` 的项目中可以存在 :file:`setup.py` 文件。  
    :file:`setup.py` 文件是一个有效的 setuptools 配置文件，虽然它是用 Python 编写的。  
    但是，以下命令已经被弃用，**不得再运行**，应使用它们推荐的替代命令：

    +---------------------------------+----------------------------------------+
    | 弃用命令                        | 推荐命令                               |
    +=================================+========================================+
    | ``python setup.py install``     | ``python -m pip install .``            |
    +---------------------------------+----------------------------------------+
    | ``python setup.py develop``     | ``python -m pip install --editable .`` |
    +---------------------------------+----------------------------------------+
    | ``python setup.py sdist``       | ``python -m build``                    |
    +---------------------------------+                                        |
    | ``python setup.py bdist_wheel`` |                                        |
    +---------------------------------+----------------------------------------+

    更多详情：

    * :ref:`setup-py-deprecated`

.. tab:: 英文

    No, :file:`setup.py` can exist in a modern :ref:`setuptools` based project.
    The :term:`setup.py` file is a valid configuration file for setuptools
    that happens to be written in Python.
    However, the following commands are deprecated and **MUST NOT** be run anymore,
    and their recommended replacement commands should be used instead:

    +---------------------------------+----------------------------------------+
    | Deprecated                      | Recommendation                         |
    +=================================+========================================+
    | ``python setup.py install``     | ``python -m pip install .``            |
    +---------------------------------+----------------------------------------+
    | ``python setup.py develop``     | ``python -m pip install --editable .`` |
    +---------------------------------+----------------------------------------+
    | ``python setup.py sdist``       | ``python -m build``                    |
    +---------------------------------+                                        |
    | ``python setup.py bdist_wheel`` |                                        |
    +---------------------------------+----------------------------------------+


    For more details:

    * :ref:`setup-py-deprecated`


从哪里开始？
===============

**Where to start?**

.. tab:: 中文

    项目必须在其源代码树的根目录下包含一个 :file:`pyproject.toml` 文件，并且该文件必须包含一个类似如下的 ``[build-system]`` 表：

    .. code:: toml

        [build-system]
        requires = ["setuptools"]
        build-backend = "setuptools.build_meta"

    这是让 :term:`构建前端 <Build Frontend>` 知道 :ref:`setuptools` 是该项目的 :term:`构建后端 <Build Backend>` 的标准化方法。

    请注意，存在 :file:`pyproject.toml` 文件（即使文件为空）会触发 :ref:`pip` 改变其默认行为，使用 *构建隔离*。

    更多详情：

    * :ref:`distributing-packages`
    * :ref:`pyproject-build-system-table`
    * :doc:`pip:reference/build-system/pyproject-toml`

.. tab:: 英文

    The :term:`project` must contain a :file:`pyproject.toml` file at the root of its source tree
    that contains a ``[build-system]`` table like so:

    .. code:: toml

        [build-system]
        requires = ["setuptools"]
        build-backend = "setuptools.build_meta"


    This is the standardized method of letting :term:`build frontends <Build Frontend>` know
    that :ref:`setuptools` is the :term:`build backend <Build Backend>` for this project.

    Note that the presence of a :file:`pyproject.toml` file (even if empty)
    triggers :ref:`pip` to change its default behavior to use *build isolation*.

    For more details:

    * :ref:`distributing-packages`
    * :ref:`pyproject-build-system-table`
    * :doc:`pip:reference/build-system/pyproject-toml`


如何处理额外的构建时依赖项？
=================================================

**How to handle additional build-time dependencies?**

.. tab:: 中文

    除了 setuptools 本身，如果 :file:`setup.py` 依赖于其他第三方库（不包括 Python 标准库中的库），这些库必须在 ``[build-system]`` 表的 ``requires`` 列表中列出，以便构建前端在构建 :term:`分发包 <Distribution Package>` 时知道安装它们。

    例如，像这样的 :file:`setup.py` 文件：

    .. code:: python

        import setuptools
        import some_build_toolkit  # 来自 `some-build-toolkit` 库

        def get_version():
            version = some_build_toolkit.compute_version()
            return version

        setuptools.setup(
            name="my-project",
            version=get_version(),
        )


    就需要一个像这样的 :file:`pyproject.toml` 文件（ :file:`setup.py` 不变 ）：

    .. code:: toml

        [build-system]
        requires = [
            "setuptools",
            "some-build-toolkit",
        ]
        build-backend = "setuptools.build_meta"


    更多详情：

    * :ref:`pyproject-build-system-table`

.. tab:: 英文

    On top of setuptools itself,
    if :file:`setup.py` depends on other third-party libraries (outside of Python's standard library),
    those must be listed in the ``requires`` list of the ``[build-system]`` table,
    so that the build frontend knows to install them
    when building the :term:`distributions <Distribution Package>`.

    For example, a :file:`setup.py` file such as this:

    .. code:: python

        import setuptools
        import some_build_toolkit  # comes from the `some-build-toolkit` library

        def get_version():
            version = some_build_toolkit.compute_version()
            return version

        setuptools.setup(
            name="my-project",
            version=get_version(),
        )


    requires a :file:`pyproject.toml` file like this (:file:`setup.py` stays unchanged):

    .. code:: toml

        [build-system]
        requires = [
            "setuptools",
            "some-build-toolkit",
        ]
        build-backend = "setuptools.build_meta"


    For more details:

    * :ref:`pyproject-build-system-table`


什么是构建隔离功能？
====================================

**What is the build isolation feature?**

.. tab:: 中文

    构建前端通常会创建一个临时的虚拟环境，在该环境中只安装 ``build-system.requires`` 下列出的构建依赖（以及它们的依赖），并在该环境中触发构建。

    对于某些项目，这种隔离是不希望的，可以通过以下方式禁用：

    * ``python -m build --no-isolation``
    * ``python -m pip install --no-build-isolation``

    更多详情：

    * :doc:`pip:reference/build-system/pyproject-toml`

.. tab:: 英文

    Build frontends typically create an ephemeral virtual environment
    where they install only the build dependencies (and their dependencies)
    that are listed under ``build-system.requires``
    and trigger the build in that environment.

    For some projects this isolation is unwanted and it can be deactivated as follows:

    * ``python -m build --no-isolation``
    * ``python -m pip install --no-build-isolation``

    For more details:

    * :doc:`pip:reference/build-system/pyproject-toml`


如何处理打包元数据？
=================================

**How to handle packaging metadata?**

.. tab:: 中文

    所有静态元数据可以选择性地移动到 :file:`pyproject.toml` 文件中的 ``[project]`` 表中。

    例如，一个如下所示的 :file:`setup.py` 文件：

    .. code:: python

        import setuptools

        setuptools.setup(
            name="my-project",
            version="1.2.3",
        )


    可以完全替换为一个如下所示的 :file:`pyproject.toml` 文件：

    .. code:: toml

        [build-system]
        requires = ["setuptools"]
        build-backend = "setuptools.build_meta"

        [project]
        name = "my-project"
        version = "1.2.3"


    请阅读 :ref:`pyproject-project-table` 了解 ``[project]`` 表中允许的内容的完整规范。

.. tab:: 英文

    All static metadata can optionally be moved to a ``[project]`` table in :file:`pyproject.toml`.

    For example, a :file:`setup.py` file such as this:

    .. code:: python

        import setuptools

        setuptools.setup(
            name="my-project",
            version="1.2.3",
        )


    can be entirely replaced by a :file:`pyproject.toml` file like this:

    .. code:: toml

        [build-system]
        requires = ["setuptools"]
        build-backend = "setuptools.build_meta"

        [project]
        name = "my-project"
        version = "1.2.3"


    Read :ref:`pyproject-project-table` for the full specification of the content allowed in the ``[project]`` table.


如何处理动态元数据？
===============================

**How to handle dynamic metadata?**

.. tab:: 中文

    如果某些打包元数据字段不是静态的，
    则需要在 ``[project]`` 表中将其列为 ``dynamic``。

    例如，一个如下所示的 :file:`setup.py` 文件：

    .. code:: python

        import setuptools
        import some_build_toolkit

        def get_version():
            version = some_build_toolkit.compute_version()
            return version

        setuptools.setup(
            name="my-project",
            version=get_version(),
        )


    可以现代化为如下形式：

    .. code:: toml

        [build-system]
        requires = [
            "setuptools",
            "some-build-toolkit",
        ]
        build-backend = "setuptools.build_meta"

        [project]
        name = "my-project"
        dynamic = ["version"]


    .. code:: python

        import setuptools
        import some_build_toolkit

        def get_version():
            version = some_build_toolkit.compute_version()
            return version

        setuptools.setup(
            version=get_version(),
        )


    更多细节请参阅：

    * :ref:`declaring-project-metadata-dynamic`

.. tab:: 英文

    If some packaging metadata fields are not static
    they need to be listed as ``dynamic`` in this ``[project]`` table.

    For example, a :file:`setup.py` file such as this:

    .. code:: python

        import setuptools
        import some_build_toolkit

        def get_version():
            version = some_build_toolkit.compute_version()
            return version

        setuptools.setup(
            name="my-project",
            version=get_version(),
        )


    can be modernized as follows:

    .. code:: toml

        [build-system]
        requires = [
            "setuptools",
            "some-build-toolkit",
        ]
        build-backend = "setuptools.build_meta"

        [project]
        name = "my-project"
        dynamic = ["version"]


    .. code:: python

        import setuptools
        import some_build_toolkit

        def get_version():
            version = some_build_toolkit.compute_version()
            return version

        setuptools.setup(
            version=get_version(),
        )


    For more details:

    * :ref:`declaring-project-metadata-dynamic`


如果某些无法更改的内容需要 ``setup.py`` 文件怎么办？
======================================================================

**What if something that can not be changed expects a** ``setup.py`` **file?**

.. tab:: 中文

    例如，某些过程可能无法轻易更改，
    并且需要执行类似 ``python setup.py --name`` 的命令。

    即使所有内容已被移至 :file:`pyproject.toml`，
    在项目源代码树中保留 :file:`setup.py` 文件也是完全可以的。
    这个文件可以简化为如下形式：

    .. code:: python

        import setuptools

        setuptools.setup()

.. tab:: 英文

    For example, a process exists that can not be changed easily
    and it needs to execute a command such as ``python setup.py --name``.

    It is perfectly fine to leave a :file:`setup.py` file in the project source tree
    even after all its content has been moved to :file:`pyproject.toml`.
    This file can be as minimalistic as this:

    .. code:: python

        import setuptools

        setuptools.setup()


在哪里可以阅读更多相关信息？
==============================

**Where to read more about this?**

* :ref:`pyproject-toml-spec`
* :doc:`pip:reference/build-system/pyproject-toml`
* :doc:`setuptools:build_meta`
