使用 pip 和 venv 在虚拟环境中安装软件包
============================================================

**Install packages in a virtual environment using pip and venv**

.. tab:: 中文

    本指南讨论了如何使用标准库中的虚拟环境工具 :ref:`venv` 创建和激活虚拟环境并安装包。该指南涵盖了以下内容：

    * 创建并激活虚拟环境
    * 准备 pip
    * 使用 ``pip`` 命令安装包到虚拟环境
    * 使用和创建 requirements 文件

    .. note:: 本指南适用于支持的 Python 版本，目前为 3.8 及更高版本。

    .. note:: 本指南使用 **包** 这个术语来指代 :term:`分发包`（Distribution Package），通常是从外部主机安装的。这与 :term:`导入包`（Import Package）有所不同，后者指的是在你的 Python 源代码中导入的模块。

    .. important::
        本指南的前提是你正在使用从 `<https://www.python.org/downloads/>`_ 获取的官方 Python 版本。如果你是通过操作系统的包管理器安装 Python，请确保 Python 已正确安装，再继续进行以下步骤。

.. tab:: 英文

    This guide discusses how to create and activate a virtual environment using
    the standard library's virtual environment tool :ref:`venv` and install packages.
    The guide covers how to:

    * Create and activate a virtual environment
    * Prepare pip
    * Install packages into a virtual environment using the ``pip`` command
    * Use and create a requirements file


    .. note:: This guide applies to supported versions of Python, currently 3.8 and higher.


    .. note:: This guide uses the term **package** to refer to a :term:`Distribution Package`, which commonly is installed from an external host. This differs from the term :term:`Import Package` which refers to import modules in your Python source code.


    .. important:: This guide has the prerequisite that you are using an official Python version obtained from `<https://www.python.org/downloads/>`_. If you are using your operating system's package manager to install Python, please ensure that Python is installed before proceeding with these steps.


创建和使用虚拟环境
-----------------------------------

**Create and Use Virtual Environments**

创建新的虚拟环境
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Create a new virtual environment**

.. tab:: 中文

    :ref:`venv` （适用于 Python 3）允许你为不同的项目管理独立的包安装。它创建了一个“虚拟”的隔离 Python 安装。当你切换项目时，可以创建一个新的虚拟环境，这个环境与其他虚拟环境是隔离的。你可以放心地安装包，因为它们不会与其他项目的环境产生冲突。

    .. tip::
        建议在使用第三方包时，使用虚拟环境。

    要创建一个虚拟环境，进入你的项目目录并运行以下命令。这将在本地创建一个名为 ``.venv`` 的新虚拟环境文件夹：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m venv .venv

    .. tab:: Windows

        .. code-block:: bat

            py -m venv .venv

    第二个参数是虚拟环境的创建位置。通常，你可以直接在项目中创建这个文件夹，并命名为 ``.venv`` 。

    ``venv`` 会在 ``.venv`` 文件夹中创建一个虚拟的 Python 安装。

    .. Note:: 你应该使用 ``.gitignore`` 或类似工具将虚拟环境目录排除在版本控制系统之外。

.. tab:: 英文

    :ref:`venv` (for Python 3) allows you to manage separate package installations for
    different projects. It creates a "virtual" isolated Python installation. When
    you switch projects, you can create a new virtual environment which is isolated
    from other virtual environments. You benefit from the virtual environment
    since packages can be installed confidently and will not interfere with
    another project's environment.

    .. tip::
    It is recommended to use a virtual environment when working with third
    party packages.

    To create a virtual environment, go to your project's directory and run the
    following command. This will create a new virtual environment in a local folder
    named ``.venv``:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m venv .venv

    .. tab:: Windows

        .. code-block:: bat

            py -m venv .venv

    The second argument is the location to create the virtual environment. Generally, you
    can just create this in your project and call it ``.venv``.

    ``venv`` will create a virtual Python installation in the ``.venv`` folder.

    .. Note:: You should exclude your virtual environment directory from your version
        control system using ``.gitignore`` or similar.


激活虚拟环境
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Activate a virtual environment**

.. tab:: 中文

    在你开始安装或使用虚拟环境中的包之前，你需要先“激活”它。激活虚拟环境后，虚拟环境特定的 ``python`` 和 ``pip`` 可执行文件将被添加到你的 shell 的 ``PATH`` 中。

    .. tab:: Unix/macOS

        .. code-block:: bash

            source .venv/bin/activate

    .. tab:: Windows

        .. code-block:: bat

            .venv\Scripts\activate

    要确认虚拟环境是否已激活，可以检查 Python 解释器的位置：

    .. tab:: Unix/macOS

        .. code-block:: bash

            which python

    .. tab:: Windows

        .. code-block:: bat

            where python

    当虚拟环境处于激活状态时，上述命令将输出一个包含 ``.venv`` 目录的文件路径，最终显示如下内容：

    .. tab:: Unix/macOS

        .. code-block:: bash

            .venv/bin/python

    .. tab:: Windows

        .. code-block:: bat

            .venv\Scripts\python

    在虚拟环境激活时，pip 会将包安装到该特定环境中。这使得你能够在 Python 应用程序中导入并使用这些包。

.. tab:: 英文

    Before you can start installing or using packages in your virtual environment you'll
    need to ``activate`` it. Activating a virtual environment will put the
    virtual environment-specific ``python`` and ``pip`` executables into your
    shell's ``PATH``.

    .. tab:: Unix/macOS

        .. code-block:: bash

            source .venv/bin/activate

    .. tab:: Windows

        .. code-block:: bat

            .venv\Scripts\activate

    To confirm the virtual environment is activated, check the location of your
    Python interpreter:

    .. tab:: Unix/macOS

        .. code-block:: bash

            which python

    .. tab:: Windows

        .. code-block:: bat

            where python

    While the virtual environment is active, the above command will output a
    filepath that includes the ``.venv`` directory, by ending with the following:

    .. tab:: Unix/macOS

        .. code-block:: bash

            .venv/bin/python

    .. tab:: Windows

        .. code-block:: bat

            .venv\Scripts\python


    While a virtual environment is activated, pip will install packages into that
    specific environment. This enables you to import and use packages in your
    Python application.


停用虚拟环境
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Deactivate a virtual environment**

.. tab:: 中文

    如果你想切换项目或离开当前的虚拟环境，可以使用 ``deactivate`` 命令来停用环境：

    .. code-block:: bash

        deactivate

    .. note::
        关闭你的 shell 会停用虚拟环境。如果你打开一个新的 shell 窗口并想使用该虚拟环境，需要重新激活它。

.. tab:: 英文

    If you want to switch projects or leave your virtual environment,
    ``deactivate`` the environment:

    .. code-block:: bash

        deactivate

    .. note::
        Closing your shell will deactivate the virtual environment. If
        you open a new shell window and want to use the virtual environment,
        reactivate it.

重新激活虚拟环境
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Reactivate a virtual environment**

.. tab:: 中文

    如果你想重新激活一个现有的虚拟环境，按照之前激活虚拟环境的步骤操作即可。无需重新创建虚拟环境。

.. tab:: 英文

    If you want to reactivate an existing virtual environment, follow the same
    instructions about activating a virtual environment. There's no need to create
    a new virtual environment.


准备 pip
-----------

**Prepare pip**

.. tab:: 中文

    :ref:`pip` 是 Python 的官方包管理工具， 用于将包安装和更新到虚拟环境中。

    .. tab:: Unix/macOS

        macOS 的 Python 安装包已经包含了 pip。在 Linux 上，可能需要安装额外的包，如 ``python3-pip``。你可以通过运行以下命令确保 pip 是最新的：

        .. code-block:: bash

            python3 -m pip install --upgrade pip
            python3 -m pip --version

        运行完后，你应该在用户站点安装了最新版本的 pip：

        .. code-block:: text

            pip 23.3.1 from .../.venv/lib/python3.9/site-packages (python 3.9)

    .. tab:: Windows

        Windows 的 Python 安装包也包含了 pip。你可以通过运行以下命令确保 pip 是最新的：

        .. code-block:: bat

            py -m pip install --upgrade pip
            py -m pip --version

        运行完后，你应该安装了最新版本的 pip：

        .. code-block:: text

            pip 23.3.1 from .venv\lib\site-packages (Python 3.9.4)

.. tab:: 英文

    :ref:`pip` is the reference Python package manager.
    It's used to install and update packages into a virtual environment.


    .. tab:: Unix/macOS

        The Python installers for macOS include pip. On Linux, you may have to install
        an additional package such as ``python3-pip``. You can make sure that pip is
        up-to-date by running:

        .. code-block:: bash

            python3 -m pip install --upgrade pip
            python3 -m pip --version

        Afterwards, you should have the latest version of pip installed in your
        user site:

        .. code-block:: text

            pip 23.3.1 from .../.venv/lib/python3.9/site-packages (python 3.9)

    .. tab:: Windows

        The Python installers for Windows include pip. You can make sure that pip is
        up-to-date by running:

        .. code-block:: bat

            py -m pip install --upgrade pip
            py -m pip --version

        Afterwards, you should have the latest version of pip:

        .. code-block:: text

            pip 23.3.1 from .venv\lib\site-packages (Python 3.9.4)


使用 pip 安装软件包
--------------------------

**Install packages using pip**

.. tab:: 中文

    当你的虚拟环境被激活时，你可以安装包。使用 ``pip install`` 命令来安装包。

.. tab:: 英文

    When your virtual environment is activated, you can install packages. Use the
    ``pip install`` command to install packages.

安装软件包
~~~~~~~~~~~~~~~~~

**Install a package**

.. tab:: 中文

    例如，让我们从 :term:`Python 包索引 (PyPI)` 安装 `Requests`_ 库：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install requests

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install requests

    pip 应该会下载 `requests` 及其所有依赖项并安装它们：

    .. code-block:: text

        Collecting requests
        Using cached requests-2.18.4-py2.py3-none-any.whl
        Collecting chardet<3.1.0,>=3.0.2 (from requests)
        Using cached chardet-3.0.4-py2.py3-none-any.whl
        Collecting urllib3<1.23,>=1.21.1 (from requests)
        Using cached urllib3-1.22-py2.py3-none-any.whl
        Collecting certifi>=2017.4.17 (from requests)
        Using cached certifi-2017.7.27.1-py2.py3-none-any.whl
        Collecting idna<2.7,>=2.5 (from requests)
        Using cached idna-2.6-py2.py3-none-any.whl
        Installing collected packages: chardet, urllib3, certifi, idna, requests
        Successfully installed certifi-2017.7.27.1 chardet-3.0.4 idna-2.6 requests-2.18.4 urllib3-1.22

.. tab:: 英文

    For example,let's install the
    `Requests`_ library from the :term:`Python Package Index (PyPI)`:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install requests

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install requests

    pip should download requests and all of its dependencies and install them:

    .. code-block:: text

        Collecting requests
        Using cached requests-2.18.4-py2.py3-none-any.whl
        Collecting chardet<3.1.0,>=3.0.2 (from requests)
        Using cached chardet-3.0.4-py2.py3-none-any.whl
        Collecting urllib3<1.23,>=1.21.1 (from requests)
        Using cached urllib3-1.22-py2.py3-none-any.whl
        Collecting certifi>=2017.4.17 (from requests)
        Using cached certifi-2017.7.27.1-py2.py3-none-any.whl
        Collecting idna<2.7,>=2.5 (from requests)
        Using cached idna-2.6-py2.py3-none-any.whl
        Installing collected packages: chardet, urllib3, certifi, idna, requests
        Successfully installed certifi-2017.7.27.1 chardet-3.0.4 idna-2.6 requests-2.18.4 urllib3-1.22

.. _Requests: https://pypi.org/project/requests/


安装特定版本的软件包
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Install a specific package version**

.. tab:: 中文

    pip 允许你指定要安装的包的版本，使用 :term:`版本标识符 <Version Specifier>`。例如，要安装指定版本的 `requests`：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install 'requests==2.18.4'

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "requests==2.18.4"

    要安装最新的 `2.x` 版本的 `requests`：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install 'requests>=2.0.0,<3.0.0'

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "requests>=2.0.0,<3.0.0"

    要安装预发布版本的包，使用 ``--pre`` 标志：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --pre requests

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --pre requests

.. tab:: 英文

    pip allows you to specify which version of a package to install using
    :term:`version specifiers <Version Specifier>`. For example, to install
    a specific version of ``requests``:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install 'requests==2.18.4'

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "requests==2.18.4"

    To install the latest ``2.x`` release of requests:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install 'requests>=2.0.0,<3.0.0'

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install "requests>=2.0.0,<3.0.0"

    To install pre-release versions of packages, use the ``--pre`` flag:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --pre requests

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --pre requests


安装附加组件
~~~~~~~~~~~~~~

**Install extras**

.. tab:: 中文

    

.. tab:: 英文

Some packages have optional `extras`_. You can tell pip to install these by
specifying the extra in brackets:

.. tab:: Unix/macOS

    .. code-block:: bash

        python3 -m pip install 'requests[security]'

.. tab:: Windows

    .. code-block:: bat

        py -m pip install "requests[security]"

.. _extras:
    https://setuptools.readthedocs.io/en/latest/userguide/dependency_management.html#optional-dependencies


从源代码安装软件包
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Install a package from source**

.. tab:: 中文

    pip 可以直接从源代码安装包。例如，要安装位于 `google-auth` 目录中的源代码：

    .. tab:: Unix/macOS

        .. code-block:: bash

            cd google-auth
            python3 -m pip install .

    .. tab:: Windows

        .. code-block:: bat

            cd google-auth
            py -m pip install .

    此外，pip 还可以以 :doc:`开发模式 <setuptools:userguide/development_mode>` 安装源代码包，这意味着对源代码目录的更改将立即影响已安装的包，而无需重新安装：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --editable .

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --editable .

.. tab:: 英文

    pip can install a package directly from its source code. For example, to install
    the source code in the ``google-auth`` directory:

    .. tab:: Unix/macOS

        .. code-block:: bash

            cd google-auth
            python3 -m pip install .

    .. tab:: Windows

        .. code-block:: bat

            cd google-auth
            py -m pip install .

    Additionally, pip can install packages from source in
    :doc:`development mode <setuptools:userguide/development_mode>`,
    meaning that changes to the source directory will immediately affect the
    installed package without needing to re-install:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --editable .

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --editable .


从版本控制系统安装
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Install from version control systems**

.. tab:: 中文

    pip 可以直接从版本控制系统安装包。例如，你可以直接从 Git 仓库安装：

    .. code-block:: bash

        google-auth @ git+https://github.com/GoogleCloudPlatform/google-auth-library-python.git

    有关支持的版本控制系统和语法的更多信息，请参阅 pip 文档中的 :ref:`VCS 支持 <pip:VCS Support>`。

.. tab:: 英文

    pip can install packages directly from their version control system. For
    example, you can install directly from a git repository:

    .. code-block:: bash

        google-auth @ git+https://github.com/GoogleCloudPlatform/google-auth-library-python.git

    For more information on supported version control systems and syntax, see pip's
    documentation on :ref:`VCS Support <pip:VCS Support>`.


从本地存档安装
~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Install from local archives**

.. tab:: 中文

    如果你有一个本地的 :term:`分发包` 存档（zip、wheel 或 tar 文件），可以使用 pip 直接安装：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install requests-2.18.4.tar.gz

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install requests-2.18.4.tar.gz

    如果你有一个包含多个包存档的目录，可以告诉 pip 从该目录查找包，而不使用 :term:`Python 包索引（PyPI）`：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --no-index --find-links=/local/dir/ requests

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --no-index --find-links=/local/dir/ requests

    这在你需要在连接受限的系统上安装包，或如果你想严格控制分发包的来源时非常有用。

.. tab:: 英文

    If you have a local copy of a :term:`Distribution Package`'s archive (a zip,
    wheel, or tar file) you can install it directly with pip:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install requests-2.18.4.tar.gz

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install requests-2.18.4.tar.gz

    If you have a directory containing archives of multiple packages, you can tell
    pip to look for packages there and not to use the
    :term:`Python Package Index (PyPI)` at all:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --no-index --find-links=/local/dir/ requests

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --no-index --find-links=/local/dir/ requests

    This is useful if you are installing packages on a system with limited
    connectivity or if you want to strictly control the origin of distribution
    packages.


从其他软件包索引安装
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Install from other package indexes**

.. tab:: 中文

    如果你想从不同于 :term:`Python 包索引（PyPI）` 的索引下载包，可以使用 ``--index-url`` 标志：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url http://index.example.com/simple/ SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url http://index.example.com/simple/ SomeProject

    如果你想允许同时从 :term:`Python 包索引（PyPI）` 和一个独立的索引安装包，可以使用 ``--extra-index-url`` 标志：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --extra-index-url http://index.example.com/simple/ SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --extra-index-url http://index.example.com/simple/ SomeProject

.. tab:: 英文

    If you want to download packages from a different index than the
    :term:`Python Package Index (PyPI)`, you can use the ``--index-url`` flag:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url http://index.example.com/simple/ SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url http://index.example.com/simple/ SomeProject

    If you want to allow packages from both the :term:`Python Package Index (PyPI)`
    and a separate index, you can use the ``--extra-index-url`` flag instead:


    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --extra-index-url http://index.example.com/simple/ SomeProject

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --extra-index-url http://index.example.com/simple/ SomeProject

升级软件包
------------------

**Upgrading packages**

.. tab:: 中文

    pip 可以使用 ``--upgrade`` 标志升级包。例如，要安装最新版本的 ``requests`` 及其所有依赖项：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade requests

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade requests

.. tab:: 英文

    pip can upgrade packages in-place using the ``--upgrade`` flag. For example, to
    install the latest version of ``requests`` and all of its dependencies:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade requests

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade requests

使用需求文件
-------------------------

**Using a requirements file**

.. tab:: 中文

    与单独安装包不同，pip 允许你在 :ref:`Requirements 文件 <pip:Requirements Files>` 中声明所有依赖项。例如，你可以创建一个 :file:`requirements.txt` 文件，内容如下：

    .. code-block:: text

        requests==2.18.4
        google-auth==1.1.0

    然后使用 ``-r`` 标志告诉 pip 安装文件中的所有包：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install -r requirements.txt

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install -r requirements.txt

.. tab:: 英文

    Instead of installing packages individually, pip allows you to declare all
    dependencies in a :ref:`Requirements File <pip:Requirements Files>`. For
    example you could create a :file:`requirements.txt` file containing:

    .. code-block:: text

        requests==2.18.4
        google-auth==1.1.0

    And tell pip to install all of the packages in this file using the ``-r`` flag:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install -r requirements.txt

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install -r requirements.txt

冻结依赖项
---------------------

**Freezing dependencies**

.. tab:: 中文

    pip 可以使用 ``freeze`` 命令导出所有已安装包及其版本的列表：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip freeze

    .. tab:: Windows

        .. code-block:: bat

            py -m pip freeze

    该命令将输出一个包含包说明符的列表，例如：

    .. code-block:: text

        cachetools==2.0.1
        certifi==2017.7.27.1
        chardet==3.0.4
        google-auth==1.1.1
        idna==2.6
        pyasn1==0.3.6
        pyasn1-modules==0.1.4
        requests==2.18.4
        rsa==3.4.2
        six==1.11.0
        urllib3==1.22

    ``pip freeze`` 命令对于创建 :ref:`pip:Requirements Files` 非常有用，它可以重新创建环境中安装的所有包的确切版本。

.. tab:: 英文

    Pip can export a list of all installed packages and their versions using the
    ``freeze`` command:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip freeze

    .. tab:: Windows

        .. code-block:: bat

            py -m pip freeze

    Which will output a list of package specifiers such as:

    .. code-block:: text

        cachetools==2.0.1
        certifi==2017.7.27.1
        chardet==3.0.4
        google-auth==1.1.1
        idna==2.6
        pyasn1==0.3.6
        pyasn1-modules==0.1.4
        requests==2.18.4
        rsa==3.4.2
        six==1.11.0
        urllib3==1.22

    The ``pip freeze`` command is useful for creating :ref:`pip:Requirements Files`
    that can re-create the exact versions of all packages installed in an environment.
