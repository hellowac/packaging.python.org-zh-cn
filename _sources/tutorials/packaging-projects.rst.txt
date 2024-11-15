打包Python项目
=========================

**Packaging Python Projects**

.. tab:: 中文

    本教程将带你了解如何打包一个简单的 Python 项目。它将展示如何添加必要的文件和结构以创建包，如何构建包，以及如何将其上传到 Python 包索引（PyPI）。

    .. tip::

        如果你在运行本教程中的命令时遇到问题，请复制命令及其输出，然后在 GitHub 上的  `packaging-problems <https://github.com/pypa/packaging-problems>`_ 仓库 `提交问题 <https://github.com/pypa/packaging-problems/issues/new?template=packaging_tutorial.yml&title=Trouble+with+the+packaging+tutorial&guide=https://packaging.python.org/tutorials/packaging-projects>`_ ，我们会尽力提供帮助！

    一些命令需要较新的 :ref:`pip` 版本，因此请首先确保你安装了最新版本：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade pip

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade pip

.. tab:: 英文

    This tutorial walks you through how to package a simple Python project. It will
    show you how to add the necessary files and structure to create the package, how
    to build the package, and how to upload it to the Python Package Index (PyPI).

    .. tip::

        If you have trouble running the commands in this tutorial, please copy the command and its output, then `open an issue <https://github.com/pypa/packaging-problems/issues/new?template=packaging_tutorial.yml&title=Trouble+with+the+packaging+tutorial&guide=https://packaging.python.org/tutorials/packaging-projects>`__ on the `packaging-problems <https://github.com/pypa/packaging-problems>`_ repository on GitHub. We'll do our best to help you!

    Some of the commands require a newer version of :ref:`pip`, so start by making
    sure you have the latest version installed:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade pip

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade pip

.. _open-an-issue: https://github.com/pypa/packaging-problems/issues/new?template=packaging_tutorial.yml&title=Trouble+with+the+packaging+tutorial&guide=https://packaging.python.org/tutorials/packaging-projects

.. _packaging-problems: https://github.com/pypa/packaging-problems


一个简单的项目
----------------

**A simple project**

.. tab:: 中文

    本教程使用一个简单的项目，名为 ``example_package_YOUR_USERNAME_HERE``。如果你的用户名是 ``me``，那么包名将是 ``example_package_me``；这样可以确保你的包名唯一，不会与其他人上传的包发生冲突。我们建议在打包你自己的项目之前，先按照本教程使用该项目进行操作。

    请在本地创建以下文件结构：

    .. code-block:: text

        packaging_tutorial/
        └── src/
            └── example_package_YOUR_USERNAME_HERE/
                ├── __init__.py
                └── example.py

    包含 Python 文件的目录应该与项目名称一致。这样做可以简化配置，并使得用户在安装包时更易于理解。

    建议创建文件 :file:`__init__.py`，因为该文件的存在允许用户将目录作为常规包导入，即使在本教程中， :file:`__init__.py` 文件是空的。[#namespace-packages]_

    :file:`example.py` 是包中的一个示例模块，可能包含包的逻辑（如函数、类、常量等）。打开该文件并输入以下内容：

    .. code-block:: python

        def add_one(number):
            return number + 1

    如果你不熟悉 Python 的 :term:`模块 <Module>` 和 :term:`导入包 <Import Package>`，建议花几分钟时间阅读 `Python 包和模块的文档  <Python documentation for packages and modules>`_ 。

    创建好这个结构后，你需要在 ``packaging_tutorial`` 目录中运行本教程中的所有命令。

.. tab:: 英文

    This tutorial uses a simple project named
    ``example_package_YOUR_USERNAME_HERE``. If your username is ``me``, then the
    package would be ``example_package_me``; this ensures that you have a unique
    package name that doesn't conflict with packages uploaded by other people
    following this tutorial. We recommend following this tutorial as-is using this
    project, before packaging your own project.

    Create the following file structure locally:

    .. code-block:: text

        packaging_tutorial/
        └── src/
            └── example_package_YOUR_USERNAME_HERE/
                ├── __init__.py
                └── example.py

    The directory containing the Python files should match the project name. This
    simplifies the configuration and is more obvious to users who install the package.

    Creating the file :file:`__init__.py` is recommended because the existence of an
    :file:`__init__.py` file allows users to import the directory as a regular package,
    even if (as is the case in this tutorial) :file:`__init__.py` is empty.
    [#namespace-packages]_

    :file:`example.py` is an example of a module within the package that could
    contain the logic (functions, classes, constants, etc.) of your package.
    Open that file and enter the following content:

    .. code-block:: python

        def add_one(number):
            return number + 1

    If you are unfamiliar with Python's :term:`modules <Module>` and
    :term:`import packages <Import Package>`, take a few minutes to read over the
    `Python documentation for packages and modules`_.

    Once you create this structure, you'll want to run all of the commands in this
    tutorial within the ``packaging_tutorial`` directory.

.. _Python documentation for packages and modules:
    https://docs.python.org/3/tutorial/modules.html#packages


创建包文件
--------------------------

**Creating the package files**

.. tab:: 中文

    现在，你将添加一些文件来为项目的分发做准备。完成后，项目结构将如下所示：

    .. code-block:: text

        packaging_tutorial/
        ├── LICENSE
        ├── pyproject.toml
        ├── README.md
        ├── src/
        │   └── example_package_YOUR_USERNAME_HERE/
        │       ├── __init__.py
        │       └── example.py
        └── tests/

.. tab:: 英文

    You will now add files that are used to prepare the project for distribution.
    When you're done, the project structure will look like this:


    .. code-block:: text

        packaging_tutorial/
        ├── LICENSE
        ├── pyproject.toml
        ├── README.md
        ├── src/
        │   └── example_package_YOUR_USERNAME_HERE/
        │       ├── __init__.py
        │       └── example.py
        └── tests/


创建测试目录
-------------------------

**Creating a test directory**

.. tab:: 中文

    :file:`tests/` 是测试文件的占位符。暂时将其留空。

.. tab:: 英文

    :file:`tests/` is a placeholder for test files. Leave it empty for now.


.. _choosing-build-backend:

选择构建后端
------------------------

**Choosing a build backend**

.. tab:: 中文

    工具如 :ref:`pip` 和 :ref:`build` 本身并不会将源代码转换为分发包（如 wheel 格式）；这项工作由 :term:`build backend` （构建后端）来完成。构建后端决定了如何指定项目的配置，包括元数据（关于项目的信息，例如名称和在 PyPI 上显示的标签）和输入文件。构建后端具有不同的功能级别，比如是否支持构建 :term:`extension modules <Extension Module>`，你应该选择一个适合你需求和偏好的后端。

    你可以选择多个构建后端；本教程默认使用 :ref:`Hatchling <hatch>`，但它与 :ref:`setuptools`、:ref:`Flit <flit>`、:ref:`PDM <pdm>` 等支持 ``[project]`` 表格和 :ref:`metadata <configuring metadata>` 的工具一样适用。

    .. note::

    一些构建后端是更大工具的一部分，这些工具提供了命令行界面，具有如项目初始化、版本管理、构建、上传和安装包等额外功能。本教程使用的是单一功能工具，它们独立工作。

    `pyproject.toml` 文件告诉 :term:`build frontend <Build Frontend>` 工具（如 :ref:`pip` 和 :ref:`build`）使用哪个后端来构建你的项目。以下是常见构建后端的一些示例，但请查看后端的文档以获取更多细节。

    .. tab:: Hatchling

        .. code-block:: toml

            [build-system]
            requires = ["hatchling"]
            build-backend = "hatchling.build"

    .. tab:: setuptools

        .. code-block:: toml

            [build-system]
            requires = ["setuptools>=61.0"]
            build-backend = "setuptools.build_meta"

    .. tab:: Flit

        .. code-block:: toml

            [build-system]
            requires = ["flit_core>=3.4"]
            build-backend = "flit_core.buildapi"

    .. tab:: PDM

        .. code-block:: toml

            [build-system]
            requires = ["pdm-backend"]
            build-backend = "pdm.backend"

    ``requires`` 键是构建包所需的依赖包列表。构建前端通常会在构建包时自动安装它们。前端通常会在隔离的环境中运行构建，因此如果遗漏了这些依赖，可能会导致构建时错误。此列表应始终包括后端包，并可能包括其他构建时依赖。

    ``build-backend`` 键是前端用来执行构建的 Python 对象名称。

    这两个值将由构建后端的文档提供，或者通过其命令行接口生成。通常情况下，你无需自定义这些设置。

    构建工具的其他配置通常会位于 ``pyproject.toml`` 文件的 ``tool`` 部分，或者位于构建工具定义的特殊文件中。例如，使用 ``setuptools`` 作为构建后端时，额外的配置可能会添加到 ``setup.py`` 或 ``setup.cfg`` 文件中，指定 ``setuptools.build_meta`` 可以让工具自动找到并使用这些文件。

.. tab:: 英文

    Tools like :ref:`pip` and :ref:`build` do not actually convert your sources
    into a :term:`distribution package <Distribution Package>` (like a wheel);
    that job is performed by a :term:`build backend <Build Backend>`. The build backend determines how
    your project will specify its configuration, including metadata (information
    about the project, for example, the name and tags that are displayed on PyPI)
    and input files. Build backends have different levels of functionality, such as
    whether they support building :term:`extension modules <Extension Module>`, and
    you should choose one that suits your needs and preferences.

    You can choose from a number of backends; this tutorial uses :ref:`Hatchling
    <hatch>` by default, but it will work identically with :ref:`setuptools`,
    :ref:`Flit <flit>`, :ref:`PDM <pdm>`, and others that support the ``[project]``
    table for :ref:`metadata <configuring metadata>`.

    .. note::

    Some build backends are part of larger tools that provide a command-line
    interface with additional features like project initialization and version
    management, as well as building, uploading, and installing packages. This
    tutorial uses single-purpose tools that work independently.

    The :file:`pyproject.toml` tells :term:`build frontend <Build Frontend>` tools like :ref:`pip` and
    :ref:`build` which backend to use for your project. Below are some
    examples for common build backends, but check your backend's own documentation
    for more details.

    .. tab:: Hatchling

        .. code-block:: toml

            [build-system]
            requires = ["hatchling"]
            build-backend = "hatchling.build"

    .. tab:: setuptools

        .. code-block:: toml

            [build-system]
            requires = ["setuptools>=61.0"]
            build-backend = "setuptools.build_meta"

    .. tab:: Flit

        .. code-block:: toml

            [build-system]
            requires = ["flit_core>=3.4"]
            build-backend = "flit_core.buildapi"

    .. tab:: PDM

        .. code-block:: toml

            [build-system]
            requires = ["pdm-backend"]
            build-backend = "pdm.backend"


    The ``requires`` key is a list of packages that are needed to build your package.
    The :term:`frontend <Build Frontend>` should install them automatically when building your package.
    Frontends usually run builds in isolated environments, so omitting dependencies
    here may cause build-time errors.
    This should always include your backend's package, and might have other build-time
    dependencies.

    The ``build-backend`` key is the name of the Python object that frontends will use
    to perform the build.

    Both of these values will be provided by the documentation for your build
    backend, or generated by its command line interface. There should be no need for
    you to customize these settings.

    Additional configuration of the build tool will either be in a ``tool`` section
    of the ``pyproject.toml``, or in a special file defined by the build tool. For
    example, when using ``setuptools`` as your build backend, additional configuration
    may be added to a ``setup.py`` or ``setup.cfg`` file, and specifying
    ``setuptools.build_meta`` in your build allows the tools to locate and use these
    automatically.

.. _configuring metadata:

配置元数据
^^^^^^^^^^^^^^^^^^^^

**Configuring metadata**

.. tab:: 中文

    打开 `pyproject.toml` 文件并输入以下内容。将 `name` 替换为包含你的用户名，这样可以确保你的包名唯一，不会与其他人上传的包名冲突。

    .. code-block:: toml

        [project]
        name = "example_package_YOUR_USERNAME_HERE"
        version = "0.0.1"
        authors = [
        { name="Example Author", email="author@example.com" },
        ]
        description = "A small example package"
        readme = "README.md"
        requires-python = ">=3.8"
        classifiers = [
            "Programming Language :: Python :: 3",
            "License :: OSI Approved :: MIT License",
            "Operating System :: OS Independent",
        ]

        [project.urls]
        Homepage = "https://github.com/pypa/sampleproject"
        Issues = "https://github.com/pypa/sampleproject/issues"

    - `name` 是你的包的 *分发名称*。包名可以包含字母、数字、`.`、`_` 和 `-`，但不能包含其他字符。此外，包名在 PyPI 上必须是唯一的。**务必使用你的用户名来替换这个名称**，这样可以确保不会上传与已存在包同名的包。
    - `version` 是包的版本号。（一些构建后端允许以其他方式指定版本，如从文件或 Git 标签中读取）
    - `authors` 用于标识包的作者；你可以为每个作者指定一个名字和电子邮件，也可以列出 `maintainers` （维护者），格式相同。
    - `description` 是包的简短说明，通常为一句话。
    - `readme` 是指向文件的路径，文件包含包的详细描述。这些信息会显示在 PyPI 上的包详情页中。通常，描述从 `README.md` 文件加载（这是一种常见做法）。更高级的表格形式也有描述，见 :ref:`pyproject.toml guide <writing-pyproject-toml>`。
    - `requires-python` 列出了你的项目支持的 Python 版本。像 :ref:`pip` 这样的安装工具会查找适配的 Python 版本来安装合适的包版本。
    - `classifiers` 为 PyPI 和 :ref:`pip` 提供一些附加的包元数据。在此示例中，包仅兼容 Python 3，采用 MIT 许可，并且是跨平台的。你应始终包含至少以下信息：支持的 Python 版本、许可协议和操作系统兼容性。完整的分类器列表可见于 https://pypi.org/classifiers/。
    - `urls` 让你列出任何额外的链接，在 PyPI 页面上展示。通常，这些链接指向源代码、文档、问题追踪器等。

    请参阅 :ref:`pyproject.toml guide <writing-pyproject-toml>` 了解更多关于这些字段以及其他可以在 ``[project]`` 部分中定义的字段。常见的字段还包括 `keywords` （改进包的可发现性）以及用于安装包的 `dependencies` 。

.. tab:: 英文

    Open :file:`pyproject.toml` and enter the following content. Change the ``name``
    to include your username; this ensures that you have a unique
    package name that doesn't conflict with packages uploaded by other people
    following this tutorial.

    .. code-block:: toml

        [project]
        name = "example_package_YOUR_USERNAME_HERE"
        version = "0.0.1"
        authors = [
        { name="Example Author", email="author@example.com" },
        ]
        description = "A small example package"
        readme = "README.md"
        requires-python = ">=3.8"
        classifiers = [
            "Programming Language :: Python :: 3",
            "License :: OSI Approved :: MIT License",
            "Operating System :: OS Independent",
        ]

        [project.urls]
        Homepage = "https://github.com/pypa/sampleproject"
        Issues = "https://github.com/pypa/sampleproject/issues"

    - ``name`` is the *distribution name* of your package. This can be any name as
      long as it only contains letters, numbers, ``.``, ``_`` , and ``-``. It also
      must not already be taken on PyPI. **Be sure to update this with your
      username** for this tutorial, as this ensures you won't try to upload a
      package with the same name as one which already exists.
    - ``version`` is the package version. (Some build backends allow it to be
      specified another way, such as from a file or Git tag.)
    - ``authors`` is used to identify the author of the package; you specify a name
      and an email for each author. You can also list ``maintainers`` in the same
      format.
    - ``description`` is a short, one-sentence summary of the package.
    - ``readme`` is a path to a file containing a detailed description of the
      package. This is shown on the package detail page on PyPI.
      In this case, the description is loaded from :file:`README.md` (which is a
      common pattern). There also is a more advanced table form described in the
      :ref:`pyproject.toml guide <writing-pyproject-toml>`.
    - ``requires-python`` gives the versions of Python supported by your
      project. An installer like :ref:`pip` will look back through older versions of
      packages until it finds one that has a matching Python version.
    - ``classifiers`` gives the index and :ref:`pip` some additional metadata
      about your package. In this case, the package is only compatible with Python
      3, is licensed under the MIT license, and is OS-independent. You should
      always include at least which version(s) of Python your package works on,
      which license your package is available under, and which operating systems
      your package will work on. For a complete list of classifiers, see
      https://pypi.org/classifiers/.
    - ``urls`` lets you list any number of extra links to show on PyPI.
      Generally this could be to the source, documentation, issue trackers, etc.

    See the :ref:`pyproject.toml guide <writing-pyproject-toml>` for details
    on these and other fields that can be defined in the ``[project]``
    table. Other common fields are ``keywords`` to improve discoverability
    and the ``dependencies`` that are required to install your package.


创建 README.md
------------------

**Creating README.md**

.. tab:: 中文

    打开 `README.md` 文件并输入以下内容。你也可以根据需要进行自定义。

    .. code-block:: md

        # Example Package

        This is a simple example package. You can use
        [GitHub-flavored Markdown](https://guides.github.com/features/mastering-markdown/)
        to write your content.

该 `README.md` 文件将作为你的包的详细描述，并在 PyPI 上显示。你可以在其中添加任何有用的信息，比如安装说明、使用示例、贡献指南等。

.. tab:: 英文

    Open :file:`README.md` and enter the following content. You can customize this
    if you'd like.

    .. code-block:: md

        # Example Package

        This is a simple example package. You can use
        [GitHub-flavored Markdown](https://guides.github.com/features/mastering-markdown/)
        to write your content.


创建 LICENSE
------------------

**Creating a LICENSE**

.. tab:: 中文

    对于上传到 Python 软件包索引的每个软件包来说，包含许可证非常重要。这会告诉安装软件包的用户他们可以使用软件包的条款。有关选择许可证的帮助，请参阅 https://choosealicense.com/ 。选择许可证后，打开 :file:`LICENSE` 并输入许可证文本。例如，如果您选择了 MIT 许可证：

    .. code-block:: text

        Copyright (c) 2018 The Python Packaging Authority

        Permission is hereby granted, free of charge, to any person obtaining a copy
        of this software and associated documentation files (the "Software"), to deal
        in the Software without restriction, including without limitation the rights
        to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
        copies of the Software, and to permit persons to whom the Software is
        furnished to do so, subject to the following conditions:

        The above copyright notice and this permission notice shall be included in all
        copies or substantial portions of the Software.

        THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
        IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
        FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
        AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
        LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
        OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
        SOFTWARE.

    大多数构建后端都会自动在包中包含许可证文件。请参阅后端的文档以了解更多详细信息。

.. tab:: 英文

    It's important for every package uploaded to the Python Package Index to include a license. This tells users who install your package the terms under which they can use your package. For help picking a license, see https://choosealicense.com/. Once you have chosen a license, open :file:`LICENSE` and enter the license text. For example, if you had chosen the MIT license:

    .. code-block:: text

        Copyright (c) 2018 The Python Packaging Authority

        Permission is hereby granted, free of charge, to any person obtaining a copy
        of this software and associated documentation files (the "Software"), to deal
        in the Software without restriction, including without limitation the rights
        to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
        copies of the Software, and to permit persons to whom the Software is
        furnished to do so, subject to the following conditions:

        The above copyright notice and this permission notice shall be included in all
        copies or substantial portions of the Software.

        THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
        IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
        FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
        AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
        LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
        OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
        SOFTWARE.

    Most build backends automatically include license files in packages. See your
    backend's documentation for more details.


包括其他文件
---------------------

**Including other files**

.. tab:: 中文

    上述列出的文件将会自动包含在你的 :term:`源代码分发 <Source Distribution (or "sdist")>` 中。如果你想包含额外的文件，请参考你所使用的构建后端的文档。

.. tab:: 英文

    The files listed above will be included automatically in your
    :term:`source distribution <Source Distribution (or "sdist")>`. If you want to
    include additional files, see the documentation for your build backend.

.. _generating archives:

生成分发档案
--------------------------------

**Generating distribution archives**

.. tab:: 中文

    下一步是为包生成 :term:`分发包 <Distribution Package>`。这些是上传到 Python 包索引（PyPI）的归档文件，可以通过 :ref:`pip` 安装。

    确保你已经安装了 PyPA 的最新版本的 :ref:`build` 工具：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade build

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade build

    .. tip:: 如果你在安装过程中遇到问题，请参考 :doc:`安装包` 教程。

    接下来，在包含 :file:`pyproject.toml` 文件的目录中运行以下命令：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m build

    .. tab:: Windows

        .. code-block:: bat

            py -m build

    该命令将输出大量文本，完成后将在 :file:`dist` 目录中生成两个文件：

    .. code-block:: text

        dist/
        ├── example_package_YOUR_USERNAME_HERE-0.0.1-py3-none-any.whl
        └── example_package_YOUR_USERNAME_HERE-0.0.1.tar.gz

    ``tar.gz`` 文件是 :term:`源代码分发 <Source Distribution (或 "sdist")>`，而 ``.whl`` 文件是 :term:`构建分发 <Built Distribution>`。
    较新的 :ref:`pip` 版本会优先安装构建分发包，但如果需要，也会回退到源代码分发包。你应始终上传源代码分发包，并为你的项目兼容的所有平台提供构建分发包。在这个例子中，我们的示例包兼容任何平台上的 Python，因此只需要一个构建分发包。

.. tab:: 英文

    The next step is to generate :term:`distribution packages <Distribution Package>`
    for the package. These are archives that are uploaded to the Python
    Package Index and can be installed by :ref:`pip`.

    Make sure you have the latest version of PyPA's :ref:`build` installed:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade build

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade build

    .. tip:: If you have trouble installing these, see the
        :doc:`installing-packages` tutorial.

    Now run this command from the same directory where :file:`pyproject.toml` is located:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m build

    .. tab:: Windows

        .. code-block:: bat

            py -m build

    This command should output a lot of text and once completed should generate two
    files in the :file:`dist` directory:

    .. code-block:: text

        dist/
        ├── example_package_YOUR_USERNAME_HERE-0.0.1-py3-none-any.whl
        └── example_package_YOUR_USERNAME_HERE-0.0.1.tar.gz


    The ``tar.gz`` file is a :term:`source distribution <Source Distribution (or "sdist")>`
    whereas the ``.whl`` file is a :term:`built distribution <Built Distribution>`.
    Newer :ref:`pip` versions preferentially install built distributions, but will
    fall back to source distributions if needed. You should always upload a source
    distribution and provide built distributions for the platforms your project is
    compatible with. In this case, our example package is compatible with Python on
    any platform so only one built distribution is needed.

上传分发档案
-----------------------------------

**Uploading the distribution archives**

.. tab:: 中文

    最后，是时候将你的包上传到 Python 包索引（PyPI）了！

    首先，你需要在 TestPyPI 上注册一个账户。TestPyPI 是一个用于测试和实验的独立包索引实例，非常适合像本教程这样的场景，避免直接上传到真实的 PyPI。要注册账户，请访问 https://test.pypi.org/account/register/ 并完成页面上的步骤。你还需要验证你的电子邮件地址，才能上传任何包。有关更多详细信息，请参见 :doc:`/guides/using-testpypi` 。

    为了安全上传你的项目，你需要一个 PyPI `API token`_ 。你可以在 https://test.pypi.org/manage/account/#api-tokens 创建一个，设置 "Scope" 为 "Entire account"。 **在复制并保存 token 之前，请不要关闭该页面 — 你将无法再次看到该 token**。

    现在你已经注册了账户，可以使用 :ref:`twine` 上传分发包。你需要安装 Twine：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade twine

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade twine

    安装完成后，运行 Twine 上传 :file:`dist` 目录下的所有归档文件：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m twine upload --repository testpypi dist/*

    .. tab:: Windows

        .. code-block:: bat

            py -m twine upload --repository testpypi dist/*

    系统会提示你输入用户名和密码。用户名填写 ``__token__`` ，密码填写 token 的值（包括 ``pypi-`` 前缀）。

    命令完成后，你应该会看到类似如下的输出：

    .. code-block::

        Uploading distributions to https://test.pypi.org/legacy/
        Enter your username: __token__
        Uploading example_package_YOUR_USERNAME_HERE-0.0.1-py3-none-any.whl
        100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.2/8.2 kB • 00:01 • ?
        Uploading example_package_YOUR_USERNAME_HERE-0.0.1.tar.gz
        100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.8/6.8 kB • 00:00 • ?

    上传完成后，你的包应该可以在 TestPyPI 上查看；例如：``https://test.pypi.org/project/example_package_YOUR_USERNAME_HERE``。

.. tab:: 英文

    Finally, it's time to upload your package to the Python Package Index!

    The first thing you'll need to do is register an account on TestPyPI, which
    is a separate instance of the package index intended for testing and
    experimentation. It's great for things like this tutorial where we don't
    necessarily want to upload to the real index. To register an account, go to
    https://test.pypi.org/account/register/ and complete the steps on that page.
    You will also need to verify your email address before you're able to upload
    any packages.  For more details, see :doc:`/guides/using-testpypi`.

    To securely upload your project, you'll need a PyPI `API token`_. Create one at
    https://test.pypi.org/manage/account/#api-tokens, setting the "Scope" to "Entire
    account". **Don't close the page until you have copied and saved the token — you
    won't see that token again.**

    Now that you are registered, you can use :ref:`twine` to upload the
    distribution packages. You'll need to install Twine:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --upgrade twine

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --upgrade twine

    Once installed, run Twine to upload all of the archives under :file:`dist`:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m twine upload --repository testpypi dist/*

    .. tab:: Windows

        .. code-block:: bat

            py -m twine upload --repository testpypi dist/*

    You will be prompted for a username and password. For the username,
    use ``__token__``. For the password, use the token value, including
    the ``pypi-`` prefix.

    After the command completes, you should see output similar to this:

    .. code-block::

        Uploading distributions to https://test.pypi.org/legacy/
        Enter your username: __token__
        Uploading example_package_YOUR_USERNAME_HERE-0.0.1-py3-none-any.whl
        100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.2/8.2 kB • 00:01 • ?
        Uploading example_package_YOUR_USERNAME_HERE-0.0.1.tar.gz
        100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.8/6.8 kB • 00:00 • ?

    Once uploaded, your package should be viewable on TestPyPI; for example:
    ``https://test.pypi.org/project/example_package_YOUR_USERNAME_HERE``.

.. _API token: https://test.pypi.org/help/#apitoken


安装新上传的包
--------------------------------------

**Installing your newly uploaded package**

.. tab:: 中文

    你可以使用 :ref:`pip` 安装你的包并验证它是否正常工作。创建一个 :ref:`虚拟环境 <Creating and using Virtual Environments>`，然后从 TestPyPI 安装你的包：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url https://test.pypi.org/simple/ --no-deps example-package-YOUR-USERNAME-HERE

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url https://test.pypi.org/simple/ --no-deps example-package-YOUR-USERNAME-HERE

    确保在包名中指定你的用户名！

    pip 应该从 TestPyPI 安装该包，输出应如下所示：

    .. code-block:: text

        Collecting example-package-YOUR-USERNAME-HERE
        Downloading https://test-files.pythonhosted.org/packages/.../example_package_YOUR_USERNAME_HERE_0.0.1-py3-none-any.whl
        Installing collected packages: example_package_YOUR_USERNAME_HERE
        Successfully installed example_package_YOUR_USERNAME_HERE-0.0.1

    .. note:: 本示例使用 ``--index-url`` 标志来指定 TestPyPI，而不是在线的 PyPI。此外，还指定了 ``--no-deps``。由于 TestPyPI 并不包含与在线 PyPI 相同的包，因此尝试安装依赖项可能会失败或安装一些意外的内容。虽然我们的示例包没有任何依赖项，但在使用 TestPyPI 时，避免安装依赖项是一个好的实践。

    你可以通过导入包来测试它是否正确安装。确保你仍然在虚拟环境中，然后运行 Python:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3

    .. tab:: Windows

        .. code-block:: bat

            py

    然后导入包：

    .. code-block:: pycon

        >>> from example_package_YOUR_USERNAME_HERE import example
        >>> example.add_one(2)
        3

.. tab:: 英文

    You can use :ref:`pip` to install your package and verify that it works.
    Create a :ref:`virtual environment <Creating and using Virtual Environments>`
    and install your package from TestPyPI:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url https://test.pypi.org/simple/ --no-deps example-package-YOUR-USERNAME-HERE

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url https://test.pypi.org/simple/ --no-deps example-package-YOUR-USERNAME-HERE

    Make sure to specify your username in the package name!

    pip should install the package from TestPyPI and the output should look
    something like this:

    .. code-block:: text

        Collecting example-package-YOUR-USERNAME-HERE
        Downloading https://test-files.pythonhosted.org/packages/.../example_package_YOUR_USERNAME_HERE_0.0.1-py3-none-any.whl
        Installing collected packages: example_package_YOUR_USERNAME_HERE
        Successfully installed example_package_YOUR_USERNAME_HERE-0.0.1

    .. note:: This example uses ``--index-url`` flag to specify TestPyPI instead of
        live PyPI. Additionally, it specifies ``--no-deps``. Since TestPyPI doesn't
        have the same packages as the live PyPI, it's possible that attempting to
        install dependencies may fail or install something unexpected. While our
        example package doesn't have any dependencies, it's a good practice to avoid
        installing dependencies when using TestPyPI.

    You can test that it was installed correctly by importing the package.
    Make sure you're still in your virtual environment, then run Python:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3

    .. tab:: Windows

        .. code-block:: bat

            py

    and import the package:

    .. code-block:: pycon

        >>> from example_package_YOUR_USERNAME_HERE import example
        >>> example.add_one(2)
        3


后续步骤
----------

**Next steps**

.. tab:: 中文

    **恭喜你，你已经成功地打包并发布了一个 Python 项目！**  
    ✨ 🍰 ✨

    请记住，本教程展示的是如何将你的包上传到 Test PyPI，而 Test PyPI 不是一个永久的存储库。测试系统会偶尔删除包和账户。因此，最好将 Test PyPI 用于像本教程这样的测试和实验。

    当你准备好将一个真实的包上传到 Python 包索引时，你可以像本教程中一样进行操作，但有以下几个重要的不同点：

    * 选择一个容易记住且独特的包名。你不需要像教程中那样附加用户名，但你不能使用已有的名称。
    * 在 https://pypi.org 上注册一个账户——注意，这是两个独立的服务器，测试服务器的登录信息与主服务器不共享。
    * 使用 ``twine upload dist/*`` 上传你的包，并输入你在真实 PyPI 上注册的账户的凭据。现在你是在生产环境中上传包，因此不需要指定 ``--repository``；包将默认上传到 https://pypi.org/。
    * 使用 ``python3 -m pip install [your-package]`` 从真实的 PyPI 安装你的包。

    如果你此时想要了解更多关于 Python 库打包的内容，以下是一些可以进一步阅读的资源：

    * 阅读关于你选择的构建后端的高级配置文档： `Hatchling <hatchling-config_>`_ ， :doc:`setuptools <setuptools:userguide/pyproject_config>`， :doc:`Flit <flit:pyproject_toml>`， `PDM <pdm-config_>`_。
    * 查阅本站的 :doc:`指南 </guides/index>`，获取更多高级实用信息，或在 :doc:`讨论 </discussions/index>` 中了解具体主题的解释和背景。
    * 考虑使用那些提供单一命令行接口来管理项目和打包的工具，如 :ref:`hatch`、:ref:`flit`、:ref:`pdm` 和 :ref:`poetry`。

.. tab:: 英文

    **Congratulations, you've packaged and distributed a Python project!**
    ✨ 🍰 ✨

    Keep in mind that this tutorial showed you how to upload your package to Test
    PyPI, which isn't a permanent storage. The Test system occasionally deletes
    packages and accounts. It is best to use TestPyPI for testing and experiments
    like this tutorial.

    When you are ready to upload a real package to the Python Package Index you can
    do much the same as you did in this tutorial, but with these important
    differences:

    * Choose a memorable and unique name for your package. You don't have to append
      your username as you did in the tutorial, but you can't use an existing name.
    * Register an account on https://pypi.org - note that these are two separate
      servers and the login details from the test server are not shared with the
      main server.
    * Use ``twine upload dist/*`` to upload your package and enter your credentials
      for the account you registered on the real PyPI.  Now that you're uploading
      the package in production, you don't need to specify ``--repository``; the
      package will upload to https://pypi.org/ by default.
    * Install your package from the real PyPI using ``python3 -m pip install [your-package]``.

    At this point if you want to read more on packaging Python libraries here are
    some things you can do:

    * Read about advanced configuration for your chosen build backend:
      `Hatchling <hatchling-config_>`_,
      :doc:`setuptools <setuptools:userguide/pyproject_config>`,
      :doc:`Flit <flit:pyproject_toml>`, `PDM <pdm-config_>`_.
    * Look at the :doc:`guides </guides/index>` on this site for more advanced
      practical information, or the :doc:`discussions </discussions/index>`
      for explanations and background on specific topics.
    * Consider packaging tools that provide a single command-line interface for
      project management and packaging, such as :ref:`hatch`, :ref:`flit`,
      :ref:`pdm`, and :ref:`poetry`.
  

----

.. rubric:: Notes

.. [#namespace-packages]
   Technically, you can also create Python packages without an ``__init__.py`` file,
   but those are called :doc:`namespace packages </guides/packaging-namespace-packages>`
   and considered an **advanced topic** (not covered in this tutorial).
   If you are only getting started with Python packaging, it is recommended to
   stick with *regular packages* and ``__init__.py`` (even if the file is empty).


.. _hatchling-config: https://hatch.pypa.io/latest/config/metadata/
.. _pdm-config: https://pdm-project.org/latest/reference/pep621/
