==================
打包流程
==================

**The Packaging Flow**

.. tab:: 中文

    本文档旨在概述发布/分发 :term:`分发包 <Distribution Package>` 的流程，通常是将其发布到 `Python 包索引（PyPI） <https://pypi.org/>`_。本文面向包的发布者，假设读者是包的作者。

    虽然 :doc:`教程 <tutorials/packaging-projects>` 介绍了准备简单包发布的过程，但它并没有完全列出所需的步骤和文件，以及它们的目的。

    发布包需要从作者的源代码到最终用户的 Python 环境的一个流程。实现这一目标的步骤如下：

    - 拥有一个包含包的源代码树。通常，这是从版本控制系统（VCS）中检出的代码。

    - 准备一个描述包元数据（如名称、版本等）以及如何创建构建产物的配置文件。对于大多数包来说，这通常是一个手动维护的 :file:`pyproject.toml` 文件，保存在源代码树中。

    - 创建要发送到包分发服务（通常是 PyPI）的构建产物。这些通常是 :term:`源分发包 ("sdist") <Source Distribution (or "sdist")>` 和一个或多个 :term:`已构建分发包 ("wheels") <Built Distribution>`。这些构建产物是通过构建工具，使用上一步的配置文件生成的。通常，对于纯 Python 包来说，只有一个通用的 wheel 文件。

    - 将构建产物上传到包分发服务。

    此时，包已经存在于包分发服务上。要使用该包，最终用户需要：

    - 从包分发服务下载包的某个构建产物。

    - 将其安装到他们的 Python 环境中，通常是安装到其 ``site-packages`` 目录中。此步骤可能涉及构建/编译步骤，如果需要，必须在包的元数据中描述该步骤。

    这最后两个步骤通常由 :ref:`pip` 执行，当最终用户运行 ``pip install`` 时。

    下面将更详细地描述这些步骤。

.. tab:: 英文

    The document aims to outline the flow involved in publishing/distributing a :term:`distribution package <Distribution Package>`, usually to the `Python Package Index (PyPI)`_. It is written for package publishers, who are assumed to be the package author.

    While the :doc:`tutorial <tutorials/packaging-projects>` walks through the process of preparing a simple package for release, it does not fully enumerate what steps and files are required, and for what purpose.

    Publishing a package requires a flow from the author's source code to an end user's Python environment. The steps to achieve this are:

    - Have a source tree containing the package. This is typically a checkout from a version control system (VCS).

    - Prepare a configuration file describing the package metadata (name, version and so forth) and how to create the build artifacts. For most packages, this will be a :file:`pyproject.toml` file, maintained manually in the source tree.

    - Create build artifacts to be sent to the package distribution service (usually PyPI); these will normally be a :term:`source distribution ("sdist") <Source Distribution (or "sdist")>` and one or more :term:`built distributions ("wheels") <Built Distribution>`. These are made by a build tool using the configuration file from the previous step. Often there is just one generic wheel for a pure Python package.

    - Upload the build artifacts to the package distribution service.

    At that point, the package is present on the package distribution service. To use the package, end users must:

    - Download one of the package's build artifacts from the package distribution service.

    - Install it in their Python environment, usually in its ``site-packages`` directory. This step may involve a build/compile step which, if needed, must be described by the package metadata.

    These last 2 steps are typically performed by :ref:`pip` when an end user runs ``pip install``.

    The steps above are described in more detail below.

.. _Python Package Index (PyPI): https://pypi.org/

源代码树
===============

**The source tree**

.. tab:: 中文

    源代码树包含包的源代码，通常是从版本控制系统（VCS）中检出的。用于创建构建产物的特定代码版本通常是基于与版本关联的标签的检出版本。

.. tab:: 英文

    The source tree contains the package source code, usually a checkout from a
    VCS. The particular version of the code used to create the build artifacts
    will typically be a checkout based on a tag associated with the version.

配置文件
======================

**The configuration file**

.. tab:: 中文

  配置文件依赖于用于创建构建产物的工具。标准做法是使用 :file:`pyproject.toml` 文件，格式为 `TOML format`_。

  至少，:file:`pyproject.toml` 文件需要一个 ``[build-system]`` 表格来指定你的构建工具。有许多可用的构建工具，包括但不限于 :ref:`flit`、:ref:`hatch`、:ref:`pdm`、:ref:`poetry`、:ref:`setuptools`、`trampolim`_ 和 `whey`_。每个工具的文档会说明需要在 ``[build-system]`` 表格中填写什么内容。

  例如，以下是使用 :ref:`hatch` 的表格示例：

  .. code-block:: toml

      [build-system]
      requires = ["hatchling"]
      build-backend = "hatchling.build"

  在 :file:`pyproject.toml` 文件中包含这样的表格后，一个 ":term:`frontend <Build Frontend>`" 工具，例如 :ref:`build`，可以运行你选择的构建工具的 ":term:`backend <Build Backend>`"，以创建构建产物。你的构建工具也可能提供自己的前端。像 :ref:`pip` 这样的安装工具也充当前端，当它运行你的构建工具的后端从源分发中进行安装时。

  你选择的具体构建工具决定了 :file:`pyproject.toml` 文件中需要包含的额外信息。例如，你可能需要指定：

  * 一个 ``[project]`` 表格，包含项目的 :doc:`核心元数据 </specifications/core-metadata/>` （如名称、版本、作者等），

  * 一个 ``[tool]`` 表格，包含工具特定的配置选项。

  请参阅 :ref:`pyproject.toml guide <writing-pyproject-toml>` 获取有关 ``pyproject.toml`` 配置的完整指南。

.. tab:: 英文

  The configuration file depends on the tool used to create the build artifacts.
  The standard practice is to use a :file:`pyproject.toml` file in the `TOML
  format`_.

  At a minimum, the :file:`pyproject.toml` file needs a ``[build-system]`` table
  specifying your build tool. There are many build tools available, including
  but not limited to :ref:`flit`, :ref:`hatch`, :ref:`pdm`, :ref:`poetry`,
  :ref:`setuptools`, `trampolim`_, and `whey`_. Each tool's documentation will
  show what to put in the ``[build-system]`` table.

  For example, here is a table for using :ref:`hatch`:

  .. code-block:: toml

      [build-system]
      requires = ["hatchling"]
      build-backend = "hatchling.build"

  With such a table in the :file:`pyproject.toml` file,
  a ":term:`frontend <Build Frontend>`" tool like
  :ref:`build` can run your chosen
  build tool's ":term:`backend <Build Backend>`"
  to create the build artifacts.
  Your build tool may also provide its own frontend. An install tool
  like :ref:`pip` also acts as a frontend when it runs your build tool's backend
  to install from a source distribution.

  The particular build tool you choose dictates what additional information is
  required in the :file:`pyproject.toml` file. For example, you might specify:

  * a ``[project]`` table containing project
    :doc:`Core Metadata </specifications/core-metadata/>`
    (name, version, author and so forth),

  * a ``[tool]`` table containing tool-specific configuration options.

  Refer to the :ref:`pyproject.toml guide <writing-pyproject-toml>` for a
  complete guide to ``pyproject.toml`` configuration.


.. _TOML format: https://github.com/toml-lang/toml
.. _trampolim: https://pypi.org/project/trampolim/
.. _whey: https://pypi.org/project/whey/

构建工件
===============

**Build artifacts**

源代码分发 (sdist)
-------------------------------

**The source distribution (sdist)**

.. tab:: 中文

  源分发包含足够的信息，用于从源代码在最终用户的 Python 环境中安装该包。因此，它需要包含包的源代码，并且可能还包括测试和文档。这些对于希望开发源代码的最终用户以及需要某些本地编译步骤的用户系统（例如 C 扩展）非常有用。

  :ref:`build` 包知道如何调用你的构建工具来创建其中之一：

  .. code-block:: bash

      python3 -m build --sdist 源代码目录

  或者，你的构建工具也可能提供自己的接口来创建源分发（sdist）。

.. tab:: 英文

  A source distribution contains enough to install the package from source in an
  end user's Python environment. As such, it needs the package source, and may
  also include tests and documentation. These are useful for end users wanting
  to develop your sources, and for end user systems where some local compilation
  step is required (such as a C extension).

  The :ref:`build` package knows how to invoke your build tool to create one of
  these:

  .. code-block:: bash

      python3 -m build --sdist source-tree-directory

  Or, your build tool may provide its own interface for creating an sdist.


构建的分发 (wheels)
--------------------------------

**The built distributions (wheels)**

.. tab:: 中文

  构建分发仅包含最终用户 Python 环境所需的文件。安装过程中不需要编译步骤，wheel 文件可以直接解压到 ``site-packages`` 目录中。这使得安装过程对最终用户来说更加快速和方便。

  一个纯 Python 包通常只需要一个“通用” wheel 文件。一个带有编译二进制扩展的包则需要为其支持的每种 Python 解释器、操作系统和 CPU 架构组合创建一个 wheel 文件。如果没有合适的 wheel 文件，像 :ref:`pip` 这样的工具将回退到安装源分发（source distribution）。

  :ref:`build` 包知道如何调用你的构建工具来创建其中之一：

  .. code-block:: bash

      python3 -m build --wheel 源代码目录

  或者，你的构建工具也可能提供自己的接口来创建 wheel 文件。

  .. note::

    :ref:`build` 的默认行为是从当前目录的源代码创建一个源分发和一个 wheel 文件；上述示例故意进行了具体化。

.. tab:: 英文

  A built distribution contains only the files needed for an end user's Python
  environment. No compilation steps are required during the install, and the
  wheel file can simply be unpacked into the ``site-packages`` directory. This
  makes the install faster and more convenient for end users.

  A pure Python package typically needs only one "generic" wheel. A package with
  compiled binary extensions needs a wheel for each supported combination of
  Python interpreter, operating system, and CPU architecture that it supports.
  If a suitable wheel file is not available, tools like :ref:`pip` will fall
  back to installing the source distribution.

  The :ref:`build` package knows how to invoke your build tool to create one of
  these:

  .. code-block:: bash

      python3 -m build --wheel source-tree-directory

  Or, your build tool may provide its own interface for creating a wheel.

  .. note::

    The default behaviour of :ref:`build` is to make both an sdist and a wheel
    from the source in the current directory; the above examples are
    deliberately specific.

上传到包分发服务
==========================================

**Upload to the package distribution service**

.. tab:: 中文

  :ref:`twine` 工具可以将构建产物上传到 PyPI 进行分发，使用如下命令：

  .. code-block:: bash

      twine upload dist/package-name-version.tar.gz dist/package-name-version-py3-none-any.whl

  或者，你的构建工具也可能提供自己的接口来进行上传。

.. tab:: 英文

  The :ref:`twine` tool can upload build artifacts to PyPI for distribution,
  using a command like:

  .. code-block:: bash

      twine upload dist/package-name-version.tar.gz dist/package-name-version-py3-none-any.whl

  Or, your build tool may provide its own interface for uploading.

下载并安装
====================

**Download and install**

.. tab:: 中文

  现在包已经发布，最终用户可以下载并将包安装到他们的 Python 环境中。通常，这可以通过 :ref:`pip` 完成，使用如下命令：

  .. code-block:: bash

      python3 -m pip install package-name

  最终用户也可以使用其他工具，如 :ref:`pipenv`、:ref:`poetry` 或 :ref:`pdm`。

.. tab:: 英文

  Now that the package is published, end users can download and install the package into their Python environment. Typically this is done with :ref:`pip`, using a command like:

  .. code-block:: bash

      python3 -m pip install package-name

  End users may also use other tools like :ref:`pipenv`, :ref:`poetry`, or :ref:`pdm`.
