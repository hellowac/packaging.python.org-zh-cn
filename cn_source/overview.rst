============================
概述
============================

**Overview of Python Packaging**

.. tab:: 中文

   .. 编辑者，请参阅文档底部的维护信息。

   作为一种通用编程语言，Python 旨在多种方式使用。您可以用它构建网站、工业机器人或为朋友们制作游戏，等等，所有这些都可以使用相同的核心技术。

   Python 的灵活性正是为什么每个 Python 项目的第一步必须是思考项目的受众和项目运行的环境。虽然在编写代码之前考虑打包可能看起来有些奇怪，但这个过程可以有效避免未来的麻烦。

   本概述提供了一个通用的决策树，帮助您理清 Python 众多打包选项。继续阅读，选择最适合您下一个项目的技术。

.. tab:: 英文


   .. Editors, see notes at the bottom of the document for maintenance info.

   As a general-purpose programming language, Python is designed to be
   used in many ways. You can build web sites or industrial robots or a
   game for your friends to play, and much more, all using the same
   core technology.

   Python's flexibility is why the first step in every Python project
   must be to think about the project's audience and the corresponding
   environment where the project will run. It might seem strange to think
   about packaging before writing code, but this process does wonders for
   avoiding future headaches.

   This overview provides a general-purpose decision tree for reasoning
   about Python's plethora of packaging options. Read on to choose the best
   technology for your next project.

考虑部署
-------------------------

**Thinking about deployment**

.. tab:: 中文

   软件包的存在是为了被安装（或 *部署* ），因此在打包任何内容之前，您需要回答以下一些关于部署的问题：

   * 谁是您的软件用户？您的软件是由从事软件开发的其他开发人员、数据中心的运维人员，还是一些不太懂软件的用户群体安装？
   * 您的软件是打算在服务器、桌面、移动客户端（手机、平板等）上运行，还是嵌入到专用设备中？
   * 您的软件是单独安装，还是以大规模批量部署的方式安装？

   打包与目标环境和部署体验密切相关。上述问题有很多不同的答案，每种情况组合都有其解决方案。根据这些信息，以下概述将帮助您选择最适合您项目的打包技术。

.. tab:: 英文

   Packages exist to be installed (or *deployed*), so before you package
   anything, you'll want to have some answers to the deployment questions
   below:

   * Who are your software's users? Will your software be installed by other developers doing software development, operations people in a datacenter, or a less software-savvy group?
   * Is your software intended to run on servers, desktops, mobile clients (phones, tablets, etc.), or embedded in dedicated devices?
   * Is your software installed individually, or in large deployment batches?

   Packaging is all about target environment and deployment
   experience. There are many answers to the questions above and each
   combination of circumstances has its own solutions. With this
   information, the following overview will guide you to the packaging
   technologies best suited to your project.

打包 Python 库和工具
------------------------------------

**Packaging Python libraries and tools**

.. tab:: 中文

   您可能听说过 PyPI、 ``setup.py`` 和 ``wheel`` 文件。这些只是 Python 生态系统提供的用于将 Python 代码分发给开发人员的一些工具，您可以在 :doc:`guides/distributing-packages-using-setuptools` 中阅读更多内容。

   以下的打包方法适用于在开发环境中由技术人员使用的库和工具。如果您正在寻找为非技术用户和/或生产环境打包 Python 的方法，请跳到 :ref:`packaging-applications`。

.. tab:: 英文

   You may have heard about PyPI, ``setup.py``, and ``wheel``
   files. These are just a few of the tools Python's ecosystem provides
   for distributing Python code to developers, which you can read about in
   :doc:`guides/distributing-packages-using-setuptools`.

   The following approaches to packaging are meant for libraries and
   tools used by technical audience in a development setting. If you're
   looking for ways to package Python for a non-technical audience and/or
   a production setting, skip ahead to :ref:`packaging-applications`.

Python 模块
^^^^^^^^^^^^^^

**Python modules**

.. tab:: 中文

   一个 Python 文件，只要它仅依赖于标准库，就可以重新分发和重用。您还需要确保它是为正确版本的 Python 编写的，并且只依赖于标准库。

   这种方式非常适合在使用兼容 Python 版本的人之间共享简单的脚本和代码片段（例如通过电子邮件、StackOverflow 或 GitHub gists）。甚至有一些整个 Python 库也提供了这种选项，比如 :doc:`bottle.py<bottle:tutorial>` 和 :doc:`boltons <boltons:architecture>`。

   然而，对于由多个文件组成、需要额外库或需要特定版本 Python 的项目，这种模式就不适用了，因此下面列出了其他选项。

.. tab:: 英文

   A Python file, provided it only relies on the standard library, can be
   redistributed and reused. You will also need to ensure it's written
   for the right version of Python, and only relies on the standard
   library.

   This is great for sharing simple scripts and snippets between people
   who both have compatible Python versions (such as via email,
   StackOverflow, or GitHub gists). There are even some entire Python
   libraries that offer this as an option, such as
   :doc:`bottle.py<bottle:tutorial>` and :doc:`boltons
   <boltons:architecture>`.

   However, this pattern won't scale for projects that consist of
   multiple files, need additional libraries, or need a specific version
   of Python, hence the options below.

Python 源代码发行版
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Python source distributions**

.. tab:: 中文

   如果您的代码由多个 Python 文件组成，通常会以目录结构的形式进行组织。任何包含 Python 文件的目录都可以构成一个 :term:`导入包 <Import Package>`。

   由于包由多个文件组成，它们的分发变得更为复杂。大多数协议仅支持一次传输一个文件（您上次点击链接并下载多个文件是什么时候？）。这增加了获取不完整传输的风险，并且更难确保目标位置的代码完整性。

   只要您的代码仅包含纯 Python 代码，并且您知道您的部署环境支持您的 Python 版本，那么您可以使用 Python 的原生打包工具创建一个 *源代码* :term:`分发包`，简称 *sdist* 。

   Python 的 *sdist* 是压缩档案文件（ ``.tar.gz`` 文件），其中包含一个或多个包或模块。如果您的代码是纯 Python 的，并且只依赖其他 Python 包，您可以查阅 :ref:`源代码分发格式` 规范了解更多信息。

   如果您的代码依赖任何非 Python 代码或非 Python 包（例如 `lxml <https://pypi.org/project/lxml/>`_ 中的 `libxml2 <https://en.wikipedia.org/wiki/Libxml2>`_，或 `numpy <https://pypi.org/project/numpy>`_ 中的 BLAS 库），您将需要使用下一节中介绍的格式，这对于纯 Python 库也有许多优点。

   .. note:: 
      
      Python 和 PyPI 支持多种分发方式，提供相同包的不同实现。例如，已不再维护但具有重要意义的 `PIL 分发包 <https://pypi.org/project/PIL/>`_ 提供了 PIL 包，而 `Pillow <https://pypi.org/project/Pillow/>`_ 是 PIL 的一个活跃维护的分支！

      这个 Python 打包的超级能力使得 Pillow 可以作为 PIL 的即插即用替代品，只需更改项目的 ``install_requires`` 或 ``requirements.txt``。

.. tab:: 英文

   If your code consists of multiple Python files, it's usually organized
   into a directory structure. Any directory containing Python files can
   comprise an :term:`Import Package`.

   Because packages consist of multiple files, they are harder to
   distribute. Most protocols support transferring only one file at a
   time (when was the last time you clicked a link and it downloaded
   multiple files?). It's easier to get incomplete transfers, and harder
   to guarantee code integrity at the destination.

   So long as your code contains nothing but pure Python code, and you
   know your deployment environment supports your version of Python, then
   you can use Python's native packaging tools to create a *source*
   :term:`Distribution Package`, or *sdist* for short.

   Python's *sdists* are compressed archives (``.tar.gz`` files)
   containing one or more packages or modules. If your code is
   pure-Python, and you only depend on other Python packages, you can
   go to the :ref:`source-distribution-format` specification to learn more.

   If you rely on any non-Python code, or non-Python packages (such as
   `libxml2 <https://en.wikipedia.org/wiki/Libxml2>`_ in the case of
   `lxml <https://pypi.org/project/lxml/>`_, or BLAS libraries in the
   case of `numpy <https://pypi.org/project/numpy>`_), you will need to
   use the format detailed in the next section, which also has many
   advantages for pure-Python libraries.

   .. note:: Python and PyPI support multiple distributions providing
      different implementations of the same package. For instance the
      unmaintained-but-seminal `PIL distribution
      <https://pypi.org/project/PIL/>`_ provides the PIL package, and so
      does `Pillow <https://pypi.org/project/Pillow/>`_, an
      actively-maintained fork of PIL!

      This Python packaging superpower makes it possible for Pillow to be
      a drop-in replacement for PIL, just by changing your project's
      ``install_requires`` or ``requirements.txt``.

Python 二进制发行版
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Python binary distributions**

.. tab:: 中文

   Python 的实际能力在于它能够与软件生态系统集成，尤其是与用 C、C++、Fortran、Rust 和其他语言编写的库进行集成。

   并不是所有开发者都有构建这些用编译语言编写的组件的工具或经验，因此 Python 创建了 :term:`Wheel`（轮子），一种旨在传输包含编译产物的库的包格式。事实上，Python 的包安装器 ``pip`` 总是优先使用 Wheel 格式，因为安装速度更快，因此即使是纯 Python 包，使用 Wheel 格式也能更好地工作。

   二进制分发包在与源代码分发包一起提供时效果最好。即使您没有为每个操作系统上传您的代码的 Wheel 文件，通过上传 sdist，您仍然能让其他平台的用户自行构建您的代码。建议默认同时发布 sdist 和 wheel 文件， *除非* 您正在为一个特定的用例创建构件，并且知道接收方只需要其中之一。

   Python 和 PyPI 使得同时上传 Wheel 和 sdist 文件变得非常简单。只需按照 :doc:`tutorials/packaging-projects` 中的教程操作。

   .. figure:: assets/py_pkg_tools_and_libs.png
      :width: 80%
      :alt: Python 的工具和库打包能力概览。

      Python 推荐的内置库和工具打包技术。摘自 `The Packaging Gradient (2017) <https://www.youtube.com/watch?v=iLVNWfPWAC8>`_。

.. tab:: 英文

   So much of Python's practical power comes from its ability to
   integrate with the software ecosystem, in particular libraries written
   in C, C++, Fortran, Rust, and other languages.

   Not all developers have the right tools or experiences to build these
   components written in these compiled languages, so Python created the
   :term:`Wheel`, a package format designed to ship libraries with
   compiled artifacts. In fact, Python's package installer, ``pip``,
   always prefers wheels because installation is always faster, so even
   pure-Python packages work better with wheels.

   Binary distributions are best when they come with source distributions
   to match. Even if you don't upload wheels of your code for every
   operating system, by uploading the sdist, you're enabling users of
   other platforms to still build it for themselves. Default to
   publishing both sdist and wheel archives together, *unless* you're
   creating artifacts for a very specific use case where you know the
   recipient only needs one or the other.

   Python and PyPI make it easy to upload both wheels and sdists
   together. Just follow the :doc:`tutorials/packaging-projects`
   tutorial.

   .. figure:: assets/py_pkg_tools_and_libs.png
      :width: 80%
      :alt: A summary of Python's packaging capabilities for tools and libraries.

      Python's recommended built-in library and tool packaging
      technologies. Excerpted from `The Packaging Gradient (2017)
      <https://www.youtube.com/watch?v=iLVNWfPWAC8>`_.

.. _packaging-applications:

打包 Python 应用程序
-----------------------------

**Packaging Python applications**

.. tab:: 中文

   到目前为止，我们只讨论了 Python 的原生分发工具。根据我们的介绍，您可以推断出这些内置方法仅适用于具有 Python 环境并且懂得如何安装 Python 包的用户。

   考虑到操作系统、配置和使用者的多样性，只有在面向开发者用户时，这种假设才是安全的。

   Python 的原生打包方式主要是为了在开发者之间分发可重用的代码，即库。您可以利用 Python 的库打包技术，借助像 :doc:`setuptools entry_points <setuptools:userguide/entry_point>` 这样的工具，将 **工具** 或开发者的基础应用程序构建在其上。

   库是构建块，而不是完整的应用程序。对于应用程序的分发，存在着一系列全新的技术可供选择。

   接下来的几个部分将根据目标环境的依赖关系来整理这些应用程序打包选项，帮助您为项目选择合适的解决方案。

.. tab:: 英文

   So far we've only discussed Python's native distribution tools. Based
   on our introduction, you would be correct to infer these built-in
   approaches only target environments which have Python, and an
   audience who knows how to install Python packages.

   With the variety of operating systems, configurations, and people out
   there, this assumption is only safe when targeting a developer
   audience.

   Python's native packaging is mostly built for distributing reusable
   code, called libraries, between developers. You can piggyback
   **tools**, or basic applications for developers, on top of Python's
   library packaging, using technologies like
   :doc:`setuptools entry_points <setuptools:userguide/entry_point>`.

   Libraries are building blocks, not complete applications. For
   distributing applications, there's a whole new world of technologies
   out there.

   The next few sections organize these application packaging options
   according to their dependencies on the target environment,
   so you can choose the right one for your project.

取决于框架
^^^^^^^^^^^^^^^^^^^^^^^^

**Depending on a framework**

.. tab:: 中文

   某些类型的 Python 应用程序，如网站后端和其他网络服务，足够常见，因此它们有相应的框架来支持开发和打包。其他类型的应用程序，如动态网页前端和移动客户端，因其复杂性而使框架不仅仅是一个便利工具。

   在所有这些情况下，从框架的打包和部署方式出发倒推是一个合理的做法。一些框架包含了一个部署系统，这个系统封装了本指南中提到的技术。在这种情况下，您将希望参考框架的打包指南，以便获得最简单和最可靠的生产体验。

   如果您曾经好奇这些平台和框架是如何在幕后工作的，您可以随时阅读接下来的章节。

.. tab:: 英文

   Some types of Python applications, like web site backends and other
   network services, are common enough that they have frameworks to
   enable their development and packaging. Other types of applications,
   like dynamic web frontends and mobile clients, are complex enough to
   target that a framework becomes more than a convenience.

   In all these cases, it makes sense to work backwards, from the
   framework's packaging and deployment story. Some frameworks include a
   deployment system which wraps the technologies outlined in the rest of
   the guide. In these cases, you'll want to defer to your framework's
   packaging guide for the easiest and most reliable production experience.

   If you ever wonder how these platforms and frameworks work under the
   hood, you can always read the sections beyond.

服务平台
*****************

**Service platforms**

.. tab:: 中文

   如果您正在为“ `平台即服务(Platform-as-a-Service : PaaS) <https://en.wikipedia.org/wiki/Platform_as_a_service>`_ ”开发应用程序，您需要遵循各自的打包指南。这类平台会处理打包和部署，只要您遵循它们的模式。大多数软件并不符合这些模板，因此才会有下面介绍的其他选项。

   如果您正在开发将部署到您自己拥有的机器、用户的个人计算机或其他任何安排的应用程序，请继续阅读。

.. tab:: 英文

   If you're developing for a
   "`Platform-as-a-Service <https://en.wikipedia.org/wiki/Platform_as_a_service>`_"
   or "PaaS", you are going to want to follow their respective packaging
   guides. These types of platforms take care of packaging and deployment,
   as long as you follow their patterns. Most software does not fit one of
   these templates, hence the existence of all the other options below.

   If you're developing software that will be deployed to machines you
   own, users' personal computers, or any other arrangement, read on.

Web 浏览器和移动应用程序
************************************

**Web browsers and mobile applications**

.. tab:: 中文

   Python 的持续进步正在将它引入新的领域。如今，您可以使用 Python 编写移动应用程序或 Web 应用程序的前端。虽然语言本身可能很熟悉，但打包和部署的实践却是全新的。

   如果您计划在这些新领域发布应用程序，您将需要查看以下框架，并参考它们的打包指南：

   * `Kivy <https://kivy.org/>`_
   * `Beeware <https://pybee.org/>`_
   * `Brython <https://brython.info/>`_
   * `Flexx <https://flexx.readthedocs.io/en/latest/>`_

   如果您不打算使用框架或平台，或者仅仅对上述框架使用的一些技术和方法感兴趣，继续阅读下面的内容。

.. tab:: 英文

   Python's steady advances are leading it into new spaces. These days
   you can write a mobile app or web application frontend in
   Python. While the language may be familiar, the packaging and
   deployment practices are brand new.

   If you're planning on releasing to these new frontiers, you'll want to
   check out the following frameworks, and refer to their packaging
   guides:

   * `Kivy <https://kivy.org/>`_
   * `Beeware <https://pybee.org/>`_
   * `Brython <https://brython.info/>`_
   * `Flexx <https://flexx.readthedocs.io/en/latest/>`_

   If you are *not* interested in using a framework or platform, or just
   wonder about some of the technologies and techniques utilized by the
   frameworks above, continue reading below.

取决于预安装的 Python
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Depending on a pre-installed Python**

.. tab:: 中文

   选择一台任意计算机，根据上下文，很有可能 Python 已经预先安装。多年来，Python 默认包含在大多数 Linux 和 Mac 操作系统中，因此您可以合理地依赖它已经存在于数据中心或开发者和数据科学家的个人机器上。

   支持此模型的技术包括：

   * :gh:`PEX <pantsbuild/pex#user-content-pex>` （Python 可执行文件）
   * :doc:`zipapp <python:library/zipapp>` （不帮助管理依赖项，要求 Python 3.5+）
   * :gh:`shiv <linkedin/shiv#user-content-shiv>` （要求 Python 3）

   .. note:: 
      
      在这里的所有方法中，依赖预安装 Python 最依赖目标环境。当然，这也使得包的体积最小，可能只有几兆字节，甚至是几千字节。

      一般来说，减少对目标系统的依赖会增加包的大小，因此这些解决方案大致按输出大小递增的顺序排列。

.. tab:: 英文

   Pick an arbitrary computer, and depending on the context, there's a very
   good chance Python is already installed. Included by default in most
   Linux and Mac operating systems for many years now, you can reasonably
   depend on Python preexisting in your data centers or on the personal
   machines of developers and data scientists.

   Technologies which support this model:

   * :gh:`PEX <pantsbuild/pex#user-content-pex>` (Python EXecutable)
   * :doc:`zipapp <python:library/zipapp>` (does not help manage dependencies, requires Python 3.5+)
   * :gh:`shiv <linkedin/shiv#user-content-shiv>` (requires Python 3)

   .. note:: 
      
      Of all the approaches here, depending on a pre-installed
      Python relies the most on the target environment. Of course,
      this also makes for the smallest package, as small as
      single-digit megabytes, or even kilobytes.

      In general, decreasing the dependency on the target system
      increases the size of our package, so the solutions here
      are roughly arranged by increasing size of output.

.. _depending-on-a-separate-ecosystem:

取决于单独的软件发行生态系统
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Depending on a separate software distribution ecosystem**

.. tab:: 中文

   长期以来，许多操作系统，包括 Mac 和 Windows，都缺乏内置的包管理功能。直到最近，这些操作系统才获得了所谓的“应用商店”，但即便如此，这些商店也主要集中于消费者应用程序，对于开发者的支持非常有限。

   开发者长期寻求解决方案，在这场斗争中，他们推出了自己的包管理工具，如 `Homebrew <https://brew.sh/>`_。对于 Python 开发者来说，最相关的替代方案是一个叫做 `Anaconda <https://en.wikipedia.org/wiki/Anaconda_(Python_distribution)>`_ 的包生态系统。Anaconda 以 Python 为基础，已经在学术、分析和其他数据导向的环境中越来越普遍，甚至逐步进入了 `面向服务器的环境 <https://web.archive.org/web/20190403064038/https://www.paypal-engineering.com/2016/09/07/python-packaging-at-paypal/>`_。

   关于如何为 Anaconda 生态系统构建和发布的说明：

   * `使用 conda 构建库和应用程序 <https://conda.io/projects/conda-build/en/latest/user-guide/tutorials/index.html>`_
   * `将原生 Python 包迁移到 Anaconda <https://conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-pkgs-skeleton.html>`_

   另一种类似的模式是安装一个替代的 Python 发行版，但不支持操作系统级别的任意包：

   * `ActiveState ActivePython <https://www.activestate.com/products/python/>`_
   * `WinPython <http://winpython.github.io/>`_

.. tab:: 英文

   For a long time many operating systems, including Mac and Windows,
   lacked built-in package management. Only recently did these OSes gain
   so-called "app stores", but even those focus on consumer applications
   and offer little for developers.

   Developers long sought remedies, and in this struggle, emerged with
   their own package management solutions, such as `Homebrew
   <https://brew.sh/>`_. The most relevant alternative for Python
   developers is a package ecosystem called `Anaconda
   <https://en.wikipedia.org/wiki/Anaconda_(Python_distribution)>`_. Anaconda
   is built around Python and is increasingly common in academic,
   analytical, and other data-oriented environments, even making its way
   `into server-oriented environments
   <https://web.archive.org/web/20190403064038/https://www.paypal-engineering.com/2016/09/07/python-packaging-at-paypal/>`_.

   Instructions on building and publishing for the Anaconda ecosystem:

   * `Building libraries and applications with conda <https://conda.io/projects/conda-build/en/latest/user-guide/tutorials/index.html>`_
   * `Transitioning a native Python package to Anaconda <https://conda.io/projects/conda-build/en/latest/user-guide/tutorials/build-pkgs-skeleton.html>`_

   A similar model involves installing an alternative Python
   distribution, but does not support arbitrary operating system-level
   packages:

   * `ActiveState ActivePython <https://www.activestate.com/products/python/>`_
   * `WinPython <http://winpython.github.io/>`_

.. _bringing-your-own-python:

自带 Python 可执行文件
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Bringing your own Python executable**

.. tab:: 中文

   计算机的定义是能够执行程序。每个操作系统都原生支持一种或多种可以本地执行的程序格式。

   有许多技术和工具将你的 Python 程序转换为这些格式，大多数方法涉及将 Python 解释器和其他依赖项嵌入到一个单独的可执行文件中。

   这种方法被称为 *冻结(freezing)*，它提供了广泛的兼容性和无缝的用户体验，但通常需要多种技术，并且需要投入相当的精力。

   以下是一些 Python 冻结工具：

   * `pyInstaller <https://pyinstaller.readthedocs.io/en/stable/>`_ - 跨平台
   * `cx_Freeze <https://marcelotduarte.github.io/cx_Freeze/>`_ - 跨平台
   * `constructor <https://github.com/conda/constructor>`_ - 用于命令行安装程序
   * `py2exe <http://www.py2exe.org/>`_ - 仅限 Windows
   * `py2app <https://py2app.readthedocs.io/en/latest/>`_ - 仅限 Mac
   * `osnap <https://github.com/jamesabel/osnap>`_ - Windows 和 Mac
   * `pynsist <https://pypi.org/project/pynsist/>`_ - 仅限 Windows

   上述大多数工具都假设是单用户部署。对于多组件服务器应用程序，参见 :gh:`Chef Omnibus <chef/omnibus#user-content--omnibus>`。

.. tab:: 英文

   Computing as we know it is defined by the ability to execute
   programs. Every operating system natively supports one or more formats
   of programs they can natively execute.

   There are many techniques and technologies which turn your Python
   program into one of these formats, most of which involve embedding the
   Python interpreter and any other dependencies into a single executable
   file.

   This approach, called *freezing*, offers wide compatibility and
   seamless user experience, though often requires multiple technologies,
   and a good amount of effort.

   A selection of Python freezers:

   * `pyInstaller <https://pyinstaller.readthedocs.io/en/stable/>`_ - Cross-platform
   * `cx_Freeze <https://marcelotduarte.github.io/cx_Freeze/>`_ - Cross-platform
   * `constructor <https://github.com/conda/constructor>`_ - For command-line installers
   * `py2exe <http://www.py2exe.org/>`_ - Windows only
   * `py2app <https://py2app.readthedocs.io/en/latest/>`_ - Mac only
   * `osnap <https://github.com/jamesabel/osnap>`_ - Windows and Mac
   * `pynsist <https://pypi.org/project/pynsist/>`_ - Windows only

   Most of the above imply single-user deployments. For multi-component
   server applications, see :gh:`Chef Omnibus
   <chef/omnibus#user-content--omnibus>`.


自带用户空间
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Bringing your own userspace**

.. tab:: 中文

   越来越多的操作系统——包括 Linux、Mac OS 和 Windows——可以通过使用一种相对较新的方式，称为 `操作系统级虚拟化 <https://en.wikipedia.org/wiki/Operating-system-level_virtualization>`_ 或 *容器化(containerization)* ，来运行作为轻量级镜像打包的应用程序。

   这些技术大多与 Python 无关，因为它们打包的是整个操作系统文件系统，而不仅仅是 Python 或 Python 包。

   这种技术在 Linux 服务器中应用最为广泛，因为它最初源于此，并且以下技术在这里效果最佳：

   * `AppImage <https://appimage.org/>`_
   * `Docker <https://www.fullstackpython.com/docker.html>`_
   * `Flatpak <https://flatpak.org/>`_
   * `Snapcraft <https://snapcraft.io/>`_

.. tab:: 英文

   An increasing number of operating systems -- including Linux, Mac OS,
   and Windows -- can be set up to run applications packaged as
   lightweight images, using a relatively modern arrangement often
   referred to as `operating-system-level virtualization
   <https://en.wikipedia.org/wiki/Operating-system-level_virtualization>`_,
   or *containerization*.

   These techniques are mostly Python agnostic, because they package
   whole OS filesystems, not just Python or Python packages.

   Adoption is most extensive among Linux servers, where the technology
   originated and where the technologies below work best:

   * `AppImage <https://appimage.org/>`_
   * `Docker <https://www.fullstackpython.com/docker.html>`_
   * `Flatpak <https://flatpak.org/>`_
   * `Snapcraft <https://snapcraft.io/>`_

自带内核
^^^^^^^^^^^^^^^^^^^^^^^^

**Bringing your own kernel**

.. tab:: 中文

   大多数操作系统支持某种形式的经典虚拟化，运行打包为包含完整操作系统的镜像的应用程序。这些虚拟机（VM）的运行是一种成熟的方法，在数据中心环境中广泛应用。

   这些技术主要用于数据中心的大规模部署，尽管某些复杂应用程序可以从这种打包方式中受益。这些技术与 Python 无关，包括：

   * `Vagrant <https://www.vagrantup.com/>`_
   * `VHD <https://en.wikipedia.org/wiki/VHD_(file_format)>`_ 、 `AMI <https://en.wikipedia.org/wiki/Amazon_Machine_Image>`_ 和 :doc:`其他格式 <openstack:user/formats>`
   * `OpenStack <https://www.redhat.com/en/topics/openstack>`_ - 一种 Python 编写的云管理系统，具有广泛的虚拟机支持

.. tab:: 英文

   Most operating systems support some form of classical virtualization,
   running applications packaged as images containing a full operating
   system of their own. Running these virtual machines, or VMs, is a
   mature approach, widespread in data center environments.

   These techniques are mostly reserved for larger scale deployments in
   data centers, though certain complex applications can benefit from
   this packaging. The technologies are Python agnostic, and include:

   * `Vagrant <https://www.vagrantup.com/>`_
   * `VHD <https://en.wikipedia.org/wiki/VHD_(file_format)>`_, `AMI <https://en.wikipedia.org/wiki/Amazon_Machine_Image>`_, and :doc:`other formats <openstack:user/formats>`
   * `OpenStack <https://www.redhat.com/en/topics/openstack>`_ - A cloud management system in Python, with extensive VM support

自带硬件
^^^^^^^^^^^^^^^^^^^^^^^^^^

**Bringing your own hardware**

.. tab:: 中文

   将软件以已安装在硬件上的方式进行交付，是最全面的方式。通过这种方式，用户只需要电力即可使用您的软件。

   与上述主要面向技术人员的虚拟机不同，硬件设备已被广泛应用，从最先进的数据中心到最年轻的儿童都能看到其身影。

   您可以将代码嵌入到 :gh:`Adafruit <adafruit/circuitpython>`、 `MicroPython <https://micropython.org/>`_ 或其他更强大的运行 Python 的硬件中，然后将其交付到数据中心或用户家中。他们插电即用，您也可以高枕无忧。

   .. figure:: assets/py_pkg_applications.png
      :width: 80%
      :alt: 用于打包 Python 应用程序的技术概述。

      用于打包 Python 应用程序的技术的简化范畴。

.. tab:: 英文

   The most all-encompassing way to ship your software would be to ship
   it already-installed on some hardware. This way, your software's user
   would require only electricity.

   Whereas the virtual machines described above are primarily reserved
   for the tech-savvy, you can find hardware appliances being used by
   everyone from the most advanced data centers to the youngest children.

   Embed your code on an :gh:`Adafruit <adafruit/circuitpython>`,
   `MicroPython <https://micropython.org/>`_, or more-powerful hardware
   running Python, then ship it to the datacenter or your users'
   homes. They plug and play, and you can call it a day.

   .. figure:: assets/py_pkg_applications.png
      :width: 80%
      :alt: A summary of technologies used to package Python applications.

      The simplified gamut of technologies used to package Python applications.

那么...
-------------

**What about...**

.. tab:: 中文

   上面的部分只能总结这么多，您可能会对一些更明显的差距感到疑惑。

.. tab:: 英文

   The sections above can only summarize so much, and you might be
   wondering about some of the more conspicuous gaps.

操作系统包
^^^^^^^^^^^^^^^^^^^^^^^^^

**Operating system packages**

.. tab:: 中文

   如在 :ref:`depending-on-a-separate-ecosystem` 中提到的，某些操作系统拥有自己的包管理器。如果您非常确定目标操作系统，可以直接依赖像 `deb <https://en.wikipedia.org/wiki/Deb_(file_format)>`_ （用于 Debian、Ubuntu 等）或 `RPM <https://en.wikipedia.org/wiki/RPM_Package_Manager>`_ （用于 Red Hat、Fedora 等）这样的格式，并使用该内置包管理器来处理安装，甚至部署。您甚至可以使用 `FPM <https://fpm.readthedocs.io/en/latest/cli-reference.html#virtualenv>`_ 从相同的源生成 deb 和 RPM 包。

   在大多数部署流水线中，操作系统包管理器只是其中的一部分。

.. tab:: 英文

   As mentioned in :ref:`depending-on-a-separate-ecosystem` above, some operating
   systems have package managers of their own. If you're very sure of the
   operating system you're targeting, you can depend directly on a format
   like `deb <https://en.wikipedia.org/wiki/Deb_(file_format)>`_ (for
   Debian, Ubuntu, etc.) or `RPM
   <https://en.wikipedia.org/wiki/RPM_Package_Manager>`_ (for Red Hat,
   Fedora, etc.), and use that built-in package manager to take care of
   installation, and even deployment. You can even use `FPM
   <https://fpm.readthedocs.io/en/latest/cli-reference.html#virtualenv>`_ to
   generate both deb and RPMs from the same source.

   In most deployment pipelines, the OS package manager is just one piece
   of the puzzle.

虚拟环境
^^^^^^^^^^

**virtualenv**

.. tab:: 中文

   :doc:`Virtualenvs <python-guide:dev/virtualenvs>` 一直是多代 Python 开发者不可或缺的工具，但随着更高层次的工具逐渐取代它们，虚拟环境的使用逐渐淡出了视野。在包装方面，虚拟环境作为一种原始工具，通常被包装在 :doc:`dh-virtualenv 工具 <dh-virtualenv:tutorial>` 和 `osnap <https://github.com/jamesabel/osnap>`_ 中，二者都以自包含的方式包装了虚拟环境。

   对于生产部署，切勿像在开发环境中那样依赖使用 ``python -m pip install`` 从互联网安装包到虚拟环境中。上面的概述提供了许多更好的解决方案。

.. tab:: 英文

   :doc:`Virtualenvs <python-guide:dev/virtualenvs>` have
   been an indispensable tool for multiple generations of Python
   developer, but are slowly fading from view, as they are being wrapped
   by higher-level tools. With packaging in particular, virtualenvs are
   used as a primitive in :doc:`the dh-virtualenv tool
   <dh-virtualenv:tutorial>` and
   `osnap <https://github.com/jamesabel/osnap>`_, both of which wrap
   virtualenvs in a self-contained way.

   For production deployments, do not rely on running ``python -m pip install``
   from the Internet into a virtualenv, as one might do in a development
   environment. The overview above is full of much better solutions.

安全
^^^^^^^^

**Security**

.. tab:: 中文

   随着您深入了解这一梯度，更新包的组件变得越来越困难，因为所有内容都被更加紧密地绑定在一起。

   例如，如果出现内核安全问题并且您部署的是容器，那么主机系统的内核可以更新，而无需为应用程序重新构建。如果您部署的是虚拟机镜像，则需要重新构建。关于这一动态是否使某种选项更安全，仍然是一个存在争议的老问题，回溯至尚未解决的 `静态链接与动态链接的对比 <https://www.google.com/search?channel=fs&q=static+vs+dynamic+linking>`_ 。

.. tab:: 英文

   The further down the gradient you come, the harder it gets to update
   components of your package. Everything is more tightly bound together.

   For example, if a kernel security issue emerges, and you're deploying
   containers, the host system's kernel can be updated without requiring
   a new build on behalf of the application. If you deploy VM images,
   you'll need a new build. Whether or not this dynamic makes one option
   more secure is still a bit of an old debate, going back to the
   still-unsettled matter of `static versus dynamic linking
   <https://www.google.com/search?channel=fs&q=static+vs+dynamic+linking>`_.

总结
-------

**Wrap up**

.. tab:: 中文

   Python 的打包系统有时给人一种颠簸的感觉。这种印象主要是因为 Python 的多功能性而产生的副作用。一旦你理解了不同打包方案之间的自然边界，你会发现，Python 程序员为了使用这种最平衡、最灵活的语言之一，而付出的代价，实际上是非常小的。

   .. 编辑笔记: 

      在更新 Python 打包概述时，请记住以下几点：

      1. **目标受众**：本文档面向的是中级读者，处于初中级到早期高级的 Python 开发者。预计大多数阅读本文档的开发者已经遇到过一些打包技术，包括包管理器、应用商店、pip 等。他们可能甚至已经发布过一些自己的包。读者有足够的聪明才智能够构建可发布的项目，并且经历过（或感到沮丧）之后，会知道如何寻找现有的解决方案。

      2. **不涉及基础知识**：本概述侧重于简洁直达要点，因此不涉及一些基础概念（例如“什么是打包？”）。真正的初学者很少会尝试发布他们的第一行代码，而且当他们确实这样做时，通常会遵循某些文本或框架中的指引。

      3. **受众的需求**：本指南的目标受众是中级开发者或初学者打包者，他们最需要一个框架来帮助他们理清不同技术之间的差异和原因。

      4. **打包技术的多样性**：我们希望帮助读者理解，打包技术并不是互相竞争的，而是为了解决一个高度可变且常常非常严格的需求集合。“复杂而微妙”要比“任意且复杂”更好。

      5. **内容和语气**：文档旨在以百科全书的方式提供适量的背景信息。务求准确实用，但正如维基百科所说，“信息不应仅仅因为它是对的或有用的就被包括。”文章应该是关于其主题的公认知识的摘要，而不是所有可能细节的完整阐述。重点在于总结，并且提供指向更多详细实用资源的链接。

      6. **风格**：本指南借鉴了 JupyterLab 的元文档风格，该风格在写作时要求：

         - 文档应使用第二人称，指读者为“你”，而不是第一人称复数“我们”。因为文档的作者并不坐在用户旁边，所以使用“我们”可能会让用户感到沮丧，特别是在出现问题时。

         - 避免使用诸如“简单”或“仅仅”这样让人觉得任务轻松的词语。开发者觉得简单的任务，可能对于用户来说并不简单。

      7. **历史背景**: 2018 年首次发布时，本文件主要基于《打包的多层次》一文，该文可以在这里找到: http://sedimental.org/the_packaging_gradient.html

.. tab:: 英文

   Packaging in Python has a bit of a reputation for being a bumpy
   ride. This impression is mostly a byproduct of Python's
   versatility. Once you understand the natural boundaries between each
   packaging solution, you begin to realize that the varied landscape is
   a small price Python programmers pay for using one of the most
   balanced, flexible languages available.


   .. Editing notes:

      Some notes to keep in mind when updating the Python Packaging Overview:

      This document targets at an intermediate audience,
      lower-mid-level to early-advanced Python developers. It's expected
      that most developers finding this document will have already
      encountered several packaging technologies, through package
      managers, app stores, pip, and so forth. They may have even
      shipped a few packages of their own. They are smart enough to have
      built something to ship, and experienced (or frustrated) enough to
      know to search for prior art.

      In the spirit of being a succinct, "to-the-point" overview, we
      forego the basics (like, "what is packaging?"). True beginners
      rarely try to ship their very first lines of code, and when they
      do, they are often working according to a text and/or framework
      with its own directions and affordances.

      Meanwhile, the target audience of intermediate
      developers/apprentice packagers will benefit most from a framework
      that helps them sort out the differences and reasons for such a
      wide variety of technologies.

      We want to foster an understanding that packaging technologies are
      not so much competing, as they are trying to cover a
      highly-variable and often very strict set of requirements. "Complex
      and nuanced" is an improvement on "arbitrary and complicated".

      As far as content and tone, the aim is to provide a modicum of
      background information in an encyclopedic fashion. Be correct and
      practical, but as they say on Wikipedia, "Information should not be
      included ... solely because it is true or useful. [An article]
      should not be a complete exposition of all possible details, but a
      summary of accepted knowledge regarding its subject." Emphasis on
      the summary, plus ideally many links to other practical resources
      for more details.

      Finally, unlike an encyclopedia, this guide takes some style points
      from JupyterLab's metadocumentation, which at the time of writing
      says:

      - The documentation should be written in the second person,
      referring to the reader as “you” and not using the first person
      plural “we.” The author of the documentation is not sitting next to
      the user, so using “we” can lead to frustration when things don’t
      work as expected.

      - Avoid words that trivialize using JupyterLab
      such as “simply” or “just.” Tasks that developers find simple or
      easy may not be for users.

      Among other useful points. Read more here:
      https://jupyterlab.readthedocs.io/en/latest/developer/documentation.html

      At its initial publication in 2018, this document was largely based
      on "The Many Layers of Packaging" essay, here:
      http://sedimental.org/the_packaging_gradient.html
