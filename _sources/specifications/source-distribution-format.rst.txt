
.. _source-distribution-format:

==========================
源分发格式
==========================

**Source distribution format**

.. tab:: 中文

  当前的标准源代码分发格式通过分发档案中存在的 :file:`pyproject.toml` 文件来标识。 这种分发的布局最初在 :pep:`517` 中指定，并在此处正式记录。

  还有传统的源代码分发格式，它是由标准库中的 ``distutils`` 模块在执行 :command:`setup.py sdist` 时的行为隐式定义的。本文档不尝试对该格式进行标准化，除了提到如果传统源代码分发包含一个使用元数据版本 2.2 或更高版本的 ``PKG-INFO`` 文件，那么它必须遵循元数据规范中定义的适用于源代码分发的规则。

  源代码分发也被称为 *sdists* （简写）。

.. tab:: 英文

  The current standard format of source distribution format is identified by the
  presence of a :file:`pyproject.toml` file in the distribution archive.  The layout
  of such a distribution was originally specified in :pep:`517` and is formally
  documented here.

  There is also the legacy source distribution format, implicitly defined by the
  behaviour of ``distutils`` module in the standard library, when executing
  :command:`setup.py sdist`. This document does not attempt to standardise this
  format, except to note that if a legacy source distribution contains a
  ``PKG-INFO`` file using metadata version 2.2 or later, then it MUST follow
  the rules applicable to source distributions defined in the metadata
  specification.

  Source distributions are also known as *sdists* for short.

源代码树
============

**Source trees**

.. tab:: 中文

  *源代码树* 是一组文件和目录的集合——类似于版本控制系统的检出——其中包含一个 :file:`pyproject.toml` 文件，可以用来从其中的文件和目录构建源代码分发。 :pep:`517` 和 :pep:`518` 规定了符合源代码树定义的要求，即 :file:`pyproject.toml` 必须包含什么内容，才能被视为源代码树。

.. tab:: 英文

  A *source tree* is a collection of files and directories -- like a version
  control system checkout -- which contains a :file:`pyproject.toml` file that
  can be use to build a source distribution from the contained files and
  directories. :pep:`517` and :pep:`518` specify what is required to meet the
  definition of what :file:`pyproject.toml` must contain for something to be
  deemed a source tree.

源分发文件名
=============================

**Source distribution file name**

.. tab:: 中文

  源代码分发文件的文件名在 :pep:`625` 中进行了标准化。文件名必须采用 ``{name}-{version}.tar.gz`` 格式，其中 ``{name}`` 按照与二进制分发相同的规则进行规范化（参见 :ref:`binary-distribution-format`）， ``{version}`` 是项目版本的规范化形式（参见 :ref:`version-specifiers`）。

  文件名中的名称和版本部分 **必须** 与文件中包含的元数据中的值匹配。

  生成源代码分发文件的代码 **必须** 给文件一个符合该规范的文件名。这包括 :term:`build backend <Build Backend>` 的 ``build_sdist`` 钩子。

  处理源代码分发文件的代码 **可以** 通过 ``.tar.gz`` 后缀和文件名中恰好 *一个* 连字符来识别源代码分发文件。这样做的代码可以直接从文件名中提取分发的名称和版本，而无需进一步验证。

.. tab:: 英文

  The file name of a sdist was standardised in :pep:`625`. The file name must be in
  the form ``{name}-{version}.tar.gz``, where ``{name}`` is normalised according to
  the same rules as for binary distributions (see :ref:`binary-distribution-format`),
  and ``{version}`` is the canonicalized form of the project version (see
  :ref:`version-specifiers`).

  The name and version components of the filename MUST match the values stored
  in the metadata contained in the file.

  Code that produces a source distribution file MUST give the file a name that matches
  this specification. This includes the ``build_sdist`` hook of a
  :term:`build backend <Build Backend>`.

  Code that processes source distribution files MAY recognise source distribution files
  by the ``.tar.gz`` suffix and the presence of precisely *one* hyphen in the filename.
  Code that does this may then use the distribution name and version from the filename
  without further verification.

源分发文件格式
===============================

**Source distribution file format**

.. tab:: 中文

  ``.tar.gz`` 源代码分发（sdist）包含一个名为 ``{name}-{version}`` 的顶级目录（例如 ``foo-1.0``），该目录包含包的源代码文件。名称和版本 **必须** 与文件中存储的元数据匹配。该目录还必须包含一个符合 :ref:`pyproject-toml-spec` 中定义格式的 :file:`pyproject.toml` 文件，以及一个包含元数据的 ``PKG-INFO`` 文件，该格式在 :ref:`core-metadata` 规范中进行了描述。元数据 **必须** 至少符合版本 2.2 的元数据规范。

  源代码分发文件中不要求或不定义其他内容。构建系统可以在源代码分发中存储它们构建项目所需的任何信息。

  tar 包应使用现代的 POSIX.1-2001 pax tar 格式，该格式指定了基于 UTF-8 的文件名。特别地，源代码分发文件必须能够通过标准库的 tarfile 模块使用 `r:gz` 打开标志进行读取。

.. tab:: 英文

  A ``.tar.gz`` source distribution (sdist) contains a single top-level directory
  called ``{name}-{version}`` (e.g. ``foo-1.0``), containing the source files of
  the package. The name and version MUST match the metadata stored in the file.
  This directory must also contain a :file:`pyproject.toml` in the format defined in
  :ref:`pyproject-toml-spec`, and a ``PKG-INFO`` file containing
  metadata in the format described in the :ref:`core-metadata` specification. The
  metadata MUST conform to at least version 2.2 of the metadata specification.

  No other content of a sdist is required or defined. Build systems can store
  whatever information they need in the sdist to build the project.

  The tarball should use the modern POSIX.1-2001 pax tar format, which specifies
  UTF-8 based file names. In particular, source distribution files must be readable
  using the standard library tarfile module with the open flag 'r:gz'.


.. _sdist-archive-features:

源分发存档功能
====================================

**Source distribution archive features**

.. tab:: 中文

  由于直接提取 tar 文件是危险的，并且结果具有平台特异性，因此源代码分发文件的归档功能是有限制的。

.. tab:: 英文

  Because extracting tar files as-is is dangerous, and the results are platform-specific, archive features of source distributions are limited.

使用数据过滤器解包
------------------------------

**Unpacking with the data filter**

.. tab:: 中文

  在提取源代码分发文件时，工具必须使用 :py:func:`tarfile.data_filter` （例如 :py:meth:`TarFile.extractall(..., filter='data') <tarfile.TarFile.extractall>`），或者遵循下面的 *没有数据过滤器的解包* 部分。

  作为例外，对于没有 :py:func:`hasattr(tarfile, 'data_filter') <tarfile.data_filter>`（参见 :pep:`706`）的 Python 解释器，通常使用该过滤器（直接或间接）的工具可以向用户发出警告并忽略此规范。在这种情况下，工具可以根据可用性（例如完全信任归档文件）和安全性（例如拒绝解包）之间的权衡来决定。

.. tab:: 英文

  When extracting a source distribution, tools MUST either use
  :py:func:`tarfile.data_filter` (e.g. :py:meth:`TarFile.extractall(..., filter='data') <tarfile.TarFile.extractall>`), OR
  follow the *Unpacking without the data filter* section below.

  As an exception, on Python interpreters without :py:func:`hasattr(tarfile, 'data_filter') <tarfile.data_filter>`
  (:pep:`706`), tools that normally use that filter (directly on indirectly)
  MAY warn the user and ignore this specification.
  The trade-off between usability (e.g. fully trusting the archive) and
  security (e.g. refusing to unpack) is left up to the tool in this case.


不使用数据过滤器解包
---------------------------------

**Unpacking without the data filter**

.. tab:: 中文

  不直接使用 ``data`` 过滤器的工具（例如出于向后兼容性、允许附加功能或未使用 Python）必须遵循本节内容。
  （截至本文写作时， ``data`` 过滤器也遵循本节，但未来可能会不同步。）

  以下文件在 *sdist* 存档中无效。遇到此类条目时，工具应通知用户，
  **不得解包该条目**，并且 **可以中止并报告失败** ：

  - 会被放置在目标目录之外的文件。
  - 指向目标目录之外的符号链接（或硬链接）。
  - 设备文件（包括管道）。

  以下内容也是无效的。工具可以像上述那样处理它们，但 **不要求必须这样做**：

  - 文件名或链接目标中包含 ``..`` 的文件。
  - 指向不属于归档文件的文件的链接。

  工具可以将链接（符号链接或硬链接）解包为常规文件，
  并使用归档中的内容。

  解包 *sdist* 存档时：

  - 文件名中的前导斜杠必须被去掉。
    （如今这是 ``tar`` 解包的标准行为。）
  - 对于每个 ``mode`` （Unix 权限）位，工具必须执行以下操作之一：

    - 使用平台的默认值来创建新文件/目录（分别适用于文件和目录），
    - 根据归档设置该位，或
    - 对于非可执行文件使用 ``rw-r--r--`` （``0o644``），
      对于可执行文件和目录使用 ``rwxr-xr-x`` （``0o755``）。

  - 高位 ``mode`` （setuid、setgid、sticky）必须被清除。
  - 推荐保留用户 *可执行* 位。

.. tab:: 英文

  Tools that do not use the ``data`` filter directly (e.g. for backwards
  compatibility, allowing additional features, or not using Python) MUST follow
  this section.
  (At the time of this writing, the ``data`` filter also follows this section,
  but it may get out of sync in the future.)

  The following files are invalid in an *sdist* archive.
  Upon encountering such an entry, tools SHOULD notify the user,
  MUST NOT unpack the entry, and MAY abort with a failure:

  - Files that would be placed outside the destination directory.
  - Links (symbolic or hard) pointing outside the destination directory.
  - Device files (including pipes).

  The following are also invalid. Tools MAY treat them as above,
  but are NOT REQUIRED to do so:

  - Files with a ``..`` component in the filename or link target.
  - Links pointing to a file that is not part of the archive.

  Tools MAY unpack links (symbolic or hard) as regular files,
  using content from the archive.

  When extracting *sdist* archives:

  - Leading slashes in file names MUST be dropped.
    (This is nowadays standard behaviour for ``tar`` unpacking.)
  - For each ``mode`` (Unix permission) bit, tools MUST either:

    - use the platform's default for a new file/directory (respectively),
    - set the bit according to the archive, or
    - use the bit from ``rw-r--r--`` (``0o644``) for non-executable files or
      ``rwxr-xr-x`` (``0o755``) for executable files and directories.

  - High ``mode`` bits (setuid, setgid, sticky) MUST be cleared.
  - It is RECOMMENDED to preserve the user *executable* bit.


更多提示
-------------

**Further hints**

.. tab:: 中文

  鼓励工具作者考虑  ``tarfile`` 文档中的 *进一步验证的提示* 如何应用于他们的工具。

.. tab:: 英文

  Tool authors are encouraged to consider how *hints for further verification* in ``tarfile`` documentation apply to their tool.


历史
=======

**History**

.. tab:: 中文

  * 2020年11月：该规范的原始版本通过了 :pep:`643` 批准。
  * 2021年7月：定义了什么是源代码树。
  * 2022年9月：通过 :pep:`625` 标准化了源代码分发文件的文件名。
  * 2023年8月：通过 :pep:`721` 标准化了源代码分发档案功能。

.. tab:: 英文

  * November 2020: The original version of this specification was approved through :pep:`643`.
  * July 2021: Defined what a source tree is.
  * September 2022: The filename of a source distribution was standardized through :pep:`625`.
  * August 2023: Source distribution archive features were standardized through :pep:`721`.
