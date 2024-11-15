========
术语
========

**Glossary**

.. glossary::


    二进制分发 
    Binary Distribution 

        .. tab:: 中文

            一种包含已编译扩展的特定类型的 :term:`Built Distribution` (构建分发)。

        .. tab:: 英文

            A specific kind of :term:`Built Distribution` that contains compiled extensions.

    构建后端
    Build Backend

        .. tab:: 中文

            一个库，它从源代码树构建 :term:`源分发 <Source Distribution (or "sdist")>` 或 :term:`构建分发 <Built Distribution>`。构建由 :term:`frontend <Build Frontend>` 委托给后端。所有后端提供标准化的接口。

            构建后端的示例包括：
            :ref:`flit 的 flit-core <flit>` ，
            :ref:`hatch 的 hatchling <hatch>` ，
            :ref:`maturin`，
            :ref:`meson-python`，
            :ref:`scikit-build-core`，
            以及 :ref:`setuptools`。

        .. tab:: 英文

            A library that takes a source tree
            and builds a :term:`source distribution <Source Distribution (or "sdist")>` or
            :term:`built distribution <Built Distribution>` from it.
            The build is delegated to the backend by a
            :term:`frontend <Build Frontend>`.
            All backends offer a standardized interface.

            Examples of build backends are
            :ref:`flit's flit-core <flit>`,
            :ref:`hatch's hatchling <hatch>`,
            :ref:`maturin`,
            :ref:`meson-python`,
            :ref:`scikit-build-core`,
            and :ref:`setuptools`.

    构建前端
    Build Frontend

        .. tab:: 中文

            一个用户可能运行的工具，它接受任意的源代码树或 :term:`源分发 <Source Distribution (or "sdist")>`，并从中构建源代码分发或 :term:`wheels <Wheel>`。实际的构建工作委托给每个源代码树的 :term:`构建后端 <Build Backend>`。

            构建前端的示例包括 :ref:`pip` 和 :ref:`build`。

        .. tab:: 英文

            A tool that users might run
            that takes arbitrary source trees or
            :term:`source distributions <Source Distribution (or "sdist")>`
            and builds source distributions or :term:`wheels <Wheel>` from them.
            The actual building is delegated to each source tree's
            :term:`build backend <Build Backend>`.

            Examples of build frontends are :ref:`pip` and :ref:`build`.


    构建分发
    Built Distribution

        .. tab:: 中文

            一种 :term:`分发 <Distribution Package>` 格式，包含文件和元数据，这些文件和元数据只需要被移动到目标系统上的正确位置即可安装。 :term:`Wheel` 就是这样一种格式，而 :term:`源分发 <Source Distribution (or "sdist")>` 则不是，因为它在安装之前需要一个构建步骤。该格式并不意味着 Python 文件必须是预编译的（ :term:`Wheel` 有意不包含已编译的 Python 文件）。有关更多信息，请参见 :ref:`package-formats`。

        .. tab:: 英文

            A :term:`Distribution <Distribution Package>` format containing files
            and metadata that only need to be moved to the correct location on the
            target system, to be installed. :term:`Wheel` is such a format, whereas
            :term:`Source Distribution <Source Distribution (or
            "sdist")>` is not, in that it requires a build step before it can be
            installed.  This format does not imply that Python files have to be
            precompiled (:term:`Wheel` intentionally does not include compiled
            Python files). See :ref:`package-formats` for more information.


    构建元数据
    Built Metadata

        .. tab:: 中文

            当包含在已安装的 :term:`Project` （ ``METADATA`` 文件）或 :term:`Distribution Archive` 中时， :term:`Core Metadata` 的具体形式（在 :term:`Sdist <Source Distribution (or "sdist")>` 中为 ``PKG-INFO`` 文件，在 :term:`Wheel` 中为 ``METADATA`` 文件）。

        .. tab:: 英文

            The concrete form :term:`Core Metadata` takes
            when included inside an installed :term:`Project` (``METADATA`` file)
            or a :term:`Distribution Archive`
            (``PKG-INFO`` in a
            :term:`Sdist <Source Distribution (or "sdist")>`
            and ``METADATA`` in a :term:`Wheel`).


    核心元数据
    Core Metadata

        .. tab:: 中文

            :ref:`specification <core-metadata>` 及其定义的一组 :term:`核心元数据字段 <Core Metadata Field>`，用于描述 :term:`分发包 <Distribution Package>` 或 :term:`已安装项目 <Installed Project>` 的关键静态属性。

        .. tab:: 英文

            The :ref:`specification <core-metadata>`
            and the set of :term:`Core Metadata Field`\s it defines
            that describe key static attributes of
            a :term:`Distribution Package` or :term:`Installed Project`.


    核心元数据字段
    Core Metadata Field

        .. tab:: 中文

            在 :term:`核心元数据 <Core Metadata>` 规范中定义的单个键值对（或者是具有相同名称的多个键值对，适用于多次使用的字段），并存储在 :term:`构建元数据 <Built Metadata>` 中。值得注意的是，它不同于 :term:`Pyproject Metadata Key`。

        .. tab:: 英文

            A single key-value pair
            (or sequence of such with the same name, for multiple-use fields)
            defined in the :term:`Core Metadata` spec
            and stored in the :term:`Built Metadata`.
            Notably, distinct from a :term:`Pyproject Metadata Key`.


    分发存档
    Distribution Archive

        .. tab:: 中文

            :term:`分发包 <Distribution Package>` 的物理分发工件（即磁盘上的文件）。

        .. tab:: 英文

            The physical distribution artifact (i.e. a file on disk) for a :term:`Distribution Package`.


    分发包
    Distribution Package

        .. tab:: 中文

            一个版本化的归档文件，其中包含用于分发 :term:`发布 <Release>` 的 Python :term:`包 <Import Package>`、:term:`模块 <Module>` 和其他资源文件。该归档文件是最终用户从互联网下载并安装的文件。

            分发包通常被简称为 “package” 或 “distribution” ，但本指南在需要更清晰区分时，会使用扩展术语，以防与 :term:`导入包 <Import Package>` （也常被称为“package”）或其他类型的分发（例如 Linux 发行版或 Python 语言发行版）混淆。后者通常也被简称为“distribution”。有关这些差异的详细说明，请参见 :ref:`distribution-package-vs-import-package`。

        .. tab:: 英文

            A versioned archive file that contains Python :term:`packages <Import
            Package>`, :term:`modules <Module>`, and other resource files that are
            used to distribute a :term:`Release`. The archive file is what an
            end-user will download from the internet and install.

            A distribution package is more commonly referred to with the single
            words "package" or "distribution", but this guide may use the expanded
            term when more clarity is needed to prevent confusion with an
            :term:`Import Package` (which is also commonly called a "package") or
            another kind of distribution (e.g. a Linux distribution or the Python
            language distribution), which are often referred to with the single term
            "distribution". See :ref:`distribution-package-vs-import-package`
            for a breakdown of the differences.

    Egg

        .. tab:: 中文

            由 :ref:`setuptools` 引入的 :term:`构建分发 <Built Distribution>` 格式，已被 :term:`Wheel` 取代。有关详细信息，请参见 :ref:`egg-format`。

        .. tab:: 英文

            A :term:`Built Distribution` format introduced by :ref:`setuptools`,
            which has been replaced by :term:`Wheel`.  For details, see
            :ref:`egg-format`.

    扩展模块
    Extension Module

        .. tab:: 中文

            一个用 Python 实现的底层语言编写的 :term:`Module` ：
            Python 使用 C/C++，Jython 使用 Java。通常包含在一个单独的动态加载的预编译文件中，例如：在 Unix 上用于 Python 扩展的共享对象文件（.so），在 Windows 上用于 Python 扩展的 DLL 文件（.pyd 扩展名），或用于 Jython 的 Java 类文件。

        .. tab:: 英文

            A :term:`Module` written in the low-level language of the Python implementation:
            C/C++ for Python, Java for Jython. Typically contained in a single
            dynamically loadable pre-compiled file, e.g.  a shared object (.so) file
            for Python extensions on Unix, a DLL (given the .pyd extension) for
            Python extensions on Windows, or a Java class file for Jython
        extensions.


    已知良好集 (KGS)
    Known Good Set (KGS)

        .. tab:: 中文

            一组在指定版本下彼此兼容的分发包。通常会运行一个测试套件，确保所有测试通过，然后才会声明一组特定的包为已知的良好集合。这个术语通常用于由多个单独分发包组成的框架和工具包。

        .. tab:: 英文

            A set of distributions at specified versions which are compatible with
            each other. Typically a test suite will be run which passes all tests
            before a specific set of packages is declared a known good set. This
            term is commonly used by frameworks and toolkits which are comprised of
            multiple individual distributions.


    导入包
    Import Package

        .. tab:: 中文

            一个 Python 模块，可以包含其他模块或递归地包含其他包。

            导入包通常简称为“包”，但本指南会在需要更多清晰度时使用扩展术语，以避免与 :term:`分发包` 混淆，后者也常被称为“包”。有关两者区别的详细解释，请参见 :ref:`分发包与导入包 <distribution-package-vs-import-package>`。

        .. tab:: 英文

            A Python module which can contain other modules or recursively, other
            packages.

            An import package is more commonly referred to with the single word
            "package", but this guide will use the expanded term when more clarity
            is needed to prevent confusion with a :term:`Distribution Package` which
            is also commonly called a "package". See :ref:`distribution-package-vs-import-package`
            for a breakdown of the differences.


    已安装项目
    Installed Project

        .. tab:: 中文

            一个 :term:`项目 <Project>`，已安装并用于 Python 解释器或 :term:`虚拟环境 <Virtual Environment>`，如 :ref:`记录已安装包 <recording-installed-packages>` 规范中所描述。

        .. tab:: 英文

            A :term:`Project` that is installed for use with
            a Python interpreter or :term:`Virtual Environment`,
            as described in the specicifcation :ref:`recording-installed-packages`.


    模块
    Module

        .. tab:: 中文

            Python 中代码可重用性的基本单元，存在于以下两种类型之一：:term:`纯模块 <Pure Module>` (Pure Module) 或扩展模块 (Extension Module)。

        .. tab:: 英文

            The basic unit of code reusability in Python, existing in one of two types: :term:`Pure Module`, or :term:`Extension Module`.


    包索引
    Package Index

        .. tab:: 中文

            带有 Web 界面的分发存储库，用于自动化 :term:`包 <Distribution Package>` 的发现和使用。

        .. tab:: 英文

            A repository of distributions with a web interface to automate :term:`package <Distribution Package>` discovery and consumption.


    每个项目索引
    Per Project Index

        .. tab:: 中文

            由特定:term:`项目 <Project>` 指示的私有或其他非规范的 :term:`包索引 <Package Index>` 作为解决该项目依赖关系的首选索引或所需索引。

        .. tab:: 英文

            A private or other non-canonical :term:`Package Index` indicated by a specific :term:`Project` as the index preferred or required to resolve dependencies of that project.


    项目
    Project

        .. tab:: 中文

            一个库、框架、脚本、插件、应用程序或数据集或其他资源的集合，或这些元素的某种组合，旨在打包为一个 :term:`分发包 <Distribution Package>`。

            由于大多数项目使用 :pep:`518` 的 ``build-system`` 、 :ref:`distutils` 或 :ref:`setuptools` 创建 :term:`分发包 <Distribution Package>`，因此另一种定义项目的实际方式是，它包含一个位于项目源代码目录根目录的 :term:`pyproject.toml`、:term:`setup.py` 或 :term:`setup.cfg` 文件。

            Python 项目必须具有唯一的名称，这些名称在 :term:`PyPI <Python Package Index (PyPI)>` 上注册。每个项目将包含一个或多个 :term:`发布 <Release>`，每个发布可能由一个或多个 :term:`分发包 <Distribution Package>` 组成。

            请注意，有一个强烈的约定，即项目的名称与用于运行该项目的包的导入名称相同。然而，这个规则并非绝对成立。你可以从名为 'foo' 的项目安装分发包，并让它提供一个只能以 'bar' 作为包名导入的包。

        .. tab:: 英文

            A library, framework, script, plugin, application, or collection of data
            or other resources, or some combination thereof that is intended to be
            packaged into a :term:`Distribution <Distribution Package>`.

            Since most projects create :term:`Distributions <Distribution Package>`
            using either :pep:`518` ``build-system``, :ref:`distutils` or
            :ref:`setuptools`, another practical way to define projects currently
            is something that contains a :term:`pyproject.toml`, :term:`setup.py`,
            or :term:`setup.cfg` file at the root of the project source directory.

            Python projects must have unique names, which are registered on
            :term:`PyPI <Python Package Index (PyPI)>`. Each project will then
            contain one or more :term:`Releases <Release>`, and each release may
            comprise one or more :term:`distributions <Distribution Package>`.

            Note that there is a strong convention to name a project after the name
            of the package that is imported to run that project. However, this
            doesn't have to hold true. It's possible to install a distribution from
            the project 'foo' and have it provide a package importable only as
            'bar'.


    项目根目录
    Project Root Directory

        .. tab:: 中文

            :term:`Project` 的 :term:`源树 <Project Source Tree>` 所在的文件系统目录。

        .. tab:: 英文

            The filesystem directory in which a :term:`Project`'s :term:`source tree <Project Source Tree>` is located.


    项目来源树
    Project Source Tree

        .. tab:: 中文

            用于开发的 :term:`项目 <Project>` 的磁盘格式，包含其原始源代码，在被打包为 :term:`源分发包 <Source Distribution (or "sdist")>` 或 :term:`构建分发包` 之前。

        .. tab:: 英文

            The on-disk format of a :term:`Project` used for development,
            containing its raw source code before being packaged
            into a
            :term:`Source Distribution <Source Distribution (or "sdist")>`
            or :term:`Built Distribution`.


    项目源元数据
    Project Source Metadata

        .. tab:: 中文

            由包作者在 :term:`项目 <Project>` 的 :term:`源代码树 <Project Source Tree>` 中定义的元数据，旨在通过项目的 :term:`构建后端 <Build Backend>` 转换为 :term:`核心元数据字段 <Core Metadata field>`，并包含在 :term:`构建元数据 <Built Metadata>` 中。

            这些元数据可以写入为 :term:`Pyproject 元数据 <Pyproject Metadata>`，或使用工具特定的格式（在 ``pyproject.toml`` 中的 ``[tool]`` 表格下，或在工具的配置文件中）。

        .. tab:: 英文

            Metadata defined by the package author
            in a :term:`Project`'s :term:`source tree <Project Source Tree>`,
            to be transformed into :term:`Core Metadata field`\s
            in the :term:`Built Metadata`
            by the project's :term:`build backend <Build Backend>`.
            Can be written as :term:`Pyproject Metadata`,
            or in a tool-specific format
            (under the ``[tool]`` table in ``pyproject.toml``,
            or in a tool's own configuration file).


    纯模块
    Pure Module

        .. tab:: 中文

            用 Python 编写的 :term:`模块 <Module>` 包含在单个 ``.py`` 文件中（可能还有相关的 ``.pyc`` 和/或 ``.pyo`` 文件）。

        .. tab:: 英文

            A :term:`Module` written in Python and contained in a single ``.py`` file (and possibly associated ``.pyc`` and/or ``.pyo`` files).


    Pyproject 元数据
    Pyproject Metadata

        .. tab:: 中文

            由 :ref:`declaring-project-metadata` 规范定义的 :term:`项目源元数据 <Project Source Metadata>` 格式，最初在 :pep:`621` 中引入，存储为 :term:`Pyproject 元数据键 <Pyproject Metadata Key>`，位于 :term:`pyproject.toml` 文件的 ``[project]`` 表格下。

            值得注意的是，这 *不是* 在 ``pyproject.toml`` 文件的 ``[tool]`` 表格下的工具特定源元数据格式。

        .. tab:: 英文

            The :term:`Project Source Metadata` format
            defined by the :ref:`declaring-project-metadata` specification
            and originally introduced in :pep:`621`,
            stored as :term:`Pyproject Metadata Key`\s
            under the ``[project]`` table of a :term:`pyproject.toml` file.
            Notably, *not* a tool-specific source metadata format
            under the ``[tool]`` table in ``pyproject.toml``.


    Pyproject 元数据键
    Pyproject Metadata Key

        .. tab:: 中文

            ``pyproject.toml`` 文件中 ``[project]`` 表格的顶级 TOML 键；是 :term:`Pyproject 元数据 <Pyproject Metadata>` 的一部分。值得注意的是，它不同于 :term:`核心元数据字段 <Core Metadata Field>`。

        .. tab:: 英文

            A top-level TOML key in the ``[project]`` table in ``pyproject.toml``; part of the :term:`Pyproject Metadata`. Notably, distinct from a :term:`Core Metadata Field`.


    Pyproject 元数据子键
    Pyproject Metadata Subkey

        .. tab:: 中文

            在一个表值型 :term:`Pyproject 元数据键 <Pyproject Metadata Key>` 下的第二级 TOML 键。

        .. tab:: 英文

            A second-level TOML key under a table-valued :term:`Pyproject Metadata Key`.


    Python 打包权威机构 (PyPA)
    Python Packaging Authority (PyPA)

        .. tab:: 中文

            PyPA 是一个工作组，负责维护许多与 Python 打包相关的项目。他们维护一个网站：:doc:`pypa.io <pypa:index>`，并在 `GitHub <https://github.com/pypa>`_ 和 `Bitbucket <https://bitbucket.org/pypa>`_ 上托管项目，讨论问题的邮件列表是 `distutils-sig mailing list <https://mail.python.org/mailman3/lists/distutils-sig.python.org/>`_，此外还在 `Python Discourse 论坛 <https://discuss.python.org/c/packaging>`_ 上讨论。

        .. tab:: 英文

            PyPA is a working group that maintains many of the relevant
            projects in Python packaging. They maintain a site at
            :doc:`pypa.io <pypa:index>`, host projects on `GitHub
            <https://github.com/pypa>`_ and `Bitbucket
            <https://bitbucket.org/pypa>`_, and discuss issues on the
            `distutils-sig mailing list
            <https://mail.python.org/mailman3/lists/distutils-sig.python.org/>`_
            and `the Python Discourse forum <https://discuss.python.org/c/packaging>`__.


    Python 软件包索引 (PyPI)
    Python Package Index (PyPI)

        .. tab:: 中文

            `PyPI <https://pypi.org>`_ 是 Python 社区的默认 :term:`包索引 <Package Index>`。它对所有 Python 开发者开放，用于消费和分发他们的发行版。

        .. tab:: 英文

            `PyPI <https://pypi.org>`_ is the default :term:`Package
            Index` for the Python community. It is open to all Python developers to
            consume and distribute their distributions.

    pypi.org

        .. tab:: 中文

            `pypi.org <https://pypi.org>`_ 是 :term:`Python 包索引 (PyPI) <Python Package Index (PyPI)>` 的域名。它于 2017 年取代了旧的索引域名 ``pypi.python.org``。该站点由 :ref:`warehouse` 提供支持。

        .. tab:: 英文

            `pypi.org <https://pypi.org>`_ is the domain name for the
            :term:`Python Package Index (PyPI)`. It replaced the legacy index
            domain name, ``pypi.python.org``, in 2017. It is powered by
            :ref:`warehouse`.

    pyproject.toml

        .. tab:: 中文

            工具无关的 :term:`项目 <Project>` 规范文件，定义于 :pep:`518` 中。

        .. tab:: 英文

            The tool-agnostic :term:`Project` specification file. Defined in :pep:`518`.

    发布
    Release

        .. tab:: 中文

            在特定时间点的 :term:`项目 <Project>` 快照，通过版本标识符表示。

            发布一个版本可能涉及多个 :term:`发行包 <Distribution Package>` 的发布。例如，如果某个项目的 1.0 版本被发布，它可能同时以源代码发行包格式和 Windows 安装程序发行包格式提供。

        .. tab:: 英文

            A snapshot of a :term:`Project` at a particular point in time, denoted
            by a version identifier.

            Making a release may entail the publishing of multiple
            :term:`Distributions <Distribution Package>`.  For example, if version
            1.0 of a project was released, it could be available in both a source
            distribution format and a Windows installer distribution format.


    需求
    Requirement

        .. tab:: 中文

            用于安装的 :term:`包 <Distribution Package>` 规范。 :ref:`pip`，即 :term:`PYPA <Python Packaging Authority (PyPA)>` 推荐的安装工具，支持多种形式的规范，这些都可以被视为“需求”。更多信息，请参见 :ref:`pip:pip install` 参考文档。

        .. tab:: 英文

            A specification for a :term:`package <Distribution Package>` to be
            installed.  :ref:`pip`, the :term:`PYPA <Python Packaging Authority
            (PyPA)>` recommended installer, allows various forms of specification
            that can all be considered a "requirement". For more information, see the
            :ref:`pip:pip install` reference.


    需求说明
    Requirement Specifier

        .. tab:: 中文

            一种由 :ref:`pip` 用于从 :term:`Package Index` 安装包的格式。有关该格式的 EBNF 图示，请参见 :ref:`dependency-specifiers`。例如，"foo>=1.3" 是一个需求规范符，其中 "foo" 是项目名称，">=1.3" 部分是 :term:`Version Specifier` （版本规范符）。

        .. tab:: 英文

            A format used by :ref:`pip` to install packages from a :term:`Package Index`. For an EBNF diagram of the format, see :ref:`dependency-specifiers`. For example, "foo>=1.3" is a requirement specifier, where "foo" is the project name, and the ">=1.3" portion is the :term:`Version Specifier`

    需求文件
    Requirements File

        .. tab:: 中文

            一个包含可以使用 :ref:`pip` 安装的 :term:`Requirements <Requirement>` 列表的文件。有关更多信息，请参阅 :ref:`pip` 文档中的 :ref:`pip:Requirements Files` 部分。

        .. tab:: 英文

            A file containing a list of :term:`Requirements <Requirement>` that can
            be installed using :ref:`pip`. For more information, see the :ref:`pip`
            docs on :ref:`pip:Requirements Files`.


    setup.py
    setup.cfg

        .. tab:: 中文

            这是 :ref:`distutils` 和 :ref:`setuptools` 的项目规范文件。另请参见 :term:`pyproject.toml`。

        .. tab:: 英文

            The project specification files for :ref:`distutils` and :ref:`setuptools`. See also :term:`pyproject.toml`.

    源存档
    Source Archive

        .. tab:: 中文

            一个包含 :term:`发布 <Release>` 原始源代码的档案，在创建 :term:`源分发包 <Source Distribution (or "sdist")>` 或 :term:`构建分发包 <Built Distribution>` 之前。

        .. tab:: 英文

            An archive containing the raw source code for a :term:`Release`, prior
            to creation of a :term:`Source Distribution <Source Distribution (or
            "sdist")>` or :term:`Built Distribution`.

    源分发
    Source Distribution (or "sdist")

        .. tab:: 中文

            一种 :term:`分发包 <Distribution Archive>` 格式（通常通过 ``python -m build --sdist`` 生成），提供元数据和安装所需的基本源文件，可以被像 :ref:`pip` 这样的工具用于安装，或用于生成 :term:`构建分发包 <Built Distribution>`。有关更多信息，请参见 :ref:`package-formats`。

        .. tab:: 英文

            A :term:`distribution <Distribution Archive>` format (usually generated
            using ``python -m build --sdist``) that provides metadata and the
            essential source files needed for installing by a tool like :ref:`pip`,
            or for generating a :term:`Built Distribution`. See :ref:`package-formats`
            for more information.


    系统包
    System Package

        .. tab:: 中文

            以操作系统原生格式提供的包，例如 rpm 或 dpkg 文件。

        .. tab:: 英文

            A package provided in a format native to the operating system, e.g. an rpm or dpkg file.

    版本说明
    Version Specifier

        .. tab:: 中文

            版本组件，属于 :term:`Requirement Specifier` 的一部分。例如，"foo>=1.3" 中的 ">=1.3" 部分。有关当前 Python 包管理支持的版本说明符的完整描述，请阅读 :ref:`Version specifier specification <version-specifiers>`。此规范的支持已在 :ref:`setuptools` v8.0 和 :ref:`pip` v6.0 中实现。

        .. tab:: 英文

            The version component of a :term:`Requirement Specifier`. For example, the ">=1.3" portion of "foo>=1.3".  Read the :ref:`Version specifier specification <version-specifiers>` for a full description of the specifiers that Python packaging currently supports.  Support for this specification was implemented in :ref:`setuptools` v8.0 and :ref:`pip` v6.0.

    虚拟环境
    Virtual Environment

        .. tab:: 中文

            一个隔离的 Python 环境，允许为特定应用程序安装包，而不是将其系统范围内安装。有关更多信息，请参见 :ref:`创建和使用虚拟环境 <Creating and using Virtual Environments>` 部分。

        .. tab:: 英文

            An isolated Python environment that allows packages to be installed for use by a particular application, rather than being installed system wide. For more information, see the section on :ref:`Creating and using Virtual Environments`.


    Wheel Format
    Wheel

        .. tab:: 中文

            最初在 :pep:`427` 中引入并由 :ref:`binary-distribution-format` 规范定义的标准 :term:`Built Distribution` 格式。有关更多信息，请参见 :ref:`package-formats`。不要与其参考实现 :term:`Wheel Project` 混淆。

        .. tab:: 英文

            The standard :term:`Built Distribution` format originally introduced in :pep:`427` and defined by the :ref:`binary-distribution-format` specification. See :ref:`package-formats` for more information. Not to be confused with its reference implementation, the :term:`Wheel Project`.

    Wheel 项目
    Wheel Project

        .. tab:: 中文

            PyPA 对 :term:`Wheel Format` 的参考实现；参见 :ref:`wheel`。

        .. tab:: 英文

            The PyPA reference implementation of the :term:`Wheel Format`; see :ref:`wheel`.

    工作集
    Working Set

        .. tab:: 中文

            一组可供导入的 :term:`distributions <Distribution Package>`。这些是位于 `sys.path` 变量中的分发包。在一个工作集（working set）中，最多只能有一个项目的 :term:`Distribution <Distribution Package>`。

        .. tab:: 英文

            A collection of :term:`distributions <Distribution Package>` available for importing. These are the distributions that are on the `sys.path` variable. At most, one :term:`Distribution <Distribution Package>` for a project is possible in a working set.
