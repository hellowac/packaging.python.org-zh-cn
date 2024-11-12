.. _distributing-packages:

===================================
打包和分发项目
===================================

**Packaging and distributing projects**

:页面状态: 过时(Outdated)
:最后查看: 2023-12-14

.. tab:: 中文

  本节介绍了使用 ``setuptools`` 配置、打包和分发 Python 项目的一些额外细节，这些内容没有包含在 :doc:`/tutorials/packaging-projects` 的入门教程中。它假定您已经熟悉 :doc:`/tutorials/installing-packages` 页面中的内容。

  本节的目标 *不是* 涵盖 Python 项目开发的最佳实践。例如，它不提供关于版本控制、文档编写或测试的指导或工具推荐。

  有关更多参考资料，请参见 :std:doc:`构建和分发包 <setuptools:userguide/index>` 中的 :ref:`setuptools` 文档，但请注意，其中的一些建议内容可能已经过时。如有冲突，请优先参考 Python 打包用户指南中的建议。

.. tab:: 英文

  This section covers some additional details on configuring, packaging and
  distributing Python projects with ``setuptools`` that aren't covered by the
  introductory tutorial in :doc:`/tutorials/packaging-projects`.  It still assumes
  that you are already familiar with the contents of the
  :doc:`/tutorials/installing-packages` page.

  The section does *not* aim to cover best practices for Python project
  development as a whole.  For example, it does not provide guidance or tool
  recommendations for version control, documentation, or testing.

  For more reference material, see :std:doc:`Building and Distributing
  Packages <setuptools:userguide/index>` in the :ref:`setuptools` docs, but note
  that some advisory content there may be outdated. In the event of
  conflicts, prefer the advice in the Python Packaging User Guide.



打包和分发的要求
===========================================

**Requirements for packaging and distributing**

.. tab:: 中文

  1. 首先，确保您已经满足了 :ref:`安装包的要求 <installing_requirements>`。
  2. 安装 "twine" [1]_:

     .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install twine

     .. tab:: Windows
        
        .. code-block:: bat
            py -m pip install twine

     您需要安装这个工具来将您的项目 :term:`分发包 <Distribution Package>` 上传到 :term:`PyPI <Python Package Index (PyPI)>` （参见 :ref:`下面的内容 <Uploading your Project to PyPI>` ）。

.. tab:: 英文

  1. First, make sure you have already fulfilled the :ref:`requirements for installing packages <installing_requirements>`.
  2. Install "twine" [1]_:

     .. tab:: Unix/macOS

        .. code-block:: bash

          python3 -m pip install twine

     .. tab:: Windows
        
        .. code-block:: bat
            py -m pip install twine

     You'll need this to upload your project :term:`distributions <Distribution Package>` to :term:`PyPI <Python Package Index (PyPI)>` (see :ref:`below <Uploading your Project to PyPI>`).


配置您的项目
========================

**Configuring your project**


初始文件
-------------

**Initial files**

setup.py
~~~~~~~~

.. tab:: 中文

  最重要的文件是 :file:`setup.py`，它位于项目目录的根目录下。示例请参见 `PyPA 示例项目 <https://github.com/pypa/sampleproject>`_ 中的 `setup.py <https://github.com/pypa/sampleproject/blob/db5806e0a3204034c51b1c00dde7d5eb3fa2532e/setup.py>`_。

  :file:`setup.py` 主要有两个功能：

  1. 它是配置项目各个方面的文件。 :file:`setup.py` 的主要特点是它包含一个全局的 ``setup()`` 函数。该函数的关键字参数定义了项目的具体细节。最相关的参数将在 :ref:`下面的部分 <setup() args>` 中进行解释。

  2. 它是一个命令行接口，用于运行与打包任务相关的各种命令。要查看可用命令的列表，请运行 ``python3 setup.py --help-commands``。

.. tab:: 英文

  The most important file is :file:`setup.py` which exists at the root of your
  project directory. For an example, see the `setup.py
  <https://github.com/pypa/sampleproject/blob/db5806e0a3204034c51b1c00dde7d5eb3fa2532e/setup.py>`_ in the `PyPA
  sample project <https://github.com/pypa/sampleproject>`_.

  :file:`setup.py` serves two primary functions:

  1. It's the file where various aspects of your project are configured. The
     primary feature of :file:`setup.py` is that it contains a global ``setup()``
     function.  The keyword arguments to this function are how specific details
     of your project are defined.  The most relevant arguments are explained in
     :ref:`the section below <setup() args>`.

  2. It's the command line interface for running various commands that
     relate to packaging tasks. To get a listing of available commands, run
     ``python3 setup.py --help-commands``.


setup.cfg
~~~~~~~~~

.. tab:: 中文

    :file:`setup.cfg` 是一个 ini 文件，包含 :file:`setup.py` 命令的选项默认值。示例请参见 `PyPA 示例项目 <https://github.com/pypa/sampleproject>`_ 中的 `setup.cfg <https://github.com/pypa/sampleproject/blob/db5806e0a3204034c51b1c00dde7d5eb3fa2532e/setup.cfg>`_。

.. tab:: 英文

  :file:`setup.cfg` is an ini file that contains option defaults for
  :file:`setup.py` commands.  For an example, see the `setup.cfg
  <https://github.com/pypa/sampleproject/blob/db5806e0a3204034c51b1c00dde7d5eb3fa2532e/setup.cfg>`_ in the `PyPA
  sample project <https://github.com/pypa/sampleproject>`_.


README.rst / README.md
~~~~~~~~~~~~~~~~~~~~~~

.. tab:: 中文

  所有项目应包含一个涵盖项目目标的自述文件。最常见的格式是 `reStructuredText <https://docutils.sourceforge.io/rst.html>`_，其扩展名为 "rst"，尽管这不是强制要求；也支持多种变体的 `Markdown <https://daringfireball.net/projects/markdown/>`_ 格式（请查看 ``setup()`` 函数的 :ref:`long_description_content_type <description>` 参数）。

  示例请参见 `PyPA 示例项目 <https://github.com/pypa/sampleproject>`_ 中的 `README.md <https://github.com/pypa/sampleproject/blob/main/README.md>`_。

  .. note:: 使用 :ref:`setuptools` 0.6.27+ 的项目默认在源分发包中包含标准的自述文件（:file:`README.rst`、:file:`README.txt` 或 :file:`README`）。内置的 :ref:`distutils` 库从 Python 3.7 开始采用这一行为。此外，:ref:`setuptools` 36.4.0+ 版本会包含找到的 :file:`README.md` 文件。如果您使用的是 setuptools，则不需要在 :file:`MANIFEST.in` 中列出自述文件，否则请显式地将其包括在内。

.. tab:: 英文

  All projects should contain a readme file that covers the goal of the project.
  The most common format is `reStructuredText
  <https://docutils.sourceforge.io/rst.html>`_ with an "rst" extension, although
  this is not a requirement; multiple variants of `Markdown
  <https://daringfireball.net/projects/markdown/>`_ are supported as well (look
  at ``setup()``'s :ref:`long_description_content_type <description>` argument).

  For an example, see `README.md
  <https://github.com/pypa/sampleproject/blob/main/README.md>`_ from the `PyPA
  sample project <https://github.com/pypa/sampleproject>`_.

  .. note:: Projects using :ref:`setuptools` 0.6.27+ have standard readme files
    (:file:`README.rst`, :file:`README.txt`, or :file:`README`) included in
    source distributions by default. The built-in :ref:`distutils` library adopts
    this behavior beginning in Python 3.7. Additionally, :ref:`setuptools`
    36.4.0+ will include a :file:`README.md` if found. If you are using
    setuptools, you don't need to list your readme file in :file:`MANIFEST.in`.
    Otherwise, include it to be explicit.

MANIFEST.in
~~~~~~~~~~~

.. tab:: 中文

  当您需要打包源分发包中未自动包含的其他文件时，您需要一个 :file:`MANIFEST.in` 文件。有关如何编写 :file:`MANIFEST.in` 文件的详细信息，包括默认包含的文件列表，请参见 ":ref:`使用 MANIFEST.in`"。

  然而，您可能不需要使用 :file:`MANIFEST.in` 文件。例如， `PyPA 示例项目 <https://github.com/pypa/sampleproject>`_ 已删除其清单文件，因为所有必要的文件都已被 :ref:`setuptools` 43.0.0 及更高版本自动包含。

  .. note:: :file:`MANIFEST.in` 不影响二进制分发包，如 wheel。

.. tab:: 英文

  A :file:`MANIFEST.in` is needed when you need to package additional files that
  are not automatically included in a source distribution.  For details on
  writing a :file:`MANIFEST.in` file, including a list of what's included by
  default, see ":ref:`Using MANIFEST.in`".

  However, you may not have to use a :file:`MANIFEST.in`. For an example, the `PyPA
  sample project <https://github.com/pypa/sampleproject>`_ has removed its manifest
  file, since all the necessary files have been included by :ref:`setuptools` 43.0.0
  and newer.

  .. note:: :file:`MANIFEST.in` does not affect binary distributions such as wheels.

LICENSE.txt
~~~~~~~~~~~

.. tab:: 中文

  每个包都应包含一个许可证文件，详细说明分发条款。在许多法域中，没有明确许可证的包不能被除版权所有者以外的任何人合法使用或分发。如果您不确定选择哪种许可证，可以使用诸如 `GitHub的选择许可证 <https://choosealicense.com/>`_ 等资源，或咨询律师。

  例如，您可以查看 `PyPA 示例项目 <https://github.com/pypa/sampleproject>`_ 中的 `LICENSE.txt <https://github.com/pypa/sampleproject/blob/main/LICENSE.txt>`_ 文件。

.. tab:: 英文

  Every package should include a license file detailing the terms of
  distribution. In many jurisdictions, packages without an explicit license can
  not be legally used or distributed by anyone other than the copyright holder.
  If you're unsure which license to choose, you can use resources such as
  `GitHub's Choose a License <https://choosealicense.com/>`_ or consult a lawyer.

  For an example, see the `LICENSE.txt
  <https://github.com/pypa/sampleproject/blob/main/LICENSE.txt>`_ from the `PyPA
  sample project <https://github.com/pypa/sampleproject>`_.

<您的包>
~~~~~~~~~~~~~~

**<your package>**

.. tab:: 中文

  尽管这不是强制要求，但最常见的做法是将您的 Python 模块和包包含在一个顶级包中，该包的名称与您的项目名称相同或非常相似。

  例如，您可以查看 `PyPA 示例项目 <https://github.com/pypa/sampleproject>`_ 中包含的 `sample <https://github.com/pypa/sampleproject/tree/main/src/sample>`_ 包。

.. tab:: 英文

  Although it's not required, the most common practice is to include your
  Python modules and packages under a single top-level package that has the same
  :ref:`name <setup() name>` as your project, or something very close.

  For an example, see the `sample
  <https://github.com/pypa/sampleproject/tree/main/src/sample>`_ package that's
  included in the `PyPA sample project <https://github.com/pypa/sampleproject>`_.


.. _`setup() args`:

setup() 参数
------------

**setup() args**

.. tab:: 中文

  如上所述， :file:`setup.py` 的主要特性是它包含一个全局的 ``setup()`` 函数。该函数的关键字参数用于定义项目的具体细节。

  其中一些参数在下面暂时解释，直到它们的信息移至其他地方。完整的列表可以在 :doc:`setuptools 文档 <setuptools:references/keywords>` 中找到。

  大部分代码片段来自于 `PyPA 示例项目 <https://github.com/pypa/sampleproject>`_ 中的 `setup.py <https://github.com/pypa/sampleproject/blob/db5806e0a3204034c51b1c00dde7d5eb3fa2532e/setup.py>`_。

  有关如何使用版本来传达兼容性信息给用户的更多信息，请参见 :ref:`选择版本控制方案 <Choosing a versioning scheme>`。

.. tab:: 英文

  As mentioned above, the primary feature of :file:`setup.py` is that it contains
  a global ``setup()`` function.  The keyword arguments to this function are how
  specific details of your project are defined.

  Some are temporarily explained below until their information is moved elsewhere.
  The full list can be found :doc:`in the setuptools documentation
  <setuptools:references/keywords>`.

  Most of the snippets given are
  taken from the `setup.py
  <https://github.com/pypa/sampleproject/blob/db5806e0a3204034c51b1c00dde7d5eb3fa2532e/setup.py>`_ contained in the
  `PyPA sample project <https://github.com/pypa/sampleproject>`_.

  See :ref:`Choosing a versioning scheme` for more information on ways to use versions to convey
  compatibility information to your users.




``packages``
~~~~~~~~~~~~

.. tab:: 中文

  ::

    packages=find_packages(include=['sample', 'sample.*']),

  将 ``packages`` 设置为项目中所有 :term:`包 <Import Package>` 的列表，包括它们的子包、子子包等。尽管可以手动列出这些包，但 ``setuptools.find_packages()`` 可以自动找到它们。使用 ``include`` 关键字参数仅查找指定的包。使用 ``exclude`` 关键字参数排除不打算发布和安装的包。

.. tab:: 英文

  ::

    packages=find_packages(include=['sample', 'sample.*']),

  Set ``packages`` to a list of all :term:`packages <Import Package>` in your
  project, including their subpackages, sub-subpackages, etc.  Although the
  packages can be listed manually, ``setuptools.find_packages()`` finds them
  automatically.  Use the ``include`` keyword argument to find only the given
  packages.  Use the ``exclude`` keyword argument to omit packages that are not
  intended to be released and installed.


``py_modules``
~~~~~~~~~~~~~~

.. tab:: 中文

  ::

      py_modules=["six"],

  如果您的项目包含任何不属于包的单文件 Python 模块，请将 ``py_modules`` 设置为模块名称的列表（不包含 ``.py`` 扩展名），以便让 :ref:`setuptools` 知道它们。

.. tab:: 英文

  ::

      py_modules=["six"],

  If your project contains any single-file Python modules that aren't part of a
  package, set ``py_modules`` to a list of the names of the modules (minus the
  ``.py`` extension) in order to make :ref:`setuptools` aware of them.


``install_requires``
~~~~~~~~~~~~~~~~~~~~

.. tab:: 中文

  ::

    install_requires=['peppercorn'],

  `install_requires` 应用于指定一个项目运行所需的最小依赖。当通过 :ref:`pip` 安装项目时，依赖项将根据此规范进行安装。

  有关使用 `install_requires` 的更多信息，请参见 :ref:`install_requires 与 Requirements 文件`。

.. tab:: 英文

  ::

    install_requires=['peppercorn'],

  "install_requires" should be used to specify what dependencies a project
  minimally needs to run. When the project is installed by :ref:`pip`, this is the
  specification that is used to install its dependencies.

  For more on using "install_requires" see :ref:`install_requires vs Requirements files`.


.. _`Package Data`:

``package_data``
~~~~~~~~~~~~~~~~

.. tab:: 中文

  ::

    package_data={
        'sample': ['package_data.dat'],
    },


  通常，额外的文件需要安装到 :term:`package <Import Package>` 中。这些文件通常是与包的实现紧密相关的数据，或是包含文档的文本文件，可能对使用该包的程序员有用。这些文件被称为 "包数据"。

  值必须是从包名到应复制到包中的相对路径名的映射。路径被解释为相对于包含该包的目录。

  有关更多信息，请参见 :std:doc:`包括数据文件 <setuptools:userguide/datafiles>`，以及 :std:doc:`setuptools 文档 <setuptools:index>`。

.. tab:: 英文

  ::

    package_data={
        'sample': ['package_data.dat'],
    },


  Often, additional files need to be installed into a :term:`package <Import
  Package>`. These files are often data that’s closely related to the package’s
  implementation, or text files containing documentation that might be of interest
  to programmers using the package. These files are called "package data".

  The value must be a mapping from package name to a list of relative path names
  that should be copied into the package. The paths are interpreted as relative to
  the directory containing the package.

  For more information, see :std:doc:`Including Data Files
  <setuptools:userguide/datafiles>` from the
  :std:doc:`setuptools docs <setuptools:index>`.


.. _`Data Files`:

``data_files``
~~~~~~~~~~~~~~

.. tab:: 中文

  ::

    data_files=[('my_data', ['data/data_file'])],

  虽然配置 :ref:`包数据` 足以满足大多数需求，但在某些情况下，您可能需要将数据文件 *放置在* :term:`包 <Import Package>` 外部。``data_files`` 指令允许您做到这一点。它主要在您需要安装的文件被其他程序使用时有用，这些程序可能不知道 Python 包的存在。

  序列中的每个 ``(directory, files)`` 对指定了安装目录和要安装到该目录的文件。 ``directory`` 必须是相对路径（尽管将来可能会改变，参见 `wheel 问题 #92 <https://github.com/pypa/wheel/issues/92>`_），并且它相对于安装前缀（对于默认安装是 Python 的 ``sys.prefix``，对于用户安装是 ``site.USER_BASE``）进行解释。 ``files`` 中的每个文件名相对于项目源分发中的 :file:`setup.py` 脚本进行解释。

  有关更多信息，请参见 distutils 部分的 :ref:`安装附加文件 <setuptools:distutils-additional-files>`。

  .. note::

    当以 egg 形式安装包时，不支持 ``data_files``。因此，如果您的项目使用 :ref:`setuptools`，您必须使用 ``pip`` 安装它。或者，如果您必须使用 ``python setup.py``，那么需要传递 ``--old-and-unmanageable`` 选项。

.. tab:: 英文

  ::

    data_files=[('my_data', ['data/data_file'])],

  Although configuring :ref:`Package Data` is sufficient for most needs, in some
  cases you may need to place data files *outside* of your :term:`packages
  <Import Package>`.  The ``data_files`` directive allows you to do that.
  It is mostly useful if you need to install files which are used by other
  programs, which may be unaware of Python packages.

  Each ``(directory, files)`` pair in the sequence specifies the installation
  directory and the files to install there. The ``directory`` must be a relative
  path (although this may change in the future, see
  `wheel Issue #92 <https://github.com/pypa/wheel/issues/92>`_),
  and it is interpreted relative to the installation prefix
  (Python’s ``sys.prefix`` for a default installation;
  ``site.USER_BASE`` for a user installation).
  Each file name in ``files`` is interpreted relative to the :file:`setup.py`
  script at the top of the project source distribution.

  For more information see the distutils section on :ref:`Installing Additional Files
  <setuptools:distutils-additional-files>`.

  .. note::

    When installing packages as egg, ``data_files`` is not supported.
    So, if your project uses :ref:`setuptools`, you must use ``pip``
    to install it. Alternatively, if you must use ``python setup.py``,
    then you need to pass the ``--old-and-unmanageable`` option.


``scripts``
~~~~~~~~~~~

.. tab:: 中文

  尽管 ``setup()`` 支持一个 :ref:`scripts <setuptools:distutils-installing-scripts>` 关键字，用于指向预先制作的脚本进行安装，但实现跨平台兼容性的推荐方法是使用 :ref:`console_scripts` 入口点（见下文）。

.. tab:: 英文

  Although ``setup()`` supports a :ref:`scripts
  <setuptools:distutils-installing-scripts>`
  keyword for pointing to pre-made scripts to install, the recommended approach to
  achieve cross-platform compatibility is to use :ref:`console_scripts` entry
  points (see below).


选择版本控制方案
----------------------------

**Choosing a versioning scheme**

.. tab:: 中文

  有关常见版本方案以及如何在它们之间进行选择的信息，请参阅 :ref:`versioning`。

.. tab:: 英文

  See :ref:`versioning` for information on common version schemes and how to
  choose between them.


在“开发模式”下工作
=============================

**Working in "development mode"**

.. tab:: 中文

  你可以在开发过程中以“可编辑”或“开发”模式安装项目。  
  当项目以可编辑模式安装时，你可以在不重新安装的情况下直接编辑项目：对安装为可编辑模式的项目中的 Python 源文件的修改将在下次启动解释器时立即反映出来。

  要以“可编辑”/“开发”模式安装 Python 包，首先进入项目根目录并运行：

  .. code-block:: bash

    python3 -m pip install -e .

  pip 命令行标志 ``-e`` 是 ``--editable`` 的简写，而 ``.`` 指当前工作目录，因此合起来表示将当前目录（即你的项目）安装为可编辑模式。这也会安装所有在 ``install_requires`` 中声明的依赖项以及在 ``console_scripts`` 中声明的任何脚本。依赖项将以通常的非可编辑模式安装。

  你可能还想以可编辑模式安装某些依赖项。例如，假设你的项目需要“foo”和“bar”，但是你希望从 VCS 中以可编辑模式安装“bar”，那么你可以构建如下的 requirements 文件::

    -e .
    -e bar @ git+https://somerepo/bar.git

  第一行表示安装你的项目及其所有依赖项。第二行覆盖了“bar”依赖项，使其从 VCS 安装，而不是从 PyPI 安装。

  然而，如果你想从本地目录以可编辑模式安装“bar”，则 requirements 文件应该如下所示，并将本地路径放在文件的顶部::

    -e /path/to/project/bar
    -e .

  否则，由于 requirements 文件的安装顺序，依赖项将从 PyPI 安装。有关 requirements 文件的更多信息，请参阅 pip 文档中的 :ref:`Requirements File <pip:Requirements Files>` 部分。有关 VCS 安装的更多信息，请参阅 pip 文档中的 :ref:`VCS Support <pip:VCS Support>` 部分。

  最后，如果你不想安装任何依赖项，可以运行：

  .. code-block:: bash

    python3 -m pip install -e . --no-deps

  有关更多信息，请参见 :doc:`Development Mode <setuptools:userguide/development_mode>` 部分，在 :ref:`setuptools` 文档中。

.. tab:: 英文

  You can install a project in "editable"
  or "develop" mode while you're working on it.
  When installed as editable, a project can be
  edited in-place without reinstallation:
  changes to Python source files in projects installed as editable will be reflected the next time an interpreter process is started.

  To install a Python package in "editable"/"development" mode
  Change directory to the root of the project directory and run:

  .. code-block:: bash

    python3 -m pip install -e .


  The pip command-line flag ``-e`` is short for ``--editable``, and ``.`` refers
  to the current working directory, so together, it means to install the current
  directory (i.e. your project) in editable mode.  This will also install any
  dependencies declared with ``install_requires`` and any scripts declared with
  ``console_scripts``.  Dependencies will be installed in the usual, non-editable
  mode.

  You may want to install some of your dependencies in editable
  mode as well. For example, supposing your project requires "foo" and "bar", but
  you want "bar" installed from VCS in editable mode, then you could construct a
  requirements file like so::

    -e .
    -e bar @ git+https://somerepo/bar.git

  The first line says to install your project and any dependencies. The second
  line overrides the "bar" dependency, such that it's fulfilled from VCS, not
  PyPI.

  If, however, you want "bar" installed from a local directory in editable mode, the requirements file should look like this, with the local paths at the top of the file::

    -e /path/to/project/bar
    -e .

  Otherwise, the dependency will be fulfilled from PyPI, due to the installation order of the requirements file.  For more on requirements files, see the :ref:`Requirements File
  <pip:Requirements Files>` section in the pip docs.  For more on VCS installs,
  see the :ref:`VCS Support <pip:VCS Support>` section of the pip docs.

  Lastly, if you don't want to install any dependencies at all, you can run:

  .. code-block:: bash

    python3 -m pip install -e . --no-deps


  For more information, see the
  :doc:`Development Mode <setuptools:userguide/development_mode>` section
  of the :ref:`setuptools` docs.

.. _`Packaging your project`:

打包您的项目
======================

**Packaging your project**

.. tab:: 中文

  要使你的项目可以从像 PyPI 这样的 :term:`包索引 <Package Index>` （Package Index）进行安装，你需要为你的项目创建一个分发 :term:`包 <Distribution Package>` （Distribution Package）。

  在你构建项目的 wheels 和 sdists 之前，需要先安装 `build` 包：

  .. tab:: Unix/macOS

      .. code-block:: bash

          python3 -m pip install build

  .. tab:: Windows

      .. code-block:: bat

          py -m pip install build  

.. tab:: 英文

  To have your project installable from a :term:`Package Index` like :term:`PyPI
  <Python Package Index (PyPI)>`, you'll need to create a :term:`Distribution
  <Distribution Package>` (aka ":term:`Package <Distribution Package>`") for your
  project.

  Before you can build wheels and sdists for your project, you'll need to install the
  ``build`` package:

  .. tab:: Unix/macOS

      .. code-block:: bash

          python3 -m pip install build

  .. tab:: Windows

      .. code-block:: bat

          py -m pip install build


源代码分发
--------------------

**Source distributions**

.. tab:: 中文

  至少，你应该创建一个 :term:`源分发包 <Source Distribution (or "sdist")>` （Source Distribution，简称 "sdist"）：

  .. tab:: Unix/macOS

      .. code-block:: bash

          python3 -m build --sdist

  .. tab:: Windows

      .. code-block:: bat

          py -m build --sdist


  “源分发包”是未构建的（即它不是 **已构建分发包**），并且在通过 pip 安装时需要进行构建步骤。即使该分发包是纯 Python（即不包含扩展），它仍然需要一个构建步骤来从 `setup.py` 和/或 `setup.cfg` 构建安装元数据。

.. tab:: 英文

  Minimally, you should create a :term:`Source Distribution <Source Distribution (or "sdist")>`:

  .. tab:: Unix/macOS

      .. code-block:: bash

          python3 -m build --sdist

  .. tab:: Windows

      .. code-block:: bat

          py -m build --sdist


  A "source distribution" is unbuilt (i.e. it's not a :term:`Built
  Distribution`), and requires a build step when installed by pip.  Even if the
  distribution is pure Python (i.e. contains no extensions), it still involves a
  build step to build out the installation metadata from :file:`setup.py` and/or
  :file:`setup.cfg`.


Wheels
------

**Wheels**

.. tab:: 中文

  你还应该为你的项目创建一个 **wheel** （Wheel 包）。Wheel 是一种 **已构建包** （Built Distribution），可以在无需经过“构建”过程的情况下直接安装。与从源分发包安装相比，安装 Wheel 包对于最终用户来说要快得多。

  如果你的项目是纯 Python 项目，那么你将创建一个 :ref:`纯 Python Wheel <Pure Python Wheels>` （参见下文部分）。

  如果你的项目包含编译扩展，那么你将创建一个所谓的 :ref:`平台 Wheel <Platform Wheels>` （参见下文部分）。

  .. note:: 
    
    如果你的项目同时支持 Python 2 且 **不包含** C 扩展，那么你应该通过在 :file:`setup.cfg` 文件中添加以下内容来创建所谓的 **通用 Wheel**：

    .. code-block:: text

      [bdist_wheel]
      universal=1

    只有在你的项目没有任何 C 扩展且支持 Python 2 和 3 时，才使用此设置。

.. tab:: 英文

  You should also create a wheel for your project. A wheel is a :term:`built
  package <Built Distribution>` that can be installed without needing to go
  through the "build" process. Installing wheels is substantially faster for the
  end user than installing from a source distribution.

  If your project is pure Python then you'll be creating a
  :ref:`"Pure Python Wheel" (see section below) <Pure Python Wheels>`.

  If your project contains compiled extensions, then you'll be creating what's
  called a :ref:`*Platform Wheel* (see section below) <Platform Wheels>`.

  .. note:: If your project also supports Python 2 *and* contains no C extensions,
    then you should create what's called a *Universal Wheel* by adding the
    following to your :file:`setup.cfg` file:

    .. code-block:: text

      [bdist_wheel]
      universal=1

    Only use this setting if your project does not have any C extensions *and*
    supports Python 2 and 3.


.. _`Pure Python Wheels`:

纯 Python Wheels
~~~~~~~~~~~~~~~~~~

**Pure Python Wheels**

.. tab:: 中文

    

.. tab:: 英文

*Pure Python Wheels* contain no compiled extensions, and therefore only require a
single Python wheel.

To build the wheel:

.. tab:: Unix/macOS

    .. code-block:: bash

        python3 -m build --wheel

.. tab:: Windows

    .. code-block:: bat

        py -m build --wheel

The ``wheel`` package will detect that the code is pure Python, and build a
wheel that's named such that it's usable on any Python 3 installation.  For
details on the naming of wheel files, see :pep:`425`.

If you run ``build`` without ``--wheel`` or ``--sdist``, it will build both
files for you; this is useful when you don't need multiple wheels.

.. _`Platform Wheels`:

平台 Wheels
~~~~~~~~~~~~~~~

**Platform Wheels**

.. tab:: 中文

    

.. tab:: 英文

*Platform Wheels* are wheels that are specific to a certain platform like Linux,
macOS, or Windows, usually due to containing compiled extensions.

To build the wheel:

.. tab:: Unix/macOS

    .. code-block:: bash

        python3 -m build --wheel

.. tab:: Windows

    .. code-block:: bat

        py -m build --wheel


The ``wheel`` package will detect that the code is not pure Python, and build
a wheel that's named such that it's only usable on the platform that it was
built on. For details on the naming of wheel files, see :pep:`425`.

.. note::

  :term:`PyPI <Python Package Index (PyPI)>` currently supports uploads of
  platform wheels for Windows, macOS, and the multi-distro ``manylinux*`` ABI.
  Details of the latter are defined in :pep:`513`.


.. _`Uploading your Project to PyPI`:

将您的项目上传到 PyPI
==============================

**Uploading your Project to PyPI**

.. tab:: 中文

    

.. tab:: 英文

When you ran the command to create your distribution, a new directory ``dist/``
was created under your project's root directory. That's where you'll find your
distribution file(s) to upload.

.. note:: These files are only created when you run the command to create your
  distribution. This means that any time you change the source of your project
  or the configuration in your :file:`setup.py` file, you will need to rebuild
  these files again before you can distribute the changes to PyPI.

.. note:: Before releasing on main PyPI repo, you might prefer
  training with the `PyPI test site <https://test.pypi.org/>`_ which
  is cleaned on a semi regular basis. See :ref:`using-test-pypi` on
  how to setup your configuration in order to use it.

.. warning:: In other resources you may encounter references to using
  ``python setup.py register`` and ``python setup.py upload``. These methods
  of registering and uploading a package are **strongly discouraged** as it may
  use a plaintext HTTP or unverified HTTPS connection on some Python versions,
  allowing your username and password to be intercepted during transmission.

.. tip:: The reStructuredText parser used on PyPI is **not** Sphinx!
  Furthermore, to ensure safety of all users, certain kinds of URLs and
  directives are forbidden or stripped out (e.g., the ``.. raw::``
  directive). **Before** trying to upload your distribution, you should check
  to see if your brief / long descriptions provided in :file:`setup.py` are
  valid.  You can do this by running :std:doc:`twine check <index>` on
  your package files:

  .. code-block:: bash

     twine check dist/*

创建帐户
-----------------

**Create an account**

.. tab:: 中文

    

.. tab:: 英文

First, you need a :term:`PyPI <Python Package Index (PyPI)>` user account. You
can create an account
`using the form on the PyPI website <https://pypi.org/account/register/>`_.

Now you'll create a PyPI `API token`_ so you will be able to securely upload
your project.

Go to https://pypi.org/manage/account/#api-tokens and create a new
`API token`_; don't limit its scope to a particular project, since you
are creating a new project.

**Don't close the page until you have copied and saved the token — you
won't see that token again.**

.. Note:: To avoid having to copy and paste the token every time you
  upload, you can create a :file:`$HOME/.pypirc` file:

  .. code-block:: text

    [pypi]
    username = __token__
    password = <the token value, including the `pypi-` prefix>

  **Be aware that this stores your token in plaintext.**

  For more details, see the :ref:`specification <pypirc>` for :file:`.pypirc`.

.. _register-your-project:
.. _API token: https://pypi.org/help/#apitoken

上传您的分发
-------------------------

**Upload your distributions**

.. tab:: 中文

    

.. tab:: 英文

Once you have an account you can upload your distributions to
:term:`PyPI <Python Package Index (PyPI)>` using :ref:`twine`.

The process for uploading a release is the same regardless of whether
or not the project already exists on PyPI - if it doesn't exist yet,
it will be automatically created when the first release is uploaded.

For the second and subsequent releases, PyPI only requires that the
version number of the new release differ from any previous releases.

.. code-block:: bash

    twine upload dist/*

You can see if your package has successfully uploaded by navigating to the URL
``https://pypi.org/project/<sampleproject>`` where ``sampleproject`` is
the name of your project that you uploaded. It may take a minute or two for
your project to appear on the site.

----

.. [1] Depending on your platform, this may require root or Administrator
       access. :ref:`pip` is currently considering changing this by `making user
       installs the default behavior
       <https://github.com/pypa/pip/issues/1668>`_.
