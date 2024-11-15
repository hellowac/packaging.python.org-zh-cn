.. _distribution-package-vs-import-package:

=======================================
分发包与导入包
=======================================

**Distribution package vs. import package**

.. tab:: 中文

   “包”这个词通常指代多个不同的概念。本页面旨在澄清 Python 打包中两个不同但相关的含义，即“发行包”（distribution package）和“导入包”（import package）之间的区别。

.. tab:: 英文

   A number of different concepts are commonly referred to by the word
   "package". This page clarifies the differences between two distinct but
   related meanings in Python packaging, "distribution package" and "import
   package".

什么是分发包？
==============================

**What's a distribution package?**

.. tab:: 中文

   发行包（distribution package）是指一个可以安装的软件包。大多数情况下，这与“项目”（project）是同义的。当你输入 ``pip install pkg``，或者在 ``pyproject.toml`` 文件中写入 ``dependencies = ["pkg"]`` 时， ``pkg`` 就是发行包的名称。当你在 PyPI 上搜索或浏览时，看到的通常是一个发行包的列表， PyPI_ 是最广为人知的用于安装 Python 库和工具的集中源。另一方面，术语“发行包”有时也可以用来指代包含某个项目特定版本的文件。

   需要注意的是，在 Linux 环境中，“发行包”（distribution package）通常被简称为“distro 包”或“包”，是由 `Linux 发行版 <distro_>`_ 的系统包管理器提供的，这与 Python 打包中的定义是不同的。

.. tab:: 英文

   A distribution package is a piece of software that you can install.
   Most of the time, this is synonymous with "project". When you type ``pip
   install pkg``, or when you write ``dependencies = ["pkg"]`` in your
   ``pyproject.toml``, ``pkg`` is the name of a distribution package. When
   you search or browse the PyPI_, the most widely known centralized source for
   installing Python libraries and tools, what you see is a list of distribution
   packages. Alternatively, the term "distribution package" can be used to
   refer to a specific file that contains a certain version of a project.

   Note that in the Linux world, a "distribution package",
   most commonly abbreviated as "distro package" or just "package",
   is something provided by the system package manager of the `Linux distribution <distro_>`_,
   which is a different meaning.


什么是导入包？
=========================

**What's an import package?**

.. tab:: 中文

   导入包（import package）是一个 Python 模块。因此，当你在 Python 代码中写 ``import pkg`` 或 ``from pkg import func`` 时， ``pkg`` 就是一个导入包的名称。更准确地说，导入包是可以包含子模块的特殊 Python 模块。例如， ``numpy`` 包包含了像 ``numpy.linalg`` 和 ``numpy.fft`` 这样的模块。通常，导入包是文件系统中的一个目录，其中包含 ``.py`` 文件作为模块，并且包含子目录作为子包（subpackage）。

   只要你安装了提供该包的发行包，就可以使用该导入包。

.. tab:: 英文

   An import package is a Python module. Thus, when you write ``import
   pkg`` or ``from pkg import func`` in your Python code, ``pkg`` is the
   name of an import package. More precisely, import packages are special
   Python modules that can contain submodules. For example, the ``numpy``
   package contains modules like ``numpy.linalg`` and
   ``numpy.fft``. Usually, an import package is a directory on the file
   system, containing modules as ``.py`` files and subpackages as
   subdirectories.

   You can use an import package as soon as you have installed a distribution
   package that provides it.


分发包和导入包之间有什么联系？
=====================================================================

**What are the links between distribution packages and import packages?**

.. tab:: 中文

   大多数情况下，一个发行包提供一个单一的导入包（或非包模块），且两者的名称是相匹配的。例如，执行 `pip install numpy` 后，你可以通过 `import numpy` 来导入该包。

   然而，这仅仅是一种约定。PyPI 和其他包索引 *并不强制* 发行包的名称与它所提供的导入包之间存在任何关系。（这意味着你不能盲目地安装 PyPI 包 `foo`，即使你在代码中看到 `import foo`；这可能会安装一个意外的，甚至是恶意的包。）

   一个发行包也可以提供一个与其名称不同的导入包。一个例子是流行的图像处理库 Pillow。它的发行包名称是 `Pillow`，但它提供的导入包是 `PIL`。这是出于历史原因：Pillow 最初是 PIL 库的一个分支，因此保留了导入名称 `PIL`，以便现有的 PIL 用户可以轻松切换到 Pillow。更一般来说，现有库的分支是导致发行包和导入包名称不同的常见原因。

   在一个给定的包索引（如 PyPI）上，发行包的名称必须是唯一的。另一方面，导入包没有这样的要求。多个发行包可以提供相同名称的导入包。再次强调，分支是造成这种情况的常见原因。

   反过来，一个发行包也可以提供多个导入包，尽管这种情况较少见。例如，`attrs` 发行包同时提供了一个带有新 API 的 `attrs` 导入包和一个较旧但仍被支持的 `attr` 导入包。

.. tab:: 英文

   Most of the time, a distribution package provides one single import
   package (or non-package module), with a matching name. For example,
   ``pip install numpy`` lets you ``import numpy``.

   However, this is only a convention. PyPI and other package indices *do not
   enforce any relationship* between the name of a distribution package and the
   import packages it provides. (A consequence of this is that you cannot blindly
   install the PyPI package ``foo`` if you see ``import foo``; this may install an
   unintended, and potentially even malicious package.)

   A distribution package could provide an import package with a different
   name. An example of this is the popular Pillow_ library for image
   processing. Its distribution package name is ``Pillow``, but it provides
   the import package ``PIL``. This is for historical reasons: Pillow
   started as a fork of the PIL library, thus it kept the import name
   ``PIL`` so that existing PIL users could switch to Pillow with little
   effort. More generally, a fork of an existing library is a common reason
   for differing names between the distribution package and the import
   package.

   On a given package index (like PyPI), distribution package names must be
   unique. On the other hand, import packages have no such requirement.
   Import packages with the same name can be provided by several
   distribution packages. Again, forks are a common reason for this.

   Conversely, a distribution package can provide several import packages,
   although this is less common. An example is the attrs_ distribution
   package, which provides both an ``attrs`` import package with a newer
   API, and an ``attr`` import package with an older but supported API.


分发包名称和导入包名称如何比较？
===================================================================

**How do distribution package names and import package names compare?**

.. tab:: 中文

   导入包的名称应该是有效的 Python 标识符（详细规则可以参见 Python 文档中的 :ref:`exact rules <python:identifiers>`）[#non-identifier-mod-name1]_ 。特别地，它们使用下划线 ``_`` 作为单词分隔符，并且名称是区分大小写的。

   另一方面，发行包的名称可以使用连字符 ``-`` 或下划线 ``_``。它们还可以包含点号 ``.``，有时用于打包一个 :ref:`namespace package <packaging-namespace-packages>` 的子包。对于大多数用途，发行包名称对大小写和 ``-`` 与 ``_`` 的差异不敏感，例如，``pip install Awesome_Package`` 和 ``pip install awesome-package`` 是等效的（具体规则见 :ref:`name normalization specification <name-normalization>`）。

.. tab:: 英文

   Import packages should have valid Python identifiers as their name (the
   :ref:`exact rules <python:identifiers>` are found in the Python
   documentation) [#non-identifier-mod-name2]_. In particular, they use underscores ``_`` as word
   separator and they are case-sensitive.

   On the other hand, distribution packages can use hyphens ``-`` or
   underscores ``_``. They can also contain dots ``.``, which is sometimes
   used for packaging a subpackage of a :ref:`namespace package
   <packaging-namespace-packages>`. For most purposes, they are insensitive
   to case and to ``-`` vs.  ``_`` differences, e.g., ``pip install
   Awesome_Package`` is the same as ``pip install awesome-package`` (the
   precise rules are given in the :ref:`name normalization specification
   <name-normalization>`).



---------------------------

.. [#non-identifier-mod-name1] 虽然从技术上讲，可以使用 :doc:`importlib <python:library/importlib>` 导入没有有效 Python 标识符名称的包/模块，但这种情况极为罕见，并且强烈不推荐这样做。

.. [#non-identifier-mod-name2] Although it is technically possible to import packages/modules that do not have a valid Python identifier as their name, using :doc:`importlib <python:library/importlib>`, this is vanishingly rare and strongly discouraged.


.. _distro: https://en.wikipedia.org/wiki/Linux_distribution
.. _PyPI: https://pypi.org
.. _Pillow: https://pypi.org/project/Pillow
.. _attrs: https://pypi.org/project/attrs
