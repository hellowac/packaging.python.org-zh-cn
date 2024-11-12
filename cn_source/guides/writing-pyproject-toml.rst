.. _writing-pyproject-toml:

===============================
编写你的 ``pyproject.toml``
===============================

**Writing your** ``pyproject.toml``

.. tab:: 中文

   ``pyproject.toml`` 是一个配置文件，供打包工具以及其他工具（如代码检查工具、类型检查器等）使用。此文件中有三个可能的 TOML 表格。

   - ``[build-system]`` 表格是 **强烈推荐** 使用的。它允许你声明所使用的 :term:`构建后端 <build backend>` 以及构建项目所需的其他依赖项。

   - ``[project]`` 表格是大多数构建后端用来指定项目基本元数据的格式，例如依赖项、你的名字等。

   - ``[tool]`` 表格包含工具特定的子表格，例如 ``[tool.hatch]``、``[tool.black]``、``[tool.mypy]``。我们这里只是简要提到这个表格，因为其内容由每个工具定义。请参考特定工具的文档，了解它可能包含的内容。

   .. note::

      ``[build-system]`` 表格应该始终存在，无论你使用的是哪个构建后端（``[build-system]`` *定义* 了你使用的构建工具）。

      另一方面， ``[project]`` 表格被 *大多数* 构建后端理解，但某些构建后端使用不同的格式。

      截至 2024 年 8 月，Poetry_ 是一个不使用 ``[project]`` 表格的著名构建后端，它使用 ``[tool.poetry]`` 表格代替。此外，setuptools_ 构建后端支持 ``[project]`` 表格和 ``setup.cfg`` 或 ``setup.py`` 中的旧格式。

      对于新项目，建议使用 ``[project]`` 表格，并仅在需要某些编程配置（例如构建 C 扩展）时保留 ``setup.py``，但 ``setup.cfg`` 和 ``setup.py`` 格式仍然有效。参见 :ref:`setup-py-deprecated`。

.. tab:: 英文

   ``pyproject.toml`` is a configuration file used by packaging tools, as
   well as other tools such as linters, type checkers, etc. There are
   three possible TOML tables in this file.

   - The ``[build-system]`` table is **strongly recommended**. It allows you to declare which :term:`build backend` you use and which other dependencies are needed to build your project.

   - The ``[project]`` table is the format that most build backends use to specify your project's basic metadata, such as the dependencies, your name, etc.

   - The ``[tool]`` table has tool-specific subtables, e.g., ``[tool.hatch]``, ``[tool.black]``, ``[tool.mypy]``. We only touch upon this table here because its contents are defined by each tool. Consult the particular tool's documentation to know what it can contain.

   .. note::

      The ``[build-system]`` table should always be present,
      regardless of which build backend you use (``[build-system]`` *defines* the
      build tool you use).

      On the other hand, the ``[project]`` table is understood by *most* build
      backends, but some build backends use a different format.

      As of August 2024, Poetry_ is a notable build backend that does not use
      the ``[project]`` table, it uses the ``[tool.poetry]`` table instead.
      Also, the setuptools_ build backend supports both the ``[project]`` table,
      and the older format in ``setup.cfg`` or ``setup.py``.

      For new projects, use the ``[project]`` table, and keep ``setup.py`` only if
      some programmatic configuration is needed (such as building C extensions),
      but the ``setup.cfg`` and ``setup.py`` formats are still valid. See
      :ref:`setup-py-deprecated`.


.. _pyproject-guide-build-system-table:

声明构建后端
===========================

**Declaring the build backend**

.. tab:: 中文

   ``[build-system]`` 表格包含一个 ``build-backend`` 键，用于指定要使用的构建后端。它还包含一个 ``requires`` 键，这是一个依赖项列表，列出了构建项目所需的依赖项——通常只是构建后端包，但也可能包含其他依赖项。你还可以约束版本，例如， ``requires = ["setuptools >= 61.0"]``。

   通常，你只需复制构建后端文档中建议的内容（参见： :ref:`选择构建后端 <choosing-build-backend>`）。以下是一些常见构建后端的配置示例：

   .. tab:: Hatchling

      .. code-block:: toml

         [build-system]
         requires = ["hatchling"]
         build-backend = "hatchling.build"

   .. tab:: setuptools

      .. code-block:: toml

         [build-system]
         requires = ["setuptools >= 61.0"]
         build-backend = "setuptools.build_meta"

   .. tab:: Flit

      .. code-block:: toml

         [build-system]
         requires = ["flit_core >= 3.4"]
         build-backend = "flit_core.buildapi"

   .. tab:: PDM

      .. code-block:: toml

         [build-system]
         requires = ["pdm-backend"]
         build-backend = "pdm.backend"

.. tab:: 英文

   The ``[build-system]`` table contains a ``build-backend`` key, which specifies
   the build backend to be used. It also contains a ``requires`` key, which is a
   list of dependencies needed to build the project -- this is typically just the
   build backend package, but it may also contain additional dependencies. You can
   also constrain the versions, e.g., ``requires = ["setuptools >= 61.0"]``.

   Usually, you'll just copy what your build backend's documentation
   suggests (after :ref:`choosing your build backend <choosing-build-backend>`).
   Here are the values for some common build backends:

   .. tab:: Hatchling

      .. code-block:: toml

         [build-system]
         requires = ["hatchling"]
         build-backend = "hatchling.build"

   .. tab:: setuptools

      .. code-block:: toml

         [build-system]
         requires = ["setuptools >= 61.0"]
         build-backend = "setuptools.build_meta"

   .. tab:: Flit

      .. code-block:: toml

         [build-system]
         requires = ["flit_core >= 3.4"]
         build-backend = "flit_core.buildapi"

   .. tab:: PDM

      .. code-block:: toml

         [build-system]
         requires = ["pdm-backend"]
         build-backend = "pdm.backend"



静态与动态元数据
===========================

**Static vs. dynamic metadata**

.. tab:: 中文

   本指南的其余部分将重点介绍 ``[project]`` 表格。

   大多数时候，你会直接写入 ``[project]`` 字段的值。例如： ``requires-python = ">= 3.8"`` ，或 ``version = "1.0"`` 。

   然而，在某些情况下，让构建后端为你计算元数据会更有用。例如：许多构建后端可以从代码中的 ``__version__`` 属性、Git 标签或类似的地方读取版本信息。在这种情况下，你应该使用如下方式将字段标记为动态：

   .. code-block:: toml

      [project]
      dynamic = ["version"]

   当一个字段是动态的时，填充该字段的责任就由构建后端来承担。请查阅构建后端的文档，了解它是如何处理的。

.. tab:: 英文

   The rest of this guide is devoted to the ``[project]`` table.

   Most of the time, you will directly write the value of a ``[project]``
   field. For example: ``requires-python = ">= 3.8"``, or ``version =
   "1.0"``.

   However, in some cases, it is useful to let your build backend compute
   the metadata for you. For example: many build backends can read the
   version from a ``__version__`` attribute in your code, a Git tag, or
   similar. In such cases, you should mark the field as dynamic using, e.g.,

   .. code-block:: toml

      [project]
      dynamic = ["version"]

   When a field is dynamic, it is the build backend's responsibility to
   fill it.  Consult your build backend's documentation to learn how it
   does it.


基本信息
=================

**Basic information**

.. _`setup() name`:

``name``
--------

.. tab:: 中文

   在 PyPI 上注册你的项目名称。这个字段是必填的，并且是唯一一个不能标记为动态的字段。

   .. code-block:: toml

      [project]
      name = "spam-eggs"

   项目名称必须由 ASCII 字母、数字、下划线 "``_``"、连字符 "``-``" 和句点 "``.``" 组成。它不能以下划线、连字符或句点开始或结束。

   项目名称的比较不区分大小写，并且将连续的下划线、连字符和/或句点视为相等。例如，如果你注册了一个名为 ``cool-stuff`` 的项目，用户将能够通过以下任何一种拼写方式下载它或声明对其的依赖：``Cool-Stuff``、``cool.stuff``、``COOL_STUFF``、``CoOl__-.-__sTuFF``。

.. tab:: 英文

   Put the name of your project on PyPI. This field is required and is the
   only field that cannot be marked as dynamic.

   .. code-block:: toml

      [project]
      name = "spam-eggs"

   The project name must consist of ASCII letters, digits, underscores "``_``",
   hyphens "``-``" and periods "``.``". It must not start or end with an
   underscore, hyphen or period.

   Comparison of project names is case insensitive and treats arbitrarily long runs
   of underscores, hyphens, and/or periods as equal.  For example, if you register
   a project named ``cool-stuff``, users will be able to download it or declare a
   dependency on it using any of the following spellings: ``Cool-Stuff``,
   ``cool.stuff``, ``COOL_STUFF``, ``CoOl__-.-__sTuFF``.


``version``
-----------

.. tab:: 中文

   填写你项目的版本。

   .. code-block:: toml

      [project]
      version = "2020.0.0"

   一些更复杂的版本说明符，如 ``2020.0.0a1`` （表示一个 alpha 版本），也是可以的；详细信息请参阅 :ref:`指定版本 <version-specifiers>` 。

   此字段是必填的，尽管它通常会标记为动态，使用如下方式：

   .. code-block:: toml

      [project]
      dynamic = ["version"]

   这允许使用场景，例如从 ``__version__`` 属性或 Git 标签填充版本号。有关更多详细信息，请参阅 :ref:`single-source-version` 讨论。

.. tab:: 英文

   Put the version of your project.

   .. code-block:: toml

      [project]
      version = "2020.0.0"

   Some more complicated version specifiers like ``2020.0.0a1`` (for an alpha
   release) are possible; see the :ref:`specification <version-specifiers>`
   for full details.

   This field is required, although it is often marked as dynamic using

   .. code-block:: toml

      [project]
      dynamic = ["version"]

   This allows use cases such as filling the version from a ``__version__``
   attribute or a Git tag. Consult the :ref:`single-source-version`
   discussion for more details.


依赖项和要求
=============================

**Dependencies and requirements**

``dependencies``/``optional-dependencies``
------------------------------------------

``dependencies``/``optional-dependencies``

.. tab:: 中文

   如果你的项目有依赖，按如下方式列出它们：

   .. code-block:: toml

      [project]
      dependencies = [
      "httpx",
      "gidgethub[httpx]>4.0.0",
      "django>2.1; os_name != 'nt'",
      "django>2.0; os_name == 'nt'",
      ]

   有关完整的版本约束语法，请参阅 :ref:`指定依赖项 <dependency-specifiers>`。

   如果某些依赖项仅用于包的特定功能，可以将它们设置为可选依赖。在这种情况下，将它们放在 ``optional-dependencies`` 中。

   .. code-block:: toml

      [project.optional-dependencies]
      gui = ["PyQt5"]
      cli = [
         "rich",
         "click",
      ]

   每个键定义一个“打包扩展”。在上面的例子中，可以使用例如 ``pip install your-project-name[gui]`` 来安装支持 GUI 的项目，并添加 PyQt5 作为依赖。

.. tab:: 英文

   If your project has dependencies, list them like this:

   .. code-block:: toml

      [project]
      dependencies = [
      "httpx",
      "gidgethub[httpx]>4.0.0",
      "django>2.1; os_name != 'nt'",
      "django>2.0; os_name == 'nt'",
      ]

   See :ref:`Dependency specifiers <dependency-specifiers>` for the full
   syntax you can use to constrain versions.

   You may want to make some of your dependencies optional, if they are
   only needed for a specific feature of your package. In that case, put
   them in ``optional-dependencies``.

   .. code-block:: toml

      [project.optional-dependencies]
      gui = ["PyQt5"]
      cli = [
         "rich",
         "click",
      ]

   Each of the keys defines a "packaging extra". In the example above, one
   could use, e.g., ``pip install your-project-name[gui]`` to install your
   project with GUI support, adding the PyQt5 dependency.


.. _requires-python:
.. _python_requires:

``requires-python``
-------------------

``requires-python``

.. tab:: 中文

   这让你声明你支持的 Python 最小版本。

   .. code-block:: toml

      [project]
      requires-python = ">= 3.8"

.. tab:: 英文

   This lets you declare the minimum version of Python that you support
   [#requires-python-upper-bounds]_. 

   .. code-block:: toml

      [project]
      requires-python = ">= 3.8"


.. _`console_scripts`:

创建可执行脚本
===========================

**Creating executable scripts**

.. tab:: 中文

   要将一个命令作为你包的一部分安装，请在 ``[project.scripts]`` 表中声明它。

   .. code-block:: toml

      [project.scripts]
      spam-cli = "spam:main_cli"

   在这个例子中，安装你的项目后， ``spam-cli`` 命令将可用。执行该命令相当于执行 ``from spam import main_cli; main_cli()`` 。

   在 Windows 上，这种方式打包的脚本需要一个终端，因此，如果你从图形应用程序中启动它们，它们会弹出一个终端窗口。为了防止这种情况发生，可以使用 ``[project.gui-scripts]`` 表，而不是 ``[project.scripts]``。

   .. code-block:: toml

      [project.gui-scripts]
      spam-gui = "spam:main_gui"

   在这种情况下，从命令行启动脚本时将立即返回控制，将脚本置于后台运行。

   ``[project.scripts]`` 和 ``[project.gui-scripts]`` 之间的区别仅在 Windows 上相关。

.. tab:: 英文

   To install a command as part of your package, declare it in the
   ``[project.scripts]`` table.

   .. code-block:: toml

      [project.scripts]
      spam-cli = "spam:main_cli"

   In this example, after installing your project, a ``spam-cli`` command
   will be available. Executing this command will do the equivalent of
   ``from spam import main_cli; main_cli()``.

   On Windows, scripts packaged this way need a terminal, so if you launch
   them from within a graphical application, they will make a terminal pop
   up. To prevent this from happening, use the ``[project.gui-scripts]``
   table instead of ``[project.scripts]``.

   .. code-block:: toml

      [project.gui-scripts]
      spam-gui = "spam:main_gui"

   In that case, launching your script from the command line will give back
   control immediately, leaving the script to run in the background.

   The difference between ``[project.scripts]`` and
   ``[project.gui-scripts]`` is only relevant on Windows.



关于您的项目
==================

**About your project**

``authors``/``maintainers``
---------------------------

.. tab:: 中文

   这两个字段包含由姓名和/或电子邮件地址标识的人员列表。

   .. code-block:: toml

      [project]
      authors = [
         {name = "Pradyun Gedam", email = "pradyun@example.com"},
         {name = "Tzu-Ping Chung", email = "tzu-ping@example.com"},
         {name = "Another person"},
         {email = "different.person@example.com"},
      ]
      maintainers = [
         {name = "Brett Cannon", email = "brett@example.com"}
      ]

.. tab:: 英文

   Both of these fields contain lists of people identified by a name and/or
   an email address.

   .. code-block:: toml

      [project]
      authors = [
         {name = "Pradyun Gedam", email = "pradyun@example.com"},
         {name = "Tzu-Ping Chung", email = "tzu-ping@example.com"},
         {name = "Another person"},
         {email = "different.person@example.com"},
      ]
      maintainers = [
         {name = "Brett Cannon", email = "brett@example.com"}
      ]


.. _description:

``description``
---------------

.. tab:: 中文

   这应该是你项目的一行描述，用作项目页面在 PyPI 上的“标题” ( `example <pypi-pip_>`_ )，以及其他地方，如搜索结果列表 ( `example <pypi-search-pip_>`_ )。

   .. code-block:: toml

      [project]
      description = "Lovely Spam! Wonderful Spam!"

.. tab:: 英文

   This should be a one-line description of your project, to show as the "headline"
   of your project page on PyPI (`example <pypi-pip_>`_), and other places such as
   lists of search results (`example <pypi-search-pip_>`_).

   .. code-block:: toml

      [project]
      description = "Lovely Spam! Wonderful Spam!"


``readme``
----------

.. tab:: 中文

   这是你项目的更长描述，将显示在 PyPI 上的项目页面中。通常，你的项目会有一个 ``README.md`` 或 ``README.rst`` 文件，你只需要在这里写出该文件的文件名。

   .. code-block:: toml

      [project]
      readme = "README.md"

   README 的格式会根据扩展名自动检测：

   - ``README.md`` → `GitHub 风格的 Markdown <gfm_>`_，
   - ``README.rst`` → `reStructuredText <rest_>`_（没有 Sphinx 扩展）。

   你也可以显式指定格式，如下所示：

   .. code-block:: toml

      [project]
      readme = {file = "README.txt", content-type = "text/markdown"}
      # 或者
      readme = {file = "README.txt", content-type = "text/x-rst"}

.. tab:: 英文

   This is a longer description of your project, to display on your project
   page on PyPI. Typically, your project will have a ``README.md`` or
   ``README.rst`` file and you just put its file name here.

   .. code-block:: toml

      [project]
      readme = "README.md"

   The README's format is auto-detected from the extension:

   - ``README.md`` → `GitHub-flavored Markdown <gfm_>`_,
   - ``README.rst`` → `reStructuredText <rest_>`_ (without Sphinx extensions).

   You can also specify the format explicitly, like this:

   .. code-block:: toml

      [project]
      readme = {file = "README.txt", content-type = "text/markdown"}
      # or
      readme = {file = "README.txt", content-type = "text/x-rst"}


``license``
-----------

.. tab:: 中文

   这可以有两种形式。你可以将许可证放在一个文件中，通常是 ``LICENSE`` 或 ``LICENSE.txt``，然后在这里链接该文件：

   .. code-block:: toml

      [project]
      license = {file = "LICENSE"}

   或者你也可以直接写出许可证的名称：

   .. code-block:: toml

      [project]
      license = {text = "MIT License"}

   如果你使用的是标准的、知名的许可证，那么不需要使用这个字段。相反，你应该使用一个以 ``License::`` 开头的 :ref:`分类器 <classifiers>`。 （一般来说，使用标准的、知名的许可证是一个好主意，既可以避免混淆，也因为一些组织会避免使用未获批准的许可证的软件。）

.. tab:: 英文

   This can take two forms. You can put your license in a file, typically
   ``LICENSE`` or ``LICENSE.txt``, and link that file here:

   .. code-block:: toml

      [project]
      license = {file = "LICENSE"}

   or you can write the name of the license:

   .. code-block:: toml

      [project]
      license = {text = "MIT License"}

   If you are using a standard, well-known license, it is not necessary to use this
   field. Instead, you should use one of the :ref:`classifiers` starting with ``License
   ::``. (As a general rule, it is a good idea to use a standard, well-known
   license, both to avoid confusion and because some organizations avoid software
   whose license is unapproved.)


``keywords``
------------

.. tab:: 中文

   这将帮助 PyPI 的搜索框在用户搜索这些关键词时建议你的项目。

   .. code-block:: toml

      [project]
      keywords = ["egg", "bacon", "sausage", "tomatoes", "Lobster Thermidor"]

.. tab:: 英文

   This will help PyPI's search box to suggest your project when people
   search for these keywords.

   .. code-block:: toml

      [project]
      keywords = ["egg", "bacon", "sausage", "tomatoes", "Lobster Thermidor"]


.. _classifiers:

``classifiers``
---------------

.. tab:: 中文

   这是一个适用于你项目的 PyPI 分类器列表。查看 `完整的分类器列表 <classifier-list_>`_ 。

   .. code-block:: toml

      classifiers = [
         # 这个项目的成熟度如何？常见的值有：
         #   3 - Alpha
         #   4 - Beta
         #   5 - 生产/稳定版
         "Development Status :: 4 - Beta",

         # 指明你的项目面向的对象
         "Intended Audience :: Developers",
         "Topic :: Software Development :: Build Tools",

         # 根据需要选择你的许可证（也见上面的 "license" 字段）
         "License :: OSI Approved :: MIT License",

         # 在这里指定你支持的 Python 版本。
         "Programming Language :: Python :: 3",
         "Programming Language :: Python :: 3.6",
         "Programming Language :: Python :: 3.7",
         "Programming Language :: Python :: 3.8",
         "Programming Language :: Python :: 3.9",
      ]

   虽然分类器列表常用于声明项目支持的 Python 版本，但这些信息仅用于 PyPI 上项目的搜索和浏览，并不会用于安装项目。要实际限制项目可以安装的 Python 版本，请使用 :ref:`requires-python` 参数。

   要防止一个包被上传到 PyPI，可以使用特殊的 ``Private :: Do Not Upload`` 分类器。PyPI 将始终拒绝带有以 ``Private ::`` 开头的分类器的包。 

.. tab:: 英文

   A list of PyPI classifiers that apply to your project. Check the
   `full list of possibilities <classifier-list_>`_.

   .. code-block:: toml

      classifiers = [
         # How mature is this project? Common values are
         #   3 - Alpha
         #   4 - Beta
         #   5 - Production/Stable
         "Development Status :: 4 - Beta",

         # Indicate who your project is intended for
         "Intended Audience :: Developers",
         "Topic :: Software Development :: Build Tools",

         # Pick your license as you wish (see also "license" above)
         "License :: OSI Approved :: MIT License",

         # Specify the Python versions you support here.
         "Programming Language :: Python :: 3",
         "Programming Language :: Python :: 3.6",
         "Programming Language :: Python :: 3.7",
         "Programming Language :: Python :: 3.8",
         "Programming Language :: Python :: 3.9",
      ]

   Although the list of classifiers is often used to declare what Python versions a
   project supports, this information is only used for searching and browsing
   projects on PyPI, not for installing projects. To actually restrict what Python
   versions a project can be installed on, use the :ref:`requires-python` argument.

   To prevent a package from being uploaded to PyPI, use the special ``Private ::
   Do Not Upload`` classifier. PyPI will always reject packages with classifiers
   beginning with ``Private ::``.

.. _writing-pyproject-toml-urls:

``urls``
--------

.. tab:: 中文

   这是与你项目相关的 URL 列表，将显示在 PyPI 项目页面的左侧边栏。

   .. note::

      请参阅 :ref:`well-known-labels` 以查看 PyPI 和其他打包工具专门识别的标签列表，并查看 `PyPI 项目的元数据文档 <https://docs.pypi.org/project_metadata/#project-urls>`_ 了解 PyPI 特定的 URL 处理方式。

   .. code-block:: toml

      [project.urls]
      Homepage = "https://example.com"
      Documentation = "https://readthedocs.org"
      Repository = "https://github.com/me/spam.git"
      Issues = "https://github.com/me/spam/issues"
      Changelog = "https://github.com/me/spam/blob/master/CHANGELOG.md"

   请注意，如果标签包含空格，则需要用引号括起来，例如：
   ``Website = "https://example.com"``，但
   ``"Official Website" = "https://example.com"``。

   建议用户在适当的情况下使用 :ref:`well-known-labels` 作为项目的 URL，因为元数据的消费者（如包索引）可以根据这些标签调整展示方式。

   例如，在以下元数据中， ``MyHomepage`` 或 ``"Download Link"`` 都不是已知标签，因此它们将原样呈现：

   .. code-block:: toml

      [project.urls]
      MyHomepage = "https://example.com"
      "Download Link" = "https://example.com/abc.tar.gz"

   而在这段元数据中， ``HomePage`` 和 ``DOWNLOAD`` 都有已知的等效标签（ ``homepage`` 和 ``download``），因此它们可以根据这些语义进行展示（项目的主页和外部下载位置，分别）。 

   .. code-block:: toml

      [project.urls]
      HomePage = "https://example.com"
      DOWNLOAD = "https://example.com/abc.tar.gz"

.. tab:: 英文

   A list of URLs associated with your project, displayed on the left
   sidebar of your PyPI project page.

   .. note::

      See :ref:`well-known-labels` for a listing
      of labels that PyPI and other packaging tools are specifically aware of,
      and `PyPI's project metadata docs <https://docs.pypi.org/project_metadata/#project-urls>`_
      for PyPI-specific URL processing.

   .. code-block:: toml

      [project.urls]
      Homepage = "https://example.com"
      Documentation = "https://readthedocs.org"
      Repository = "https://github.com/me/spam.git"
      Issues = "https://github.com/me/spam/issues"
      Changelog = "https://github.com/me/spam/blob/master/CHANGELOG.md"

   Note that if the label contains spaces, it needs to be quoted, e.g.,
   ``Website = "https://example.com"`` but
   ``"Official Website" = "https://example.com"``.

   Users are advised to use :ref:`well-known-labels` for their project URLs
   where appropriate, since consumers of metadata (like package indices) can
   specialize their presentation.

   For example in the following metadata, neither ``MyHomepage`` nor
   ``"Download Link"`` is a well-known label, so they will be rendered verbatim:

   .. code-block:: toml

      [project.urls]
      MyHomepage = "https://example.com"
      "Download Link" = "https://example.com/abc.tar.gz"


   Whereas in this metadata ``HomePage`` and ``DOWNLOAD`` both have
   well-known equivalents (``homepage`` and ``download``), and can be presented
   with those semantics in mind (the project's home page and its external
   download location, respectively).

   .. code-block:: toml

      [project.urls]
      HomePage = "https://example.com"
      DOWNLOAD = "https://example.com/abc.tar.gz"

高级插件
================

**Advanced plugins**

.. tab:: 中文

   一些包可以通过插件进行扩展。例如 Pytest_ 和 Pygments_ 。要创建这样的插件，你需要在 ``[project.entry-points]`` 的子表中声明它，如下所示：

   .. code-block:: toml

      [project.entry-points."spam.magical"]
      tomatoes = "spam:main_tomatoes"

   有关更多信息，请参阅 :ref:`插件指南 <plugin-entry-points>`。

.. tab:: 英文

   Some packages can be extended through plugins. Examples include Pytest_
   and Pygments_. To create such a plugin, you need to declare it in a subtable
   of ``[project.entry-points]`` like this:

   .. code-block:: toml

      [project.entry-points."spam.magical"]
      tomatoes = "spam:main_tomatoes"

   See the :ref:`Plugin guide <plugin-entry-points>` for more information.



完整示例
==============

**A full example**

.. code-block:: toml

   [build-system]
   requires = ["hatchling"]
   build-backend = "hatchling.build"

   [project]
   name = "spam-eggs"
   version = "2020.0.0"
   dependencies = [
     "httpx",
     "gidgethub[httpx]>4.0.0",
     "django>2.1; os_name != 'nt'",
     "django>2.0; os_name == 'nt'",
   ]
   requires-python = ">=3.8"
   authors = [
     {name = "Pradyun Gedam", email = "pradyun@example.com"},
     {name = "Tzu-Ping Chung", email = "tzu-ping@example.com"},
     {name = "Another person"},
     {email = "different.person@example.com"},
   ]
   maintainers = [
     {name = "Brett Cannon", email = "brett@example.com"}
   ]
   description = "Lovely Spam! Wonderful Spam!"
   readme = "README.rst"
   license = {file = "LICENSE.txt"}
   keywords = ["egg", "bacon", "sausage", "tomatoes", "Lobster Thermidor"]
   classifiers = [
     "Development Status :: 4 - Beta",
     "Programming Language :: Python"
   ]

   [project.optional-dependencies]
   gui = ["PyQt5"]
   cli = [
     "rich",
     "click",
   ]

   [project.urls]
   Homepage = "https://example.com"
   Documentation = "https://readthedocs.org"
   Repository = "https://github.com/me/spam.git"
   "Bug Tracker" = "https://github.com/me/spam/issues"
   Changelog = "https://github.com/me/spam/blob/master/CHANGELOG.md"

   [project.scripts]
   spam-cli = "spam:main_cli"

   [project.gui-scripts]
   spam-gui = "spam:main_gui"

   [project.entry-points."spam.magical"]
   tomatoes = "spam:main_tomatoes"


------------------

.. [#requires-python-upper-bounds] 在这里使用类似 ``requires-python = "<= 3.10"`` 的上限时要三思。`这篇博客文章 <requires-python-blog-post_>`_ 提供了一些有关可能问题的信息。

.. [#requires-python-upper-bounds] Think twice before applying an upper bound
   like ``requires-python = "<= 3.10"`` here. `This blog post <requires-python-blog-post_>`_
   contains some information regarding possible problems.

.. _gfm: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
.. _setuptools: https://setuptools.pypa.io
.. _poetry: https://python-poetry.org
.. _pypi-pip: https://pypi.org/project/pip
.. _pypi-search-pip: https://pypi.org/search?q=pip
.. _classifier-list: https://pypi.org/classifiers
.. _requires-python-blog-post: https://iscinumpy.dev/post/bound-version-constraints/#pinning-the-python-version-is-special
.. _pytest: https://pytest.org
.. _pygments: https://pygments.org
.. _rest: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
