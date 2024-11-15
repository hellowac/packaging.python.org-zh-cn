
.. _pypirc:

========================
:file:`.pypirc` 文件
========================

**The** :file:`.pypirc` **file**

.. tab:: 中文

    :file:`.pypirc` 文件允许你为 :term:`package indexes <Package Index>` （在此称为“仓库”）定义配置，这样你就不必在每次使用 :ref:`twine` 或 :ref:`flit` 上传包时输入 URL、用户名或密码。

    格式（最初由 :ref:`distutils` 包定义）如下：

    .. code-block:: ini

        [distutils]
        index-servers =
            first-repository
            second-repository

        [first-repository]
        repository = <first-repository URL>
        username = <first-repository username>
        password = <first-repository password>

        [second-repository]
        repository = <second-repository URL>
        username = <second-repository username>
        password = <second-repository password>

    ``distutils`` 部分定义了一个 ``index-servers`` 字段，列出了描述仓库的所有部分的名称。

    每个描述仓库的部分定义了三个字段：

    - ``repository``：仓库的 URL。
    - ``username``：在仓库中注册的用户名。
    - ``password``：用于验证用户名的密码。

    .. warning::

        请注意，这将密码以明文形式存储。为了更好的安全性，考虑使用 `keyring`_、设置环境变量，或在命令行中提供密码。

        否则，请设置 :file:`.pypirc` 的权限，使得只有你自己可以查看或修改它。例如，在 Linux 或 macOS 上，运行：

        .. code-block:: bash

            chmod 600 ~/.pypirc

.. tab:: 英文

    A :file:`.pypirc` file allows you to define the configuration for :term:`package
    indexes <Package Index>` (referred to here as "repositories"), so that you don't
    have to enter the URL, username, or password whenever you upload a package with
    :ref:`twine` or :ref:`flit`.

    The format (originally defined by the :ref:`distutils` package) is:

    .. code-block:: ini

        [distutils]
        index-servers =
            first-repository
            second-repository

        [first-repository]
        repository = <first-repository URL>
        username = <first-repository username>
        password = <first-repository password>

        [second-repository]
        repository = <second-repository URL>
        username = <second-repository username>
        password = <second-repository password>

    The ``distutils`` section defines an ``index-servers`` field that lists the
    name of all sections describing a repository.

    Each section describing a repository defines three fields:

    - ``repository``: The URL of the repository.
    - ``username``: The registered username on the repository.
    - ``password``: The password that will used to authenticate the username.

    .. warning::

        Be aware that this stores your password in plain text. For better security,
        consider an alternative like `keyring`_, setting environment variables, or
        providing the password on the command line.

        Otherwise, set the permissions on :file:`.pypirc` so that only you can view
        or modify it. For example, on Linux or macOS, run:

        .. code-block:: bash

            chmod 600 ~/.pypirc

.. _keyring: https://pypi.org/project/keyring/

常用配置
=====================

**Common configurations**

.. tab:: 中文

    .. note::

        这些示例适用于 :ref:`twine`。其他项目（例如 :ref:`flit`）也使用
        :file:`.pypirc`，但默认设置不同。请参阅各个项目的文档以获取更多细节和使用说明。

    Twine 的默认配置模仿了一个带有 PyPI 和 TestPyPI 仓库部分的 :file:`.pypirc`：

    .. code-block:: ini

        [distutils]
        index-servers =
            pypi
            testpypi

        [pypi]
        repository = https://upload.pypi.org/legacy/

        [testpypi]
        repository = https://test.pypi.org/legacy/

    Twine 会从 :file:`$HOME/.pypirc`、命令行和环境变量中添加额外的配置到这个默认配置中。

.. tab:: 英文

    .. note::

        These examples apply to :ref:`twine`. Other projects (e.g. :ref:`flit`) also use
        :file:`.pypirc`, but with different defaults. Please refer to each project's
        documentation for more details and usage instructions.

    Twine's default configuration mimics a :file:`.pypirc` with repository sections
    for PyPI and TestPyPI:

    .. code-block:: ini

        [distutils]
        index-servers =
            pypi
            testpypi

        [pypi]
        repository = https://upload.pypi.org/legacy/

        [testpypi]
        repository = https://test.pypi.org/legacy/

    Twine will add additional configuration from :file:`$HOME/.pypirc`, the command
    line, and environment variables to this default configuration.

使用 PyPI 令牌
------------------

**Using a PyPI token**

.. tab:: 中文

    要为 PyPI 设置 `API token`_ ，你可以创建一个类似于以下内容的 :file:`$HOME/.pypirc` 文件：

    .. code-block:: ini

        [pypi]
        username = __token__
        password = <PyPI token>

    对于 :ref:`TestPyPI <using-test-pypi>`，添加一个 ``[testpypi]`` 部分，并使用来自你的 TestPyPI 账户的 API token。

.. tab:: 英文

    To set your `API token`_ for PyPI, you can create a :file:`$HOME/.pypirc`
    similar to:

    .. code-block:: ini

        [pypi]
        username = __token__
        password = <PyPI token>

    For :ref:`TestPyPI <using-test-pypi>`, add a ``[testpypi]`` section, using the
    API token from your TestPyPI account.

.. _API token: https://pypi.org/help/#apitoken

使用另一个包索引
---------------------------

**Using another package index**

.. tab:: 中文

    要配置一个额外的仓库，你需要重新定义 ``index-servers`` 字段以包括仓库名称。以下是一个完整的 :file:`$HOME/.pypirc` 示例，包含了 PyPI、TestPyPI 和一个私有仓库：

    .. code-block:: ini

        [distutils]
        index-servers =
            pypi
            testpypi
            private-repository

        [pypi]
        username = __token__
        password = <PyPI token>

        [testpypi]
        username = __token__
        password = <TestPyPI token>

        [private-repository]
        repository = <private-repository URL>
        username = <private-repository username>
        password = <private-repository password>

    .. warning::

        考虑使用 `keyring`_ 安全地保存你的 API token 和密码，而不是使用 ``password`` 字段（Twine 已安装 `keyring`）：

        .. code-block:: bash

            keyring set https://upload.pypi.org/legacy/ __token__
            keyring set https://test.pypi.org/legacy/ __token__
            keyring set <private-repository URL> <private-repository username>

.. tab:: 英文

    To configure an additional repository, you'll need to redefine the
    ``index-servers`` field to include the repository name. Here is a complete
    example of a :file:`$HOME/.pypirc` for PyPI, TestPyPI, and a private repository:

    .. code-block:: ini

        [distutils]
        index-servers =
            pypi
            testpypi
            private-repository

        [pypi]
        username = __token__
        password = <PyPI token>

        [testpypi]
        username = __token__
        password = <TestPyPI token>

        [private-repository]
        repository = <private-repository URL>
        username = <private-repository username>
        password = <private-repository password>

    .. warning::

        Instead of using the ``password`` field, consider saving your API tokens
        and passwords securely using `keyring`_ (which is installed by Twine):

        .. code-block:: bash

            keyring set https://upload.pypi.org/legacy/ __token__
            keyring set https://test.pypi.org/legacy/ __token__
            keyring set <private-repository URL> <private-repository username>
