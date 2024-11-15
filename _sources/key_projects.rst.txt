
.. _projects:

=================
项目摘要
=================

**Project Summaries**

.. tab:: 中文

    Python 安装和打包领域最相关项目的摘要和链接。

.. tab:: 英文

    Summaries and links for the most relevant projects in the space of Python installation and packaging.

.. _pypa_projects:

PyPA 项目
#############

**PyPA Projects**

.. _bandersnatch:

bandersnatch
============

`Docs <https://bandersnatch.readthedocs.io>`__ |
`Issues <https://github.com/pypa/bandersnatch/issues>`__ |
`GitHub <https://github.com/pypa/bandersnatch>`__ |
`PyPI <https://pypi.org/project/bandersnatch>`__

.. tab:: 中文

    ``bandersnatch`` 是一个 PyPI 镜像客户端，旨在高效地创建 PyPI 内容的完整镜像。通过使用该工具，组织可以节省带宽和延迟，特别是在自动化测试的上下文中，并防止对 PyPI 内容分发网络（CDN）造成过大负载。文件可以从本地目录或 `AWS S3`_ 提供服务。

.. tab:: 英文

    ``bandersnatch`` is a PyPI mirroring client designed to efficiently
    create a complete mirror of the contents of PyPI. Organizations thus
    save bandwidth and latency on package downloads (especially in the
    context of automated tests) and to prevent heavily loading PyPI's
    Content Delivery Network (CDN).
    Files can be served from a local directory or `AWS S3`_.


.. _build:

build
=====

:any:`Docs <build:index>` |
`Issues <https://github.com/pypa/build/issues>`__ |
`GitHub <https://github.com/pypa/build>`__ |
`PyPI <https://pypi.org/project/build>`__

.. tab:: 中文

    ``build`` 是一个兼容 :pep:`517` 的 Python 包构建工具。它提供了一个命令行接口（CLI）来构建包，以及一个 Python API。

.. tab:: 英文

    ``build`` is a :pep:`517` compatible Python package builder. It provides a CLI to build packages, as well as a Python API.


.. _cibuildwheel:

cibuildwheel
============

`Docs <https://cibuildwheel.readthedocs.io/>`__ |
`Issues <https://github.com/pypa/cibuildwheel/issues>`__ |
`GitHub <https://github.com/pypa/cibuildwheel>`__ |
`PyPI <https://pypi.org/project/cibuildwheel>`__ |
`Discussions <https://github.com/pypa/cibuildwheel/discussions>`__ |
`Discord #cibuildwheel <https://discord.com/invite/pypa>`__

.. tab:: 中文

    ``cibuildwheel`` 是一个 Python 包，它可以在大多数 CI 系统上为所有常见平台和 Python 版本构建 :term:`wheels <Wheel>`。另请参见 :ref:`multibuild`。

.. tab:: 英文

    ``cibuildwheel`` is a Python package that builds :term:`wheels <Wheel>` for all common platforms and Python versions on most CI systems. Also see :ref:`multibuild`.

.. _distlib:

distlib
=======

:doc:`Docs <distlib:index>` |
`Issues <https://github.com/pypa/distlib/issues>`__ |
`GitHub <https://github.com/pypa/distlib>`__ |
`PyPI <https://pypi.org/project/distlib>`__

.. tab:: 中文

    ``distlib`` 是一个实现与 Python 软件的打包和分发相关的低级函数的库。 ``distlib`` 实现了多个相关的 PEP（Python 增强提案标准），并且对第三方打包工具的开发者很有用，可以用来创建和上传二进制和源代码 :term:`distributions <Distribution Package>`，实现互操作性，解决依赖关系，管理包资源，执行其他类似的功能。

    与更严格的 :ref:`packaging` 项目（下文所述）不同，后者专门实现了现代 Python 打包互操作性标准，``distlib`` 还尝试在处理遗留包和元数据时提供合理的回退行为，这些遗留包和元数据早于现代互操作性标准，并且属于与这些标准不兼容的包的子集。

.. tab:: 英文

    ``distlib`` is a library which implements low-level functions that
    relate to packaging and distribution of Python software.  ``distlib``
    implements several relevant PEPs (Python Enhancement Proposal
    standards) and is useful for developers of third-party packaging tools
    to make and upload binary and source :term:`distributions
    <Distribution Package>`, achieve interoperability, resolve
    dependencies, manage package resources, and do other similar
    functions.

    Unlike the stricter :ref:`packaging` project (below), which
    specifically implements modern Python packaging interoperability
    standards, ``distlib`` also attempts to provide reasonable fallback
    behaviours when asked to handle legacy packages and metadata that
    predate the modern interoperability standards and fall into the subset
    of packages that are incompatible with those standards.


.. _distutils:

distutils
=========

.. tab:: 中文

    原始的 Python 打包系统，最早在 Python 2.0 中加入标准库，并在 3.12 中被移除。

    由于维护一个打包系统面临的挑战，特别是当功能更新与语言运行时更新紧密耦合时，已不再推荐直接使用 :ref:`distutils`，而是推荐使用 :ref:`Setuptools` 作为替代方案。 :ref:`Setuptools` 不仅提供了 :ref:`distutils` 所不具备的功能（例如依赖声明和入口点声明），它还提供了一个一致的构建接口和功能集，适用于所有受支持的 Python 版本。

    因此， :ref:`distutils` 在 Python 3.10 中被 :pep:`632` 弃用，并在 Python 3.12 中被 :doc:`移除 <python:whatsnew/3.12>`。Setuptools 打包了 distutils 的独立副本，并且即使在 Python < 3.12 中，如果先导入 setuptools 或使用 pip，也会注入 distutils。

.. tab:: 英文

    The original Python packaging system, added to the standard library in
    Python 2.0 and removed in 3.12.

    Due to the challenges of maintaining a packaging system
    where feature updates are tightly coupled to language runtime updates,
    direct usage of :ref:`distutils` has been actively discouraged, with
    :ref:`Setuptools` being the preferred replacement. :ref:`Setuptools`
    not only provides features that plain :ref:`distutils` doesn't offer
    (such as dependency declarations and entry point declarations), it
    also provides a consistent build interface and feature set across all
    supported Python versions.

    Consequently, :ref:`distutils` was deprecated in Python 3.10 by :pep:`632` and
    has been :doc:`removed <python:whatsnew/3.12>` from the standard library in
    Python 3.12.  Setuptools bundles the standalone copy of distutils, and it is
    injected even on Python < 3.12 if you import setuptools first or use pip.


.. _flit:

flit
====

`Docs <https://flit.readthedocs.io/en/latest/>`__ |
`Issues <https://github.com/pypa/flit/issues>`__ |
`PyPI <https://pypi.org/project/flit>`__

.. tab:: 中文

    Flit 提供了一种简单的方法，用于将纯 Python 包和模块创建并上传到 PyPI。它专注于 `让简单的事情变得更简单 <flit-rationale_>`_，以简化打包过程。Flit 可以生成配置文件，快速设置一个简单的项目，构建源代码分发包和 wheel 文件，并将它们上传到 PyPI。

    Flit 使用 ``pyproject.toml`` 来配置项目。Flit 不依赖于 :ref:`setuptools` 等工具来构建分发包，也不依赖于 :ref:`twine` 将其上传到 PyPI。Flit 需要 Python 3，但你可以使用它来分发适用于 Python 2 的模块，只要这些模块可以在 Python 3 中导入。

    Flit 包由 `Matthias Bussonnier <https://github.com/Carreau>`__ 自 2023 年 10 月起在 `tidelift 平台 <https://tidelift.com/lifter/search/pypi/flit>`__ 提供支持，资金会捐赠给 PSF，并指定用于 PyPA 使用。

.. tab:: 英文

    Flit provides a simple way to create and upload pure Python packages and
    modules to PyPI.  It focuses on `making the easy things easy <flit-rationale_>`_
    for packaging.  Flit can generate a configuration file to quickly set up a
    simple project, build source distributions and wheels, and upload them to PyPI.

    Flit uses ``pyproject.toml`` to configure a project. Flit does not rely on tools
    such as :ref:`setuptools` to build distributions, or :ref:`twine` to upload them
    to PyPI. Flit requires Python 3, but you can use it to distribute modules for
    Python 2, so long as they can be imported on Python 3.

    The flit package is lifted by `Matthias Bussonnier
    <https://github.com/Carreau>`__ since October 2023 on the `tidelift platform
    <https://tidelift.com/lifter/search/pypi/flit>`__, and funds sent to the PSF and
    earmarked for PyPA usage.

.. _flit-rationale: https://flit.readthedocs.io/en/latest/rationale.html

.. _hatch:

hatch
=====

`Docs <https://hatch.pypa.io/latest/>`__ |
`GitHub <https://github.com/pypa/hatch>`__ |
`PyPI <https://pypi.org/project/hatch>`__

.. tab:: 中文

    Hatch 是一个统一的命令行工具，旨在方便地管理 Python 开发者的依赖关系和环境隔离。Python 包开发者使用 Hatch 及其 :term:`build backend <Build Backend>` Hatchling 来配置、版本控制、指定依赖关系并将包发布到 PyPI。其插件系统允许轻松扩展功能。

.. tab:: 英文

    Hatch is a unified command-line tool meant to conveniently manage
    dependencies and environment isolation for Python developers. Python
    package developers use Hatch and its :term:`build backend <Build Backend>` Hatchling to
    configure, version, specify dependencies for, and publish packages
    to PyPI. Its plugin system allows for easily extending functionality.

.. _packaging:

packaging
=========

:doc:`Docs <packaging:index>` |
`Issues <https://github.com/pypa/packaging/issues>`__ |
`GitHub <https://github.com/pypa/packaging>`__ |
`PyPI <https://pypi.org/project/packaging>`__

.. tab:: 中文

    Python 打包的核心工具，供 :ref:`pip` 和 :ref:`setuptools` 使用。

    打包库中的核心工具处理 Python 包的版本管理、规范、标记、需求、标签以及类似的属性和任务。大多数 Python 用户在不需要显式调用它的情况下依赖于这个库；这里列出的其他 Python 打包、分发和安装工具的开发者通常会使用它的功能来解析、发现以及处理依赖属性。

    该项目特别专注于实现现代 Python 打包互操作性标准，这些标准定义在 :ref:`packaging-specifications` 中，并且会为那些与这些标准不兼容的足够古老的遗留包报告错误。与此相比， :ref:`distlib` 项目是一个更宽松的库，它试图在 :ref:`packaging` 会报告错误的情况下，对模糊的元数据提供一个合理的解释。

.. tab:: 英文

    Core utilities for Python packaging used by :ref:`pip` and :ref:`setuptools`.

    The core utilities in the packaging library handle version handling,
    specifiers, markers, requirements, tags, and similar attributes and
    tasks for Python packages. Most Python users rely on this library
    without needing to explicitly call it; developers of the other Python
    packaging, distribution, and installation tools listed here often use
    its functionality to parse, discover, and otherwise handle dependency
    attributes.

    This project specifically focuses on implementing the modern Python
    packaging interoperability standards defined at
    :ref:`packaging-specifications`, and will report errors for
    sufficiently old legacy packages that are incompatible with those
    standards. In contrast, the :ref:`distlib` project is a more
    permissive library that attempts to provide a plausible reading of
    ambiguous metadata in cases where :ref:`packaging` will instead report
    on error.

.. _pip:

pip
===

`Docs <https://pip.pypa.io/>`__ |
`Issues <https://github.com/pypa/pip/issues>`__ |
`GitHub <https://github.com/pypa/pip>`__ |
`PyPI <https://pypi.org/project/pip/>`__

.. tab:: 中文

    最流行的 Python 包安装工具，也是现代版本 Python 附带的工具。

    它提供了从 PyPI 和其他 Python 包索引查找、下载和安装包的基本核心功能，并且可以通过其命令行接口（CLI）融入到各种开发工作流中。

.. tab:: 英文

    The most popular tool for installing Python packages, and the one
    included with modern versions of Python.

    It provides the essential core features for finding, downloading, and
    installing packages from PyPI and other Python package indexes, and can be
    incorporated into a wide range of development workflows via its
    command-line interface (CLI).

.. _Pipenv:

Pipenv
======

:doc:`Docs <pipenv:index>` |
`Source <https://github.com/pypa/pipenv>`__ |
`Issues <https://github.com/pypa/pipenv/issues>`__ |
`PyPI <https://pypi.org/project/pipenv>`__

.. tab:: 中文

    Pipenv 是一个旨在将所有打包领域的最佳实践带入 Python 世界的项目。它将 :ref:`Pipfile`、:ref:`pip` 和 :ref:`virtualenv` 融合成一个单一的工具链。它可以自动导入 ``requirements.txt``，并使用 `safety <https://pyup.io/safety>`_ 检查 `Pipfile`_ 中的 CVE 漏洞。

    Pipenv 旨在帮助用户在命令行上管理环境、依赖关系和导入的包。它在 Windows 上也表现良好（这是其他工具常常忽视的），可以生成和检查文件哈希，以确保依赖关系规范与哈希锁定的依赖一致，并简化包和依赖的卸载过程。

.. tab:: 英文

    Pipenv is a project that aims to bring the best of all packaging worlds to the
    Python world. It harnesses :ref:`Pipfile`, :ref:`pip`, and :ref:`virtualenv`
    into one single toolchain. It can autoimport ``requirements.txt`` and also
    check for CVEs in `Pipfile`_ using `safety <https://pyup.io/safety>`_.

    Pipenv aims to help users manage environments, dependencies, and
    imported packages on the command line. It also works well on Windows
    (which other tools often underserve), makes and checks file hashes,
    to ensure compliance with hash-locked dependency specifiers, and eases
    uninstallation of packages and dependencies.

.. _Pipfile:

Pipfile
=======

`Source <https://github.com/pypa/pipfile>`__

.. tab:: 中文

    :file:`Pipfile` 及其姊妹文件 :file:`Pipfile.lock` 是一个面向应用的高层次替代方案，用于取代 :ref:`pip` 的低层次 :file:`requirements.txt` 文件。

.. tab:: 英文

    :file:`Pipfile` and its sister :file:`Pipfile.lock` are a higher-level
    application-centric alternative to :ref:`pip`'s lower-level
    :file:`requirements.txt` file.

.. _pipx:

pipx
====

`Docs <https://pipx.pypa.io/>`__ |
`GitHub <https://github.com/pypa/pipx>`__ |
`PyPI <https://pypi.org/project/pipx/>`__

.. tab:: 中文

    pipx 是一个工具，用于安装和运行 Python 命令行应用程序，而不会导致与系统中安装的其他包发生依赖冲突。

.. tab:: 英文

    pipx is a tool to install and run Python command-line applications without causing dependency conflicts with other packages installed on the system.


Python 打包用户指南
===========================

**Python Packaging User Guide**

:doc:`Docs <index>` |
`Issues <https://github.com/pypa/packaging.python.org/issues>`__ |
`GitHub <https://github.com/pypa/packaging.python.org>`__

本指南！

This guide!

.. _readme_renderer:

readme_renderer
===============

`GitHub and docs <https://github.com/pypa/readme_renderer/>`__ |
`PyPI <https://pypi.org/project/readme-renderer/>`__

.. tab:: 中文

    ``readme_renderer`` 是一个供包开发者使用的库，用于将其用户文档（README）文件从标记语言（如 Markdown 或 reStructuredText）渲染为 HTML。开发者可以单独调用它，也可以通过 :ref:`twine` 在发布管理过程中使用，检查其包描述是否能在 PyPI 上正确显示。

.. tab:: 英文

    ``readme_renderer`` is a library that package developers use to render
    their user documentation (README) files into HTML from markup
    languages such as Markdown or reStructuredText. Developers call it on
    its own or via :ref:`twine`, as part of their release management
    process, to check that their package descriptions will properly
    display on PyPI.

.. _setuptools:
.. _easy_install:

Setuptools
==========

`Docs <https://setuptools.readthedocs.io/en/latest/>`__ |
`Issues <https://github.com/pypa/setuptools/issues>`__ |
`GitHub <https://github.com/pypa/setuptools>`__ |
`PyPI <https://pypi.org/project/setuptools>`__

.. tab:: 中文

    Setuptools（包括 ``easy_install``）是对 Python distutils 的一组增强，旨在让你更轻松地构建和分发 Python :term:`distributions <Distribution Package>`，特别是那些依赖于其他包的分发包。

.. tab:: 英文

    Setuptools (which includes ``easy_install``) is a collection of
    enhancements to the Python distutils that allow you to more easily
    build and distribute Python :term:`distributions <Distribution
    Package>`, especially ones that have dependencies on other packages.

.. _trove-classifiers:

trove-classifiers
=================

`Issues <https://github.com/pypa/trove-classifiers/issues>`__ | `GitHub
<https://github.com/pypa/trove-classifiers>`__ | `PyPI
<https://pypi.org/project/trove-classifiers/>`__

.. tab:: 中文

    trove-classifiers 是 PyPI 上 `分类器 <https://pypi.org/classifiers/>`_ 的标准来源，项目维护者使用它来 :ref:`系统地描述他们的项目 <core-metadata-classifier>`，以便用户可以更容易地在 PyPI 上找到符合他们需求的项目。

    trove-classifiers 包含了有效的分类器列表和已弃用的分类器列表（已弃用的分类器与替代它们的分类器配对）。使用此包可以验证用于准备上传到 PyPI 的包中的分类器。由于这个分类器列表是作为代码发布的，你可以安装并导入它，与参考 PyPI 上发布的 `列表 <https://pypi.org/classifiers/>`_ 相比，这样可以提供更便捷的工作流。该项目的 `问题追踪器 <https://github.com/pypa/trove-classifiers/issues>`_ 用于讨论提议的分类器和新分类器的请求。

.. tab:: 英文

    trove-classifiers is the canonical source for `classifiers on PyPI
    <https://pypi.org/classifiers/>`_, which project maintainers use to
    :ref:`systematically describe their projects <core-metadata-classifier>`
    so that users can better find projects that match their needs on the PyPI.

    The trove-classifiers package contains a list of valid classifiers and
    deprecated classifiers (which are paired with the classifiers that replace
    them).  Use this package to validate classifiers used in packages intended for
    uploading to PyPI. As this list of classifiers is published as code, you
    can install and import it, giving you a more convenient workflow compared to
    referring to the `list published on PyPI <https://pypi.org/classifiers/>`_. The
    `issue tracker <https://github.com/pypa/trove-classifiers/issues>`_ for the
    project hosts discussions on proposed classifiers and requests for new
    classifiers.


.. _twine:

twine
=====

`Docs <https://twine.readthedocs.io/en/latest/>`__ |
`Issues <https://github.com/pypa/twine/issues>`__ |
`GitHub <https://github.com/pypa/twine>`__ |
`PyPI <https://pypi.org/project/twine>`__


.. tab:: 中文

    Twine 是开发者用来将包上传到 Python 包索引或其他 Python 包索引的主要工具。它是一个命令行程序，将程序文件和元数据传递给 Web API。开发者使用它是因为它是官方的 PyPI 上传工具，快速且安全，得到维护，并且可靠地工作。

.. tab:: 英文

    Twine is the primary tool developers use to upload packages to the
    Python Package Index or other Python package indexes. It is a
    command-line program that passes program files and metadata to a web
    API. Developers use it because it's the official PyPI upload tool,
    it's fast and secure, it's maintained, and it reliably works.


.. _virtualenv:

virtualenv
==========

`Docs <https://virtualenv.pypa.io/en/stable/index.html>`__ |
`Issues <https://github.com/pypa/virtualenv/issues>`__ |
`GitHub <https://github.com/pypa/virtualenv>`__ |
`PyPI <https://pypi.org/project/virtualenv/>`__

.. tab:: 中文

    virtualenv 是一个用于创建隔离的 Python :term:`Virtual Environments <Virtual Environment>` 的工具，类似于 :ref:`venv`。与 :ref:`venv` 不同，virtualenv 可以为其他版本的 Python 创建虚拟环境，它通过 PATH 环境变量来定位这些版本。它还提供了便捷的功能来配置、维护、复制和排查虚拟环境的问题。欲了解更多信息，请参阅 :ref:`创建和使用虚拟环境` 部分。

.. tab:: 英文

    virtualenv is a tool for creating isolated Python :term:`Virtual Environments
    <Virtual Environment>`, like :ref:`venv`. Unlike :ref:`venv`, virtualenv can
    create virtual environments for other versions of Python, which it locates
    using the PATH environment variable. It also provides convenient features for
    configuring, maintaining, duplicating, and troubleshooting virtual environments.
    For more information, see the section on :ref:`Creating and using Virtual
    Environments`.


.. _warehouse:

Warehouse
=========

`Docs <https://warehouse.pypa.io/>`__ |
`Issues <https://github.com/pypa/warehouse/issues>`__ |
`GitHub <https://github.com/pypa/warehouse>`__

.. tab:: 中文

    当前为 :term:`Python Package Index (PyPI)` 提供支持的代码库。它托管在 `pypi.org <https://pypi.org/>`_ 上。是 :ref:`pip` 下载的默认来源。

.. tab:: 英文

    The current codebase powering the :term:`Python Package Index
    (PyPI)`. It is hosted at `pypi.org <https://pypi.org/>`_. The default
    source for :ref:`pip` downloads.


.. _wheel:

wheel
=====

`Docs <https://wheel.readthedocs.io/en/latest/>`__ |
`Issues <https://github.com/pypa/wheel/issues>`__ |
`GitHub <https://github.com/pypa/wheel>`__ |
`PyPI <https://pypi.org/project/wheel>`__

.. tab:: 中文

    主要来说，wheel 项目提供了 ``bdist_wheel`` :ref:`setuptools` 扩展，用于创建 :term:`wheel distributions <Wheel>`。此外，它还提供了自己的命令行工具，用于创建和安装 wheel 文件。

    另请参见 `auditwheel <https://github.com/pypa/auditwheel>`__，这是一个供包开发者使用的工具，用于检查和修复他们正在制作的二进制 wheel 格式的 Python 包。它提供了发现依赖关系、检查元数据合规性以及修复 wheel 和元数据的功能，以正确链接和包含包中的外部共享库。

.. tab:: 英文

    Primarily, the wheel project offers the ``bdist_wheel`` :ref:`setuptools` extension for
    creating :term:`wheel distributions <Wheel>`.  Additionally, it offers its own
    command line utility for creating and installing wheels.

    See also `auditwheel <https://github.com/pypa/auditwheel>`__, a tool
    that package developers use to check and fix Python packages they are
    making in the binary wheel format. It provides functionality to
    discover dependencies, check metadata for compliance, and repair the
    wheel and metadata to properly link and include external shared
    libraries in a package.


非 PyPA 项目
#################

**Non-PyPA Projects**

.. _buildout:

buildout
========

`Docs <http://www.buildout.org/en/latest/>`__ |
`Issues <https://bugs.launchpad.net/zc.buildout>`__ |
`PyPI <https://pypi.org/project/zc.buildout>`__ |
`GitHub <https://github.com/buildout/buildout/>`__

.. tab:: 中文

    Buildout 是一个基于 Python 的构建系统，用于从多个部分创建、组装和部署应用程序，其中一些部分可能不是基于 Python 的。它允许你创建一个 buildout 配置，并在以后重新生成相同的软件。

.. tab:: 英文

    Buildout is a Python-based build system for creating, assembling and deploying
    applications from multiple parts, some of which may be non-Python-based.  It
    lets you create a buildout configuration and reproduce the same software later.

.. _conda:

conda
=====

:doc:`Docs <conda:index>`

.. tab:: 中文

    conda 是 `Anaconda <https://docs.anaconda.com/anaconda/>`__ Python 安装的包管理工具。Anaconda Python 是由 `Anaconda, Inc <https://www.anaconda.com/products/individual>`__ 提供的一个专门面向科学社区的发行版，特别是在 Windows 上，那里安装二进制扩展通常较为困难。

    Conda 是与 :ref:`pip`、virtualenv 和 wheel 完全独立的工具，但在包管理、虚拟环境管理和二进制扩展部署方面，提供了它们的许多组合功能。

    Conda 不从 PyPI 安装包，只能从官方 Anaconda 仓库、anaconda.org（一个用户贡献的 *conda* 包的地方）或本地（例如内网）包服务器安装包。然而，请注意， :ref:`pip` 可以安装到 conda 环境中，并与 conda 并行工作，用于管理来自 PyPI 的 :term:`distributions <Distribution Package>`。另外， `conda skeleton <https://docs.conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-pkgs-skeleton.html>`__ 是一个工具，它通过先从 PyPI 获取包并修改其元数据，使得 Python 包可以被 conda 安装。

.. tab:: 英文

    conda is the package management tool for `Anaconda
    <https://docs.anaconda.com/anaconda/>`__ Python installations.
    Anaconda Python is a distribution from `Anaconda, Inc
    <https://www.anaconda.com/products/individual>`__ specifically aimed at the scientific
    community, and in particular on Windows where the installation of binary
    extensions is often difficult.

    Conda is a completely separate tool from :ref:`pip`, virtualenv and wheel, but provides
    many of their combined features in terms of package management, virtual environment
    management and deployment of binary extensions.

    Conda does not install packages from PyPI and can install only from
    the official Anaconda repositories, or anaconda.org (a place for
    user-contributed *conda* packages), or a local (e.g. intranet) package
    server.  However, note that :ref:`pip` can be installed into, and work
    side-by-side with conda for managing :term:`distributions
    <Distribution Package>` from PyPI. Also, `conda skeleton
    <https://docs.conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-pkgs-skeleton.html>`__
    is a tool to make Python packages installable by conda by first
    fetching them from PyPI and modifying their metadata.

.. _devpi:

devpi
=====

`Docs <http://doc.devpi.net/latest/>`__ |
:gh:`Issues <devpi/devpi/issues>` |
`PyPI <https://pypi.org/project/devpi>`__

.. tab:: 中文

    devpi 具有一个强大的 PyPI 兼容服务器和 PyPI 代理缓存，并配有一个互补的命令行工具，用于推动 Python 的打包、测试和发布活动。devpi 还提供了一个可浏览和可搜索的 Web 界面。  
    devpi 支持镜像 PyPI、多个 :term:`package indexes <Package Index>` 及其继承、这些索引之间的同步、索引复制和故障切换，以及包的上传。

.. tab:: 英文

    devpi features a powerful PyPI-compatible server and PyPI proxy cache
    with a complementary command line tool to drive packaging, testing and
    release activities with Python. devpi also provides a browsable and
    searchable web interface.
    devpi supports mirroring PyPI, multiple
    :term:`package indexes <Package Index>` with inheritance, syncing between
    these indexes, index replication and fail-over, and package upload.

.. _dumb-pypi:

dumb-pypi
=========

`GitHub <https://github.com/chriskuehl/dumb-pypi>`__ |
`PyPI <https://pypi.org/project/dumb-pypi>`__

.. tab:: 中文

    dumb-pypi 是一个简单的 :term:`package index <Package Index>` 静态文件站点生成器，生成的站点必须由静态文件 Web 服务器托管，以成为包索引。它支持提供哈希、核心元数据和 yank 状态。

.. tab:: 英文

    dumb-pypi is a simple :term:`package index <Package Index>` static file site
    generator, which then must be hosted by a static file webserver to become the
    package index. It supports serving the hash, core-metadata, and yank-status.

.. _enscons:

enscons
=======

:gh:`Source <dholth/enscons>` |
:gh:`Issues <dholth/enscons/issues>` |
`PyPI <https://pypi.org/project/enscons>`__

.. tab:: 中文

    Enscons 是一个基于 `SCons`_ 的 Python 打包工具。它构建与 :ref:`pip` 兼容的源代码分发包和 wheel 文件，而无需使用 distutils 或 setuptools，包括带有 C 扩展的分发包。Enscons 的架构和理念与 :ref:`distutils` 不同。与其将构建功能添加到 Python 打包系统中，enscons 将 Python 打包功能添加到一个通用的构建系统中。Enscons 帮助你构建可以被 :ref:`pip` 自动构建的源代码分发包（sdist）和独立于 enscons 的 wheel 文件。

.. tab:: 英文

    Enscons is a Python packaging tool based on `SCons`_. It builds
    :ref:`pip`-compatible source distributions and wheels without using
    distutils or setuptools, including distributions with C
    extensions. Enscons has a different architecture and philosophy than
    :ref:`distutils`. Rather than adding build features to a Python
    packaging system, enscons adds Python packaging to a general purpose
    build system. Enscons helps you to build sdists that can be
    automatically built by :ref:`pip`, and wheels that are independent of
    enscons.

.. _SCons: https://scons.org/

.. _flaskpypiproxy:

Flask-Pypi-Proxy
================

`Docs <https://flask-pypi-proxy.readthedocs.io>`__ |
:gh:`GitHub <tzulberti/Flask-PyPi-Proxy>` |
`PyPI <https://pypi.org/project/Flask-Pypi-Proxy/>`__

.. tab:: 中文

    .. warning:: 不再维护，项目已归档

    Flask-Pypi-Proxy 是一个作为 PyPI 缓存代理的 :term:`package index <Package Index>`。

.. tab:: 英文

    .. warning:: Not maintained, project archived

    Flask-Pypi-Proxy is a :term:`package index <Package Index>` as a cached
    proxy for PyPI.

.. _hashdist:

Hashdist
========

`Docs <https://hashdist.readthedocs.io/en/latest/>`__ |
`GitHub <https://github.com/hashdist/hashdist/>`__

.. tab:: 中文

    Hashdist 是一个用于构建非根软件分发的库。Hashdist 力图成为“在 Debian 技术无法使用的情况下，选择的 Debian”。对 Python 开发者来说，理解 Hashdist 最好的方式可能是将其视为 :ref:`virtualenv` 和 :ref:`buildout` 的一个更强大的混合体。它旨在解决安装科学软件的问题，并使包分发无状态、可缓存且可分支。它被一些研究人员使用，但自 2016 年以来一直缺乏维护。

.. tab:: 英文

    Hashdist is a library for building non-root software
    distributions. Hashdist is trying to be “the Debian of choice for
    cases where Debian technology doesn’t work”. The best way for
    Pythonistas to think about Hashdist may be a more powerful hybrid of
    :ref:`virtualenv` and :ref:`buildout`. It is aimed at solving the
    problem of installing scientific software, and making package
    distribution stateless, cached, and branchable. It is used by some
    researchers but has been lacking in maintenance since 2016.

.. _maturin:

Maturin
=======

`Docs <https://www.maturin.rs>`__ |
`GitHub <https://github.com/PyO3/maturin>`__

.. tab:: 中文

    Maturin 是一个用于 Rust 扩展模块的构建后端，也用 Rust 编写。它支持在 Windows、Linux、macOS 和 FreeBSD 上为 Python 3.7+ 构建 wheel 文件，可以将它们上传到 PyPI，并且具有基本的 PyPy 和 GraalPy 支持。

.. tab:: 英文

    Maturin is a build backend for Rust extension modules, also written in
    Rust. It supports building wheels for python 3.7+ on Windows, Linux, macOS and
    FreeBSD, can upload them to PyPI and has basic PyPy and GraalPy support.


.. _meson-python:

meson-python
============

`Docs <https://meson-python.readthedocs.io/en/latest/>`__ |
`GitHub <https://github.com/mesonbuild/meson-python>`__

.. tab:: 中文

    ``meson-python`` 是一个使用 Meson_ 构建系统的构建后端。它允许 Python 包作者将 Meson_ 作为其包的构建系统。它支持多种语言，包括 C，并能够满足大多数复杂构建配置的需求。

.. tab:: 英文

    ``meson-python`` is a build backend that uses the Meson_ build system. It enables
    Python package authors to use Meson_ as the build system for their package. It
    supports a wide variety of languages, including C, and is able to fill the needs
    of most complex build configurations.

.. _Meson: https://github.com/mesonbuild/meson

.. _multibuild:

multibuild
==========

`GitHub <https://github.com/multi-build/multibuild>`__

.. tab:: 中文

    Multibuild 是一组用于构建和测试 Python :term:`wheels <Wheel>` 的 CI 脚本，支持 Linux、macOS 和（较少灵活的）Windows。另请参见 :ref:`cibuildwheel`。

.. tab:: 英文

    Multibuild is a set of CI scripts for building and testing Python :term:`wheels <Wheel>` for
    Linux, macOS, and (less flexibly) Windows. Also see :ref:`cibuildwheel`.

.. _nginx_pypi_cache:

nginx_pypi_cache
================

:gh:`GitHub <hauntsaninja/nginx_pypi_cache>`

.. tab:: 中文

    nginx_pypi_cache 是一个使用 `nginx <https://nginx.org/en/>`_ 的 :term:`package index <Package Index>` 缓存代理。

.. tab:: 英文

    nginx_pypi_cache is a :term:`package index <Package Index>` caching proxy using `nginx <https://nginx.org/en/>`_.

.. _pdm:

pdm
===

`Docs <https://pdm.fming.dev/>`__ |
`GitHub <https://github.com/pdm-project/pdm/>`__ |
`PyPI <https://pypi.org/project/pdm>`__

.. tab:: 中文

    PDM 是一个现代的 Python 包管理器。它使用 :term:`pyproject.toml` 来存储 :pep:`621` 中定义的项目元数据。

.. tab:: 英文

    PDM is a modern Python package manager. It uses :term:`pyproject.toml` to store project metadata as defined in :pep:`621`.

.. _pex:

pex
===

`Docs <https://pex.readthedocs.io/en/latest/>`__ |
`GitHub <https://github.com/pantsbuild/pex/>`__ |
`PyPI <https://pypi.org/project/pex>`__

.. tab:: 中文

    Pex 是一个用于生成 :file:`.pex`（Python EXecutable）文件的工具，提供独立的 Python 环境，类似于 :ref:`virtualenv`。PEX 文件是 :doc:`zipapps <python:library/zipapp>`，使得 Python 应用程序的部署像 ``cp`` 一样简单。单个 PEX 文件可以支持多个目标平台，并且可以从标准的 :ref:`pip` 可解析的依赖项、使用 ``pex3 lock ...`` 生成的锁文件，甚至另一个 PEX 文件中创建。PEX 文件可以选择性地嵌入支持将 PEX 文件转换为标准 venv、绘制依赖关系图等功能的工具。

.. tab:: 英文

    Pex is a tool for generating :file:`.pex` (Python EXecutable)
    files, standalone Python environments in the spirit of :ref:`virtualenv`.
    PEX files are :doc:`zipapps <python:library/zipapp>` that
    make deployment of Python applications as simple as ``cp``. A single PEX
    file can support multiple target platforms and can be created from standard
    :ref:`pip`-resolvable requirements, a lockfile generated with ``pex3 lock ...``
    or even another PEX. PEX files can optionally have tools embedded that support
    turning the PEX file into a standard venv, graphing dependencies and more.

.. _pip-tools:

pip-tools
=========

`Docs <https://pip-tools.readthedocs.io/en/latest/>`__ |
`GitHub <https://github.com/jazzband/pip-tools/>`__ |
`PyPI <https://pypi.org/project/pip-tools/>`__

.. tab:: 中文

    pip-tools 是一套工具，专为 Python 系统管理员和发布经理设计，特别适合那些希望保持构建的确定性同时又能跟上其依赖项新版本的用户。用户可以通过哈希指定依赖项的特定版本，方便地从程序的其他部分的信息中生成正确格式的需求列表，更新所有依赖项（这是 :ref:`pip` 当前不提供的功能），并为程序创建约束层以遵循。

.. tab:: 英文

    pip-tools is a suite of tools meant for Python system administrators
    and release managers who particularly want to keep their builds
    deterministic yet stay up to date with new versions of their
    dependencies. Users can specify particular release of their
    dependencies via hash, conveniently make a properly formatted list of
    requirements from information in other parts of their program, update
    all dependencies (a feature :ref:`pip` currently does not provide), and
    create layers of constraints for the program to obey.

.. _pip2pi:

pip2pi
=========

:gh:`GitHub <wolever/pip2pi>` |
`PyPI <https://pypi.org/project/pip2pi/>`__

.. tab:: 中文

    pip2pi 是一个 :term:`包索引 <Package Index>` 服务器，其中特定的包是手动同步的。

.. tab:: 英文

    pip2pi is a :term:`package index <Package Index>` server where specific packages are manually synchronised.

.. _piwheels:

piwheels
========

`Website <https://www.piwheels.org/>`__ |
:doc:`Docs <piwheels:index>` |
`GitHub <https://github.com/piwheels/piwheels/>`__

.. tab:: 中文

    piwheels 是一个网站及其底层软件，它从 PyPI 获取源代码分发包，并将它们编译成针对 Raspberry Pi 计算机安装优化的二进制 wheel 文件。Raspberry Pi OS 预先配置了 pip，将 piwheels.org 作为 PyPI 的附加索引。

.. tab:: 英文

    piwheels is a website, and software underpinning it, that fetches
    source code distribution packages from PyPI and compiles them into
    binary wheels that are optimized for installation onto Raspberry Pi
    computers. Raspberry Pi OS pre-configures pip to use piwheels.org as
    an additional index to PyPI.

.. _poetry:

poetry
======

`Docs <https://python-poetry.org/>`__ |
`GitHub <https://github.com/python-poetry/poetry>`__ |
`PyPI <https://pypi.org/project/poetry/>`__

.. tab:: 中文

    poetry 是一个命令行工具，用于处理依赖项安装和隔离，以及 Python 包的构建和打包。它使用 ``pyproject.toml``，并且不依赖于 :ref:`pip` 中的解析器功能，而是提供了自己的依赖解析器。它通过本地缓存关于依赖项的元数据，试图加速用户的安装和依赖解析体验。

.. tab:: 英文

    poetry is a command-line tool to handle dependency installation and
    isolation as well as building and packaging of Python packages. It
    uses ``pyproject.toml`` and, instead of depending on the resolver
    functionality within :ref:`pip`, provides its own dependency resolver.
    It attempts to speed users' experience of installation and dependency
    resolution by locally caching metadata about dependencies.

.. _proxpi:

proxpi
======

:gh:`GitHub <EpicWink/proxpi>` |
`PyPI <https://pypi.org/project/proxpi/>`__

.. tab:: 中文

    proxpi 是一个简单的 :term:`包索引 <Package Index>`，它使用缓存代理 PyPI 和其他索引。

.. tab:: 英文

    proxpi is a simple :term:`package index <Package Index>` which proxies PyPI and other indexes with caching.

.. _pulppython:

Pulp-python
===========

`Docs <https://docs.pulpproject.org/pulp_python/>`__ |
:gh:`GitHub <pulp/pulp_python>` |
`PyPI <https://pypi.org/project/pulp-python/>`__

.. tab:: 中文

    Pulp-python 是 `Pulp <https://pulpproject.org/>`_ 的 Python :term:`package index <Package Index>` 插件。Pulp-python 支持由本地或 `AWS S3`_ 支持的镜像、包上传以及代理到多个包索引。

.. tab:: 英文

    Pulp-python is the Python :term:`package index <Package Index>` plugin for
    `Pulp <https://pulpproject.org/>`_. Pulp-python supports mirrors backed by
    local or `AWS S3`_, package upload, and proxying to multiple package
    indexes.

.. _pypicloud:

PyPI Cloud
==========

`Docs <https://pypicloud.readthedocs.io/>`__ |
:gh:`GitHub <stevearc/pypicloud>` |
`PyPI <https://pypi.org/project/pypicloud/>`__

.. tab:: 中文

    .. warning:: 未维护，项目已归档

    PyPI Cloud 是一个 :term:`package index <Package Index>` 服务器，由 `AWS S3`_ 或其他云存储服务，或者本地文件提供支持。PyPI Cloud 支持对 PyPI 的重定向/缓存代理，以及认证和授权。

.. tab:: 英文

    .. warning:: Not maintained, project archived

    PyPI Cloud is a :term:`package index <Package Index>` server, backed by
    `AWS S3`_ or another cloud storage service, or local files. PyPI Cloud
    supports redirect/cached proxying for PyPI, as well as authentication and
    authorisation.

.. _pypiprivate:

pypiprivate
===========

:gh:`GitHub <helpshift/pypiprivate>` |
`PyPI <https://pypi.org/project/pypiprivate/>`__

.. tab:: 中文

    pypiprivate 将本地（或 `AWS S3`_ 托管的）包目录作为 :term:`包索引 <Package Index>` 提供。

.. tab:: 英文

    pypiprivate serves a local (or `AWS S3`_-hosted) directory of packages as a :term:`package index <Package Index>`.

.. _pypiserver:

pypiserver
==========

`GitHub <https://github.com/pypiserver/pypiserver>`__ |
`PyPI <https://pypi.org/project/pypiserver/>`__

.. tab:: 中文

    pypiserver 是一个极简的应用程序，作为组织内的私有 Python :term:`package index <Package Index>`（来自本地目录）使用，提供一个简单的 API 和浏览器界面。你可以使用标准上传工具上传私有包，用户可以使用 :ref:`pip` 下载和安装这些包，而无需公开发布它们。使用 pypiserver 的组织通常会同时从 pypiserver 和 PyPI 下载包。

.. tab:: 英文

    pypiserver is a minimalist application that serves as a private Python
    :term:`package index <Package Index>` (from a local directory) within
    organizations, implementing a simple API and
    browser interface. You can upload private packages using standard
    upload tools, and users can download and install them with :ref:`pip`,
    without publishing them publicly. Organizations who use pypiserver
    usually download packages both from pypiserver and from PyPI.

.. _pyscaffold:

PyScaffold
==========

`Docs <https://pyscaffold.org>`__ |
`GitHub <https://github.com/pyscaffold/pyscaffold>`__ |
`PyPI <https://pypi.org/project/pyscaffold/>`__

.. tab:: 中文

    PyScaffold 是一个用于引导 Python 包的项目生成器，生成的包已准备好共享到 PyPI 并通过 :ref:`pip` 安装。它依赖于一组合理的默认配置，适用于已建立的工具（如 :ref:`setuptools`、pytest_ 和 Sphinx_），提供一个高效的开发环境，使开发者能够立即开始编码。PyScaffold 还可以与现有项目一起使用，简化打包过程。

.. tab:: 英文

    PyScaffold is a project generator for bootstrapping Python packages,
    ready to be shared on PyPI and installable via :ref:`pip`.
    It relies on a set of sane default configurations for established tools
    (such as :ref:`setuptools`, pytest_ and Sphinx_) to provide a productive
    environment so developers can start coding right away.
    PyScaffold can also be used with existing projects to make packaging
    easier.

.. _pywharf:

pywharf
=======

:gh:`GitHub <pywharf/pywharf>` |
`PyPI <https://pypi.org/project/pywharf>`__

.. tab:: 中文

    .. warning:: 未维护，项目已归档

    pywharf 是一个 :term:`包索引 <Package Index>` 服务器，在本地或从 `GitHub <https://github.com/>`_ 提供文件。

.. tab:: 英文

    .. warning:: Not maintained, project archived

    pywharf is a :term:`package index <Package Index>` server, serving files locally or from `GitHub <https://github.com/>`_.

.. _scikit-build:

scikit-build
============

`Docs <https://scikit-build.readthedocs.io/en/latest/>`__ |
`GitHub <https://github.com/scikit-build/scikit-build/>`__ |
`PyPI <https://pypi.org/project/scikit-build>`__

.. tab:: 中文

    Scikit-build 是一个用于 CPython 的 :ref:`setuptools` 封装器，用于构建 C/C++/Fortran/Cython 扩展。它使用 `cmake <https://pypi.org/project/cmake>`__ （可在 PyPI 上获取）提供更好的支持，适用于额外的编译器、构建系统、交叉编译，以及定位依赖项及其相关的构建需求。为了加速和并行化大型项目的构建，用户可以安装 `ninja <https://pypi.org/project/ninja>`__ （同样可在 PyPI 上获取）。

.. tab:: 英文

    Scikit-build is a :ref:`setuptools` wrapper for CPython that builds
    C/C++/Fortran/Cython extensions It uses
    `cmake <https://pypi.org/project/cmake>`__ (available on PyPI) to provide
    better support for additional compilers, build systems, cross compilation, and
    locating dependencies and their associated build requirements. To speed up and
    parallelize the build of large projects, the user can install `ninja
    <https://pypi.org/project/ninja>`__ (also available on PyPI).

.. _scikit-build-core:

scikit-build-core
=================

`Docs <https://scikit-build-core.readthedocs.io/en/latest/>`__ |
`GitHub <https://github.com/scikit-build/scikit-build-core/>`__ |
`PyPI <https://pypi.org/project/scikit-build-core>`__

.. tab:: 中文

    Scikit-build-core 是一个用于 CPython C/C++/Fortran/Cython 扩展的构建后端。它使用户能够使用 `cmake <https://pypi.org/project/cmake>`__ （可在 PyPI 上获取）编写扩展，以提供对额外编译器、构建系统、交叉编译以及定位依赖项及其相关构建需求的更好支持。如果系统上没有 CMake/Ninja，它们会自动从 PyPI 下载。

.. tab:: 英文

    Scikit-build-core is a build backend for CPython C/C++/Fortran/Cython
    extensions.  It enables users to write extensions with `cmake
    <https://pypi.org/project/cmake>`__ (available on PyPI) to provide better
    support for additional compilers, build systems, cross compilation, and
    locating dependencies and their associated build requirements. CMake/Ninja
    are automatically downloaded from PyPI if not available on the system.

.. _shiv:

shiv
====

`Docs <https://shiv.readthedocs.io/en/latest/>`__ |
`GitHub <https://github.com/linkedin/shiv>`__ |
`PyPI <https://pypi.org/project/shiv/>`__

.. tab:: 中文

    shiv 是一个命令行工具，用于构建完全自包含的 Python zipapp，如 :pep:`441` 中所述，但包含了所有的依赖项。它的主要目标是使得分发 Python 应用程序和命令行工具变得快速且简便。

.. tab:: 英文

    shiv is a command line utility for building fully self contained
    Python zipapps as outlined in :pep:`441`, but with all their
    dependencies included. Its primary goal is making distributing Python
    applications and command line tools fast & easy.

.. _simpleindex:

simpleindex
===========

:gh:`GitHub <uranusjr/simpleindex>` |
`PyPI <https://pypi.org/project/simpleindex/>`__

.. tab:: 中文

    simpleindex 是一个 :term:`package index <Package Index>`，它将 URL 路由到多个包索引（包括 PyPI），提供本地（或云托管，例如 `AWS S3`_，通过自定义插件）包目录的服务，并支持自定义插件。

.. tab:: 英文

    simpleindex is a :term:`package index <Package Index>` which routes URLs to
    multiple package indexes (including PyPI), serves local (or cloud-hosted,
    for example `AWS S3`_, with a custom plugin) directories of packages, and
    supports custom plugins.

.. _spack:

Spack
=====

:doc:`Docs <spack:index>` |
`GitHub <https://github.com/spack/spack>`__ |
`Paper <https://www.computer.org/csdl/proceedings-article/sc/2015/2807623/12OmNBf94Xq>`__ |
`Slides <https://tgamblin.github.io/files/Gamblin-Spack-SC15-Talk.pdf>`__

.. tab:: 中文

    Spack 是一个灵活的包管理器，旨在支持多个版本、配置、平台和编译器。Spack 类似于 Homebrew，但包是用 Python 编写的，并且进行了参数化，以允许轻松切换编译器、库版本、构建选项等。多个版本的包可以在同一系统上共存。Spack 旨在快速构建高性能科学应用程序，特别是在集群和超级计算机上。

    Spack 目前不在 PyPI 上（尚未），但它无需安装，可以在从 GitHub 克隆后立即使用。

.. tab:: 英文

    A flexible package manager designed to support multiple versions,
    configurations, platforms, and compilers.  Spack is like Homebrew, but
    packages are written in Python and parameterized to allow easy
    swapping of compilers, library versions, build options,
    etc. Arbitrarily many versions of packages can coexist on the same
    system. Spack was designed for rapidly building high performance
    scientific applications on clusters and supercomputers.

    Spack is not in PyPI (yet), but it requires no installation and can be
    used immediately after cloning from GitHub.

.. _zestreleaser:

zest.releaser
=============

`Docs <https://zestreleaser.readthedocs.io/en/latest/>`__ |
`GitHub <https://github.com/zestsoftware/zest.releaser/>`__ |
`PyPI <https://pypi.org/project/zest.releaser/>`__

.. tab:: 中文

    ``zest.releaser`` 是一个 Python 包发布工具，提供了一个基于 :ref:`twine` 的抽象层。Python 开发者使用 ``zest.releaser`` 来自动化增量包版本号、更新变更日志、在版本控制中标记发布以及将新包上传到 PyPI。

.. tab:: 英文

    ``zest.releaser`` is a Python package release tool providing an
    abstraction layer on top of :ref:`twine`. Python developers use
    ``zest.releaser`` to automate incrementing package version numbers,
    updating changelogs, tagging releases in source control, and uploading
    new packages to PyPI.

标准库项目
#########################

**Standard Library Projects**

.. _ensurepip:

ensurepip
=========

`Docs <https://docs.python.org/3/library/ensurepip.html>`__ |
`Issues <https://bugs.python.org/>`__

.. tab:: 中文

    Python 标准库中的一个包，提供了将 :ref:`pip` 引导到现有 Python 安装或虚拟环境中的支持。在大多数情况下，最终用户不会直接使用这个模块，而是在构建 Python 发行版时使用它。

.. tab:: 英文

    A package in the Python Standard Library that provides support for bootstrapping
    :ref:`pip` into an existing Python installation or virtual environment.  In most
    cases, end users won't use this module, but rather it will be used during the
    build of the Python distribution.

.. _httpserver:

http.server
===========

:doc:`Docs <python:library/http.server>` |
:gh:`Issues <python/cpython/issues>`

.. tab:: 中文

    一个包和命令行接口，可以将目录托管为网站，例如作为一个 :term:`package index <Package Index>` （参见 :ref:`Hosting your Own Simple Repository`）。

.. tab:: 英文    

    A package and command-line interface which can host a directory as a
    website, for example as a :term:`package index <Package Index>` (see
    :ref:`Hosting your Own Simple Repository`).

.. _venv:

venv
====

`Docs <https://docs.python.org/3/library/venv.html>`__ |
`Issues <https://github.com/python/cpython/issues>`__

.. tab:: 中文

    Python 标准库中的一个包（从 Python 3.3 开始），用于创建 :term:`Virtual Environments <Virtual Environment>`。有关更多信息，请参见 :ref:`创建和使用虚拟环境 <Creating and using Virtual Environments>` 部分。

.. tab:: 英文

    A package in the Python Standard Library (starting with Python 3.3) for creating :term:`Virtual Environments <Virtual Environment>`.  For more information, see the section on :ref:`Creating and using Virtual Environments`.


----

.. _Sphinx: https://www.sphinx-doc.org/en/master/
.. _pytest: https://docs.pytest.org/en/stable/
.. _`AWS S3`: https://aws.amazon.com/s3/
