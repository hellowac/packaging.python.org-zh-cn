
=============================
部署 Python 应用程序
=============================

**Deploying Python applications**

:Page Status: Incomplete
:Last Reviewed: 2021-8-24


概述
========

**Overview**


支持多种硬件平台
--------------------------------------

**Supporting multiple hardware platforms**

.. tab:: 中文

  ::

    FIXME

    意思是: x86、x64、ARM，其他平台？

    对于仅包含 Python 的分发版，应该可以在所有支持 Python 运行的平台上轻松部署。

    对于包含二进制扩展的分发版，部署就成了一个大难题。不仅需要在所有操作系统和硬件平台的组合上构建扩展，还需要对它们进行测试，最好是在持续集成平台上进行测试。这些问题与上面提到的“多个 Python 版本”部分类似，不确定是否应将其作为一个单独的部分。即使在 Windows x64 上，32 位和 64 位版本的 Python 都有相当大的使用量。

.. tab:: 英文

  ::

    FIXME

    Meaning: x86, x64, ARM, others?

    For Python-only distributions, it *should* be straightforward to deploy on all
    platforms where Python can run.

    For distributions with binary extensions, deployment is major headache.  Not only
    must the extensions be built on all the combinations of operating system and
    hardware platform, but they must also be tested, preferably on continuous
    integration platforms.  The issues are similar to the "multiple Python
    versions" section above, not sure whether this should be a separate section.
    Even on Windows x64, both the 32 bit and 64 bit versions of Python enjoy
    significant usage.



操作系统打包和安装程序
=========================

**OS packaging & installers**

.. tab:: 中文

  ::

    FIXME

    - 为项目构建 rpm/deb 包
    - 为整个虚拟环境构建 rpm/deb 包
    - 为 Python 项目构建 macOS 安装包
    - 使用 Kivy+P4A 或 P4A & Buildozer 构建 Android APK

.. tab:: 英文

  ::

    FIXME

    - Building rpm/debs for projects
    - Building rpms/debs for whole virtualenvs
    - Building macOS installers for Python projects
    - Building Android APKs with Kivy+P4A or P4A & Buildozer

Windows
-------

**Windows**

.. tab:: 中文

  ::

    FIXME

    - 为 Python 项目构建 Windows 安装程序

.. tab:: 英文

  ::

    FIXME

    - Building Windows installers for Python projects

Pynsist
^^^^^^^

.. tab:: 中文

  `Pynsist <https://pypi.org/project/pynsist>`__ 是一个将 Python 程序与 Python 解释器打包成一个基于 NSIS 的单一安装包的工具。在大多数情况下，打包只需要用户选择一个 Python 解释器版本并声明程序的依赖。该工具会下载指定的 Windows 版 Python 解释器，并将其与所有依赖一起打包成一个单独的 Windows 可执行安装包。

  安装后的程序可以通过安装程序在开始菜单中添加的快捷方式启动。它使用安装在应用程序目录中的 Python 解释器，与计算机上的其他 Python 安装无关。

  Pynsist 的一个大优点是，Windows 包可以在 Linux 上构建。 :any:`文档 <pynsist:index>` 中提供了多个不同类型程序（控制台、GUI）的示例。该工具以 MIT 许可证发布。

.. tab:: 英文

  `Pynsist <https://pypi.org/project/pynsist>`__ is a tool that bundles Python
  programs together with the Python-interpreter into a single installer based on
  NSIS. In most cases, packaging only requires the user to choose a version of
  the Python-interpreter and declare the dependencies of the program. The tool
  downloads the specified Python-interpreter for Windows and packages it with all
  the dependencies in a single Windows-executable installer.

  The installed program can be started from a shortcut that the installer adds to
  the start-menu. It uses a Python interpreter installed within its application
  directory, independent of any other Python installation on the computer.

  A big advantage of Pynsist is that the Windows packages can be built on Linux.
  There are several examples for different kinds of programs (console, GUI) in
  the :any:`documentation <pynsist:index>`. The tool is released
  under the MIT-licence.

应用程序包
===================

**Application bundles**

.. tab:: 中文

  ::

    FIXME

    - wheels kinda/sorta

.. tab:: 英文

  ::

    FIXME

    - wheels kinda/sorta

Windows
-------

py2exe
^^^^^^

.. tab:: 中文

  `py2exe <https://pypi.org/project/py2exe/>`__ 是一个 distutils 扩展，允许将 Python 脚本构建成独立的 Windows 可执行程序（32 位和 64 位）。它支持官方开发周期中包含的 Python 版本（参见 `Python 分支的状态 <https://devguide.python.org/#status-of-python-branches>`_ ）。py2exe 可以构建控制台可执行文件和 Windows（GUI）可执行文件。构建 Windows 服务和 DLL/EXE COM 服务器可能会起作用，但并不积极支持。该 distutils 扩展以 MIT 许可证和 Mozilla 公共许可证 2.0 版发布。

.. tab:: 英文

  `py2exe <https://pypi.org/project/py2exe/>`__ is a distutils extension which
  allows to build standalone Windows executable programs (32-bit and 64-bit)
  from Python scripts. Python versions included in the official development
  cycle are supported (refers to `Status of Python branches`__). py2exe can
  build console executables and windows (GUI) executables. Building windows
  services, and DLL/EXE COM servers might work but it is not actively supported.
  The distutils extension is released under the MIT-licence and Mozilla
  Public License 2.0.

.. __: https://devguide.python.org/#status-of-python-branches

macOS
-----

py2app
^^^^^^

.. tab:: 中文

  `py2app <https://pypi.org/project/py2app/>`__ 是一个 Python setuptools 命令，允许你从 Python 脚本创建独立的 macOS 应用程序包和插件。请注意，py2app 必须在 macOS 上使用来构建应用程序，不能在其他平台上创建 Mac 应用程序。py2app 以 MIT 许可证发布。

.. tab:: 英文

  `py2app <https://pypi.org/project/py2app/>`__ is a Python setuptools
  command which will allow you to make standalone macOS application
  bundles and plugins from Python scripts. Note that py2app MUST be used
  on macOS to build applications, it cannot create Mac applications on other
  platforms. py2app is released under the MIT-license.

Unix（包括 Linux 和 macOS）
-----------------------------------

**Unix (including Linux and macOS)**

pex
^^^

.. tab:: 中文

  `pex <https://pypi.org/project/pex/>`__ 是一个用于生成 .pex（Python 可执行文件）文件的库，这些文件是类似于 virtualenv 的可执行 Python 环境。pex 扩展了 :pep:`441` 中概述的理念，使得 Python 应用程序的部署像 ``cp`` 一样简单。pex 文件甚至可以包含多个平台特定的 Python 发行版，这意味着一个单一的 pex 文件可以跨 Linux 和 macOS 平台移植。pex 以 Apache License 2.0 许可证发布。

.. tab:: 英文

  `pex <https://pypi.org/project/pex/>`__ is  a library for generating .pex
  (Python EXecutable) files which are executable Python environments in the
  spirit of virtualenvs. pex is an expansion upon the ideas outlined in :pep:`441`
  and makes the deployment of Python applications as simple as cp. pex files may
  even include multiple platform-specific Python distributions, meaning that a
  single pex file can be portable across Linux and macOS. pex is released under the
  Apache License 2.0.

配置管理
========================

**Configuration management**

::

  FIXME

  puppet
  salt
  chef
  ansible
  fabric
