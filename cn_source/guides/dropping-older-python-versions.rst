.. _`Dropping support for older Python versions`:

==========================================
放弃对旧版 Python 的支持
==========================================

**Dropping support for older Python versions**

.. tab:: 中文

    通过标准的 :ref:`核心元数据 <core-metadata>` 1.2 规范中的 :ref:`“Requires-Python” <core-metadata-requires-python>` 属性，可以实现停止支持旧版 Python 的功能。

    像 Pip 这样的 Metadata 1.2+ 安装工具将会依据此规范，通过匹配当前的 Python 运行环境并与包元数据中的要求版本进行比较。如果不匹配，它会尝试安装最后一个支持该 Python 版本的包分发版本。

    此机制可以通过在包元数据中修改 ``Requires-Python`` 属性来停止对旧版 Python 的支持。

.. tab:: 英文

    The ability to drop support for older Python versions is enabled by the standard :ref:`core-metadata` 1.2 specification via the :ref:`"Requires-Python" <core-metadata-requires-python>` attribute.

    Metadata 1.2+ installers, such as Pip, will adhere to this specification by matching the current Python runtime and comparing it with the required version
    in the package metadata. If they do not match, it will attempt to install the last package distribution that supported that Python runtime.

    This mechanism can be used to drop support for older Python versions, by amending the ``Requires-Python`` attribute in the package metadata.

要求
------------

**Requirements**

.. tab:: 中文

    此工作流要求安装包的用户使用 Pip [#]_ 或其他支持 Metadata 1.2 规范的安装工具。

.. tab:: 英文

    This workflow requires that the user installing the package uses Pip [#]_, or another installer that supports the Metadata 1.2 specification.

处理通用轮子
---------------------------------

**Dealing with the universal wheels**

.. tab:: 中文

    传统上，提供与 Python 2 和 Python 3 语义兼容的 Python 代码的 :ref:`setuptools` 项目，会生成在名称中带有 ``py2.py3`` 标签的 :term:`wheels <Wheel>`。在放弃对 Python 2 的支持时，重要的是不要忘记将这个标签更改为仅 ``py3``。这个配置通常在 :file:`setup.cfg` 文件的 ``[bdist_wheel]`` 部分中，通过设置 ``universal = 1`` 来完成。

    如果你使用此方法，应该移除该选项或部分，或者显式将 ``universal`` 设置为 ``0``：

    .. code-block:: ini

        # setup.cfg

        [bdist_wheel]
        universal = 0  # Make the generated wheels have "py3" tag

    .. hint::

        关于 :ref:`deprecated <setup-py-deprecated>` 直接调用 ``setup.py``，在命令行中传递 ``--universal`` 标志可能会覆盖此设置。

.. tab:: 英文

    Traditionally, :ref:`setuptools` projects providing Python code that is semantically
    compatible with both Python 2 and Python 3, produce :term:`wheels
    <Wheel>` that have a ``py2.py3`` tag in their names. When dropping
    support for Python 2, it is important not to forget to change this tag
    to just ``py3``. It is often configured within :file:`setup.cfg` under
    the ``[bdist_wheel]`` section by setting ``universal = 1``.

    If you use this method, either remove this option or section, or
    explicitly set ``universal`` to ``0``:

    .. code-block:: ini

        # setup.cfg

        [bdist_wheel]
        universal = 0  # Make the generated wheels have "py3" tag

    .. hint::

    Regarding :ref:`deprecated <setup-py-deprecated>` direct ``setup.py`` invocations,
    passing the ``--universal`` flag on the command line could override this setting.

定义所需的 Python 版本
------------------------------------

**Defining the Python version required**

.. tab:: 中文

    

.. tab:: 英文

1. 安装 twine
~~~~~~~~~~~~~~~~

**1. Install twine**

.. tab:: 中文

    确保你安装了最新版本的 twine。
    步骤：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade twine

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade twine

.. tab:: 英文

    Ensure that you have twine available at its latest version.
    Steps:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade twine

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade twine

2. 指定受支持的 Python 发行版的版本范围
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**2. Specify the version ranges for supported Python distributions**

.. tab:: 中文

    在你的项目的 :file:`pyproject.toml` 文件中设置声明支持哪些 Python 版本范围。 :ref:`requires-python` 配置字段对应于 :ref:`Requires-Python <core-metadata-requires-python>` 核心元数据字段：

    .. code-block:: toml

        [build-system]
        ...

        [project]
        requires-python = ">= 3.8" # 至少需要 Python 3.8

    你可以指定版本范围和排除规则（遵循 :ref:`version-specifiers` 规范），例如至少 Python 3.9，或者至少 Python 3.7 及以后的版本，但跳过 3.7.0 和 3.7.1 这两个小版本：

    .. code-block:: toml

        requires-python = ">= 3.9"
        requires-python = ">= 3.7, != 3.7.0, != 3.7.1"

    如果使用 :ref:`setuptools` 构建后端，请参阅 `dependency-management`_ 文档了解更多选项。

    .. caution::
            避免在版本范围中添加上限，例如 ``">= 3.8, < 3.10"``。这样做可能会导致不同的错误和版本冲突。有关更多信息，请参阅 `discourse-discussion`_。

.. tab:: 英文

    Set the version ranges declaring which Python distributions are supported
    within your project's :file:`pyproject.toml`. The :ref:`requires-python` configuration field
    corresponds to the :ref:`Requires-Python <core-metadata-requires-python>` core metadata field:

    .. code-block:: toml

        [build-system]
        ...

        [project]
        requires-python = ">= 3.8" # At least Python 3.8

    You can specify version ranges and exclusion rules (complying with the :ref:`version-specifiers` specification),
    such as at least Python 3.9. Or, at least Python 3.7 and beyond, skipping the 3.7.0 and 3.7.1 point releases:

    .. code-block:: toml

        requires-python = ">= 3.9"
        requires-python = ">= 3.7, != 3.7.0, != 3.7.1"


    If using the :ref:`setuptools` build backend, consult the `dependency-management`_ documentation for more options.

    .. caution::
            Avoid adding upper bounds to the version ranges, e. g. ``">= 3.8, < 3.10"``. Doing so can cause different errors
            and version conflicts. See the `discourse-discussion`_ for more information.

3. 发布前验证元数据
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**3. Validating the Metadata before publishing**

.. tab:: 中文

    在 Python 源代码包（即你下载的 zip 或 tar-gz 文件）中，有一个名为 PKG-INFO 的文本文件。

    此文件由 :term:`build backend <Build Backend>` 在生成源代码包时生成。文件包含一组键值对，键的列表是 PyPA 标准元数据格式的一部分。

    你可以通过以下命令查看生成的文件内容：

    .. code-block:: bash

        tar xfO dist/my-package-1.0.0.tar.gz my-package-1.0.0/PKG-INFO

    在发布包之前，请验证以下内容是否就绪：

    - 如果已正确升级， ``Metadata-Version`` 的值应为 1.2 或更高版本。
    - ``Requires-Python`` 字段已设置，并与配置文件中的规范相匹配。

.. tab:: 英文

    Within a Python source package (the zip or the tar-gz file you download) is a text file called PKG-INFO.

    This file is generated by the :term:`build backend <Build Backend>` when it generates the source package.
    The file contains a set of keys and values, the list of keys is part of the PyPA standard metadata format.

    You can see the contents of the generated file like this:

    .. code-block:: bash

        tar xfO dist/my-package-1.0.0.tar.gz my-package-1.0.0/PKG-INFO

    Validate that the following is in place, before publishing the package:

    - If you have upgraded correctly, the ``Metadata-Version`` value should be 1.2 or higher.
    - The ``Requires-Python`` field is set and matches your specification in the configuration file.

4. 发布包
~~~~~~~~~~~~~~~~~~~~~~~~~

**4. Publishing the package**

.. tab:: 中文

    按照 :ref:`Uploading your Project to PyPI` 中的建议进行操作。

.. tab:: 英文

    Proceed as suggested in :ref:`Uploading your Project to PyPI`.

放弃 Python 版本
-------------------------

**Dropping a Python version**

.. tab:: 中文

    原则上，至少应该尽可能长时间保留对 Python 版本的元数据支持，因为一旦删除了对某个版本的支持，仍然依赖该版本的用户将被迫降级。然而，如果支持特定版本成为新功能的阻碍，或者出现其他问题，则应修改元数据中的 ``Requires-Python`` 字段。当然，这也取决于项目是否需要保持稳定并覆盖更广泛的用户群体。

    每次版本兼容性的变更应该有自己的发布版本。

    .. 提示::

            在放弃支持某个 Python 版本时，除了更新可见位置（如测试环境）中使用的版本外，通常也值得升级项目的代码语法。像 pyupgrade_ 或 `ruff <https://docs.astral.sh/ruff/linter/>`_ 这样的工具可以自动化一些工作。

.. tab:: 英文

    In principle, at least metadata support for Python versions should be kept as long as possible, because
    once that has been dropped, people still depending on a version will be forced to downgrade.
    If however supporting a specific version becomes a blocker for a new feature or other issues occur, the metadata
    ``Requires-Python`` should be amended. Of course this also depends on whether the project needs to be stable and
    well-covered for a wider range of users.

    Each version compatibility change should have its own release.

    .. tip::

            When dropping a Python version, it might also be rewarding to upgrade the project's code syntax generally, apart from updating the versions used in visible places (like the testing environment). Tools like pyupgrade_ or `ruff <https://docs.astral.sh/ruff/linter/>`_ can automate some of this work.

.. _discourse-discussion: https://discuss.python.org/t/requires-python-upper-limits/12663
.. _pyupgrade: https://pypi.org/project/pyupgrade/
.. _dependency-management: https://setuptools.pypa.io/en/latest/userguide/dependency_management.html#python-requirement

.. [#] Pip 9.0 中增加了对 Metadata 1.2 规范的支持。
.. [#] Support for the Metadata 1.2 specification has been added in Pip 9.0.
