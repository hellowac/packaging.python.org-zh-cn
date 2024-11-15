.. _`NumPy and the Science Stack`:

==============================
安装科学软件包
==============================

**Installing scientific packages**

.. tab:: 中文

    科学软件通常比大多数软件有更复杂的依赖关系，并且通常会有多个构建选项，以便利用不同类型的硬件，或与不同的外部软件进行互操作。

    特别是， `NumPy <https://numpy.org/>`__ ，作为大多数 `科学 Python 堆栈 <https://scientific-python.org>`_ 软件的基础，可以配置为与不同的 FORTRAN 库互操作，并且能够利用现代 CPU 中提供的不同级别的向量化指令。

    从 NumPy 版本 1.10.4 和 SciPy 版本 1.0.0 开始，PyPI 上提供了适用于所有主要操作系统（Windows、macOS 和 Linux）的预构建 32 位和 64 位二进制文件，格式为 ``wheel`` 。但请注意，在 Windows 上，NumPy 二进制文件是链接到 `ATLAS <http://www.netlib.org/atlas/>`__ BLAS/LAPACK 库的，并且仅限于 SSE2 指令，因此它们可能无法提供最佳的线性代数性能。

    对于获取科学 Python 库（或任何其他需要编译环境才能从源代码安装且未提供预构建 wheel 文件的 Python 库），有许多替代选项。

.. tab:: 英文


    Scientific software tends to have more complex dependencies than most, and
    it will often have multiple build options to take advantage of different
    kinds of hardware, or to interoperate with different pieces of external
    software.

    In particular, `NumPy <https://numpy.org/>`__, which provides the basis
    for most of the software in the `scientific Python stack
    <https://scientific-python.org>`_ can be configured
    to interoperate with different FORTRAN libraries, and can take advantage
    of different levels of vectorised instructions available in modern CPUs.

    Starting with version 1.10.4 of NumPy and version 1.0.0 of SciPy, pre-built
    32-bit and 64-bit binaries in the ``wheel`` format are available for all major
    operating systems (Windows, macOS, and Linux) on PyPI. Note, however, that on
    Windows, NumPy binaries are linked against the `ATLAS
    <http://www.netlib.org/atlas/>`__ BLAS/LAPACK library, restricted to SSE2
    instructions, so they may not provide optimal linear algebra performance.

    There are a number of alternative options for obtaining scientific Python
    libraries (or any other Python libraries that require a compilation environment
    to install from source and don't provide pre-built wheel files on PyPI).


从源代码构建
--------------------

**Building from source**

.. tab:: 中文

    正是这种复杂性使得将 NumPy（以及许多依赖于它的项目）作为 wheel 文件分发变得困难，同时也使得它们从源代码构建变得更加复杂。然而，对于那些愿意花时间处理 C 和 FORTRAN 编译器及链接器的勇敢者来说，从源代码构建始终是一个可选的方案。

.. tab:: 英文

    The same complexity which makes it difficult to distribute NumPy (and many
    of the projects that depend on it) as wheel files also make them difficult
    to build from source yourself. However, for intrepid folks that are willing
    to spend the time wrangling compilers and linkers for both C and FORTRAN,
    building from source is always an option.


Linux 发行版软件包
---------------------------

**Linux distribution packages**

.. tab:: 中文

    对于 Linux 用户，系统包管理器通常会提供一些科学软件的预编译版本，包括 NumPy 以及科学 Python 堆栈的其他部分。

    如果使用可能有几个月历史的旧版本是可以接受的，那么这是一个不错的选择（只需确保在使用虚拟环境时允许访问安装在系统 Python 中的分发包）。

.. tab:: 英文

    For Linux users, the system package manager will often have pre-compiled
    versions of various pieces of scientific software, including NumPy and
    other parts of the scientific Python stack.

    If using versions which may be several months old is acceptable, then this is
    likely to be a good option (just make sure to allow access to distributions
    installed into the system Python when using virtual environments).


Windows 安装程序
------------------

**Windows installers**

.. tab:: 中文

    许多目前无法（或不愿）发布 wheel 文件的 Python 项目，至少会发布 Windows 安装程序，这些安装程序可以在 PyPI 或项目的下载页面上找到。使用这些安装程序可以让用户避免需要设置适合的环境来本地构建扩展。

    这些安装程序提供的扩展通常与 python.org 上发布的 CPython Windows 安装程序兼容。

    与 Linux 系统包一样，Windows 安装程序只能安装到系统 Python 安装中——它们不支持在虚拟环境中安装。使用虚拟环境时，允许访问安装在系统 Python 中的分发包是一种常见的解决方法。

    :term:`Wheel` 项目还提供了一个 :command:`wheel convert` 子命令，可以将 Windows 的 :command:`bdist_wininst` 安装程序转换为 wheel 文件。    

.. tab:: 英文

    Many Python projects that don't (or can't) currently publish wheel files at
    least publish Windows installers, either on PyPI or on their project
    download page. Using these installers allows users to avoid the need to set
    up a suitable environment to build extensions locally.

    The extensions provided in these installers are typically compatible with
    the CPython Windows installers published on python.org.

    As with Linux system packages, the Windows installers will only install into a
    system Python installation - they do not support installation in virtual
    environments. Allowing access to distributions installed into the system Python
    when using virtual environments is a common approach to working around this
    limitation.

    The :term:`Wheel` project also provides a :command:`wheel convert` subcommand that can
    convert a Windows :command:`bdist_wininst` installer to a wheel.

.. preserve old links to this heading
.. _mac-os-x-installers-and-package-managers:

macOS 安装程序和软件包管理器
-------------------------------------

**macOS installers and package managers**

.. tab:: 中文

    与 Windows 上的情况类似，许多项目（包括 NumPy）发布与 python.org 上发布的 macOS CPython 二进制文件兼容的 macOS 安装程序。

    macOS 用户还可以使用类似于 Linux 发行版的包管理器，如 **Homebrew** 。有关如何使用 Homebrew 安装 SciPy 的更多信息，请参见 SciPy 网站上的 `在 macOS 上安装 SciPy <https://scipy.org/install/#macos>`_ 章节。

.. tab:: 英文

    Similar to the situation on Windows, many projects (including NumPy) publish
    macOS installers that are compatible with the macOS CPython binaries
    published on python.org.

    macOS users also have access to Linux distribution style package managers
    such as ``Homebrew``. The SciPy site has more details on using Homebrew to
    `install SciPy on macOS <https://scipy.org/install/#macos>`_.


SciPy 发行版
-------------------

**SciPy distributions**

.. tab:: 中文

    SciPy 网站列出了 `几种发行版 <https://scipy.org/install/#distributions>`_ ，这些发行版以易于使用和更新的格式向最终用户提供完整的 SciPy 堆栈。

    其中一些发行版可能与标准的 ``pip`` 和 ``virtualenv`` 工具链不兼容。

.. tab:: 英文

    The SciPy site lists `several distributions
    <https://scipy.org/install/#distributions>`_
    that provide the full SciPy stack to
    end users in an easy to use and update format.

    Some of these distributions may not be compatible with the standard ``pip``
    and ``virtualenv`` based toolchain.

Spack
------

**Spack**

.. tab:: 中文

    `Spack <https://github.com/spack/spack>`_ 是一个灵活的包管理器，旨在支持多版本、多配置、多平台和多编译器。它是为了满足大型超级计算中心和科学应用团队的需求而构建的，这些团队经常需要以多种不同的方式构建软件。Spack 不仅限于 Python，它还可以安装 ``C``、 ``C++``、 ``Fortran``、 ``R`` 和其他语言的包。Spack 是非破坏性的；安装一个新版本的包不会破坏现有的安装，因此多个配置可以在同一系统上共存。

    Spack 提供了一个简单但强大的语法，允许用户简洁地指定版本和配置选项。包文件使用纯 Python 编写，并且是模板化的，这样可以通过一个包文件轻松地更换编译器、依赖项实现（如 MPI）、版本和构建选项。Spack 还生成 *module* 文件，以便包可以从用户环境中加载和卸载。

.. tab:: 英文

    `Spack <https://github.com/spack/spack>`_ is a flexible package manager
    designed to support multiple versions, configurations, platforms, and compilers.
    It was built to support the needs of large supercomputing centers and scientific
    application teams, who must often build software many different ways.
    Spack is not limited to Python; it can install packages for ``C``, ``C++``,
    ``Fortran``, ``R``, and other languages.  It is non-destructive; installing
    a new version of one package does not break existing installations, so many
    configurations can coexist on the same system.

    Spack offers a simple but powerful syntax that allows users to specify
    versions and configuration options concisely. Package files are written in
    pure Python, and they are templated so that it is easy to swap compilers,
    dependency implementations (like MPI), versions, and build options with a single
    package file.  Spack also generates *module* files so that packages can
    be loaded and unloaded from the user's environment.


conda 跨平台软件包管理器
----------------------------------------

**The conda cross-platform package manager**

.. tab:: 中文

    `Anaconda <https://www.anaconda.com/products/individual/>`_ 是由 Anaconda 公司发布的 Python 发行版。它是一个稳定的开源软件包集合，主要用于大数据和科学计算。自 Anaconda 5.0 版本发布以来，约有 200 个包被默认安装，此外，Anaconda 仓库中总共有 400 到 500 个包可以安装和更新。

    ``conda`` 是一个开源（BSD 许可）的包管理系统和环境管理系统，包含在 Anaconda 中，允许用户安装多个版本的二进制软件包及其依赖项，并轻松在它们之间切换。它是一个跨平台工具，支持 Windows、macOS 和 Linux。Conda 不仅限于管理 Python 包，还可以用于打包和分发各种软件包。它完全支持原生的虚拟环境。Conda 将环境作为一等公民，使得创建独立的环境（即使是 C 库）也变得简单。它是用 Python 编写的，但与 Python 无关。Conda 将 Python 本身作为一个包进行管理，因此可以使用 :command:`conda update python` 命令进行更新，这与只能管理 Python 包的 pip 不同。Conda 可通过 Anaconda 和 Miniconda（一个简易安装包，仅包含 Python 和 conda）进行获取。

.. tab:: 英文

    `Anaconda <https://www.anaconda.com/products/individual/>`_ is a Python distribution
    published by Anaconda, Inc. It is a stable collection of Open Source
    packages for big data and scientific use.  As of the 5.0 release of Anaconda,
    about 200 packages are installed by default, and a total of 400-500 can be
    installed and updated from the Anaconda repository.

    ``conda`` is an open source (BSD licensed) package management system and
    environment management system included in Anaconda that allows users to install
    multiple versions of binary software packages and their dependencies, and
    easily switch between them. It is a cross-platform tool working on Windows,
    macOS, and Linux. Conda can be used to package up and distribute all kinds of
    packages, it is not limited to just Python packages. It has full support for
    native virtual environments. Conda makes environments first-class citizens,
    making it easy to create independent environments even for C libraries. It is
    written in Python, but is Python-agnostic. Conda manages Python itself as a
    package, so that :command:`conda update python` is possible, in contrast to
    pip, which only manages Python packages. Conda is available in Anaconda and
    Miniconda (an easy-to-install download with just Python and conda).
