.. highlight:: text

.. _recording-installed-packages:

============================
记录已安装的项目
============================

**Recording installed projects**

.. tab:: 中文

  本文档指定了记录 Python :term:`项目 <Project>` 在环境中安装信息的通用格式。
  一种通用的元数据格式允许工具查询、管理或卸载项目，
  无论它们是如何安装的。

  几乎所有信息都是可选的。
  这使得 Python 生态系统以外的工具（如 Linux 包管理器）能够尽可能地与 Python 工具集成。
  例如，即使安装程序无法轻松提供以 Python 工具特定格式列出的已安装文件，
  它仍应记录已安装项目的名称和版本。

.. tab:: 英文

  This document specifies a common format of recording information
  about Python :term:`projects <Project>` installed in an environment.
  A common metadata format allows tools to query, manage or uninstall projects,
  regardless of how they were installed.

  Almost all information is optional.
  This allows tools outside the Python ecosystem, such as Linux package managers,
  to integrate with Python tooling as much as possible.
  For example, even if an installer cannot easily provide a list of installed
  files in a format specific to Python tooling, it should still record the name
  and version of the installed project.


.dist-info 目录
========================

**The .dist-info directory**

.. tab:: 中文

  从分发中安装的每个项目必须除了文件外，还安装一个名为 "``.dist-info``" 的目录，
  该目录位于可导入的模块和包旁边（通常是 ``site-packages`` 目录）。

  该目录命名为 ``{name}-{version}.dist-info``，其中 ``name`` 和
  ``version`` 字段对应于 :ref:`core-metadata`。这两个字段必须
  标准化（请参见 :ref:`name normalization specification <name-normalization>`
  和 :ref:`version normalization specification <version-specifiers-normalization>`），
  并将破折号（``-``）字符替换为下划线（``_``）字符，
  因此 ``.dist-info`` 目录的名称总是包含且仅包含一个破折号（``-``），
  用于分隔 ``name`` 和 ``version`` 字段。

  历史上，工具未能替换点字符或标准化 ``name`` 字段中的大小写，
  或者未对 ``version`` 字段进行标准化。
  消费 ``.dist-info`` 目录的工具应预期这些字段未标准化，并将它们视为与其标准化形式等效。
  新的工具在写入 ``.dist-info`` 目录时，必须使用上述规则标准化 ``name`` 和 ``version`` 字段，
  并鼓励现有工具开始标准化这些字段。

  .. note::

      ``.dist-info`` 目录的名称格式旨在明确表示一个分发作为文件系统路径。
      向用户展示分发名称的工具应避免使用标准化后的名称，而应呈现指定的名称
      （在解析到已安装包之前需要时），或者读取核心元数据中的相关字段，
      因为那里列出的值是未转义的，准确反映了该分发。
      库应为这些工具提供 API，以便工具在显示分发信息时能够访问未标准化的名称。

  该 ``.dist-info`` 目录可能包含以下文件，具体说明如下：

  * ``METADATA``: 包含项目元数据
  * ``RECORD``: 记录已安装文件的列表。
  * ``INSTALLER``: 记录用于安装项目的工具的名称。
  * ``entry_points.txt``: 详见 :ref:`entry-points`
  * ``direct_url.json``: 详见 :ref:`direct-url`

  ``METADATA`` 文件是强制性的。
  其他所有文件可以根据安装工具的判断决定是否省略。
  可能会有其他特定于安装工具的文件。

  .. note::

    :ref:`binary-distribution-format` 规范描述了可能出现在 :term:`Wheel`
    的 ``.dist-info`` 目录中的附加文件。这些文件可以被复制到已安装项目的
    ``.dist-info`` 目录中。

  该规范的早期版本还指定了一个 ``REQUESTED`` 文件。这个文件现在被视为工具特定的扩展，
  但未来可能会重新标准化。有关其原始含义，请参见 `PEP 376 <https://www.python.org/dev/peps/pep-0376/#requested>`_。

.. tab:: 英文

  Each project installed from a distribution must, in addition to files,
  install a "``.dist-info``" directory located alongside importable modules and
  packages (commonly, the ``site-packages`` directory).

  This directory is named as ``{name}-{version}.dist-info``, with ``name`` and
  ``version`` fields corresponding to :ref:`core-metadata`. Both fields must be
  normalized (see the :ref:`name normalization specification <name-normalization>`
  and the :ref:`version normalization specification <version-specifiers-normalization>`),
  and replace dash (``-``) characters with underscore (``_``) characters,
  so the ``.dist-info`` directory always has exactly one dash (``-``) character in
  its stem, separating the ``name`` and ``version`` fields.

  Historically, tools have failed to replace dot characters or normalize case in
  the ``name`` field, or not perform normalization in the ``version`` field.
  Tools consuming ``.dist-info`` directories should expect those fields to be
  unnormalized, and treat them as equivalent to their normalized counterparts.
  New tools that write ``.dist-info`` directories MUST normalize both ``name``
  and ``version`` fields using the rules described above, and existing tools are
  encouraged to start normalizing those fields.

  .. note::

      The ``.dist-info`` directory's name is formatted to unambiguously represent
      a distribution as a filesystem path. Tools presenting a distribution name
      to a user should avoid using the normalized name, and instead present the
      specified name (when needed prior to resolution to an installed package),
      or read the respective fields in Core Metadata, since values listed there
      are unescaped and accurately reflect the distribution. Libraries should
      provide API for such tools to consume, so tools can have access to the
      unnormalized name when displaying distribution information.

  This ``.dist-info`` directory may contain the following files, described in
  detail below:

  * ``METADATA``: contains project metadata
  * ``RECORD``: records the list of installed files.
  * ``INSTALLER``: records the name of the tool used to install the project.
  * ``entry_points.txt``: see :ref:`entry-points` for details
  * ``direct_url.json``: see :ref:`direct-url` for details

  The ``METADATA`` file is mandatory.
  All other files may be omitted at the installing tool's discretion.
  Additional installer-specific files may be present.

  .. note::

    The :ref:`binary-distribution-format` specification describes additional
    files that may appear in the ``.dist-info`` directory of a :term:`Wheel`.
    Such files may be copied to the ``.dist-info`` directory of an
    installed project.

  The previous versions of this specification also specified a ``REQUESTED``
  file. This file is now considered a tool-specific extension, but may be
  standardized again in the future. See `PEP 376 <https://www.python.org/dev/peps/pep-0376/#requested>`_
  for its original meaning.


METADATA 文件
=================

**The METADATA file**

.. tab:: 中文

  ``METADATA`` 文件包含如 :ref:`core-metadata`
  规范中所描述的元数据，版本为 1.1 或更高版本。

  ``METADATA`` 文件是强制性的。
  如果无法创建该文件，或者所需的核心元数据不可用，
  安装程序必须报告错误并停止安装项目。

.. tab:: 英文

  The ``METADATA`` file contains metadata as described in the :ref:`core-metadata`
  specification, version 1.1 or greater.

  The ``METADATA`` file is mandatory.
  If it cannot be created, or if required core metadata is not available,
  installers must report an error and fail to install the project.


RECORD 文件
===============

**The RECORD file**

.. tab:: 中文

  ``RECORD`` 文件保存已安装文件的列表。
  它是一个 CSV 文件，每个已安装文件占用一条记录（行）。

  CSV 方言必须可以通过 Python 的 ``csv`` 模块的默认 ``reader`` 读取：

  * 字段分隔符：``,`` （逗号），
  * 引号字符：``"`` （直引号），
  * 行终止符：``\r\n`` 或 ``\n``。

  每条记录由三个元素组成：文件的 **路径**、文件内容的 **哈希值** 和 **大小**。

  *路径* 可以是绝对路径，或者相对于包含 ``.dist-info`` 目录的目录（通常是 ``site-packages`` 目录）。
  在 Windows 上，目录分隔符可以是正斜杠或反斜杠（``/`` 或 ``\``）。

  *哈希值* 要么是空字符串，要么是来自 :py:data:`hashlib.algorithms_guaranteed` 的哈希算法名称，
  后跟等号字符 ``=`` 和文件内容的摘要，摘要使用 urlsafe-base64-nopad 编码（即使用
  :py:func:`base64.urlsafe_b64encode(digest) <base64.urlsafe_b64encode()>`，去掉尾部的 ``=``）。

  *大小* 要么是空字符串，要么是文件的大小（以字节为单位），以十进制整数表示。

  对于任何文件，*哈希值* 和 *大小* 字段可以留空，或者两者都为空。
  通常，``.pyc`` 文件和 ``RECORD`` 文件本身的条目会有空的 *哈希值* 和 *大小*。
  对于其他文件，不建议省略这些信息，因为它阻碍了对已安装项目完整性的验证。

  如果 ``RECORD`` 文件存在，它必须列出项目的所有已安装文件，
  但不包括与 ``.py`` 文件对应的 ``.pyc`` 文件，这些文件是可选的。
  特别地，``.dist-info`` 目录的内容（包括 ``RECORD`` 文件本身）必须列出。
  目录不应列出。

  为了完全卸载一个包，工具需要删除 ``RECORD`` 中列出的所有文件，
  删除与已移除的 ``.py`` 文件对应的所有 ``.pyc`` 文件（包括所有优化级别），
  以及卸载过程中被清空的任何目录。

  以下是一个可能的 ``RECORD`` 文件的示例片段::

      /usr/bin/black,sha256=iFlOnL32lIa-RKk-MDihcbJ37wxmRbE4xk6eVYVTTeU,220
      ../../../bin/blackd,sha256=lCadt4mcU-B67O1gkQVh7-vsKgLpx6ny1le34Jz6UVo,221
      __pycache__/black.cpython-38.pyc,,
      __pycache__/blackd.cpython-38.pyc,,
      black-19.10b0.dist-info/INSTALLER,sha256=zuuue4knoyJ-UwPPXg8fezS7VCrXJQrAP7zeNuwvFQg,4
      black-19.10b0.dist-info/LICENSE,sha256=nAQo8MO0d5hQz1vZbhGqqK_HLUqG1KNiI9erouWNbgA,1080
      black-19.10b0.dist-info/METADATA,sha256=UN40nGoVVTSpvLrTBwNsXgZdZIwoKFSrrDDHP6B7-A0,58841
      black-19.10b0.dist-info/RECORD,,
      black.py,sha256=45IF72OgNfF8WpeNHnxV2QGfbCLubV5Xjl55cI65kYs,140161
      blackd.py,sha256=JCxaK4hLkMRwVfZMj8FRpRRYC0172-juKqbN22bISLE,6672
      blib2to3/__init__.py,sha256=9_8wL9Scv8_Cs8HJyJHGvx1vwXErsuvlsAqNZLcJQR0,8
      blib2to3/__pycache__/__init__.cpython-38.pyc,,
      blib2to3/__pycache__/pygram.cpython-38.pyc,sha256=zpXgX4FHDuoeIQKO_v0sRsB-RzQFsuoKoBYvraAdoJw,1512
      blib2to3/__pycache__/pytree.cpython-38.pyc,sha256=LYLplXtG578ZjaFeoVuoX8rmxHn-BMAamCOsJMU1b9I,24910
      blib2to3/pygram.py,sha256=mXpQPqHcamFwch0RkyJsb92Wd0kUP3TW7d-u9dWhCGY,2085
      blib2to3/pytree.py,sha256=RWj3IL4U-Ljhkn4laN0C3p7IRdfvT3aIRjTV-x9hK1c,28530

  如果 ``RECORD`` 文件丢失，依赖于 ``.dist-info`` 的工具不得尝试卸载或升级包。
  （此限制不适用于依赖其他信息来源的工具，例如 Linux 发行版中的系统包管理器。）

  .. note::

    强烈不建议已安装的包自行修改自己
    （例如，将缓存文件存储在其命名空间下的 ``site-packages`` 中）。
    ``site-packages`` 中的更改应留给专门的安装工具，如 pip。
    如果包仍然以这种方式被修改，则必须更新 ``RECORD``，
    否则卸载包时将留下未列出的文件（可能导致僵尸命名空间包）。

.. tab:: 英文

  The ``RECORD`` file holds the list of installed files.
  It is a CSV file containing one record (line) per installed file.

  The CSV dialect must be readable with the default ``reader`` of Python's
  ``csv`` module:

  * field delimiter: ``,`` (comma),
  * quoting char: ``"`` (straight double quote),
  * line terminator: either ``\r\n`` or ``\n``.

  Each record is composed of three elements: the file's **path**, the **hash**
  of the contents, and its **size**.

  The *path* may be either absolute, or relative to the directory containing
  the ``.dist-info`` directory (commonly, the ``site-packages`` directory).
  On Windows, directories may be separated either by forward- or backslashes
  (``/`` or ``\``).

  The *hash* is either an empty string or the name of a hash algorithm from
  :py:data:`hashlib.algorithms_guaranteed`, followed by the equals character ``=`` and
  the digest of the file's contents, encoded with the urlsafe-base64-nopad
  encoding (:py:func:`base64.urlsafe_b64encode(digest) <base64.urlsafe_b64encode()>` with trailing ``=`` removed).

  The *size* is either the empty string, or file's size in bytes,
  as a base 10 integer.

  For any file, either or both of the *hash* and *size* fields may be left empty.
  Commonly, entries for ``.pyc`` files and the ``RECORD`` file itself have empty
  *hash* and *size*.
  For other files, leaving the information out is discouraged, as it
  prevents verifying the integrity of the installed project.

  If the ``RECORD`` file is present, it must list all installed files of the
  project, except ``.pyc`` files corresponding to ``.py`` files listed in
  ``RECORD``, which are optional.
  Notably, the contents of the ``.dist-info`` directory (including the ``RECORD``
  file itself) must be listed.
  Directories should not be listed.

  To completely uninstall a package, a tool needs to remove all
  files listed in ``RECORD``, all ``.pyc`` files (of all optimization levels)
  corresponding to removed ``.py`` files, and any directories emptied by
  the uninstallation.

  Here is an example snippet of a possible ``RECORD`` file::

      /usr/bin/black,sha256=iFlOnL32lIa-RKk-MDihcbJ37wxmRbE4xk6eVYVTTeU,220
      ../../../bin/blackd,sha256=lCadt4mcU-B67O1gkQVh7-vsKgLpx6ny1le34Jz6UVo,221
      __pycache__/black.cpython-38.pyc,,
      __pycache__/blackd.cpython-38.pyc,,
      black-19.10b0.dist-info/INSTALLER,sha256=zuuue4knoyJ-UwPPXg8fezS7VCrXJQrAP7zeNuwvFQg,4
      black-19.10b0.dist-info/LICENSE,sha256=nAQo8MO0d5hQz1vZbhGqqK_HLUqG1KNiI9erouWNbgA,1080
      black-19.10b0.dist-info/METADATA,sha256=UN40nGoVVTSpvLrTBwNsXgZdZIwoKFSrrDDHP6B7-A0,58841
      black-19.10b0.dist-info/RECORD,,
      black.py,sha256=45IF72OgNfF8WpeNHnxV2QGfbCLubV5Xjl55cI65kYs,140161
      blackd.py,sha256=JCxaK4hLkMRwVfZMj8FRpRRYC0172-juKqbN22bISLE,6672
      blib2to3/__init__.py,sha256=9_8wL9Scv8_Cs8HJyJHGvx1vwXErsuvlsAqNZLcJQR0,8
      blib2to3/__pycache__/__init__.cpython-38.pyc,,
      blib2to3/__pycache__/pygram.cpython-38.pyc,sha256=zpXgX4FHDuoeIQKO_v0sRsB-RzQFsuoKoBYvraAdoJw,1512
      blib2to3/__pycache__/pytree.cpython-38.pyc,sha256=LYLplXtG578ZjaFeoVuoX8rmxHn-BMAamCOsJMU1b9I,24910
      blib2to3/pygram.py,sha256=mXpQPqHcamFwch0RkyJsb92Wd0kUP3TW7d-u9dWhCGY,2085
      blib2to3/pytree.py,sha256=RWj3IL4U-Ljhkn4laN0C3p7IRdfvT3aIRjTV-x9hK1c,28530

  If the ``RECORD`` file is missing, tools that rely on ``.dist-info`` must not
  attempt to uninstall or upgrade the package.
  (This restriction does not apply to tools that rely on other sources of information,
  such as system package managers in Linux distros.)

  .. note::

    It is *strongly discouraged* for an installed package to modify itself
    (e.g., store cache files under its namespace in ``site-packages``).
    Changes inside ``site-packages`` should be left to specialized installer
    tools such as pip. If a package is nevertheless modified in this way,
    then the ``RECORD`` must be updated, otherwise uninstalling the package
    will leave unlisted files in place (possibly resulting in a zombie
    namespace package).

INSTALLER 文件
==================

**The INSTALLER file**

.. tab:: 中文

  如果存在，``INSTALLER`` 是一个单行文本文件，列出用于安装该项目的工具名称。
  如果安装工具可以通过命令行执行，``INSTALLER`` 应包含命令名称。
  否则，它应包含一个可打印的 ASCII 字符串。

  该文件可以以零个或多个 ASCII 空白字符结尾。

  以下是两个可能的 ``INSTALLER`` 文件示例::

      pip

  ::

      MegaCorp Cloud Install-O-Matic

  此值仅应用于参考目的。
  例如，如果一个工具被要求卸载一个项目，但找不到 ``RECORD`` 文件，
  它可以提示 ``INSTALLER`` 中列出的工具可能能够进行卸载。

.. tab:: 英文

  If present, ``INSTALLER`` is a single-line text file naming the tool used to
  install the project.
  If the installer is executable from the command line, ``INSTALLER``
  should contain the command name.
  Otherwise, it should contain a printable ASCII string.

  The file can be terminated by zero or more ASCII whitespace characters.

  Here are examples of two possible ``INSTALLER`` files::

      pip

  ::

      MegaCorp Cloud Install-O-Matic

  This value should be used for informational purposes only.
  For example, if a tool is asked to uninstall a project but finds no ``RECORD``
  file, it may suggest that the tool named in ``INSTALLER`` may be able to do the
  uninstallation.


entry_points.txt 文件
=========================

**The entry_points.txt file**

.. tab:: 中文

  此文件可以由安装程序创建，用于指示包中包含供其他代码发现和使用的组件，
  包括控制台脚本和其他安装程序已使其可执行的应用程序。

  其详细规范请参见 :ref:`entry-points`。

.. tab:: 英文

  This file MAY be created by installers to indicate when packages contain
  components intended for discovery and use by other code, including console
  scripts and other applications that the installer has made available for
  execution.

  Its detailed specification is at :ref:`entry-points`.


direct_url.json 文件
========================

**The direct_url.json file**

.. tab:: 中文

  当从指定直接 URL 引用（包括 VCS URL）的需求安装分发包时，安装程序 **必须** 创建此文件。

  当从其他类型的需求（即名称加版本说明符）安装分发包时，**不得** 创建此文件。

  其详细规范请参见 :ref:`direct-url`。

.. tab:: 英文

  This file MUST be created by installers when installing a distribution from a
  requirement specifying a direct URL reference (including a VCS URL).

  This file MUST NOT be created when installing a distribution from an other type
  of requirement (i.e. name plus version specifier).

  Its detailed specification is at :ref:`direct-url`.


有意阻止对已安装软件包的更改
======================================================

**Intentionally preventing changes to installed packages**

.. tab:: 中文

  在某些情况下（例如需要管理除了 Python 生态系统依赖项外的外部依赖项时），
  希望安装包到 Python 环境中的工具能够确保其他工具无法卸载或修改已安装的包，
  因为这样做可能会导致与更广泛环境的兼容性问题。

  为此，受影响的工具应采取以下步骤：

  * 重命名或删除 ``RECORD`` 文件，以防止通过其他工具进行更改（例如，
    如果工具本身需要这些信息，可以通过附加后缀创建一个非标准的 ``RECORD.tool`` 文件，
    或者如果包内容通过其他方式进行跟踪和管理，则完全省略此文件）
  * 写入 ``INSTALLER`` 文件，指明应该用于管理包的工具名称
    （这允许 ``RECORD`` 相关工具在要求修改受影响的包时提供更好的错误提示）

  Python 运行时提供者还可以通过修改默认的 Python 包安装方案，
  使用与平台提供的包不同的位置，从而防止平台提供的包被意外修改
  （同时确保这两个位置都出现在默认的 Python 导入路径中）。

  在某些情况下，可能希望甚至阻止通过 Python 特定工具安装额外的包。
  对于这些情况，请参阅 :ref:`externally-managed-environments`。

.. tab:: 英文

  In some cases (such as when needing to manage external dependencies in addition
  to Python ecosystem dependencies), it is desirable for a tool that installs
  packages into a Python environment to ensure that other tools are not used to
  uninstall or otherwise modify that installed package, as doing so may cause
  compatibility problems with the wider environment.

  To achieve this, affected tools should take the following steps:

  * Rename or remove the ``RECORD`` file to prevent changes via other tools (e.g.
    appending a suffix to create a non-standard ``RECORD.tool`` file if the tool
    itself needs the information, or omitting the file entirely if the package
    contents are tracked and managed via other means)
  * Write an ``INSTALLER`` file indicating the name of the tool that should be used
    to manage the package (this allows ``RECORD``-aware tools to provide better
    error notices when asked to modify affected packages)

  Python runtime providers may also prevent inadvertent modification of platform
  provided packages by modifying the default Python package installation scheme
  to use a location other than that used by platform provided packages (while also
  ensuring both locations appear on the default Python import path).

  In some circumstances, it may be desirable to block even installation of
  additional packages via Python-specific tools. For these cases refer to
  :ref:`externally-managed-environments`


历史记录
=======

**History**

.. tab:: 中文

  - 2009年6月：该规范的原始版本通过 :pep:`376` 获得批准。 当时，它被称为 *已安装 Python 分发包数据库*。
  - 2020年3月：``direct_url.json`` 文件的规范通过 :pep:`610` 获得批准。它仅在此页面中提及；请参见 :ref:`direct-url` 获取完整定义。
  - 2020年9月：通过 :pep:`627` 批准了各种修订和澄清。

.. tab:: 英文

  - June 2009: The original version of this specification was approved through
    :pep:`376`.  At the time, it was known as the *Database of Installed Python
    Distributions*.
  - March 2020: The specification of the ``direct_url.json`` file was approved
    through :pep:`610`. It is only mentioned on this page; see :ref:`direct-url`
    for the full definition.
  - September 2020: Various amendments and clarifications were approved through
    :pep:`627`.
