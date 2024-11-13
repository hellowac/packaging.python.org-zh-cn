.. _`Tool Recommendations`:

====================
工具推荐
====================

**Tool recommendations**

.. tab:: 中文

  Python 打包生态系统包含了许多不同的工具。对于许多任务，:term:`Python Packaging Authority <Python Packaging Authority (PyPA)>` （PyPA，负责维护许多打包工具并维护本指南的工作组）故意没有做出统一的推荐；例如，之所以有多个构建后端，是因为生态系统已经开放，以便开发出能更好地满足某些用户需求的新后端，而不是仅仅依赖以前唯一的后端 setuptools。本指南确实指向了一些广泛认可的工具，并且也提出了一些不应使用的工具推荐，因为它们已经被弃用或存在安全隐患。

.. tab:: 英文

  The Python packaging landscape consists of many different tools. For many tasks,
  the :term:`Python Packaging Authority <Python Packaging Authority (PyPA)>`
  (PyPA, the working group which encompasses many packaging tools and
  maintains this guide) purposefully does not make a blanket recommendation; for
  example, the reason there are many build backends is that the landscape was
  opened up in order to enable the development of new backends serving certain users'
  needs better than the previously unique backend, setuptools. This guide does
  point to some tools that are widely recognized, and also makes some
  recommendations of tools that you should *not* use because they are deprecated
  or insecure.


虚拟环境
====================

**Virtual environments**

.. tab:: 中文

  创建和手动使用虚拟环境的标准工具是 :ref:`virtualenv` （PyPA 项目）和 :doc:`venv <python:library/venv>` （Python 标准库的一部分，但缺少一些 virtualenv 的功能）。

.. tab:: 英文

  The standard tools to create and use virtual environments manually are
  :ref:`virtualenv` (PyPA project) and :doc:`venv <python:library/venv>` (part of
  the Python standard library, though missing some features of virtualenv).


安装包
===================

**Installing packages**

.. tab:: 中文

  :ref:`Pip` 是从 :term:`PyPI <Python Package Index (PyPI)>` 安装包的标准工具。你可能需要阅读 pip 对 :doc:`安全安装 <pip:topics/secure-installs>` 的推荐。pip 默认包含在大多数 Python 安装中，通过标准库包 :doc:`ensurepip <python:library/ensurepip>` 提供。

  另外，可以考虑 :ref:`pipx` 来安装通过 PyPI 分发并通过命令行运行的 Python 应用程序。Pipx 是一个包装工具，结合了 pip 和 venv，会将每个应用程序安装到一个独立的虚拟环境中。这避免了不同应用程序之间的依赖冲突，也避免了系统范围内的应用程序使用相同的 Python 解释器时产生冲突（尤其在 Linux 上）。

  对于科学软件，考虑使用 :ref:`Conda` 或 :ref:`Spack`。

  .. todo:: 在这里或在新讨论中写一个 "pip 与 Conda" 的对比。

  **不要** 使用 ``easy_install`` （是 :ref:`setuptools` 的一部分），因为它已经被弃用，建议使用 pip（有关详情，请参见 :ref:`pip vs easy_install`）。同样，**不要** 使用 ``python setup.py install`` 或 ``python setup.py develop``，它们也已被弃用（有关背景，请参见 :ref:`setup-py-deprecated`，迁移建议见 :ref:`modernize-setup-py-project`）。

.. tab:: 英文

  :ref:`Pip` is the standard tool to install packages from :term:`PyPI <Python
  Package Index (PyPI)>`. You may want to read pip's recommendations for
  :doc:`secure installs <pip:topics/secure-installs>`. Pip is available by default
  in most Python installations through the standard library package
  :doc:`ensurepip <python:library/ensurepip>`.

  Alternatively, consider :ref:`pipx` for the specific use case of installing Python
  applications that are distributed through PyPI and run from the command line.
  Pipx is a wrapper around pip and venv that installs each
  application into a dedicated virtual environment. This avoids conflicts between
  the dependencies of different applications, and also with system-wide applications
  making use of the same Python interpreter (especially on Linux).

  For scientific software specifically, consider :ref:`Conda` or :ref:`Spack`.

  .. todo:: Write a "pip vs. Conda" comparison, here or in a new discussion.

  Do **not** use ``easy_install`` (part of :ref:`setuptools`), which is deprecated
  in favor of pip (see :ref:`pip vs easy_install` for details). Likewise, do
  **not** use ``python setup.py install`` or ``python setup.py develop``, which
  are also deprecated (see :ref:`setup-py-deprecated` for background and
  :ref:`modernize-setup-py-project` for migration advice).


锁定文件
==========

**Lock files**

.. tab:: 中文

  :ref:`pip-tools` 和 :ref:`Pipenv` 是两个公认的工具，用于创建锁定文件，这些文件包含安装到环境中的所有包的确切版本，以确保可重现性。

.. tab:: 英文

  :ref:`pip-tools` and :ref:`Pipenv` are two recognized tools to create lock
  files, which contain the exact versions of all packages installed into an
  environment, for reproducibility purposes.


构建后端
==============

**Build backends**

.. tab:: 中文

  .. important::

     请记住：本文档并不旨在引导读者选择特定工具，而是列举常见的工具。不同的使用场景通常需要专门化的工作流。

  常见的纯 Python 包的 :term:`构建后端 <build backend>` 包括，按字母顺序排列：

  - :doc:`Flit-core <flit:pyproject_toml>` — 与 :ref:`Flit` 一同开发，但独立于它。一个简约且有明确观点的构建后端。不支持插件。

  - Hatchling_ — 与 :ref:`Hatch` 一同开发，但独立于它。支持插件。

  - PDM-backend_ — 与 :ref:`PDM` 一同开发，但独立于它。支持插件。

  - Poetry-core_ — 与 :ref:`Poetry` 一同开发，但独立于它。支持插件。

    与列表中的其他后端不同，Poetry-core 不支持标准的 :ref:`[project] 表 <writing-pyproject-toml>` （它使用不同的格式，在 ``[tool.poetry]`` 表中）。

  - :ref:`setuptools`，曾经是唯一的构建后端。支持插件。

    .. 警告::

      如果使用 setuptools，请注意，一些早于标准化工作引入的功能现在已被弃用，仅仅是*暂时保留*以确保兼容性。

      特别是， **不要** 使用直接的 ``python setup.py`` 调用。另一方面，使用 :file:`setup.py` 文件配置 setuptools 仍然完全支持，但建议尽可能使用现代的 :ref:`[project] 表
      在 pyproject.toml <writing-pyproject-toml>` （或 :file:`setup.cfg`）中配置，并仅在需要编程配置时保留 :file:`setup.py`。请参阅 :ref:`setup-py-deprecated`。

      其他弃用的功能示例，您**不应**使用，包括 ``setup_requires`` 参数（请使用 :ref:`[build-system] 表
      <pyproject-guide-build-system-table>` 在 :file:`pyproject.toml` 中替代）以及 ``easy_install`` 命令（参见 :ref:`pip vs easy_install`）。

  **不要** 使用 :ref:`distutils`，它已被弃用，并且从 Python 3.12 开始已从标准库中移除，尽管它仍然可以通过 setuptools 使用。

  对于包含 :term:`扩展模块 <extension module>` 的包，最好使用专门支持扩展语言的构建系统，例如：

  - :ref:`setuptools` — 原生支持 C 和 C++（通过第三方插件支持 Go 和 Rust），
  - :ref:`meson-python` — 支持 C、C++、Fortran、Rust 以及其他 Meson 支持的语言，
  - :ref:`scikit-build-core` — 支持 C、C++、Fortran 以及其他 CMake 支持的语言，
  - :ref:`maturin` — 支持 Rust，通过 Cargo。

.. tab:: 英文

  .. important::

     Please, remember: this document does not seek to steer the reader towards
     a particular tool, only to enumerate common tools. Different use cases often
     need specialized workflows.

  Popular :term:`build backends <build backend>` for pure-Python packages include,
  in alphabetical order:

  - :doc:`Flit-core <flit:pyproject_toml>` -- developed with but separate from :ref:`Flit`.
    A minimal and opinionated build backend. It does not support plugins.

  - Hatchling_ -- developed with but separate from :ref:`Hatch`. Supports plugins.

  - PDM-backend_ -- developed with but separate from :ref:`PDM`. Supports plugins.

  - Poetry-core_ -- developed with but separate from :ref:`Poetry`. Supports
    plugins.

    Unlike other backends on this list, Poetry-core does not support the standard
    :ref:`[project] table <writing-pyproject-toml>` (it uses a different format,
    in the ``[tool.poetry]`` table).

  - :ref:`setuptools`, which used to be the only build backend. Supports plugins.

    .. caution::

      If you use setuptools, please be aware that some features that predate
      standardisation efforts are now deprecated and only *temporarily kept*
      for compatibility.

      In particular, do **not** use direct ``python setup.py`` invocations. On the
      other hand, configuring setuptools with a :file:`setup.py` file is still fully
      supported, although it is recommended to use the modern :ref:`[project] table
      in pyproject.toml <writing-pyproject-toml>` (or :file:`setup.cfg`) whenever possible and keep
      :file:`setup.py` only if programmatic configuration is needed. See
      :ref:`setup-py-deprecated`.

      Other examples of deprecated features you should **not** use include the
      ``setup_requires`` argument to ``setup()`` (use the :ref:`[build-system] table
      <pyproject-guide-build-system-table>` in :file:`pyproject.toml` instead), and
      the ``easy_install`` command (cf. :ref:`pip vs easy_install`).

  Do **not** use :ref:`distutils`, which is deprecated, and has been removed from
  the standard library in Python 3.12, although it still remains available from
  setuptools.

  For packages with :term:`extension modules <extension module>`, it is best to use
  a build system with dedicated support for the language the extension is written in,
  for example:

  - :ref:`setuptools` -- natively supports C and C++ (with third-party plugins for Go and Rust),
  - :ref:`meson-python` -- C, C++, Fortran, Rust, and other languages supported by Meson,
  - :ref:`scikit-build-core` -- C, C++, Fortran, and other languages supported by CMake,
  - :ref:`maturin` -- Rust, via Cargo.


构建发行版
======================

**Building distributions**

.. tab:: 中文

  构建 :term:`源代码分发 <source distribution (or "sdist")>` 和 :term:`wheel <wheel>` 以便上传到 PyPI 的标准工具是 :ref:`build`。它会调用您在 :file:`pyproject.toml` 中声明的构建后端。

  **不要** 使用 ``python setup.py sdist`` 和 ``python setup.py bdist_wheel`` 来执行此任务。所有直接调用 :file:`setup.py` 的操作都是 :ref:`弃用的 <setup-py-deprecated>`。

  如果您有 :term:`扩展模块 <extension module>` 并且希望为多个平台分发 wheel 文件，请使用 :ref:`cibuildwheel` 作为 CI 设置的一部分来构建可分发的 wheel 文件。

.. tab:: 英文

  The standard tool to build :term:`source distributions <source distribution (or
  "sdist")>` and :term:`wheels <wheel>` for uploading to PyPI is :ref:`build`.  It
  will invoke whichever build backend you :ref:`declared
  <pyproject-guide-build-system-table>` in :file:`pyproject.toml`.

  Do **not** use ``python setup.py sdist`` and ``python setup.py bdist_wheel`` for
  this task. All direct invocations of :file:`setup.py` are :ref:`deprecated
  <setup-py-deprecated>`.

  If you have :term:`extension modules <extension module>` and want to distribute
  wheels for multiple platforms, use :ref:`cibuildwheel` as part of your CI setup
  to build distributable wheels.


上传至 PyPI
=================

**Uploading to PyPI**

.. tab:: 中文

  对于托管在 GitHub 上的项目，建议使用 :ref:`受信发布 <trusted-publishing>`，该方法允许从 GitHub Actions 作业中安全地将包上传到 PyPI。（目前，除 GitHub 外的其他软件托管平台不支持此功能。）

  另一种可用的方法是使用 :ref:`twine` 手动上传包。

  **绝对不要** 使用 ``python setup.py upload`` 来执行此任务。除了被 :ref:`弃用 <setup-py-deprecated>` 外，它还不安全。

.. tab:: 英文

  For projects hosted on GitHub, it is recommended to use the :ref:`trusted publishing
  <trusted-publishing>`, which allows the package to be securely uploaded to PyPI
  from a GitHub Actions job. (This is not yet supported on software forges other
  than GitHub.)

  The other available method is to upload the package manually using :ref:`twine`.

  **Never** use ``python setup.py upload`` for this task. In addition to being
  :ref:`deprecated <setup-py-deprecated>`, it is insecure.


工作流工具
==============

**Workflow tools**

.. tab:: 中文

  这些工具是环境管理器，能够自动管理项目的虚拟环境。它们还充当“任务运行器”，允许你定义和调用任务，如运行测试、编译文档、重新生成某些文件等。一些工具提供了构建分发包和上传到 PyPI 的快捷方式，还有一些支持应用程序的锁文件。它们通常在幕后调用上面提到的工具。按字母顺序排列：

  - :ref:`Flit`，
  - :ref:`Hatch`，
  - :doc:`nox <nox:index>`，
  - :ref:`PDM`，
  - :ref:`Pipenv`，
  - :ref:`Poetry`，
  - :doc:`tox <tox:index>`。

.. tab:: 英文

  These tools are environment managers that automatically manage virtual
  environments for a project. They also act as "task runners", allowing you to
  define and invoke tasks such as running tests, compiling documentation,
  regenerating some files, etc. Some of them provide shortcuts for building
  distributions and uploading to PyPI, and some support lock files for
  applications. They often call the tools mentioned above under the hood. In
  alphabetical order:

  - :ref:`Flit`,
  - :ref:`Hatch`,
  - :doc:`nox <nox:index>`,
  - :ref:`PDM`,
  - :ref:`Pipenv`,
  - :ref:`Poetry`,
  - :doc:`tox <tox:index>`.


.. _hatchling: https://pypi.org/project/hatchling/
.. _pdm-backend: https://backend.pdm-project.org
.. _poetry-core: https://pypi.org/project/poetry-core/
