
.. _platform-compatibility-tags:

===========================
平台兼容性标签
===========================

**Platform compatibility tags**

.. tab:: 中文

    平台兼容性标签允许构建工具将发行版标记为与特定平台兼容，并使安装程序能够了解哪些发行版与其运行的系统兼容。

.. tab:: 英文

    Platform compatibility tags allow build tools to mark distributions as being
    compatible with specific platforms, and allows installers to understand which
    distributions are compatible with the system they are running on.


概述
========

**Overview**

.. tab:: 中文

    标签格式为 ``{python tag}-{abi tag}-{platform tag}``。

    python tag  
        'py27', 'cp33'
    abi tag
        'cp32dmu', 'none'
    platform tag
        'linux_x86_64', 'any'

    例如，标签 ``py27-none-any`` 表示兼容 Python 2.7（任何 Python 2.7 实现），无 ABI 要求，并适用于任何平台。

    ``wheel`` 构建包格式在其文件名中包含这些标签，格式如下：
    ``{distribution}-{version}(-{build tag})?-{python tag}-{abi tag}-{platform tag}.whl``。其他包格式可能有其自身的命名规范。

    任何标签中的空格都应替换为 ``_``。

.. tab:: 英文

    The tag format is ``{python tag}-{abi tag}-{platform tag}``.

    python tag
        'py27', 'cp33'
    abi tag
        'cp32dmu', 'none'
    platform tag
        'linux_x86_64', 'any'

    For example, the tag ``py27-none-any`` indicates compatibility with Python 2.7
    (any Python 2.7 implementation) with no abi requirement, on any platform.

    The ``wheel`` built package format includes these tags in its filenames,
    of the form
    ``{distribution}-{version}(-{build tag})?-{python tag}-{abitag}-{platform tag}.whl``.
    Other package formats may have their own conventions.

    Any potential spaces in any tag should be replaced with ``_``.


Python 标签
=================

**Python Tag**

.. tab:: 中文

    Python 标签表示发行版所需的实现和版本。主要实现有以下缩写代码：

    * py: 通用 Python（不需要特定实现的特性）
    * cp: CPython
    * ip: IronPython
    * pp: PyPy
    * jy: Jython

    其他 Python 实现应使用 :py:data:`sys.implementation.name <sys.implementation>`。

    版本为 ``py_version_nodot`` 格式。CPython 通常省略点，但如果需要，使用下划线 ``_`` 替代。PyPy 应使用其特有版本，例如 ``pp18``，``pp19``。

    对于许多纯 Python 发行版，版本可以仅为主版本号 ``2`` 或 ``3`` （如 ``py2``，``py3``）。

    重要的是，仅包含主版本的标签如 ``py2`` 和 ``py3`` 并不是 ``py20`` 和 ``py30`` 的简写。这些标签意味着发行方有意发布一个跨版本兼容的发行版。

    一个单一源码的 Python 2/3 兼容发行版可以使用复合标签 ``py2.py3``。详见下文的 `压缩标签集 <Compressed Tag Sets_>`_。

.. tab:: 英文

    The Python tag indicates the implementation and version required by
    a distribution.  Major implementations have abbreviated codes, initially:

    * py: Generic Python (does not require implementation-specific features)
    * cp: CPython
    * ip: IronPython
    * pp: PyPy
    * jy: Jython

    Other Python implementations should use :py:data:`sys.implementation.name <sys.implementation>`.

    The version is ``py_version_nodot``.  CPython gets away with no dot,
    but if one is needed the underscore ``_`` is used instead.  PyPy should
    probably use its own versions here ``pp18``, ``pp19``.

    The version can be just the major version ``2`` or ``3`` ``py2``, ``py3`` for
    many pure-Python distributions.

    Importantly, major-version-only tags like ``py2`` and ``py3`` are not
    shorthand for ``py20`` and ``py30``.  Instead, these tags mean the packager
    intentionally released a cross-version-compatible distribution.

    A single-source Python 2/3 compatible distribution can use the compound
    tag ``py2.py3``.  See `Compressed Tag Sets`_, below.


ABI 标签
===========

**ABI Tag**

.. tab:: 中文

    ABI 标签表示任何包含的扩展模块所需的 Python ABI。对于特定实现的 ABI，使用与 Python 标签相同的缩写方式，例如 ``cp33d`` 表示 CPython 3.3 的调试版本 ABI。

    CPython 的稳定 ABI 为 ``abi3``，与共享库后缀一致。

    对于 ABI 非常不稳定的实现，可以使用其源代码修订和编译标志等的 SHA-256 哈希的前 6 个字节（编码为 8 个 base64 字符），但这些实现可能不会有分发二进制发行版的强烈需求。每个实现的社区可以决定如何最佳使用 ABI 标签。

.. tab:: 英文

    The ABI tag indicates which Python ABI is required by any included
    extension modules.  For implementation-specific ABIs, the implementation
    is abbreviated in the same way as the Python Tag, e.g. ``cp33d`` would be
    the CPython 3.3 ABI with debugging.

    The CPython stable ABI is ``abi3`` as in the shared library suffix.

    Implementations with a very unstable ABI may use the first 6 bytes (as
    8 base64-encoded characters) of the SHA-256 hash of their source code
    revision and compiler flags, etc, but will probably not have a great need
    to distribute binary distributions. Each implementation's community may
    decide how to best use the ABI tag.


平台标签
============

**Platform Tag**

基本平台标签
-------------------

**Basic platform tags**

.. tab:: 中文

    在其最简单的形式中，平台标签为 :py:func:`sysconfig.get_platform()` 的输出，其中所有的连字符 ``-`` 和句点 ``.`` 替换为下划线 ``_``。在 Python 3.12 移除 :ref:`distutils` 之前，该方法为 ``distutils.util.get_platform()``。例如：

    * win32
    * linux_i386
    * linux_x86_64

.. tab:: 英文

    In its simplest form, the platform tag is :py:func:`sysconfig.get_platform()` with
    all hyphens ``-`` and periods ``.`` replaced with underscore ``_``.
    Until the removal of :ref:`distutils` in Python 3.12, this
    was ``distutils.util.get_platform()``. For example:

    * win32
    * linux_i386
    * linux_x86_64


.. _manylinux:

``manylinux``
-------------

.. tab:: 中文

    上述简单的方案对于 Linux 平台的 wheel 文件的公共分发是不够的，因为 Linux 平台的生态系统庞大且它们之间存在微妙的差异。

    因此，对于这些平台，``manylinux`` 标准表示一组共同的 Linux 平台子集，并允许构建带有 ``manylinux`` 平台标签的 wheel 文件，这些文件可以在大多数常见的 Linux 发行版中使用。

    当前的标准是具有前瞻性的 ``manylinux_x_y`` 标准。它定义了形如 ``manylinux_x_y_arch`` 的标签，其中 ``x`` 和 ``y`` 是支持的 glibc 主版本和次版本（例如，``manylinux_2_24_xxx`` 应该可以在任何使用 glibc 2.24+ 的发行版上工作），``arch`` 是架构，与系统上 :py:func:`sysconfig.get_platform()` 的值相匹配，如上面的“简单”形式。

    以下较旧的标签仍然为了向后兼容而被支持：

    * ``manylinux1`` 支持在 ``x86_64`` 和 ``i686`` 架构上使用 glibc 2.5。
    * ``manylinux2010`` 支持在 ``x86_64`` 和 ``i686`` 架构上使用 glibc 2.12。
    * ``manylinux2014`` 支持在 ``x86_64``、``i686``、``aarch64``、``armv7l``、``ppc64``、``ppc64le`` 和 ``s390x`` 架构上使用 glibc 2.17。

    通常，为旧版本规范构建的发行版是向前兼容的（意味着 ``manylinux1`` 发行版应继续在现代系统上工作），但不是向后兼容的（意味着 ``manylinux2010`` 发行版预计不会在 2010 年之前存在的平台上工作）。

    包维护者应尝试以最兼容的规范为目标，但需要注意的是，``manylinux1`` 和 ``manylinux2010`` 的构建环境已经达到生命周期结束，这意味着这些镜像将不再收到安全更新。

    以下表格显示了支持各种 ``manylinux`` 标准的相关项目的最低版本：

    ==========  ==============  =================  =================  =================
    Tool        ``manylinux1``  ``manylinux2010``  ``manylinux2014``  ``manylinux_x_y``
    ==========  ==============  =================  =================  =================
    pip         ``>=8.1.0``     ``>=19.0``         ``>=19.3``         ``>=20.3``
    auditwheel  ``>=1.0.0``     ``>=2.0.0``        ``>=3.0.0``        ``>=3.3.0`` [1]_
    ==========  ==============  =================  =================  =================

.. tab:: 英文

    The simple scheme above is insufficient for public distribution of wheel files
    to Linux platforms, due to the large ecosystem of Linux platforms and subtle
    differences between them.

    Instead, for those platforms, the ``manylinux`` standard represents a common
    subset of Linux platforms, and allows building wheels tagged with the
    ``manylinux`` platform tag which can be used across most common Linux
    distributions.

    The current standard is the future-proof ``manylinux_x_y`` standard. It defines
    tags of the form ``manylinux_x_y_arch``, where ``x`` and ``y`` are glibc major
    and minor versions supported (e.g. ``manylinux_2_24_xxx`` should work on any
    distro using glibc 2.24+), and ``arch`` is the architecture, matching the value
    of :py:func:`sysconfig.get_platform()` on the system as in the "simple" form above.

    The following older tags are still supported for backward compatibility:

    * ``manylinux1`` supports glibc 2.5 on ``x86_64`` and ``i686`` architectures.
    * ``manylinux2010`` supports glibc 2.12 on ``x86_64`` and ``i686``.
    * ``manylinux2014`` supports glibc 2.17 on ``x86_64``, ``i686``, ``aarch64``, ``armv7l``, ``ppc64``, ``ppc64le``, and ``s390x``.

    In general, distributions built for older versions of the specification are
    forwards-compatible (meaning that ``manylinux1`` distributions should continue
    to work on modern systems) but not backwards-compatible (meaning that
    ``manylinux2010`` distributions are not expected to work on platforms that
    existed before 2010).

    Package maintainers should attempt to target the most compatible specification
    possible, with the caveat that the provided build environment for
    ``manylinux1`` and ``manylinux2010`` have reached end-of-life meaning that
    these images will no longer receive security updates.

    The following table shows the minimum versions of relevant projects to support
    the various ``manylinux`` standards:

    ==========  ==============  =================  =================  =================
    Tool        ``manylinux1``  ``manylinux2010``  ``manylinux2014``  ``manylinux_x_y``
    ==========  ==============  =================  =================  =================
    pip         ``>=8.1.0``     ``>=19.0``         ``>=19.3``         ``>=20.3``
    auditwheel  ``>=1.0.0``     ``>=2.0.0``        ``>=3.0.0``        ``>=3.3.0`` [1]_
    ==========  ==============  =================  =================  =================

.. [1] Only support for ``manylinux_2_24`` has been added in auditwheel 3.3.0


``musllinux``
-------------

.. tab:: 中文

    ``musllinux`` 标签系列类似于 ``manylinux``，但用于使用 musl_ libc 而不是 glibc 的 Linux 平台（一个主要的例子是 Alpine Linux）。其模式为 ``musllinux_x_y_arch``，支持 musl ``x.y`` 及更高版本，并且适用于架构 ``arch``。

    musl 版本值可以通过执行 Python 解释器当前运行的 musl libc 共享库并解析输出获取：


    .. code-block:: python

        import re
        import subprocess

        def get_musl_major_minor(so: str) -> tuple[int, int] | None:
            """检测 musl 运行时版本。

            返回一个包含 ``(major, minor)`` 的元组，表示 musl 库的版本，
            如果给定的 libc .so 未输出预期信息，则返回 ``None``。

            libc 库应输出类似下面的信息到 stderr::

                musl libc (x86_64)
                Version 1.2.2
                Dynamic Program Loader
            """
            proc = subprocess.run([so], stderr=subprocess.PIPE, text=True)
            lines = (line.strip() for line in proc.stderr.splitlines())
            lines = [line for line in lines if line]
            if len(lines) < 2 or lines[0][:4] != "musl":
                return None
            match = re.match(r"Version (\d+)\.(\d+)", lines[1])
            if match:
                return (int(match.group(1)), int(match.group(2)))
            return None

    目前，有两种可能的方法可以找到 Python 解释器所运行的 musl 库的位置，分别是使用系统的 ldd_ 命令，或者通过解析可执行文件 ELF_ 头中的 ``PT_INTERP`` 部分的值。

.. tab:: 英文

    The ``musllinux`` family of tags is similar to ``manylinux``, but for Linux
    platforms that use the musl_ libc rather than glibc (a prime example being Alpine
    Linux). The schema is ``musllinux_x_y_arch``, supporting musl ``x.y`` and higher
    on the architecture ``arch``.

    The musl version values can be obtained by executing the musl libc shared
    library the Python interpreter is currently running on, and parsing the output:

    .. code-block:: python

        import re
        import subprocess

        def get_musl_major_minor(so: str) -> tuple[int, int] | None:
            """Detect musl runtime version.

            Returns a two-tuple ``(major, minor)`` that indicates musl
            library's version, or ``None`` if the given libc .so does not
            output expected information.

            The libc library should output something like this to stderr::

                musl libc (x86_64)
                Version 1.2.2
                Dynamic Program Loader
            """
            proc = subprocess.run([so], stderr=subprocess.PIPE, text=True)
            lines = (line.strip() for line in proc.stderr.splitlines())
            lines = [line for line in lines if line]
            if len(lines) < 2 or lines[0][:4] != "musl":
                return None
            match = re.match(r"Version (\d+)\.(\d+)", lines[1])
            if match:
                return (int(match.group(1)), int(match.group(2)))
            return None

    There are currently two possible ways to find the musl library’s location that a
    Python interpreter is running on, either with the system ldd_ command, or by
    parsing the ``PT_INTERP`` section’s value from the executable’s ELF_ header.


用法
=======

**Use**

.. tab:: 中文

    这些标签由安装程序用来决定从潜在的已构建发行版列表中下载哪个已构建的发行版（如果有的话）。安装程序维护一个（pyver, abi, arch）元组的列表，表示它所支持的发行版。如果已构建发行版的标签在该列表中，则可以安装。

    建议安装程序默认选择最具功能完整性的已构建发行版（即最适合当前安装环境的版本），然后再回退到为旧版 Python 发布的纯 Python 版本。还建议安装程序提供一种配置和重新排序允许的兼容标签列表的方法；例如，用户可能只接受 ``*-none-any`` 标签，以便只下载标明自己为纯 Python 的已构建包。

    另一个理想的安装程序特性是，如果可能，优先考虑“从源代码重新编译”而不是一些兼容但过时的预构建选项。

    以下示例列表适用于在 linux_x86_64 系统上运行 CPython 3.3 的安装程序。该列表按优先级排序，从最优选（包含编译扩展模块、为当前 Python 版本构建的发行版）到最不优选（为较旧 Python 版本构建的纯 Python 发行版）：

    1.  cp33-cp33m-linux_x86_64
    2.  cp33-abi3-linux_x86_64
    3.  cp3-abi3-linux_x86_64
    4.  cp33-none-linux_x86_64*
    5.  cp3-none-linux_x86_64*
    6.  py33-none-linux_x86_64*
    7.  py3-none-linux_x86_64*
    8.  cp33-none-any
    9.  cp3-none-any
    10.  py33-none-any
    11.  py3-none-any
    12.  py32-none-any
    13.  py31-none-any
    14.  py30-none-any

    * 预构建发行版可能是平台特定的，原因不仅仅是 C 扩展，例如通过包含作为子进程调用的本地可执行文件。

    有时，对于某个特定版本的软件包，可能会有多个支持的已构建发行版。例如，打算发布带有可选 C 扩展的 ``cp33-abi3-linux_x86_64`` 标签的包，并且同一发行版也可以用 ``py3-none-any`` 标签发布，但没有 C 扩展。此时，支持标签列表中的标签索引打破了平局，具有 C 扩展的包将优先安装，因为该标签在列表中排在前面。

.. tab:: 英文

    The tags are used by installers to decide which built distribution
    (if any) to download from a list of potential built distributions.
    The installer maintains a list of (pyver, abi, arch) tuples that it
    will support.  If the built distribution's tag is ``in`` the list, then
    it can be installed.

    It is recommended that installers try to choose the most feature complete
    built distribution available (the one most specific to the installation
    environment) by default before falling back to pure Python versions
    published for older Python releases. Installers are also recommended to
    provide a way to configure and re-order the list of allowed compatibility
    tags; for example, a user might accept only the ``*-none-any`` tags to only
    download built packages that advertise themselves as being pure Python.

    Another desirable installer feature might be to include "re-compile from
    source if possible" as more preferable than some of the compatible but
    legacy pre-built options.

    This example list is for an installer running under CPython 3.3 on a
    linux_x86_64 system. It is in order from most-preferred (a distribution
    with a compiled extension module, built for the current version of
    Python) to least-preferred (a pure-Python distribution built with an
    older version of Python):

    1.  cp33-cp33m-linux_x86_64
    2.  cp33-abi3-linux_x86_64
    3.  cp3-abi3-linux_x86_64
    4.  cp33-none-linux_x86_64*
    5.  cp3-none-linux_x86_64*
    6.  py33-none-linux_x86_64*
    7.  py3-none-linux_x86_64*
    8.  cp33-none-any
    9.  cp3-none-any
    10.  py33-none-any
    11.  py3-none-any
    12.  py32-none-any
    13.  py31-none-any
    14.  py30-none-any

    * Built distributions may be platform specific for reasons other than C
      extensions, such as by including a native executable invoked as
      a subprocess.

    Sometimes there will be more than one supported built distribution for a
    particular version of a package.  For example, a packager could release
    a package tagged ``cp33-abi3-linux_x86_64`` that contains an optional C
    extension and the same distribution tagged ``py3-none-any`` that does not.
    The index of the tag in the supported tags list breaks the tie, and the
    package with the C extension is installed in preference to the package
    without because that tag appears first in the list.

.. _Compressed Tag Sets:

压缩标签集
===================

**Compressed Tag Sets**

.. tab:: 中文

    为了允许兼容多个标签三元组的 bdists 使用简洁的文件名，每个文件名中的标签可以是由点 ``.`` 分隔、按顺序排列的标签集合。例如，pip 是一个纯 Python 包，旨在通过相同的源代码在 Python 2 和 3 下运行，可以分发一个带有标签 ``py2.py3-none-any`` 的 bdist。

    简单标签的完整列表是::

        for x in pytag.split('.'):
            for y in abitag.split('.'):
                for z in platformtag.split('.'):
                    yield '-'.join((x, y, z))

    实现此方案的 bdist 格式应在 bdist 特定的元数据中包含扩展标签。该压缩方案可能会生成大量不受支持的标签和“不可用”标签，这些标签并未被任何 Python 实现所支持，例如 "cp33-cp31u-win64"，因此应谨慎使用。

.. tab:: 英文

    To allow for compact filenames of bdists that work with more than
    one compatibility tag triple, each tag in a filename can instead be a
    '.'-separated, sorted, set of tags.  For example, pip, a pure-Python
    package that is written to run under Python 2 and 3 with the same source
    code, could distribute a bdist with the tag ``py2.py3-none-any``.
    The full list of simple tags is::

        for x in pytag.split('.'):
            for y in abitag.split('.'):
                for z in platformtag.split('.'):
                    yield '-'.join((x, y, z))

    A bdist format that implements this scheme should include the expanded
    tags in bdist-specific metadata.  This compression scheme can generate
    large numbers of unsupported tags and "impossible" tags that are supported
    by no Python implementation e.g. "cp33-cp31u-win64", so use it sparingly.

常见问题
=========

**FAQ**

.. tab:: 中文

    默认使用哪些标签？
        工具应默认使用最优先的与架构相关的标签，例如 ``cp33-cp33m-win32``，或者最优先的纯 Python 标签，例如 ``py33-none-any``。如果包管理者覆盖了默认标签，则表示他们打算提供跨 Python 版本的兼容性。

    如果我的分发使用了 Python 最新版本特有的功能，我应该使用什么标签？
        兼容性标签帮助安装程序选择 *最兼容* 的 *单一版本* 分发。例如，当 ``beaglevote-1.2.0`` 没有 Python 3.3 兼容版本（因为它使用了 Python 3.4 特有的功能）时，它可能仍然使用 ``py3-none-any`` 标签，而不是 ``py34-none-any`` 标签。Python 3.3 用户必须结合其他限定条件，比如需要使用不包含新功能的旧版本 ``beaglevote-1.1.0``，才能获取兼容的构建版本。

    为什么 Python 版本号中没有 ``.``？
        CPython 已经有 20 多年没有使用 3 位数的主版本号了，预计这种情况会持续一段时间。其他实现可能使用下划线 ``_`` 作为分隔符，因为 ``-`` 和 ``.`` 都会分隔文件名的各个部分。

    为什么将连字符和其他非字母数字字符规范化为下划线？
        为了避免与文件名中分隔组件的 ``.`` 和 ``-`` 字符冲突，并且为了更好地兼容各种文件系统对文件名的限制（包括可以在 URL 路径中使用而无需引用）。

    为什么不使用特殊字符 <X> 而使用 ``.`` 或 ``-``？
        可能是因为该字符在某些上下文中不方便或容易引起混淆（例如，``+`` 必须在 URL 中引用，``~`` 在 POSIX 中表示用户的主目录），或者是因为这些优势不足以让人改变在 :pep:`427` 中定义的 wheel 格式的现有参考实现（例如，使用 ``,`` 而不是 ``.`` 来分隔压缩标签中的组件）。

    谁将维护缩写实现的注册表？
        可以在 python-dev 邮件列表上申请新的两字母缩写。作为一条经验法则，缩写仅保留给当前最突出的 4 个实现。

    兼容性标签应该放入 METADATA 还是 PKG-INFO？
        不应该。兼容性标签是构建分发的元数据的一部分。METADATA / PKG-INFO 应该是整个分发的有效数据，而不是某个构建版本的元数据。

    为什么没有提到我最喜欢的 Python 实现？
        缩写标签便于在公共索引中共享编译后的 Python 代码。您的 Python 实现也可以使用此规范，但需要使用更长的标签。请记住，所有“纯 Python”构建分发仅使用 ``py``。

    为什么在参考实现中 ABI 标签（第二个标签）有时是 "none"？
        由于 Python 2 没有简单的方法获取 SOABI（该概念出现在较新的 Python 3 版本中），因此在撰写时参考实现猜测为 "none"。理想情况下，它应该检测到类似于 Python 3 的 "py27(d|m|u)"，但在此之前，"none" 足够表示 "不知道"。

.. tab:: 英文

    What tags are used by default?
        Tools should use the most-preferred architecture dependent tag
        e.g. ``cp33-cp33m-win32`` or the most-preferred pure python tag
        e.g. ``py33-none-any`` by default.  If the packager overrides the
        default it indicates that they intended to provide cross-Python
        compatibility.

    What tag do I use if my distribution uses a feature exclusive to the newest version of Python?
        Compatibility tags aid installers in selecting the *most compatible*
        build of a *single version* of a distribution. For example, when
        there is no Python 3.3 compatible build of ``beaglevote-1.2.0``
        (it uses a Python 3.4 exclusive feature) it may still use the
        ``py3-none-any`` tag instead of the ``py34-none-any`` tag. A Python
        3.3 user must combine other qualifiers, such as a requirement for the
        older release ``beaglevote-1.1.0`` that does not use the new feature,
        to get a compatible build.

    Why isn't there a ``.`` in the Python version number?
        CPython has lasted 20+ years without a 3-digit major release. This
        should continue for some time.  Other implementations may use _ as
        a delimiter, since both - and . delimit the surrounding filename.

    Why normalise hyphens and other non-alphanumeric characters to underscores?
        To avoid conflicting with the ``.`` and ``-`` characters that separate
        components of the filename, and for better compatibility with the
        widest range of filesystem limitations for filenames (including
        being usable in URL paths without quoting).

    Why not use special character <X> rather than ``.`` or ``-``?
        Either because that character is inconvenient or potentially confusing
        in some contexts (for example, ``+`` must be quoted in URLs, ``~`` is
        used to denote the user's home directory in POSIX), or because the
        advantages weren't sufficiently compelling to justify changing the
        existing reference implementation for the wheel format defined in :pep:`427`
        (for example, using ``,`` rather than ``.`` to separate components
        in a compressed tag).

    Who will maintain the registry of abbreviated implementations?
        New two-letter abbreviations can be requested on the python-dev
        mailing list.  As a rule of thumb, abbreviations are reserved for
        the current 4 most prominent implementations.

    Does the compatibility tag go into METADATA or PKG-INFO?
        No.  The compatibility tag is part of the built distribution's
        metadata.  METADATA / PKG-INFO should be valid for an entire
        distribution, not a single build of that distribution.

    Why didn't you mention my favorite Python implementation?
        The abbreviated tags facilitate sharing compiled Python code in a
        public index.  Your Python implementation can use this specification
        too, but with longer tags.
        Recall that all "pure Python" built distributions just use ``py``.

    Why is the ABI tag (the second tag) sometimes "none" in the reference implementation?
        Since Python 2 does not have an easy way to get to the SOABI
        (the concept comes from newer versions of Python 3) the reference
        implementation at the time of writing guesses "none".  Ideally it
        would detect "py27(d|m|u)" analogous to newer versions of Python,
        but in the meantime "none" is a good enough way to say "don't know".


历史记录
==========

**History**

.. tab:: 中文

    - 2013年2月：该规范的原始版本通过 :pep:`425` 得到批准。
    - 2016年1月：``manylinux1`` 标签通过 :pep:`513` 得到批准。
    - 2018年4月：``manylinux2010`` 标签通过 :pep:`571` 得到批准。
    - 2019年7月：``manylinux2014`` 标签通过 :pep:`599` 得到批准。
    - 2019年11月：``manylinux_x_y`` 永久性标签通过 :pep:`600` 得到批准。
    - 2021年4月：``musllinux_x_y`` 标签通过 :pep:`656` 得到批准。

.. tab:: 英文

    - February 2013: The original version of this specification was approved through :pep:`425`.
    - January 2016: The ``manylinux1`` tag was approved through :pep:`513`.
    - April 2018: The ``manylinux2010`` tag was approved through :pep:`571`.
    - July 2019: The ``manylinux2014`` tag was approved through :pep:`599`.
    - November 2019: The ``manylinux_x_y`` perennial tag was approved through :pep:`600`.
    - April 2021: The ``musllinux_x_y`` tag was approved through :pep:`656`.



.. _musl: https://musl.libc.org
.. _ldd: https://www.man7.org/linux/man-pages/man1/ldd.1.html
.. _elf: https://refspecs.linuxfoundation.org/elf/elf.pdf
