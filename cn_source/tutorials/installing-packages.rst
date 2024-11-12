.. _installing-packages:

===================
安装软件包
===================

**Installing Packages**

.. tab:: 中文

    本节介绍了如何安装 Python :term:`包 <Distribution Package>` 的基础知识。

    需要注意的是，在此上下文中，“包(package)”一词用于描述一个软件包（即作为 :term:`distribution <Distribution Package>` 的同义词）进行安装。它并不指你在 Python 源代码中导入的 :term:`包 <Import Package>`（即一个模块容器）。在 Python 社区中，通常使用“包”一词来指代 :term:`distribution <Distribution Package>`，而不常使用“distribution”一词，因为它容易与 Linux 发行版或其他更大规模的软件发行版（如 Python 本身）混淆。

.. tab:: 英文

    This section covers the basics of how to install Python :term:`packages
    <Distribution Package>`.

    It's important to note that the term "package" in this context is being used to
    describe a bundle of software to be installed (i.e. as a synonym for a
    :term:`distribution <Distribution Package>`). It does not refer to the kind
    of :term:`package <Import Package>` that you import in your Python source code
    (i.e. a container of modules). It is common in the Python community to refer to
    a :term:`distribution <Distribution Package>` using the term "package".  Using
    the term "distribution" is often not preferred, because it can easily be
    confused with a Linux distribution, or another larger software distribution
    like Python itself.


.. _installing_requirements:

安装软件包的要求
====================================

**Requirements for Installing Packages**

.. tab:: 中文

    本节介绍安装其他 Python 包之前要遵循的步骤。

.. tab:: 英文

    This section describes the steps to follow before installing other Python packages.


确保您可以从命令行运行 Python
-----------------------------------------------

**Ensure you can run Python from the command line**

.. tab:: 中文

    在继续之前，请确保你已经安装了 Python，并且可以在命令行中访问到期望的版本。你可以通过运行以下命令来检查：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 --version

    .. tab:: Windows

        .. code-block:: bat

            py --version


    你应该看到类似 ``Python 3.6.3`` 的输出。如果你没有安装 Python，请从 `python.org`_ 安装最新的 3.x 版本，或者参考《Python 新手指南》中 :ref:`安装 Python <python-guide:installation>` 部分。

    .. note:: 如果你是新手，并且遇到类似以下错误：

        .. code-block:: pycon

            >>> python3 --version
            Traceback (most recent call last):
            File "<stdin>", line 1, in <module>
            NameError: name 'python3' is not defined

        这是因为这个命令以及本教程中其他推荐的命令是设计为在 *shell*（也称为 *terminal* 或 *console*）中运行的。有关使用操作系统 shell 并与 Python 交互的介绍，请参阅 Python 新手 `入门教程`_。

    .. note:: 如果你正在使用增强型 shell，如 IPython 或 Jupyter notebook，你可以通过在命令前加上 ``!`` 来运行本教程中的系统命令：

    .. code-block:: text

            In [1]: import sys
                    !{sys.executable} --version
            Python 3.6.3

    推荐使用 ``{sys.executable}`` 而不是直接使用 ``python`` ，这样可以确保命令在当前运行的 notebook 所匹配的 Python 安装中执行（这可能不同于 ``python`` 命令所指的 Python 安装）。

    .. note:: 
        
        由于大多数 Linux 发行版处理 Python 3 迁移的方式，使用系统 Python 而没有先创建虚拟环境的 Linux 用户，应该将本教程中的 ``python`` 命令替换为 ``python3``，并将 ``python -m pip`` 命令替换为 ``python3 -m pip --user``。请不要使用 ``sudo`` 运行本教程中的任何命令：如果你遇到权限错误，请返回到创建虚拟环境的部分，先设置一个虚拟环境，然后按照教程继续进行。

.. tab:: 英文

    Before you go any further, make sure you have Python and that the expected
    version is available from your command line. You can check this by running:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 --version

    .. tab:: Windows

        .. code-block:: bat

            py --version


    You should get some output like ``Python 3.6.3``. If you do not have Python,
    please install the latest 3.x version from `python.org`_ or refer to the
    :ref:`Installing Python <python-guide:installation>` section of the Hitchhiker's Guide to Python.

    .. Note:: If you're a newcomer and you get an error like this:

        .. code-block:: pycon

            >>> python3 --version
            Traceback (most recent call last):
            File "<stdin>", line 1, in <module>
            NameError: name 'python3' is not defined

        It's because this command and other suggested commands in this tutorial
        are intended to be run in a *shell* (also called a *terminal* or
        *console*). See the Python for Beginners `getting started tutorial`_ for
        an introduction to using your operating system's shell and interacting with
        Python.

    .. Note:: If you're using an enhanced shell like IPython or the Jupyter
    notebook, you can run system commands like those in this tutorial by
    prefacing them with a ``!`` character:

    .. code-block:: text

            In [1]: import sys
                    !{sys.executable} --version
            Python 3.6.3

    It's recommended to write ``{sys.executable}`` rather than plain ``python`` in
    order to ensure that commands are run in the Python installation matching
    the currently running notebook (which may not be the same Python
    installation that the ``python`` command refers to).

    .. Note:: Due to the way most Linux distributions are handling the Python 3
    migration, Linux users using the system Python without creating a virtual
    environment first should replace the ``python`` command in this tutorial
    with ``python3`` and the ``python -m pip`` command with ``python3 -m pip --user``. Do *not*
    run any of the commands in this tutorial with ``sudo``: if you get a
    permissions error, come back to the section on creating virtual environments,
    set one up, and then continue with the tutorial as written.

.. _getting started tutorial: https://opentechschool.github.io/python-beginners/en/getting_started.html#what-is-python-exactly
.. _python.org: https://www.python.org

确保您可以从命令行运行 pip
--------------------------------------------

**Ensure you can run pip from the command line**

.. tab:: 中文

    此外，你还需要确保 `pip` 可用。你可以通过运行以下命令来检查：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip --version

    .. tab:: Windows

        .. code-block:: bat

            py -m pip --version

    如果你是从源代码安装 Python，或者使用 `python.org`_ 提供的安装程序，或通过 `Homebrew`_ 安装的，那么你应该已经有了 pip。如果你使用 Linux，并且是通过操作系统的包管理器安装的，你可能需要单独安装 pip，参见 :doc:`/guides/installing-using-linux-tools`。

    如果 ``pip`` 尚未安装，可以先尝试从标准库中引导安装：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m ensurepip --default-pip

    .. tab:: Windows

        .. code-block:: bat

            py -m ensurepip --default-pip

    如果这仍然无法让你运行 ``python -m pip``，可以按照以下步骤操作：

    * 安全下载 `get-pip.py <https://bootstrap.pypa.io/get-pip.py>`_ [1]_

    * 运行 ``python get-pip.py``。[2]_ 这将安装或升级 pip。同时，如果没有安装，它还会安装 :ref:`setuptools` 和 :ref:`wheel`。

    .. warning::

        如果你使用的是由操作系统或其他包管理器管理的 Python 安装，请小心。get-pip.py 不会与这些工具协调，可能会导致系统处于不一致的状态。你可以使用 ``python get-pip.py --prefix=/usr/local/`` 将其安装到 ``/usr/local``，这是为本地安装的软件设计的路径。

.. tab:: 英文

    Additionally, you'll need to make sure you have :ref:`pip` available. You can
    check this by running:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip --version

    .. tab:: Windows

        .. code-block:: bat

            py -m pip --version

    If you installed Python from source, with an installer from `python.org`_, or
    via `Homebrew`_ you should already have pip. If you're on Linux and installed
    using your OS package manager, you may have to install pip separately, see
    :doc:`/guides/installing-using-linux-tools`.

    If ``pip`` isn't already installed, then first try to bootstrap it from the
    standard library:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m ensurepip --default-pip

    .. tab:: Windows

        .. code-block:: bat

            py -m ensurepip --default-pip

    If that still doesn't allow you to run ``python -m pip``:

    * Securely Download `get-pip.py <https://bootstrap.pypa.io/get-pip.py>`_ [1]_

    * Run ``python get-pip.py``. [2]_  This will install or upgrade pip. Additionally, it will install :ref:`setuptools` and :ref:`wheel` if they're not installed already.

    .. warning::

        Be cautious if you're using a Python install that's managed by your
        operating system or another package manager. get-pip.py does not
        coordinate with those tools, and may leave your system in an
        inconsistent state. You can use ``python get-pip.py --prefix=/usr/local/``
        to install in ``/usr/local`` which is designed for locally-installed
        software.

.. _Homebrew: https://brew.sh


确保 pip、setuptools 和 wheel 是最新的
------------------------------------------------

**Ensure pip, setuptools, and wheel are up to date**

.. tab:: 中文

    虽然仅使用 ``pip`` 就足够从预构建的二进制档案安装软件，但为了确保你也能从源代码档案安装，保持最新版本的 ``setuptools`` 和 ``wheel`` 项目是非常有用的：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade pip setuptools wheel

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade pip setuptools wheel

.. tab:: 英文

    While ``pip`` alone is sufficient to install from pre-built binary archives,
    up to date copies of the ``setuptools`` and ``wheel`` projects are useful
    to ensure you can also install from source archives:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade pip setuptools wheel

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade pip setuptools wheel

（可选）创建虚拟环境
----------------------------------------

**Optionally, create a virtual environment**

.. tab:: 中文

    有关详细信息，请参阅下文的 :ref:`部分 <Creating and using Virtual Environments>`，但这是在典型的 Linux 系统上使用的基本 :doc:`venv <python:library/venv>` [3]_ 命令：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m venv tutorial_env
            source tutorial_env/bin/activate

    .. tab:: Windows

        .. code-block:: bat

            py -m venv tutorial_env
            tutorial_env\Scripts\activate

    这将会在 ``tutorial_env`` 子目录中创建一个新的虚拟环境，并将当前 shell 配置为使用该环境作为默认的 ``python`` 环境。

.. tab:: 英文

    See :ref:`section below <Creating and using Virtual Environments>` for details,
    but here's the basic :doc:`venv <python:library/venv>` [3]_ command to use on a typical Linux system:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m venv tutorial_env
            source tutorial_env/bin/activate

    .. tab:: Windows

        .. code-block:: bat

            py -m venv tutorial_env
            tutorial_env\Scripts\activate

    This will create a new virtual environment in the ``tutorial_env`` subdirectory,
    and configure the current shell to use it as the default ``python`` environment.


.. _Creating and using Virtual Environments:

创建虚拟环境
=============================

**Creating Virtual Environments**

.. tab:: 中文

    Python "虚拟环境" 允许将 Python :term:`包 <Distribution Package>` 安装到特定应用程序的隔离位置，而不是全局安装。如果你希望安全地安装全局命令行工具，请参阅 :doc:`/guides/installing-stand-alone-command-line-tools`。

    想象一下，你有一个应用程序需要版本 1 的 LibFoo，而另一个应用程序需要版本 2。你如何同时使用这两个应用程序？如果你将所有内容安装到 `/usr/lib/python3.6/site-packages` （或你平台的标准位置），很容易陷入一个情况，不小心升级了一个不应该升级的应用程序。

    或者更普遍地，假设你想安装一个应用程序并保持不变。如果一个应用程序已经正常工作，那么它的库或库版本的任何变化都可能会导致应用程序崩溃。

    还有，如果你不能将 :term:`包 <Distribution Package>` 安装到全局的 site-packages 目录呢？例如，在共享主机上。

    在所有这些情况下，虚拟环境都可以帮助你。它们有自己的安装目录，不与其他虚拟环境共享库。

    目前，有两个常见的工具用于创建 Python 虚拟环境：

    * :doc:`venv <python:library/venv>` 从 Python 3.3 及更高版本开始默认可用，并且会在创建的虚拟环境中安装 :ref:`pip` （Python 3.4 及更高版本），（Python 3.12 之前的版本还会安装 :ref:`setuptools` ）。
    * :ref:`virtualenv` 需要单独安装，但支持 Python 2.7+ 和 Python 3.3+，并且默认情况下会将 :ref:`pip`、:ref:`setuptools` 和 :ref:`wheel` 安装到创建的虚拟环境中（不管 Python 版本如何）。

    基本用法如下：

    使用 :doc:`venv <python:library/venv>`：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m venv <DIR>
            source <DIR>/bin/activate

    .. tab:: Windows

        .. code-block:: bat

            py -m venv <DIR>
            <DIR>\Scripts\activate

    使用 :ref:`virtualenv`：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m virtualenv <DIR>
            source <DIR>/bin/activate

    .. tab:: Windows

        .. code-block:: bat

            virtualenv <DIR>
            <DIR>\Scripts\activate

    有关更多信息，请参阅 :doc:`venv <python:library/venv>` 文档或 :doc:`virtualenv <virtualenv:index>` 文档。

    在 Unix shell 下使用 :command:`source` 命令可以确保虚拟环境的变量设置在当前 shell 中，而不是在子进程中（子进程会消失，没有任何有用的效果）。

    在上述两种情况下，Windows 用户 *不应* 使用 :command:`source` 命令，而应该直接从命令行运行 :command:`activate` 脚本，如下所示：

    .. code-block:: bat

        <DIR>\Scripts\activate

    直接管理多个虚拟环境可能会变得繁琐，因此 :ref:`依赖管理教程 <managing-dependencies>` 介绍了一个更高层次的工具 :ref:`Pipenv`，它可以自动为你正在使用的每个项目和应用程序管理一个独立的虚拟环境。

.. tab:: 英文

    Python "Virtual Environments" allow Python :term:`packages <Distribution
    Package>` to be installed in an isolated location for a particular application,
    rather than being installed globally. If you are looking to safely install
    global command line tools,
    see :doc:`/guides/installing-stand-alone-command-line-tools`.

    Imagine you have an application that needs version 1 of LibFoo, but another
    application requires version 2. How can you use both these applications? If you
    install everything into /usr/lib/python3.6/site-packages (or whatever your
    platform’s standard location is), it’s easy to end up in a situation where you
    unintentionally upgrade an application that shouldn’t be upgraded.

    Or more generally, what if you want to install an application and leave it be?
    If an application works, any change in its libraries or the versions of those
    libraries can break the application.

    Also, what if you can’t install :term:`packages <Distribution Package>` into the
    global site-packages directory? For instance, on a shared host.

    In all these cases, virtual environments can help you. They have their own
    installation directories and they don’t share libraries with other virtual
    environments.

    Currently, there are two common tools for creating Python virtual environments:

    * :doc:`venv <python:library/venv>` is available by default in Python 3.3 and later, and installs
    :ref:`pip` into created virtual environments in Python 3.4 and later
    (Python versions prior to 3.12 also installed :ref:`setuptools`).
    * :ref:`virtualenv` needs to be installed separately, but supports Python 2.7+
    and Python 3.3+, and :ref:`pip`, :ref:`setuptools` and :ref:`wheel` are
    always installed into created virtual environments by default (regardless of
    Python version).

    The basic usage is like so:

    Using :doc:`venv <python:library/venv>`:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m venv <DIR>
            source <DIR>/bin/activate

    .. tab:: Windows

        .. code-block:: bat

            py -m venv <DIR>
            <DIR>\Scripts\activate

    Using :ref:`virtualenv`:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m virtualenv <DIR>
            source <DIR>/bin/activate

    .. tab:: Windows

        .. code-block:: bat

            virtualenv <DIR>
            <DIR>\Scripts\activate

    For more information, see the :doc:`venv <python:library/venv>` docs or
    the :doc:`virtualenv <virtualenv:index>` docs.

    The use of :command:`source` under Unix shells ensures
    that the virtual environment's variables are set within the current
    shell, and not in a subprocess (which then disappears, having no
    useful effect).

    In both of the above cases, Windows users should *not* use the
    :command:`source` command, but should rather run the :command:`activate`
    script directly from the command shell like so:

    .. code-block:: bat

    <DIR>\Scripts\activate



    Managing multiple virtual environments directly can become tedious, so the
    :ref:`dependency management tutorial <managing-dependencies>` introduces a
    higher level tool, :ref:`Pipenv`, that automatically manages a separate
    virtual environment for each project and application that you work on.


使用 pip 进行安装
======================

**Use pip for Installing**

.. tab:: 中文

    :ref:`pip` 是推荐的安装工具。下面，我们将介绍最常见的使用场景。有关更多详细信息，请参阅 :doc:`pip 文档 <pip:index>`，其中包括完整的 :doc:`参考指南 <pip:cli/index>`。

.. tab:: 英文

    :ref:`pip` is the recommended installer.  Below, we'll cover the most common
    usage scenarios. For more detail, see the :doc:`pip docs <pip:index>`,
    which includes a complete :doc:`Reference Guide <pip:cli/index>`.


从 PyPI 安装
====================

**Installing from PyPI**

.. tab:: 中文

    最常见的 :ref:`pip` 用法是使用 :term:`要求规范符 <Requirement Specifier>` 从 :term:`Python 包索引 <Python Package Index (PyPI)>` 安装包。一般来说，要求规范符由项目名称和可选的 :term:`版本规范符 <Version Specifier>` 组成。有关支持的所有规范符的完整描述，请参见 :ref:`版本规范符规范 <version-specifiers>`。以下是一些示例。

    安装 "SomeProject" 的最新版本：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install "SomeProject"

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomeProject"

    安装特定版本：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install "SomeProject==1.4"

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomeProject==1.4"

    安装一个版本大于或等于某个版本且小于另一个版本：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install "SomeProject>=1,<2"

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomeProject>=1,<2"

    安装与某个特定版本 :ref:`兼容 <version-specifiers-compatible-release>` 的版本：[4]_

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install "SomeProject~=1.4.2"

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomeProject~=1.4.2"

    在这种情况下，意味着安装任何 "==1.4.*" 且版本 ">=1.4.2" 的版本。

.. tab:: 英文

    The most common usage of :ref:`pip` is to install from the :term:`Python Package
    Index <Python Package Index (PyPI)>` using a :term:`requirement specifier
    <Requirement Specifier>`. Generally speaking, a requirement specifier is
    composed of a project name followed by an optional :term:`version specifier
    <Version Specifier>`.  A full description of the supported specifiers can be
    found in the :ref:`Version specifier specification <version-specifiers>`.
    Below are some examples.

    To install the latest version of "SomeProject":

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install "SomeProject"

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomeProject"

    To install a specific version:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install "SomeProject==1.4"

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomeProject==1.4"

    To install greater than or equal to one version and less than another:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install "SomeProject>=1,<2"

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomeProject>=1,<2"


    To install a version that's :ref:`compatible <version-specifiers-compatible-release>`
    with a certain version: [4]_

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install "SomeProject~=1.4.2"

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomeProject~=1.4.2"

    In this case, this means to install any version "==1.4.*" version that's also ">=1.4.2".


源发行版与 Wheels
==============================

**Source Distributions vs Wheels**

.. tab:: 中文

    :ref:`pip` 可以从 :term:`源代码分发包 (sdist) <Source Distribution (or "sdist")>` 或 :term:`Wheel` 安装软件包，但如果 PyPI 上同时存在两者，pip 会优先选择兼容的 :term:`wheel <Wheel>`。你可以通过使用例如 :ref:`--no-binary <pip:install_--no-binary>` 选项来覆盖 pip 的默认行为。

    :term:`Wheel` 是一种预构建的 :term:`分发包 <Distribution Package>` 格式，与 :term:`源代码分发包 (sdist) <Source Distribution (or "sdist")>` 相比，它提供了更快的安装速度，尤其是当项目包含编译的扩展时。

    如果 :ref:`pip` 找不到合适的 wheel 文件来安装，它将会本地构建一个 wheel 并将其缓存，以便将来安装时使用，而不是每次都重新构建源代码分发包。

.. tab:: 英文

    :ref:`pip` can install from either :term:`Source Distributions (sdist) <Source
    Distribution (or "sdist")>` or :term:`Wheels <Wheel>`, but if both are present
    on PyPI, pip will prefer a compatible :term:`wheel <Wheel>`. You can override
    pip`s default behavior by e.g. using its :ref:`--no-binary
    <pip:install_--no-binary>` option.

    :term:`Wheels <Wheel>` are a pre-built :term:`distribution <Distribution
    Package>` format that provides faster installation compared to :term:`Source
    Distributions (sdist) <Source Distribution (or "sdist")>`, especially when a
    project contains compiled extensions.

    If :ref:`pip` does not find a wheel to install, it will locally build a wheel
    and cache it for future installs, instead of rebuilding the source distribution
    in the future.


升级软件包
==================

**Upgrading packages**

.. tab:: 中文

    将已安装的 ``SomeProject`` 升级到 PyPI 上的最新版本。

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade SomeProject

.. tab:: 英文

    Upgrade an already installed ``SomeProject`` to the latest from PyPI.

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade SomeProject

.. _`Installing to the User Site`:

安装到用户站点
===========================

**Installing to the User Site**

.. tab:: 中文

    要安装仅限当前用户的 :term:`包 <Distribution Package>`，请使用 ``--user`` 标志：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --user SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --user SomeProject

    有关更多信息，请参阅 pip 文档中的 `用户安装 <https://pip.pypa.io/en/latest/user_guide/#user-installs>`_ 部分。

    请注意，在虚拟环境中使用 ``--user`` 标志不会产生任何效果——所有安装命令将影响虚拟环境。

    如果 ``SomeProject`` 定义了任何命令行脚本或控制台入口点，使用 ``--user`` 标志会将它们安装到 `user base`_ 的二进制目录中，该目录可能已经存在，也可能不存在于你的 shell 的 :envvar:`PATH` 中。（从版本 10 开始，pip 会在将脚本安装到 :envvar:`PATH` 之外的目录时显示警告。）如果安装后脚本在 shell 中不可用，你需要将该目录添加到你的 :envvar:`PATH` :

    - 在 Linux 和 macOS 上，你可以通过运行 ``python -m site --user-base`` 来找到用户基础的二进制目录，并在末尾加上 ``bin``。例如，通常会返回 ``~/.local`` （其中 ``~`` 会展开为你的主目录的绝对路径），因此你需要将 ``~/.local/bin`` 添加到你的 ``PATH``。你可以通过 `修改 ~/.profile <modifying ~/.profile>`_ 来永久设置你的 ``PATH``。

    - 在 Windows 上，你可以通过运行 ``py -m site --user-site`` 来找到用户基础的二进制目录，然后将 ``site-packages`` 替换为 ``Scripts``。例如，这可能返回 ``C:\Users\Username\AppData\Roaming\Python36\site-packages``，因此你需要将 ``C:\Users\Username\AppData\Roaming\Python36\Scripts`` 添加到你的 ``PATH``。你可以在 `控制面板 <Control Panel>`_ 中永久设置用户的 ``PATH``。你可能需要注销账户才能使 ``PATH`` 更改生效。

.. tab:: 英文

    To install :term:`packages <Distribution Package>` that are isolated to the
    current user, use the ``--user`` flag:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --user SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --user SomeProject

    For more information see the `User Installs
    <https://pip.pypa.io/en/latest/user_guide/#user-installs>`_ section
    from the pip docs.

    Note that the ``--user`` flag has no effect when inside a virtual environment
    - all installation commands will affect the virtual environment.

    If ``SomeProject`` defines any command-line scripts or console entry points,
    ``--user`` will cause them to be installed inside the `user base`_'s binary
    directory, which may or may not already be present in your shell's
    :envvar:`PATH`.  (Starting in version 10, pip displays a warning when
    installing any scripts to a directory outside :envvar:`PATH`.)  If the scripts
    are not available in your shell after installation, you'll need to add the
    directory to your :envvar:`PATH`:

    - On Linux and macOS you can find the user base binary directory by running
    ``python -m site --user-base`` and adding ``bin`` to the end. For example,
    this will typically print ``~/.local`` (with ``~`` expanded to the absolute
    path to your home directory) so you'll need to add ``~/.local/bin`` to your
    ``PATH``.  You can set your ``PATH`` permanently by `modifying ~/.profile`_.

    - On Windows you can find the user base binary directory by running ``py -m
    site --user-site`` and replacing ``site-packages`` with ``Scripts``. For
    example, this could return
    ``C:\Users\Username\AppData\Roaming\Python36\site-packages`` so you would
    need to set your ``PATH`` to include
    ``C:\Users\Username\AppData\Roaming\Python36\Scripts``. You can set your user
    ``PATH`` permanently in the `Control Panel`_. You may need to log out for the
    ``PATH`` changes to take effect.

.. _user base: https://docs.python.org/3/library/site.html#site.USER_BASE
.. _modifying ~/.profile: https://stackoverflow.com/a/14638025
.. _Control Panel: https://docs.microsoft.com/en-us/windows/win32/shell/user-environment-variables?redirectedfrom=MSDN

要求文件
==================

**Requirements files**

.. tab:: 中文

    安装在 :ref:`要求文件 <pip:Requirements Files>` 中指定的需求列表。

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install -r requirements.txt

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install -r requirements.txt

.. tab:: 英文

    Install a list of requirements specified in a :ref:`Requirements File
    <pip:Requirements Files>`.

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install -r requirements.txt

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install -r requirements.txt

从 VCS 安装
===================

**Installing from VCS**

.. tab:: 中文

    从版本控制系统 (VCS) 安装一个项目，并以“可编辑”模式进行安装。有关完整的语法说明，请参阅 pip 的 :ref:`VCS 支持 <pip:VCS Support>` 部分。

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install -e SomeProject @ git+https://git.repo/some_pkg.git          # 从 git 安装
            python3 -m pip install -e SomeProject @ hg+https://hg.repo/some_pkg                # 从 mercurial 安装
            python3 -m pip install -e SomeProject @ svn+svn://svn.repo/some_pkg/trunk/         # 从 svn 安装
            python3 -m pip install -e SomeProject @ git+https://git.repo/some_pkg.git@feature  # 从某个分支安装

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install -e SomeProject @ git+https://git.repo/some_pkg.git          # 从 git 安装
            py -m pip install -e SomeProject @ hg+https://hg.repo/some_pkg                # 从 mercurial 安装
            py -m pip install -e SomeProject @ svn+svn://svn.repo/some_pkg/trunk/         # 从 svn 安装
            py -m pip install -e SomeProject @ git+https://git.repo/some_pkg.git@feature  # 从某个分支安装

.. tab:: 英文

    Install a project from VCS in "editable" mode.  For a full breakdown of the
    syntax, see pip's section on :ref:`VCS Support <pip:VCS Support>`.

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install -e SomeProject @ git+https://git.repo/some_pkg.git          # from git
            python3 -m pip install -e SomeProject @ hg+https://hg.repo/some_pkg                # from mercurial
            python3 -m pip install -e SomeProject @ svn+svn://svn.repo/some_pkg/trunk/         # from svn
            python3 -m pip install -e SomeProject @ git+https://git.repo/some_pkg.git@feature  # from a branch

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install -e SomeProject @ git+https://git.repo/some_pkg.git          # from git
            py -m pip install -e SomeProject @ hg+https://hg.repo/some_pkg                # from mercurial
            py -m pip install -e SomeProject @ svn+svn://svn.repo/some_pkg/trunk/         # from svn
            py -m pip install -e SomeProject @ git+https://git.repo/some_pkg.git@feature  # from a branch

从其他索引安装
=============================

**Installing from other Indexes**

.. tab:: 中文

    从备用索引安装

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url http://my.package.repo/simple/ SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url http://my.package.repo/simple/ SomeProject

    在安装过程中，除了 :term:`PyPI <Python Package Index (PyPI)>` 之外，还搜索其他索引

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --extra-index-url http://my.package.repo/simple SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --extra-index-url http://my.package.repo/simple SomeProject

.. tab:: 英文

    Install from an alternate index

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url http://my.package.repo/simple/ SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url http://my.package.repo/simple/ SomeProject

    Search an additional index during install, in addition to :term:`PyPI <Python Package Index (PyPI)>`

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --extra-index-url http://my.package.repo/simple SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --extra-index-url http://my.package.repo/simple SomeProject

从本地 src 树安装
================================

**Installing from a local src tree**

.. tab:: 中文

    以开发模式从本地源安装项目，即使项目看起来已安装，但仍可以从源代码树中进行编辑。

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install -e <path>

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install -e <path>

    你也可以从源代码中正常安装：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install <path>

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install <path>

.. tab:: 英文


    Installing from local src in
    :doc:`Development Mode <setuptools:userguide/development_mode>`,
    i.e. in such a way that the project appears to be installed, but yet is
    still editable from the src tree.

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install -e <path>

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install -e <path>

    You can also install normally from src

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install <path>

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install <path>

从本地存档安装
==============================

**Installing from local archives**

.. tab:: 中文

    安装特定的源代码归档文件。

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install ./downloads/SomeProject-1.0.4.tar.gz

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install ./downloads/SomeProject-1.0.4.tar.gz

    从包含归档文件的本地目录安装（并且不检查 :term:`PyPI <Python Package Index (PyPI)>`）：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --no-index --find-links=file:///local/dir/ SomeProject
            python3 -m pip install --no-index --find-links=/local/dir/ SomeProject
            python3 -m pip install --no-index --find-links=relative/dir/ SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --no-index --find-links=file:///local/dir/ SomeProject
            py -m pip install --no-index --find-links=/local/dir/ SomeProject
            py -m pip install --no-index --find-links=relative/dir/ SomeProject

.. tab:: 英文

    Install a particular source archive file.

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install ./downloads/SomeProject-1.0.4.tar.gz

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install ./downloads/SomeProject-1.0.4.tar.gz

    Install from a local directory containing archives (and don't check :term:`PyPI
    <Python Package Index (PyPI)>`)

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --no-index --find-links=file:///local/dir/ SomeProject
            python3 -m pip install --no-index --find-links=/local/dir/ SomeProject
            python3 -m pip install --no-index --find-links=relative/dir/ SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --no-index --find-links=file:///local/dir/ SomeProject
            py -m pip install --no-index --find-links=/local/dir/ SomeProject
            py -m pip install --no-index --find-links=relative/dir/ SomeProject

从其他来源安装
=============================

**Installing from other sources**

.. tab:: 中文

    要从其他数据源（例如 Amazon S3 存储）安装，你可以创建一个辅助应用程序，将数据以符合 :ref:`simple repository API <simple-repository-api>` 格式提供，并使用 ``--extra-index-url`` 标志将 pip 指向该索引。

    .. code-block:: bash

        ./s3helper --port=7777
        python -m pip install --extra-index-url http://localhost:7777 SomeProject

.. tab:: 英文

    To install from other data sources (for example Amazon S3 storage)
    you can create a helper application that presents the data
    in a format compliant with the :ref:`simple repository API <simple-repository-api>`:,
    and use the ``--extra-index-url`` flag to direct pip to use that index.

    .. code-block:: bash

        ./s3helper --port=7777
        python -m pip install --extra-index-url http://localhost:7777 SomeProject


安装预发布版本
======================

**Installing Prereleases**

.. tab:: 中文

    查找预发布和开发版本，除了稳定版本外。默认情况下，pip 只会查找稳定版本。

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --pre SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --pre SomeProject

.. tab:: 英文

    Find pre-release and development versions, in addition to stable versions.  By
    default, pip only finds stable versions.

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --pre SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --pre SomeProject

安装“附加内容”
===================

**Installing "Extras"**

.. tab:: 中文

    Extras 是软件包的可选“变体(variants)”，可能包含额外的依赖项，从而启用软件包的附加功能。如果你希望安装某个包的额外功能（假设该包提供了额外功能），可以在 pip 安装命令中包括它：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install 'SomePackage[PDF]'
            python3 -m pip install 'SomePackage[PDF]==3.0'
            python3 -m pip install -e '.[PDF]'  # 当前目录中的可编辑项目

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomePackage[PDF]"
            py -m pip install "SomePackage[PDF]==3.0"
            py -m pip install -e ".[PDF]"  # 当前目录中的可编辑项目

.. tab:: 英文

    Extras are optional "variants" of a package, which may include
    additional dependencies, and thereby enable additional functionality
    from the package.  If you wish to install an extra for a package which
    you know publishes one, you can include it in the pip installation command:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install 'SomePackage[PDF]'
            python3 -m pip install 'SomePackage[PDF]==3.0'
            python3 -m pip install -e '.[PDF]'  # editable project in current directory

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "SomePackage[PDF]"
            py -m pip install "SomePackage[PDF]==3.0"
            py -m pip install -e ".[PDF]"  # editable project in current directory

----

.. [1] "Secure" in this context means using a modern browser or a
       tool like :command:`curl` that verifies SSL certificates when
       downloading from https URLs.

.. [2] Depending on your platform, this may require root or Administrator
       access. :ref:`pip` is currently considering changing this by `making user
       installs the default behavior
       <https://github.com/pypa/pip/issues/1668>`_.

.. [3] Beginning with Python 3.4, ``venv`` (a stdlib alternative to
       :ref:`virtualenv`) will create virtualenv environments with ``pip``
       pre-installed, thereby making it an equal alternative to
       :ref:`virtualenv`.

.. [4] The compatible release specifier was accepted in :pep:`440`
       and support was released in :ref:`setuptools` v8.0 and
       :ref:`pip` v6.0
