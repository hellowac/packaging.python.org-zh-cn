.. _setup-py-deprecated:


===========================
``setup.py`` 已被弃用吗？
===========================

**Is** ``setup.py`` **deprecated?**

.. tab:: 中文

    不，:term:`setup.py` 和 :ref:`setuptools` 并没有被废弃。

    Setuptools 作为 Python 项目的 :term:`build backend`，完全可以正常使用。  
    而 :file:`setup.py` 是一个有效的配置文件，适用于 :ref:`setuptools`，它是用 Python 编写的，而不是例如 *TOML* 格式（其他工具，如 *nox* 及其 :file:`noxfile.py` 配置文件，或 *pytest* 和 :file:`conftest.py` 也采用了类似的做法）。

    然而，``python setup.py`` 和将 :file:`setup.py` 用作命令行工具的做法已经被废弃。

    这意味着以下命令 **不得** 再运行 :

    * ``python setup.py install``
    * ``python setup.py develop``
    * ``python setup.py sdist``
    * ``python setup.py bdist_wheel``

.. tab:: 英文

    No, :term:`setup.py` and :ref:`setuptools` are not deprecated.

    Setuptools is perfectly usable as a :term:`build backend`
    for packaging Python projects.
    And :file:`setup.py` is a valid configuration file for :ref:`setuptools`
    that happens to be written in Python, instead of in *TOML* for example
    (a similar practice is used by other tools
    like *nox* and its :file:`noxfile.py` configuration file,
    or *pytest* and :file:`conftest.py`).

    However, ``python setup.py`` and the use of :file:`setup.py`
    as a command line tool are deprecated.

    This means that commands such as the following **MUST NOT** be run anymore:

    * ``python setup.py install``
    * ``python setup.py develop``
    * ``python setup.py sdist``
    * ``python setup.py bdist_wheel``


应该使用什么命令？
=====================================

**What commands should be used instead?**

.. tab:: 中文

    +---------------------------------+----------------------------------------+
    | 已废弃                          | 推荐                                   |
    +=================================+========================================+
    | ``python setup.py install``     | ``python -m pip install .``            |
    +---------------------------------+----------------------------------------+
    | ``python setup.py develop``     | ``python -m pip install --editable .`` |
    +---------------------------------+----------------------------------------+
    | ``python setup.py sdist``       | ``python -m build`` [#needs-build1]_   |
    +---------------------------------+                                        |
    | ``python setup.py bdist_wheel`` |                                        |
    +---------------------------------+----------------------------------------+


    .. [#needs-build1] 这需要 :ref:`build` 依赖。  
        推荐始终构建并发布项目的源代码分发包（source distribution）和 wheel 包，这正是 ``python -m build`` 所做的。  
        如果需要，可以使用 ``--sdist`` 和 ``--wheel`` 选项来仅生成其中一个。

    为了安装基于 setuptools 的项目，过去常用运行 :file:`setup.py` 的 ``install`` 命令，例如 :  
    ``python setup.py install``。  
    如今，推荐的方法是直接使用 :ref:`pip`，命令如下 :  
    ``python -m pip install .``。  
    其中的点 ``.`` 实际上是一个文件系统路径，表示当前目录。  
    实际上，*pip* 接受一个指向项目源代码树目录的本地文件系统路径作为 ``install`` 子命令的参数。  
    因此，以下命令也是有效的 :  
    ``python -m pip install path/to/project``。

    至于 *develop* 模式（即可编辑模式）的安装，  
    不再使用 ``python setup.py develop``，  
    而是可以使用 pip 的 *install* 子命令中的 ``--editable`` 选项 :  
    ``python -m pip install --editable .``。

    构建 :term:`源代码分发包 <Source Distribution (or "sdist")>` 和 :term:`wheel` 的一种推荐、简单且直接的方法是使用 :ref:`build` 工具，命令如下 :  
    ``python -m build``，  
    该命令会触发生成这两种分发格式。  
    如有需要，可以使用 ``--sdist`` 和 ``--wheel`` 选项来仅生成其中一个。  
    请注意，build 工具需要单独安装。

    命令 ``python setup.py install`` 在 setuptools 版本 *58.3.0* 中已被废弃。

.. tab:: 英文

    +---------------------------------+----------------------------------------+
    | Deprecated                      | Recommendation                         |
    +=================================+========================================+
    | ``python setup.py install``     | ``python -m pip install .``            |
    +---------------------------------+----------------------------------------+
    | ``python setup.py develop``     | ``python -m pip install --editable .`` |
    +---------------------------------+----------------------------------------+
    | ``python setup.py sdist``       | ``python -m build`` [#needs-build]_    |
    +---------------------------------+                                        |
    | ``python setup.py bdist_wheel`` |                                        |
    +---------------------------------+----------------------------------------+


    .. [#needs-build] This requires the :ref:`build` dependency.
        It is recommended to always build and publish both the source distribution
        and wheel of a project, which is what ``python -m build`` does.
        If necessary the ``--sdist`` and ``--wheel`` options can be used
        to generate only one or the other.


    In order to install a setuptools based project,
    it was common to run :file:`setup.py`'s ``install`` command such as:
    ``python setup.py install``.
    Nowadays, the recommended method is to use :ref:`pip` directly
    with a command like this one: ``python -m pip install .``.
    Where the dot ``.`` is actually a file system path,
    it is the path notation for the current directory.
    Indeed, *pip* accepts a path to
    a project's source tree directory on the local filesystem
    as argument to its ``install`` sub-command.
    So this would also be a valid command:
    ``python -m pip install path/to/project``.

    As for the installation in *develop* mode aka *editable* mode,
    instead of ``python setup.py develop``
    one can use the ``--editable`` option of pip's *install* sub-command:
    ``python -m pip install --editable .``.

    One recommended, simple, and straightforward method of building
    :term:`source distributions <Source Distribution (or "sdist")>`
    and :term:`wheels <Wheel>`
    is to use the :ref:`build` tool with a command like
    ``python -m build``
    which triggers the generation of both distribution formats.
    If necessary the ``--sdist`` and ``--wheel`` options can be used
    to generate only one or the other.
    Note that the build tool needs to be installed separately.

    The command ``python setup.py install`` was deprecated
    in setuptools version *58.3.0*.


其他命令呢？
==========================

**What about other commands?**

.. tab:: 中文

    其他 ``python setup.py`` 命令有哪些替代品？

.. tab:: 英文

    What are some replacements for the other ``python setup.py`` commands?


``python setup.py test``
------------------------

.. tab:: 中文

    建议使用测试运行器，例如 pytest_ 。

.. tab:: 英文

    The recommendation is to use a test runner such as pytest_.

.. _pytest: https://docs.pytest.org/


``python setup.py check``, ``python setup.py register``, 和 ``python setup.py upload``
---------------------------------------------------------------------------------------

.. tab:: 中文

    一个值得信赖的替代工具是 :ref:`twine` :

    * ``python -m twine check --strict dist/*``
    * ``python -m twine register dist/*.whl`` [#not-pypi1]_
    * ``python -m twine upload dist/*``

    .. [#not-pypi1] 这在 :term:`PyPI <Python Package Index (PyPI)>` 上不是必需的，也不受支持。但在其他 :term:`包索引 <package index>` 上可能是必需的（例如：:ref:`devpi`）。

.. tab:: 英文

    A trusted replacement is :ref:`twine`:

    * ``python -m twine check --strict dist/*``
    * ``python -m twine register dist/*.whl`` [#not-pypi]_
    * ``python -m twine upload dist/*``

    .. [#not-pypi] Not necessary, nor supported on :term:`PyPI <Python Package Index (PyPI)>`. But might be necessary on other :term:`package indexes <package index>` (for example :ref:`devpi`).


``python setup.py --version``
-----------------------------

.. tab:: 中文

    一种可能的替代解决方案（除其他外）是依赖 setuptools-scm_ :

    * ``python -m setuptools_scm``

.. tab:: 英文

    A possible replacement solution (among others) is to rely on setuptools-scm_:

    * ``python -m setuptools_scm``

.. _setuptools-scm: https://setuptools-scm.readthedocs.io/en/latest/usage/#as-cli-tool


剩余命令
------------------

**Remaining commands**

.. tab:: 中文

    本指南不对这些命令提出替代解决方案的建议：

.. tab:: 英文

    This guide does not make suggestions of replacement solutions for those commands:

.. hlist::
    :columns: 4

    * ``alias``
    * ``bdist``
    * ``bdist_dumb``
    * ``bdist_egg``
    * ``bdist_rpm``
    * ``build``
    * ``build_clib``
    * ``build_ext``
    * ``build_py``
    * ``build_scripts``
    * ``clean``
    * ``dist_info``
    * ``easy_install``
    * ``editable_wheel``
    * ``egg_info``
    * ``install_data``
    * ``install_egg_info``
    * ``install_headers``
    * ``install_lib``
    * ``install_scripts``
    * ``rotate``
    * ``saveopts``
    * ``setopt``
    * ``upload_docs``


自定义命令呢？
===========================

**What about custom commands?**

.. tab:: 中文

    同样，自定义的 :file:`setup.py` 命令也已弃用。  
    建议将这些自定义命令迁移到任务运行工具或其他类似工具。  
    一些此类工具的例子包括: chuy、make、nox 或 tox、pydoit、pyinvoke、taskipy 和 thx。

.. tab:: 英文

    Likewise, custom :file:`setup.py` commands are deprecated.
    The recommendation is to migrate those custom commands
    to a task runner tool or any other similar tool.
    Some examples of such tools are:
    chuy, make, nox or tox, pydoit, pyinvoke, taskipy, and thx.


自定义构建步骤呢？
==============================

**What about custom build steps?**

.. tab:: 中文

    自定义构建步骤，例如覆盖现有步骤（如 ``build_py``、 ``build_ext`` 和 ``bdist_wheel``）或添加新的构建步骤，并未被弃用。这些步骤将会按预期自动调用。

.. tab:: 英文

    Custom build steps that for example
    either overwrite existing steps such as ``build_py``, ``build_ext``, and ``bdist_wheel``
    or add new build steps are not deprecated.
    Those will be automatically called as expected.


是否应删除 ``setup.py``？
===============================

**Should ``setup.py`` be deleted?**

.. tab:: 中文

    尽管将 :file:`setup.py` 用作可执行脚本的方式已被弃用，但作为 setuptools 的配置文件仍然是完全可行的。通常不需要修改 :file:`setup.py` 文件。

.. tab:: 英文

    Although the usage of :file:`setup.py` as an executable script is deprecated,
    its usage as a configuration file for setuptools is absolutely fine.
    There is likely no modification needed in :file:`setup.py`.


``pyproject.toml`` 是必需的吗？
================================

**Is ``pyproject.toml`` mandatory?**

.. tab:: 中文

    虽然目前从技术上讲还不是必需的，但 **强烈推荐** 项目在源代码树的根目录下拥有一个 :file:`pyproject.toml` 文件，其内容应如下所示：

    .. code:: toml

        [build-system]
        requires = ["setuptools"]
        build-backend = "setuptools.build_meta"

    关于此内容的更多细节，请参阅指南 :ref:`modernize-setup-py-project`。

    在没有 :file:`pyproject.toml` 文件及其 ``[build-system]`` 表的情况下，构建前端的标准回退行为是假设构建后端为 setuptools。

.. tab:: 英文

    While it is not technically necessary yet,
    it is **STRONGLY RECOMMENDED** for a project to have a :file:`pyproject.toml` file
    at the root of its source tree with a content like this:

    .. code:: toml

        [build-system]
        requires = ["setuptools"]
        build-backend = "setuptools.build_meta"


    The guide :ref:`modernize-setup-py-project` has more details about this.

    The standard fallback behavior for a :term:`build frontend <Build Frontend>`
    in the absence of a :file:`pyproject.toml` file and its ``[build-system]`` table
    is to assume that the :term:`build backend <Build Backend>` is setuptools.


为什么？这一切意味着什么？
===========================

**Why? What does it all mean?**

.. tab:: 中文

    一种看法是，setuptools 的范围现在已经缩小到构建后端的角色。

.. tab:: 英文

    One way to look at it is that the scope of setuptools
    has now been reduced to the role of a build backend.


在哪里可以阅读更多相关信息？
==============================

**Where to read more about this?**

* https://blog.ganssle.io/articles/2021/10/setup-py-deprecated.html

* :doc:`setuptools:deprecated/commands`
