
.. _virtual-environments:

===========================
Python 虚拟环境
===========================

**Python Virtual Environments**

.. tab:: 中文

    对于 Python 3.3 及更高版本，:pep:`405` 引入了对“Python 虚拟环境”概念的解释器级支持。每个虚拟环境都有自己的 Python 二进制文件（允许创建具有不同 Python 版本的环境），并且可以在其站点目录中拥有自己独立的一组已安装的 Python 包，但与基础安装的 Python 共享标准库。在此更新之前，虚拟环境的概念已经存在，但没有先前标准化的声明或发现它们的机制。

.. tab:: 英文

    For Python 3.3 and later versions, :pep:`405` introduced interpreter level support
    for the concept of "Python Virtual Environments". Each virtual environment has
    its own Python binary (allowing creation of environments with various Python
    versions) and can have its own independent set of installed Python packages in
    its site directories, but shares the standard library with the base installed
    Python. While the concept of virtual environments existed prior to this update,
    there was no previously standardised mechanism for declaring or discovering them.


虚拟环境的运行时检测
=========================================

**Runtime detection of virtual environments**

.. tab:: 中文

    在运行时，可以通过 :py:data:`sys.prefix`（运行的解释器的文件系统位置）与 :py:data:`sys.base_prefix`（标准库目录的默认文件系统位置）值不同来识别虚拟环境。

    Python 标准库文档中的 :ref:`venv-explanation` 解释了这一点，并涵盖了在交互式操作系统 shell 中“激活”虚拟环境的概念（此激活步骤是可选的，因此它所做的更改不能可靠地用于检测 Python 程序是否在虚拟环境中运行）。

.. tab:: 英文

    At runtime, virtual environments can be identified by virtue of
    :py:data:`sys.prefix` (the filesystem location of the running interpreter)
    having a different value from :py:data:`sys.base_prefix` (the default filesystem
    location of the standard library directories).

    :ref:`venv-explanation` in the Python standard library documentation for the
    :py:mod:`venv` module covers this along with the concept of "activating" a
    virtual environment in an interactive operating system shell (this activation
    step is optional and hence the changes it makes can't be reliably used to
    detect whether a Python program is running in a virtual environment or not).


将安装环境声明为 Python 虚拟环境
==================================================================

**Declaring installation environments as Python virtual environments**

.. tab:: 中文

    如 :pep:`405` 所描述，Python 虚拟环境的最简单形式仅由 Python 二进制文件的副本或符号链接、一个 ``site-packages`` 目录和一个 ``pyvenv.cfg`` 文件组成，该文件包含一个 ``home`` 键，指示在哪里可以找到 Python 标准库模块。

    虽然该设计是为了满足标准 :py:mod:`venv` 模块的需求，但这种拆分安装和 ``pyvenv.cfg`` 文件的方式可以被 *任何* 需要 Python 特定工具意识到它们已经在虚拟环境中运行且不需要进一步环境嵌套的 Python 安装提供商使用。

    即使没有 ``pyvenv.cfg`` 文件，任何导致 :py:data:`sys.prefix` 和 :py:data:`sys.base_prefix` 值不同的方式（例如 ``sitecustomize.py``，或者对已安装的 Python 运行时进行修补），只要它仍然提供与 :py:mod:`sysconfig` 中的默认包安装方案相匹配的设置，就会被检测并表现得像一个 Python 虚拟环境。

.. tab:: 英文

    As described in :pep:`405`, a Python virtual environment in its simplest form
    consists of nothing more than a copy or symlink of the Python binary accompanied
    by a ``site-packages`` directory and a ``pyvenv.cfg`` file with a ``home`` key
    that indicates where to find the Python standard library modules.

    While designed to meet the needs of the standard :py:mod:`venv` module, this
    split installation and ``pyvenv.cfg`` file approach can be used by *any*
    Python installation provider that desires Python-specific tools to be aware that
    they are already operating in a virtual environment and no further environment
    nesting is required or desired.

    Even in the absence of a ``pyvenv.cfg`` file, any approach (e.g.
    ``sitecustomize.py``, patching the installed Python runtime) that results in
    :py:data:`sys.prefix` and :py:data:`sys.base_prefix` having different values,
    while still providing a matching default package installation scheme in
    :py:mod:`sysconfig`, will be detected and behave as a Python virtual environment.


历史记录
=======

**History**

.. tab:: 中文

    - 2012 年 5 月：该规范通过 :pep:`405` 批准。

.. tab:: 英文

    - May 2012: This specification was approved through :pep:`405`.
