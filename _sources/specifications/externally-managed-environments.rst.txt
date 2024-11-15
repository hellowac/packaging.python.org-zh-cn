
.. _externally-managed-environments:

===============================
外部管理环境
===============================

**Externally Managed Environments**

.. tab:: 中文

    尽管有些 Python 安装完全由安装 Python 的用户管理，但也有一些可能是由其他方式提供和管理的（例如，在 Linux 发行版中的操作系统包管理器，或作为应用程序中带有专用安装程序的捆绑 Python 环境）。

    尝试使用常规的 Python 打包工具来操作此类环境，最好的情况可能会令人困惑，最坏的情况甚至可能会彻底破坏底层操作系统。文档和互操作性指南只能在一定程度上解决这些问题。

    本规范定义了一个 ``EXTERNALLY-MANAGED`` 标记文件，允许 Python 安装向像 ``pip`` 这样的 Python 特定工具指示，它们既不安装也不移除包到解释器的默认安装环境中，而应该引导最终用户使用 :ref:`virtual-environments`。

    它还标准化了解释 ``sysconfig`` 安装方案的方式，以便在 Python 特定的包管理器即将安装包到解释器级别的上下文中时，可以以避免与外部包管理器冲突的方式进行操作，并减少破坏外部包管理器所提供软件的风险。

.. tab:: 英文

    While some Python installations are entirely managed by the user that installed
    Python, others may be provided and managed by another means (such as the
    operating system package manager in a Linux distribution, or as a bundled
    Python environment in an application with a dedicated installer).

    Attempting to use conventional Python packaging tools to manipulate such
    environments can be confusing at best and outright break the entire underlying
    operating system at worst. Documentation and interoperability guides only go
    so far in resolving such problems.

    This specification defines an ``EXTERNALLY-MANAGED`` marker file that allows a
    Python installation to indicate to Python-specific tools such as ``pip`` that they
    neither install nor remove packages into the interpreter’s default installation
    environment, and should instead guide the end user towards using
    :ref:`virtual-environments`.

    It also standardizes an interpretation of the ``sysconfig`` schemes so
    that, if a Python-specific package manager is about to install a
    package in an interpreter-wide context, it can do so in a manner that
    will avoid conflicting with the external package manager and reduces
    the risk of breaking software shipped by the external package manager.


术语
===========

**Terminology**

.. tab:: 中文

    本规范中使用的一些术语在其涵盖的不同上下文中有多个含义。为确保清晰，本规范对以下术语进行了特定的定义：

    distro
        "distribution"（分发版）的缩写，是指一种包含各种软件的集合，理想情况下这些软件能够正确地协同工作，包括（在本文件相关上下文中）Python 解释器本身，使用 Python 编写的软件，以及使用其他语言编写的软件。也就是说，这与 "Linux distro"（Linux 发行版）或 "Berkeley Software Distribution"（伯克利软件分发版）这样的短语中的意思相同。

        一个发行版可以是一个独立的操作系统（OS），例如 Debian、Fedora 或 FreeBSD。它也可以是一个安装在现有操作系统之上的覆盖发行版，例如 Homebrew 或 MacPorts。

        本文件使用缩写 "distro"，因为在 Python 打包的上下文中，"distribution" 另有含义，指的是单个 Python 软件包的源代码或二进制分发包，也就是指 ``setuptools.dist.Distribution`` 或 "sdist" 的意思。为避免混淆，本文件不使用 "distribution" 一词，而是使用 "distribution package" 或简称 "package"（见下文）。

        一个发行版的提供者 —— 即收集和发布软件并做出必要修改的团队或公司 —— 称为 **distributor** （分发者）。

    package
        一个可以在 Python 中安装并使用的软件单元。也就是说，这指的是 Python 特定的打包工具所称的 :term:`distribution package` 或简写为 "distribution"；在 Python 包索引（PyPI）中，"package" 被用作通俗的缩写。

        本文件中不会将 "package" 用来指代可导入的 Python 模块包，尽管在许多情况下，一个分发包可能由一个单一的可导入包组成，并且两者同名。

        本文件一般不会使用 "package" 来指代发行版包管理器（如 ``.deb`` 或 ``.rpm`` 文件）安装的单元。当需要时，会使用诸如 "a distro's package"（发行版的包）之类的措辞。（同样，许多情况下，Python 包会作为类似 ``python-`` 后缀的发行版包的一部分进行分发。）

    Python-specific package manager
        用于安装、升级和/或移除符合 Python 打包标准的 Python 包的工具。最流行的 Python 特定包管理器是 pip_，其他例子包括旧版的 `Easy Install 命令 <easy-install_>`_ 以及直接使用 ``setup.py`` 命令。

        （注意，``easy_install`` 命令已从 2021 年 1 月 23 日发布的 setuptools 版本 52 中移除。）

        （Conda_ 是一个特殊的案例，因为 ``conda`` 命令不仅能安装 Python 包，还能安装其他软件包，因此在某些情况下，它更像是一个发行版包管理器。由于 ``conda`` 命令通常仅在 Conda 创建的环境中运行，因此本文件中的大多数问题不适用于 ``conda``，当其作为 Python 特定的包管理器时。）

    distro package manager
        用于在已安装的发行版实例中安装、升级和/或移除发行版包的工具，该工具能够安装 Python 包和非 Python 包，并因此通常有自己独立的已安装软件数据库，与 :ref:`recording-installed-packages` 中的已安装分发包数据库无关。例子包括 ``apt``、``dpkg``、``dnf``、``rpm``、``pacman`` 和 ``brew``。关键特性是，如果某个包是由发行版包管理器安装的，那么以 Python 特定的包管理器方式移除或升级它，通常会使发行版包管理器处于不一致状态。

        本文件也使用诸如 "external package manager"（外部包管理器）或 "system's package manager"（系统包管理器）这样的短语来指代发行版包管理器。

    shadow
        "shadow" 已安装的 Python 包是指使另一个包在导入时被优先使用，而不移除任何来自被遮蔽包的文件。这需要在 ``sys.path`` 上有多个条目：例如，如果 A 2.0 包将模块 ``a.py`` 安装在某个 ``sys.path`` 条目中，而 A 1.0 包将模块 ``a.py`` 安装在后面的条目中，那么 ``import a`` 会返回来自 A 2.0 的模块，我们称 A 2.0 遮蔽了 A 1.0。

.. tab:: 英文

    A few terms used in this specification have multiple meanings in the
    contexts that it spans. For clarity, this specification uses the following
    terms in specific ways:

    distro
        Short for "distribution," a collection of various sorts of
        software, ideally designed to work properly together, including
        (in contexts relevant to this document) the Python interpreter
        itself, software written in Python, and software written in other
        languages. That is, this is the sense used in phrases such as
        "Linux distro" or "Berkeley Software Distribution."

        A distro can be an operating system (OS) of its own, such as
        Debian, Fedora, or FreeBSD. It can also be an overlay distribution
        that installs on top of an existing OS, such as Homebrew or
        MacPorts.

        This document uses the short term "distro," because the term
        "distribution" has another meaning in Python packaging contexts: a
        source or binary distribution package of a single piece of Python
        language software, that is, in the sense of
        ``setuptools.dist.Distribution`` or "sdist". To avoid confusion,
        this document does not use the plain term "distribution" at all.
        In the Python packaging sense, it uses the full phrase
        "distribution package" or just "package" (see below).

        The provider of a distro - the team or company that collects and
        publishes the software and makes any needed modifications - is its
        **distributor**.
    package
        A unit of software that can be installed and used within Python.
        That is, this refers to what Python-specific packaging tools tend
        to call a :term:`distribution package` or simply a "distribution";
        the colloquial abbreviation "package" is used in the sense of the
        Python Package Index.

        This document does not use "package" in the sense of an importable
        name that contains Python modules, though in many cases, a
        distribution package consists of a single importable package of
        the same name.

        This document generally does not use the term "package" to refer
        to units of installation by a distro's package manager (such as
        ``.deb`` or ``.rpm`` files). When needed, it uses phrasing such as
        "a distro's package." (Again, in many cases, a Python package is
        shipped inside a distro's package named something like ``python-``
        plus the Python package name.)
    Python-specific package manager
        A tool for installing, upgrading, and/or removing Python packages
        in a manner that conforms to Python packaging standards.
        The most popular Python-specific package
        manager is pip_; other examples include the old `Easy
        Install command <easy-install_>`_ as well as direct usage of a
        ``setup.py`` command.

        (Note that the ``easy_install`` command was removed in
        setuptools version 52, released 23 January 2021.)


        (Conda_ is a bit of a special case, as the ``conda``
        command can install much more than just Python packages, making it
        more like a distro package manager in some senses. Since the
        ``conda`` command generally only operates on Conda-created
        environments, most of the concerns in this document do not apply
        to ``conda`` when acting as a Python-specific package manager.)

    distro package manager
        A tool for installing, upgrading, and/or removing a distro's
        packages in an installed instance of that distro, which is capable
        of installing Python packages as well as non-Python packages, and
        therefore generally has its own database of installed software
        unrelated to the :ref:`database of installed distributions
        <recording-installed-packages>`. Examples include ``apt``, ``dpkg``,
        ``dnf``, ``rpm``, ``pacman``, and ``brew``. The salient feature is
        that if a package was installed by a distro package manager, removing or
        upgrading it in a way that would satisfy a Python-specific package
        manager will generally leave a distro package manager in an
        inconsistent state.

        This document also uses phrases like "external package manager" or
        "system's package manager" to refer to a distro package manager in
        certain contexts.
    shadow
        To shadow an installed Python package is to cause some other
        package to be preferred for imports without removing any files
        from the shadowed package. This requires multiple entries on
        ``sys.path``: if package A 2.0 installs module ``a.py`` in one
        ``sys.path`` entry, and package A 1.0 installs module ``a.py`` in
        a later ``sys.path`` entry, then ``import a`` returns the module
        from the former, and we say that A 2.0 shadows A 1.0.

.. _conda: https://conda.io
.. _pip: https://pip.pypa.io/en/stable/
.. _easy-install: https://setuptools.readthedocs.io/en/latest/deprecated/easy_install.html

概述
========

**Overview**

.. tab:: 中文

    本规范有两个方面。

    首先，它描述了 **一种标记 Python 解释器的方式，用于表明该解释器的包由 Python 之外的方式进行管理**，以便 Python 特定的工具（如 pip）在未明确覆盖的情况下，不应以任何方式（添加、升级/降级或移除）更改解释器的全局 ``sys.path`` 中安装的包。同时，它还提供了一种方式供发行版提供者指示如何使用虚拟环境作为替代方案。

    这是一个选择性机制：默认情况下，从上游源代码编译的 Python 解释器不会被标记，因此在使用自编译的解释器或未明确标记其解释器的发行版时，运行 ``pip install`` 将照常工作。

    其次，它设定了一个规则：当向解释器的全局上下文安装包时（无论是向未标记的解释器安装，还是覆盖标记），**Python 特定的包管理器应仅在它们会创建文件的 sysconfig 方案中的目录内修改或删除文件**。这使得 Python 解释器的发行者能够设置两个目录，一个用于其管理的包，一个用于最终用户安装的未管理包，并确保安装未管理的包时不会删除（或覆盖）外部包管理器所拥有的文件。

.. tab:: 英文

    This specification is twofold.

    First, it describes **a way for distributors of a Python interpreter to
    mark that interpreter as having its packages managed by means external
    to Python**, such that Python-specific tools like pip should not
    change the installed packages in the interpreter's global ``sys.path``
    in any way (add, upgrade/downgrade, or remove) unless specifically
    overridden. It also provides a means for the distributor to indicate
    how to use a virtual environment as an alternative.

    This is an opt-in mechanism: by default, the Python interpreter
    compiled from upstream sources will not be so marked, and so running
    ``pip install`` with a self-compiled interpreter, or with a distro
    that has not explicitly marked its interpreter, will work as it always
    has worked.

    Second, it sets the rule that when installing packages to an
    interpreter's global context (either to an unmarked interpreter, or if
    overriding the marking), **Python-specific package managers should
    modify or delete files only within the directories of the sysconfig
    scheme in which they would create files**. This permits a distributor
    of a Python interpreter to set up two directories, one for its own
    managed packages, and one for unmanaged packages installed by the end
    user, and ensure that installing unmanaged packages will not delete
    (or overwrite) files owned by the external package manager.


将解释器标记为使用外部包管理器
===========================================================

**Marking an interpreter as using an external package manager**

.. tab:: 中文

    在安装 Python 特定包的工具（例如 pip，而非外部工具如 apt）向某个 Python 环境中安装包之前，默认情况下，它应进行以下检查：

    1. 是否在虚拟环境之外运行？可以通过检查 ``sys.prefix == sys.base_prefix`` 来判断。

    2. 在由 ``sysconfig.get_path("stdlib", sysconfig.get_default_scheme())`` 指定的目录中是否存在 ``EXTERNALLY-MANAGED`` 文件？

    如果这两个条件都成立，安装器应退出并显示一条错误消息，指示在虚拟环境之外不允许向该 Python 解释器的目录中安装包。

    安装器应允许用户覆盖这些规则，例如通过命令行选项 ``--break-system-packages``。该选项默认不启用，并且应有一定的风险提示。

    ``EXTERNALLY-MANAGED`` 文件是一个 INI 风格的元数据文件，旨在通过标准库中的 configparser_ 模块进行解析。如果该文件可以通过 ``configparser.ConfigParser(interpolation=None)`` 以 UTF-8 编码进行解析，并且文件中包含 ``[externally-managed]`` 节，则安装器应查找文件中指定的错误消息，并将其作为错误的一部分输出。如果返回的元组的第一个元素（即语言代码）不是 ``None``，则应查找以 ``Error-`` 后跟语言代码的键。如果该键不存在，并且语言代码中包含下划线或连字符，则应查找以 ``Error-`` 后跟语言代码中下划线或连字符之前的部分的键。如果找不到这些键，或者语言代码为 ``None``，则应查找一个名为 ``Error`` 的键。

    如果安装器在文件中找不到错误消息（因为文件无法解析或没有找到合适的错误键），则应使用预定义的错误消息，建议用户创建一个虚拟环境以安装包。

    那些使用非 Python 特定包管理器（管理其 Python 包中 ``sys.path`` 中的库的包管理器）的软件发行商，通常应在其标准库目录中提供一个 ``EXTERNALLY-MANAGED`` 文件。例如，Debian 可能会在 ``/usr/lib/python3.9/EXTERNALLY-MANAGED`` 中提供一个类似如下内容的文件：

    .. code-block:: ini

        [externally-managed]
        Error=要在系统范围内安装 Python 包，请尝试使用 apt 安装
        python3-xyz，其中 xyz 是您尝试安装的包。

        如果您希望安装非 Debian 打包的 Python 包，
        请使用 python3 -m venv path/to/venv 创建虚拟环境。
        然后使用 path/to/venv/bin/python 和 path/to/venv/bin/pip。确保安装了 python3-full。

        如果您希望安装非 Debian 打包的 Python 应用程序，
        使用 pipx install xyz 可能是最简单的方法，它会为您管理虚拟环境。确保安装了 pipx。

        更多信息请参阅 /usr/share/doc/python3.9/README.venv。

    该文件为尝试安装包的用户提供了有用的、与发行版相关的信息。可选地，翻译也可以包含在同一文件中：

    .. code-block:: ini

        Error-de_DE=Wenn ist das Nunstück git und Slotermeyer?

        Ja! Beiherhund das Oder die Virtualenvironment gersput!

    在某些上下文中，如单一应用程序容器镜像（在创建后不会更新），发行版可能选择不提供 ``EXTERNALLY-MANAGED`` 文件，以便用户可以按照现在的方式安装任何他们喜欢的包，而无需手动覆盖该规则。

.. tab:: 英文

    Before a Python-specific package installer (that is, a tool such as
    pip - not an external tool such as apt) installs a package into a
    certain Python context, it should make the following checks by
    default:

    1. Is it running outside of a virtual environment? It can determine
       this by whether ``sys.prefix == sys.base_prefix``.

    2. Is there an ``EXTERNALLY-MANAGED`` file in the directory identified
       by ``sysconfig.get_path("stdlib", sysconfig.get_default_scheme())``?

    If both of these conditions are true, the installer should exit with
    an error message indicating that package installation into this Python
    interpreter's directory are disabled outside of a virtual environment.

    The installer should have a way for the user to override these rules,
    such as a command-line flag ``--break-system-packages``. This option
    should not be enabled by default and should carry some connotation
    that its use is risky.

    The ``EXTERNALLY-MANAGED`` file is an INI-style metadata file intended
    to be parsable by the standard library configparser_ module. If the
    file can be parsed by
    ``configparser.ConfigParser(interpolation=None)`` using the UTF-8
    encoding, and it contains a section ``[externally-managed]``, then the
    installer should look for an error message specified in the file and
    output it as part of its error. If the first element of the tuple
    returned by ``locale.getlocale(locale.LC_MESSAGES)``, i.e., the
    language code, is not ``None``, it should look for the error message
    as the value of a key named ``Error-`` followed by the language code.
    If that key does not exist, and if the language code contains
    underscore or hyphen, it should look for a key named ``Error-``
    followed by the portion of the language code before the underscore or
    hyphen. If it cannot find either of those, or if the language code is
    ``None``, it should look for a key simply named ``Error``.

    If the installer cannot find an error message in the file (either
    because the file cannot be parsed or because no suitable error key
    exists), then the installer should just use a pre-defined error
    message of its own, which should suggest that the user create a
    virtual environment to install packages.

    Software distributors who have a non-Python-specific package manager
    that manages libraries in the ``sys.path`` of their Python package
    should, in general, ship an ``EXTERNALLY-MANAGED`` file in their
    standard library directory. For instance, Debian may ship a file in
    ``/usr/lib/python3.9/EXTERNALLY-MANAGED`` consisting of something like

    .. code-block:: ini

        [externally-managed]
        Error=To install Python packages system-wide, try apt install
        python3-xyz, where xyz is the package you are trying to
        install.

        If you wish to install a non-Debian-packaged Python package,
        create a virtual environment using python3 -m venv path/to/venv.
        Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
        sure you have python3-full installed.

        If you wish to install a non-Debian packaged Python application,
        it may be easiest to use pipx install xyz, which will manage a
        virtual environment for you. Make sure you have pipx installed.

        See /usr/share/doc/python3.9/README.venv for more information.

    which provides useful and distro-relevant information
    to a user trying to install a package. Optionally,
    translations can be provided in the same file:

    .. code-block:: ini

        Error-de_DE=Wenn ist das Nunstück git und Slotermeyer?

        Ja! Beiherhund das Oder die Virtualenvironment gersput!

    In certain contexts, such as single-application container images that
    aren't updated after creation, a distributor may choose not to ship an
    ``EXTERNALLY-MANAGED`` file, so that users can install whatever they
    like (as they can today) without having to manually override this
    rule.

.. _configparser: https://docs.python.org/3/library/configparser.html

仅写入目标“sysconfig”方案
===============================================

**Writing to only the target ``sysconfig`` scheme**

.. tab:: 中文

    通常，Python 包安装器会将包安装到 ``sysconfig`` 标准库包返回的目录中。通常，这是由 ``sysconfig.get_default_scheme()`` 返回的方案，但根据配置（例如 ``pip install --user``），它可能会使用不同的方案。

    每当安装器向 ``sysconfig`` 方案安装时，本规范声明安装器不应修改或删除该方案以外的文件。例如，如果它正在升级一个包，而该包已经安装在该方案之外的某个目录（可能是另一个方案中的目录），则应保持现有文件不变。

    如果安装器在升级过程中确实覆盖了现有的安装，我们建议它在运行结束时产生警告。

    如果安装器正在向 ``sysconfig`` 方案之外的目录安装（例如， ``pip install --target`` ），则本小节不适用。

.. tab:: 英文

    Usually, a Python package installer installs to directories in a
    scheme returned by the ``sysconfig`` standard library package.
    Ordinarily, this is the scheme returned by
    ``sysconfig.get_default_scheme()``, but based on configuration (e.g.
    ``pip install --user``), it may use a different scheme.

    Whenever the installer is installing to a ``sysconfig`` scheme, this
    specification declares that the installer should never modify or delete files
    outside of that scheme. For instance, if it's upgrading a package, and
    the package is already installed in a directory outside that scheme
    (perhaps in a directory from another scheme), it should leave the
    existing files alone.

    If the installer does end up shadowing an existing installation during
    an upgrade, we recommend that it produces a warning at the end of its
    run.

    If the installer is installing to a location outside of a
    ``sysconfig`` scheme (e.g., ``pip install --target``), then this
    subsection does not apply.

发行版建议
===========================

**Recommendations for distros**

.. tab:: 中文

    本节为非规范性章节。它提供了我们认为发行版应遵循的最佳实践，除非有其他特殊原因。

.. tab:: 英文

    This section is non-normative. It provides best practices we believe
    distros should follow unless they have a specific reason otherwise.

将安装标记为外部管理
-------------------------------------------

**Mark the installation as externally managed**

.. tab:: 中文

    发行版应在其 ``stdlib`` 目录中创建一个 ``EXTERNALLY-MANAGED`` 文件。

.. tab:: 英文

    Distros should create an ``EXTERNALLY-MANAGED`` file in their ``stdlib`` directory.

引导用户进入虚拟环境
----------------------------------------

**Guide users towards virtual environments**

.. tab:: 中文

    该文件应包含有用且与发行版相关的错误消息，指明如何通过发行版的包管理器安装系统范围的包，并如何设置虚拟环境。如果您的发行版常被用户处于 ``python3`` 命令可用（尤其是 ``pip`` 或 ``get-pip`` 可用）但 ``python3 -m venv`` 无法正常工作的状态，消息应清晰地说明如何正确使 ``python3 -m venv`` 生效。

    考虑打包 ``pipx_``，这是一个用于安装 Python 语言应用程序的工具，并在错误消息中建议使用它。``pipx`` 会为该应用程序单独创建一个虚拟环境，这是一个更好的默认方案，适用于希望安装某些不在发行版中提供的 Python 语言软件，但不打算深入使用 Python 的终端用户。将 ``pipx`` 打包到发行版中，可以避免用户被告知要执行 ``pip install --user --break-system-packages pipx`` 来 *避免* 损坏系统包的矛盾局面。

    考虑安排您的发行版的 Python 环境（例如 Fedora 上的 ``python3`` 或 Debian 上的 ``python3-full``）依赖于 ``pipx``。

.. tab:: 英文

    The file should contain a useful and distro-relevant error message
    indicating both how to install system-wide packages via the distro's
    package manager and how to set up a virtual environment. If your
    distro is often used by users in a state where the ``python3`` command
    is available (and especially where ``pip`` or ``get-pip`` is
    available) but ``python3 -m venv`` does not work, the message should
    indicate clearly how to make ``python3 -m venv`` work properly.

    Consider packaging pipx_, a tool for installing Python-language
    applications, and suggesting it in the error. pipx automatically
    creates a virtual environment for that application alone, which is a
    much better default for end users who want to install some
    Python-language software (which isn't available in the distro) but are
    not themselves Python users. Packaging pipx in the distro avoids the
    irony of instructing users to ``pip install --user
    --break-system-packages pipx`` to *avoid* breaking system packages.
    Consider arranging things so your distro's package / environment for
    Python for end users (e.g., ``python3`` on Fedora or ``python3-full``
    on Debian) depends on pipx.

.. _pipx: https://github.com/pypa/pipx

将标记文件保存在容器镜像中
----------------------------------------

**Keep the marker file in container images**

.. tab:: 中文

    为单一应用程序容器（例如 Docker 容器镜像）生成官方镜像的发行版，应保留 ``EXTERNALLY-MANAGED`` 文件，最好以某种方式确保即使用户在该镜像内安装包更新（例如执行 ``RUN apt-get dist-upgrade``），该文件也不会消失。

.. tab:: 英文

    Distros that produce official images for single-application containers
    (e.g., Docker container images) should keep the
    ``EXTERNALLY-MANAGED`` file, preferably in a way that makes it not
    go away if a user of that image installs package updates inside
    their image (think ``RUN apt-get dist-upgrade``).

创建单独的发行版和本地目录
--------------------------------------------

**Create separate distro and local directories**

.. tab:: 中文

    发行版应在系统解释器的 ``sys.path`` 上设置两个独立的路径，一个用于发行版安装的包，另一个用于本地系统管理员安装的包，并配置 ``sysconfig.get_default_scheme()`` 指向后者的路径。这确保了像 pip 这样的工具不会修改发行版安装的包。对于本地系统管理员的路径应排在发行版路径之前，以便本地安装的包优先于发行版包。

    例如，Fedora 和 Debian（及其衍生版）通过将本地安装的包放在 ``/usr/local``，将发行版安装的包放在 ``/usr`` 来实现这种分离。Fedora 使用 ``/usr/local/lib/python3.x/site-packages`` 和 ``/usr/lib/python3.x/site-packages``。Debian 使用 ``/usr/local/lib/python3/dist-packages`` 和 ``/usr/lib/python3/dist-packages``，并在本地编译的 Python 解释器和发行版的 Python 解释器之间增加了一层分隔：如果你在 ``/usr/local/bin`` 中构建并安装上游 CPython，它将查看 ``/usr/local/lib/python3/site-packages``，而 Debian 想确保通过本地构建的解释器安装的包不会出现在发行版解释器的 ``sys.path`` 上。

    请注意，``/usr/local`` 与 ``/usr`` 的分离类似于 ``PATH`` 环境变量通常包括 ``/usr/local/bin:/usr/bin``，非发行版软件默认安装到 ``/usr/local``。这种分离是 `文件系统层次结构标准 <recommended by the Filesystem Hierarchy Standard_>`_ 推荐的。

    __ https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04s09.html

    有两种方式可以做到这一点。其一，如果你正在直接构建和打包 Python 库（例如，你的打包工具解包一个 wheel 文件或调用 ``setup.py install``），请安排这些工具使用一个不在 ``sysconfig`` 方案中的目录，但该目录仍在 ``sys.path`` 上。

    另一种方式是安排在包构建中运行时与在安装系统中运行时的默认 ``sysconfig`` 方案不同。bpo-43976_ 中的 ``sysconfig`` 自定义钩子应该使这变得简单（当它被接受并实现后）：使你的打包工具设置一个环境变量或其他可检测的配置，并定义一个 ``get_preferred_schemes`` 函数，在包构建中调用时返回不同的方案。然后，你可以在发行版打包时使用 ``pip install``。

    我们建议为 pip 添加一个 ``--scheme=...`` 选项，指示 pip 针对特定方案运行。（有关 pip 当前如何确定方案，请参见 `实施说明 <Implementation Notes_>`_ 。）一旦可用，你就可以在本地测试，甚至在实际打包时，运行类似 ``pip install --scheme=posix_distro`` 以显式将包安装到发行版的路径中（绕过 ``get_preferred_schemes``）。如果绝对需要，也可以使用 ``pip uninstall --scheme=posix_distro`` 来使用 pip 从系统管理目录中移除包。

    要使用 pip 安装包，你还需要抑制 ``EXTERNALLY-MANAGED`` 标记文件，以允许 pip 运行，或者在命令行中覆盖它。你可能希望在构建 chroot 环境中和容器镜像中使用相同的方法来抑制标记文件。

    自动设置这些（在构建环境中抑制标记文件，并使 ``get_preferred_schemes`` 自动返回发行版的方案）的好处是，未修改的 ``pip install`` 在包构建中将能正常工作，这通常意味着内部调用 ``pip install`` 的未修改的上游构建脚本会做正确的事。当然，你也可以确保你的打包过程始终调用 ``pip install --scheme=posix_distro --break-system-packages``，这也能正常工作。

    这里的最佳做法取决于你的发行版的打包约定和机制。

    类似地，``sysconfig`` 中不用于可导入 Python 代码的路径——即 ``include``、``platinclude``、``scripts`` 和 ``data``——也应具有两种变体，一种供发行版打包的软件使用，另一种供本地安装的软件使用，并且发行版应进行配置，使两者都可以使用。例如，典型的符合 FHS 标准的发行版将使用 ``/usr/local/include`` 作为默认方案的 ``include`` 目录，使用 ``/usr/include`` 作为发行版打包的头文件目录，并将两者都放在编译器的搜索路径中；它将使用 ``/usr/local/bin`` 作为默认方案的 ``scripts`` 目录，使用 ``/usr/bin`` 作为发行版打包的入口点，并将两者都放在 ``$PATH`` 中。

.. tab:: 英文

    Distros should place two separate paths on the system interpreter's
    ``sys.path``, one for distro-installed packages and one for packages
    installed by the local system administrator, and configure
    ``sysconfig.get_default_scheme()`` to point at the latter path. This
    ensures that tools like pip will not modify distro-installed packages.
    The path for the local system administrator should come before the
    distro path on ``sys.path`` so that local installs take preference
    over distro packages.

    For example, Fedora and Debian (and their derivatives) both implement
    this split by using ``/usr/local`` for locally-installed packages and
    ``/usr`` for distro-installed packages. Fedora uses
    ``/usr/local/lib/python3.x/site-packages`` vs.
    ``/usr/lib/python3.x/site-packages``. (Debian uses
    ``/usr/local/lib/python3/dist-packages`` vs.
    ``/usr/lib/python3/dist-packages`` as an additional layer of
    separation from a locally-compiled Python interpreter: if you build
    and install upstream CPython in ``/usr/local/bin``, it will look at
    ``/usr/local/lib/python3/site-packages``, and Debian wishes to make
    sure that packages installed via the locally-built interpreter don't
    show up on ``sys.path`` for the distro interpreter.)

    Note that the ``/usr/local`` vs. ``/usr`` split is analogous to how
    the ``PATH`` environment variable typically includes
    ``/usr/local/bin:/usr/bin`` and non-distro software installs to
    ``/usr/local`` by default. This split is `recommended by the
    Filesystem Hierarchy Standard`_.

    There are two ways you could do this. One is, if you are building and
    packaging Python libraries directly (e.g., your packaging helpers
    unpack a wheel or call ``setup.py install``), arrange
    for those tools to use a directory that is not in a ``sysconfig``
    scheme but is still on ``sys.path``.

    The other is to arrange for the default ``sysconfig`` scheme to change
    when running inside a package build versus when running on an
    installed system. The ``sysconfig`` customization hooks from
    bpo-43976_ should make this easy (once accepted and implemented):
    make your packaging tool set an
    environment variable or some other detectable configuration, and
    define a ``get_preferred_schemes`` function to return a different
    scheme when called from inside a package build. Then you can use ``pip
    install`` as part of your distro packaging.

    We propose adding a ``--scheme=...`` option to instruct pip to run
    against a specific scheme. (See `Implementation Notes`_ below for how
    pip currently determines schemes.) Once that's available, for local
    testing and possibly for actual packaging, you would be able to run
    something like ``pip install --scheme=posix_distro`` to explicitly
    install a package into your distro's location (bypassing
    ``get_preferred_schemes``). One could also, if absolutely needed, use
    ``pip uninstall --scheme=posix_distro`` to use pip to remove packages
    from the system-managed directory.

    To install packages with pip, you would also need to either suppress
    the ``EXTERNALLY-MANAGED`` marker file to allow pip to run or to
    override it on the command line. You may want to use the same means
    for suppressing the marker file in build chroots as you do in
    container images.

    The advantage of setting these up to be automatic (suppressing the
    marker file in your build environment and having
    ``get_preferred_schemes`` automatically return your distro's scheme)
    is that an unadorned ``pip install`` will work inside a package build,
    which generally means that an unmodified upstream build script that
    happens to internally call ``pip install`` will do the right thing.
    You can, of course, just ensure that your packaging process always
    calls ``pip install --scheme=posix_distro --break-system-packages``,
    which would work too.

    The best approach here depends a lot on your distro's conventions and
    mechanisms for packaging.

    Similarly, the ``sysconfig`` paths that are not for importable Python
    code - that is, ``include``, ``platinclude``, ``scripts``, and
    ``data`` - should also have two variants, one for use by
    distro-packaged software and one for use for locally-installed
    software, and the distro should be set up such that both are usable.
    For instance, a typical FHS-compliant distro will use
    ``/usr/local/include`` for the default scheme's ``include`` and
    ``/usr/include`` for distro-packaged headers and place both on the
    compiler's search path, and it will use ``/usr/local/bin`` for the
    default scheme's ``scripts`` and ``/usr/bin`` for distro-packaged
    entry points and place both on ``$PATH``.

.. _recommended by the Filesystem Hierarchy Standard: https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch04s09.html

.. _bpo-43976: https://bugs.python.org/issue43976


.. _Implementation Notes:

实施说明
====================

**Implementation Notes**

.. tab:: 中文

    本节是非规范性的，包含与规范和潜在实现相关的注释。

    目前（截至 2021 年 5 月），pip 并没有直接提供选择目标 ``sysconfig`` 方案的方法，但它有三种查找方案的方式：

    ``pip install``
        调用 ``sysconfig.get_default_scheme()``，通常（在上游 CPython 和大多数当前的发行版中）这与 ``get_preferred_scheme('prefix')`` 相同。

    ``pip install --prefix=/some/path``
        调用 ``sysconfig.get_preferred_scheme('prefix')``。

    ``pip install --user``
        调用 ``sysconfig.get_preferred_scheme('user')``。

    最后，``pip install --target=/some/path`` 直接写入 ``/some/path``，不查找任何方案。

    Debian 当前携带了一个 `补丁以更改虚拟环境中的默认安装位置 <patch to change the default install location inside a virtual environment_>`__，通过一些启发式方法（包括检查 ``VIRTUAL_ENV`` 环境变量），主要是为了确保虚拟环境中的目录保持为 ``site-packages`` 而不是 ``dist-packages``。这对本提案没有特别影响，因为该补丁的实现并未真正更改默认的 ``sysconfig`` 方案，特别是并没有更改 ``sysconfig.get_path("stdlib")`` 的结果。

    Fedora 当前携带了一个 `补丁以更改非 rpmbuild 环境中的默认安装位置 <patch to change the default install location when not running inside rpmbuild_>`__，它们用于实现两个系统范围目录的方法。这在概念上与 bpo-43976_ 中设想的钩子类似，除了它是作为代码补丁应用到 ``distutils`` 中，而不是作为更改的 ``sysconfig`` 方案。

    上文中实现的 ``is_virtual_environment`` 逻辑，以及加载 ``EXTERNALLY-MANAGED`` 文件并从中查找错误消息的逻辑，可以添加到标准库（分别是 ``sys`` 和 ``sysconfig`` 中），以集中管理这些实现，但目前不需要立即添加。

.. tab:: 英文

    This section is non-normative and contains notes relevant to both the
    specification and potential implementations.

    Currently (as of May 2021), pip does not directly expose a way to choose
    a target ``sysconfig`` scheme, but it has three ways of looking up schemes
    when installing:

    ``pip install``
        Calls ``sysconfig.get_default_scheme()``, which is usually (in
        upstream CPython and most current distros) the same as
        ``get_preferred_scheme('prefix')``.

    ``pip install --prefix=/some/path``
        Calls ``sysconfig.get_preferred_scheme('prefix')``.

    ``pip install --user``
        Calls ``sysconfig.get_preferred_scheme('user')``.

    Finally, ``pip install --target=/some/path`` writes directly to
    ``/some/path`` without looking up any schemes.

    Debian currently carries a `patch to change the default install
    location inside a virtual environment`__, using a few heuristics
    (including checking for the ``VIRTUAL_ENV`` environment variable),
    largely so that the directory used in a virtual environment remains
    ``site-packages`` and not ``dist-packages``. This does not
    particularly affect this proposal, because the implementation of that
    patch does not actually change the default ``sysconfig`` scheme, and
    notably does not change the result of
    ``sysconfig.get_path("stdlib")``.

    Fedora currently carries a `patch to change the default install
    location when not running inside rpmbuild`__, which they use to
    implement the two-system-wide-directories approach. This is
    conceptually the sort of hook envisioned by bpo-43976_, except
    implemented as a code patch to ``distutils`` instead of as a changed
    ``sysconfig`` scheme.

    The implementation of ``is_virtual_environment`` above, as well as the
    logic to load the ``EXTERNALLY-MANAGED`` file and find the error
    message from it, may as well get added to the standard library
    (``sys`` and ``sysconfig``, respectively), to centralize their
    implementations, but they don't need to be added yet.

.. _patch to change the default install location inside a virtual environment: https://sources.debian.org/src/python3.7/3.7.3-2+deb10u3/debian/patches/distutils-install-layout.diff/

.. _patch to change the default install location when not running inside rpmbuild: https://src.fedoraproject.org/rpms/python3.9/blob/f34/f/00251-change-user-install-location.patch




版权
=========

**Copyright**

.. tab:: 中文

    本文档属于公共领域或 CC0-1.0-Universal 许可证（以更宽松的为准）。

.. tab:: 英文

    This document is placed in the public domain or under the CC0-1.0-Universal license, whichever is more permissive.



历史
=======

**History**

.. tab:: 中文

    - 2022 年 6 月：该规范已通过 :pep:`668` 批准。

.. tab:: 英文

    - June 2022: This specification was approved through :pep:`668`.
