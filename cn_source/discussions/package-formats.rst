.. _package-formats:

===============
软件包格式
===============

**Package Formats**

.. tab:: 中文

  本页讨论了用于分发 Python 包的文件格式及其之间的区别。

  在像 PyPI_ 这样的包索引中，您会看到两种格式的文件： **源代码分发** （简称 **sdist**）和 **二进制分发**，通常称为 **wheel**。例如， `pip 23.3.1 的 PyPI 页面 <pip-pypi_>`_ 允许您下载两个文件， ``pip-23.3.1.tar.gz`` 和 ``pip-23.3.1-py3-none-any.whl``。前者是源代码分发文件（sdist），后者是 wheel 文件。如下面所述，这两者有不同的用途。当在 PyPI（或其他地方）发布包时，您应始终上传一个源代码分发文件和一个或多个 wheel 文件。

.. tab:: 英文

  This page discusses the file formats that are used to distribute Python packages
  and the differences between them.

  You will find files in two formats on package indices such as PyPI_: **source
  distributions**, or **sdists** for short, and **binary distributions**, commonly
  called **wheels**.  For example, the `PyPI page for pip 23.3.1 <pip-pypi_>`_
  lets you download two files, ``pip-23.3.1.tar.gz`` and
  ``pip-23.3.1-py3-none-any.whl``.  The former is an sdist, the latter is a
  wheel. As explained below, these serve different purposes. When publishing a
  package on PyPI (or elsewhere), you should always upload both an sdist and one
  or more wheel.


什么是源发行版？
==============================

**What is a source distribution?**

.. tab:: 中文

  从概念上讲，源代码分发（sdist）是源代码的原始归档文件。具体来说，sdist 是一个 ``.tar.gz`` 归档文件，包含源代码以及一个名为 ``PKG-INFO`` 的特殊文件，该文件包含项目元数据。此文件的存在帮助打包工具提高效率，因为它不需要自行计算元数据。 ``PKG-INFO`` 文件遵循 :ref:`core-metadata` 中指定的格式，通常不应手动编写 [#core-metadata-format1]_ 。

  因此，您可以通过使用标准的解压缩工具来检查 sdist 的内容，例如在 UNIX 平台（如 Linux 和 macOS）上使用 ``tar -xvf``，或在任何平台上使用 :ref:`Python tarfile 模块的命令行界面 <python:tarfile-commandline>`。

  源代码分发在打包生态系统中有多个用途。当标准的 Python 包管理器 ``pip`` 找不到可以安装的 wheel 时，它会回退到下载源代码分发文件，从中编译 wheel 文件并安装该 wheel。此外，源代码分发常被下游打包工具（如 Linux 发行版、Conda、macOS 上的 Homebrew 和 MacPorts 等）作为包源使用，原因各异，这些工具可能会更倾向于使用源代码分发而非从 Git 仓库拉取。

  源代码分发通过文件名进行识别，其格式为： :samp:`{package_name}-{version}.tar.gz`，例如， ``pip-23.3.1.tar.gz``。

  .. TODO: 提供明确的指导，说明源代码分发是否应包含文档和测试。
    讨论：https://discuss.python.org/t/should-sdists-include-docs-and-tests/14578

  如果您想了解有关 sdist 格式的技术细节，请阅读 :ref:`sdist 规范 <source-distribution-format>`。

.. tab:: 英文

  Conceptually, a source distribution is an archive of the source code in raw
  form. Concretely, an sdist is a ``.tar.gz`` archive containing the source code
  plus an additional special file called ``PKG-INFO``, which holds the project
  metadata. The presence of this file helps packaging tools to be more efficient
  by not needing to compute the metadata themselves. The ``PKG-INFO`` file follows
  the format specified in :ref:`core-metadata` and is not intended to be written
  by hand [#core-metadata-format]_.

  You can thus inspect the contents of an sdist by unpacking it using standard
  tools to work with tar archives, such as ``tar -xvf`` on UNIX platforms (like
  Linux and macOS), or :ref:`the command line interface of Python's tarfile module
  <python:tarfile-commandline>` on any platform.

  Sdists serve several purposes in the packaging ecosystem. When :ref:`pip`, the
  standard Python package installer, cannot find a wheel to install, it will fall
  back on downloading a source distribution, compiling a wheel from it, and
  installing the wheel. Furthermore, sdists are often used as the package source
  by downstream packagers (such as Linux distributions, Conda, Homebrew and
  MacPorts on macOS, ...), who, for various reasons, may prefer them over, e.g.,
  pulling from a Git repository.

  A source distribution is recognized by its file name, which has the form
  :samp:`{package_name}-{version}.tar.gz`, e.g., ``pip-23.3.1.tar.gz``.

  .. TODO: provide clear guidance on whether sdists should contain docs and tests.
    Discussion: https://discuss.python.org/t/should-sdists-include-docs-and-tests/14578

  If you want technical details on the sdist format, read the :ref:`sdist
  specification <source-distribution-format>`.


什么是wheel？
================

**What is a wheel?**

.. tab:: 中文

  从概念上讲，wheel 包含安装包时需要复制的所有文件。

  对于带有 **扩展模块** （例如用 C、C++ 或 Rust 等编译语言编写的模块）的包，sdist 和 wheel 之间有很大的区别。这些模块需要编译成平台相关的机器码。对于这些包，wheel 不包含源代码（如 C 源文件），而是包含已编译的可执行代码（如 Linux 上的 ``.so`` 文件或 Windows 上的 DLL）。

  此外，虽然每个版本的项目只有一个源代码分发（sdist），但可能有多个 wheel。这一点在扩展模块的上下文中尤为重要。扩展模块的编译代码与操作系统和处理器架构紧密绑定，并且通常还与 Python 解释器的版本相关（除非使用 :ref:`Python 稳定 ABI <cpython-stable-abi>`）。

  对于纯 Python 包，sdist 和 wheel 之间的区别较小。通常情况下，所有平台和 Python 版本都有一个单独的 wheel。Python 是一种解释型语言，不需要预先编译，因此 wheel 包也像 sdist 一样包含 ``.py`` 文件。

  如果你想了解 ``.pyc`` 字节码文件：它们不会包含在 wheel 中，因为它们生成起来很简单，包含它们会迫使大量包为每个 Python 版本分发一个 wheel，而不是一个统一的 wheel。相反，像 :ref:`pip` 这样的安装程序会在安装包时生成它们。

  尽管如此，sdist 和 wheel 之间仍然存在重要区别，即使是纯 Python 项目也是如此。wheel 的目标是仅包含需要安装的内容，其他内容一概不包含。特别地，wheel 应该永远不包含测试和文档，而 sdist 通常会包含这些内容。此外，wheel 格式比 sdist 更复杂。例如，它包含一个特殊文件 —— ``RECORD`` —— 该文件列出了 wheel 中的所有文件以及它们内容的哈希值，用于检查下载的完整性。

  乍一看，你可能会怀疑 "纯粹的基础" 纯 Python 项目是否真的需要 wheel。请记住，由于 sdist 的灵活性，像 pip 这样的安装程序不能直接从 sdist 安装 —— 它们需要先通过调用 sdist 指定的 :term:`构建后端` 来构建一个 wheel（构建后端在构建 wheel 时可以进行各种转换，比如编译 C 扩展）。因此，即使是纯 Python 项目，你也应该总是将 *源代码分发* 和 *wheel* 都上传到 PyPI 或其他包索引。这将使得用户的安装速度更快，因为 wheel 是可以直接安装的。通过只包含必须安装的文件，wheel 还使得下载更小。

  在技术层面上，wheel 是一个 ZIP 归档（与 sdist 使用的 TAR 归档不同）。你可以通过将其解压缩为普通的 ZIP 归档来检查其内容，例如，在 UNIX 平台（如 Linux 和 macOS）上使用 ``unzip``, 在 Windows 上使用 Powershell 的 ``Expand-Archive``，或者使用 :ref:`Python zipfile 模块的命令行界面 <python:zipfile-commandline>`。这对于检查 wheel 是否包含所有你需要的文件非常有用。

  在 wheel 中，你会找到包的文件，以及一个名为 :samp:`{package_name}-{version}.dist-info` 的额外目录。该目录包含多个文件，其中包括与 sdist 中的 ``PKG-INFO`` 相同的 ``METADATA`` 文件，以及 ``RECORD`` 文件。这可以帮助确保你的 wheel 中没有缺少文件。

  wheel 的文件名（忽略一些不常用的特性）大致如下：`:samp:{package_name}-{version}-{python_tag}-{abi_tag}-{platform_tag}.whl`。这个命名规范标识了 wheel 兼容的平台和 Python 版本。例如，``pip-23.3.1-py3-none-any.whl`` 表示：

  - (``py3``) 该 wheel 可以在任何 Python 3 的实现中安装，无论是 CPython（最广泛使用的 Python 实现）还是类似 PyPy_ 的替代实现；
  - (``none``) 它不依赖于 Python 版本；
  - (``any``) 它不依赖于平台。

  ``py3-none-any`` 这种模式通常用于纯 Python 项目。带有扩展模块的包通常会提供多个带有更复杂标签的 wheel。

  关于 wheel 格式的所有技术细节，可以参考 :ref:`wheel 规范 <binary-distribution-format>`。

.. tab:: 英文

  Conceptually, a wheel contains exactly the files that need to be copied when
  installing the package.

  There is a big difference between sdists and wheels for packages with
  :term:`extension modules <extension module>`, written in compiled languages like
  C, C++ and Rust, which need to be compiled into platform-dependent machine code.
  With these packages, wheels do not contain source code (like C source files) but
  compiled, executable code (like ``.so`` files on Linux or DLLs on Windows).

  Furthermore, while there is only one sdist per version of a project, there may
  be many wheels. Again, this is most relevant in the context of extension
  modules. The compiled code of an extension module is tied to an operating system
  and processor architecture, and often also to the version of the Python
  interpreter (unless the :ref:`Python stable ABI <cpython-stable-abi>` is used).

  For pure-Python packages, the difference between sdists and wheels is less
  marked. There is normally one single wheel, for all platforms and Python
  versions.  Python is an interpreted language, which does not need ahead-of-time
  compilation, so wheels contain ``.py`` files just like sdists.

  If you are wondering about ``.pyc`` bytecode files: they are not included in
  wheels, since they are cheap to generate, and including them would unnecessarily
  force a huge number of packages to distribute one wheel per Python version
  instead of one single wheel. Instead, installers like :ref:`pip` generate them
  while installing the package.

  With that being said, there are still important differences between sdists and
  wheels, even for pure Python projects. Wheels are meant to contain exactly what
  is to be installed, and nothing more. In particular, wheels should never include
  tests and documentation, while sdists commonly do.  Also, the wheel format is
  more complex than sdist. For example, it includes a special file -- called
  ``RECORD`` -- that lists all files in the wheel along with a hash of their
  content, as a safety check of the download's integrity.

  At a glance, you might wonder if wheels are really needed for "plain and basic"
  pure Python projects. Keep in mind that due to the flexibility of sdists,
  installers like pip cannot install from sdists directly -- they need to first
  build a wheel, by invoking the :term:`build backend` that the sdist specifies
  (the build backend may do all sorts of transformations while building the wheel,
  such as compiling C extensions). For this reason, even for a pure Python
  project, you should always upload *both* an sdist and a wheel to PyPI or other
  package indices. This makes installation much faster for your users, since a
  wheel is directly installable. By only including files that must be installed,
  wheels also make for smaller downloads.

  On the technical level, a wheel is a ZIP archive (unlike sdists which are TAR
  archives). You can inspect its contents by unpacking it as a normal ZIP archive,
  e.g., using ``unzip`` on UNIX platforms like Linux and macOS, ``Expand-Archive``
  in Powershell on Windows, or :ref:`the command line interface of Python's
  zipfile module <python:zipfile-commandline>`. This can be very useful to check
  that the wheel includes all the files you need it to.

  Inside a wheel, you will find the package's files, plus an additional directory
  called :samp:`{package_name}-{version}.dist-info`. This directory contains
  various files, including a ``METADATA`` file which is the equivalent of
  ``PKG-INFO`` in sdists, as well as ``RECORD``. This can be useful to ensure no
  files are missing from your wheels.

  The file name of a wheel (ignoring some rarely used features) looks like this:
  :samp:`{package_name}-{version}-{python_tag}-{abi_tag}-{platform_tag}.whl`.
  This naming convention identifies which platforms and Python versions the wheel
  is compatible with. For example, the name ``pip-23.3.1-py3-none-any.whl`` means
  that:

  - (``py3``) This wheel can be installed on any implementation of Python 3,
    whether CPython, the most widely used Python implementation, or an alternative
    implementation like PyPy_;
  - (``none``) It does not depend on the Python version;
  - (``any``) It does not depend on the platform.

  The pattern ``py3-none-any`` is common for pure Python projects. Packages with
  extension modules typically ship multiple wheels with more complex tags.

  All technical details on the wheel format can be found in the :ref:`wheel
  specification <binary-distribution-format>`.


.. _egg-format:
.. _`Wheel vs Egg`:

那 eggs 呢？
================

**What about eggs?**

.. tab:: 中文

  "Egg" 是一种旧的包格式，已经被 wheel 格式取代，不应再使用。从 2023 年 8 月起，PyPI 已经 `拒绝上传 egg 格式的包 <pypi-eggs-deprecation_>`_。

  以下是 wheel 和 egg 格式之间的重要区别：

  * egg 格式由 :ref:`setuptools` 在 2004 年引入，而 wheel 格式由 :pep:`427` 在 2012 年引入。

  * Wheel 有一个 :doc:`官方标准规范 </specifications/binary-distribution-format>`，而 egg 没有。

  * Wheel 是一种 :term:`分发 <Distribution Package>` 格式，也就是一种打包格式。 [#wheel-importable1]_ 而 egg 既是分发格式，也是运行时安装格式（如果保持压缩），并且被设计为可以被导入。

  * Wheel 档案不包含 ``.pyc`` 文件。因此，当分发包只包含 Python 文件（即没有编译扩展），并且兼容 Python 2 和 3 时，wheel 可以是“通用”的，类似于 :term:`sdist <源分发 (或 "sdist")>` 格式。

  * Wheel 使用标准的 :ref:`.dist-info 目录 <recording-installed-packages>`，而 egg 使用 ``.egg-info``。

  * Wheel 具有更丰富的文件命名规范 :ref:`<wheel-file-name-spec>`。一个 wheel 档案可以指示其与多个 Python 语言版本和实现、ABI 以及系统架构的兼容性。

  * Wheel 是版本化的。每个 wheel 文件都包含 wheel 规范的版本和打包该文件的实现版本。

  * Wheel 按照 `sysconfig 路径类型 <https://docs.python.org/2/library/sysconfig.html#installation-paths>`_ 内部组织，因此更容易转换为其他格式。

.. tab:: 英文

  "Egg" is an old package format that has been replaced with the wheel format.  It
  should not be used anymore. Since August 2023, PyPI `rejects egg uploads
  <pypi-eggs-deprecation_>`_.

  Here's a breakdown of the important differences between wheel and egg.

  * The egg format was introduced by :ref:`setuptools` in 2004, whereas the wheel
    format was introduced by :pep:`427` in 2012.

  * Wheel has an :doc:`official standard specification
    </specifications/binary-distribution-format>`. Egg did not.

  * Wheel is a :term:`distribution <Distribution Package>` format, i.e a packaging
    format. [#wheel-importable]_ Egg was both a distribution format and a runtime
    installation format (if left zipped), and was designed to be importable.

  * Wheel archives do not include ``.pyc`` files. Therefore, when the distribution
    only contains Python files (i.e. no compiled extensions), and is compatible
    with Python 2 and 3, it's possible for a wheel to be "universal", similar to
    an :term:`sdist <Source Distribution (or "sdist")>`.

  * Wheel uses standard :ref:`.dist-info directories
    <recording-installed-packages>`.  Egg used ``.egg-info``.

  * Wheel has a :ref:`richer file naming convention <wheel-file-name-spec>`. A
    single wheel archive can indicate its compatibility with a number of Python
    language versions and implementations, ABIs, and system architectures.

  * Wheel is versioned. Every wheel file contains the version of the wheel
    specification and the implementation that packaged it.

  * Wheel is internally organized by `sysconfig path type
    <https://docs.python.org/2/library/sysconfig.html#installation-paths>`_,
    therefore making it easier to convert to other formats.

--------------------------------------------------------------------------------

.. [#core-metadata-format1] 这种格式是基于电子邮件的。尽管今天不太可能选择这种格式，但出于向后兼容的考虑，它仍然被保留为标准格式。从用户的角度来看，这种格式大多数情况下是不可见的，因为元数据是由用户以构建后端理解的方式指定的，通常是在 ``pyproject.toml`` 文件中的 ``[project]`` 部分，并由构建后端将其转换为 ``PKG-INFO`` 文件。

.. [#core-metadata-format] This format is email-based. Although this would
   be unlikely to be chosen today, backwards compatibility considerations lead to
   it being kept as the canonical format. From the user point of view, this
   is mostly invisible, since the metadata is specified by the user in a way
   understood by the build backend, typically ``[project]`` in ``pyproject.toml``,
   and translated by the build backend into ``PKG-INFO``.

.. [#wheel-importable1] 在某些情况下，wheel 可以作为可导入的运行时格式使用，尽管 :ref:`目前官方并不支持这种用法 <binary-distribution-format-import-wheel>` 。

.. [#wheel-importable] Circumstantially, in some cases, wheels can be used
   as an importable runtime format, although :ref:`this is not officially supported
   at this time <binary-distribution-format-import-wheel>`.



.. _pip-pypi: https://pypi.org/project/pip/23.3.1/#files
.. _pypi: https://pypi.org
.. _pypi-eggs-deprecation: https://blog.pypi.org/posts/2023-06-26-deprecate-egg-uploads/
.. _pypy: https://pypy.org
