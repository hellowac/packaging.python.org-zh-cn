.. highlight:: text

.. _binary-distribution-format:

==========================
二进制分发格式
==========================

**Binary distribution format**

.. tab:: 中文

   本页面规范了 Python 包的二进制分发格式，也称为 wheel 格式。

   Wheel 是一个 ZIP 格式的归档文件，具有特别格式化的文件名和 ``.whl`` 扩展名。它包含一个几乎与根据 PEP 376 安装方案安装的分发相同的内容。尽管推荐使用专门的安装工具，但 wheel 文件也可以通过标准的 'unzip' 工具直接解压到 site-packages 目录中，同时保留足够的信息，以便在任何以后时刻将其内容展开到最终路径。

.. tab:: 英文

   This page specifies the binary distribution format for Python packages,
   also called the wheel format.

   A wheel is a ZIP-format archive with a specially formatted file name and
   the ``.whl`` extension.  It contains a single distribution nearly as it
   would be installed according to PEP 376 with a particular installation
   scheme.  Although a specialized installer is recommended, a wheel file
   may be installed by simply unpacking into site-packages with the standard
   'unzip' tool while preserving enough information to spread its contents
   out onto their final paths at any later time.


详细信息
=======

**Details**

安装 wheel 'distribution-1.0-py32-none-any.whl'
-------------------------------------------------------

**Installing a wheel 'distribution-1.0-py32-none-any.whl'**

.. tab:: 中文

   Wheel 安装大致分为两个阶段：

   - 解压。

   a. 解析 ``distribution-1.0.dist-info/WHEEL`` 文件。
   b. 检查安装程序与 Wheel-Version 的兼容性。如果次版本大于当前版本则发出警告，若主版本大于则中止。
   c. 如果 Root-Is-Purelib 为 'true'，则将归档解压到 purelib（site-packages）。
   d. 否则将归档解压到 platlib（site-packages）。

   - 分发。

   a. 解压的归档包含 ``distribution-1.0.dist-info/`` 和（如果有数据的话） ``distribution-1.0.data/``。
   b. 将 ``distribution-1.0.data/`` 的每个子树移动到其目标路径。``distribution-1.0.data/`` 的每个子目录都是一个目标目录字典的键，例如 ``distribution-1.0.data/(purelib|platlib|headers|scripts|data)``。这些子目录是 :ref:`sysconfig 定义的安装路径 <python:installation_paths>`。
   c. 如果适用，更新以 ``#!python`` 开头的脚本指向正确的解释器。
   d. 更新 ``distribution-1.0.dist-info/RECORD``，记录已安装路径。
   e. 删除空的 ``distribution-1.0.data`` 目录。
   f. 将已安装的 .py 文件编译为 .pyc 文件。（卸载程序应该足够智能，能够在 RECORD 中未提到的情况下删除 .pyc 文件。）

.. tab:: 英文

   Wheel installation notionally consists of two phases:

   - Unpack.

      a. Parse ``distribution-1.0.dist-info/WHEEL``.
      b. Check that installer is compatible with Wheel-Version.  Warn if
         minor version is greater, abort if major version is greater.
      c. If Root-Is-Purelib == 'true', unpack archive into purelib
         (site-packages).
      d. Else unpack archive into platlib (site-packages).

   - Spread.

      a. Unpacked archive includes ``distribution-1.0.dist-info/`` and (if
         there is data) ``distribution-1.0.data/``.
      b. Move each subtree of ``distribution-1.0.data/`` onto its
         destination path. Each subdirectory of ``distribution-1.0.data/``
         is a key into a dict of destination directories, such as
         ``distribution-1.0.data/(purelib|platlib|headers|scripts|data)``.
         These subdirectories are :ref:`installation paths defined by sysconfig
         <python:installation_paths>`.
      c. If applicable, update scripts starting with ``#!python`` to point
         to the correct interpreter.
      d. Update ``distribution-1.0.dist-info/RECORD`` with the installed
         paths.
      e. Remove empty ``distribution-1.0.data`` directory.
      f. Compile any installed .py to .pyc. (Uninstallers should be smart
         enough to remove .pyc even if it is not mentioned in RECORD.)

推荐的安装程序功能
''''''''''''''''''''''''''''''

**Recommended installer features**

.. tab:: 中文

   重写 ``#!python``。
      在 wheel 中，脚本打包在 ``{distribution}-{version}.data/scripts/`` 目录下。如果 ``scripts/`` 中的文件的第一行正好以 ``b'#!python'`` 开头，则将其重写为指向正确的解释器。Unix 安装程序可能需要为这些文件添加可执行位（+x），如果归档文件是在 Windows 上创建的。

      允许使用 ``b'#!pythonw'`` 约定。``b'#!pythonw'`` 表示一个 GUI 脚本，而不是控制台脚本。

   生成脚本包装器。
      在 wheel 中，打包在 Unix 系统上的脚本通常不会有附带的 .exe 包装器。Windows 安装程序可能希望在安装过程中添加这些包装器。

.. tab:: 英文

   Rewrite ``#!python``.
      In wheel, scripts are packaged in
      ``{distribution}-{version}.data/scripts/``.  If the first line of
      a file in ``scripts/`` starts with exactly ``b'#!python'``, rewrite to
      point to the correct interpreter.  Unix installers may need to add
      the +x bit to these files if the archive was created on Windows.

      The ``b'#!pythonw'`` convention is allowed. ``b'#!pythonw'`` indicates
      a GUI script instead of a console script.

   Generate script wrappers.
      In wheel, scripts packaged on Unix systems will certainly not have
      accompanying .exe wrappers.  Windows installers may want to add them
      during install.

推荐的归档器功能
'''''''''''''''''''''''''''''

**Recommended archiver features**

.. tab:: 中文

   将 ``.dist-info`` 放在归档的末尾。
      鼓励归档工具将 ``.dist-info`` 文件物理地放在归档的末尾。这可以实现一些潜在有趣的 ZIP 技巧，包括在不重写整个归档文件的情况下修改元数据。

.. tab:: 英文

   Place ``.dist-info`` at the end of the archive.
      Archivers are encouraged to place the ``.dist-info`` files physically
      at the end of the archive.  This enables some potentially interesting
      ZIP tricks including the ability to amend the metadata without
      rewriting the entire archive.


文件格式
-----------

**File Format**

.. _wheel-file-name-spec:

文件名约定
''''''''''''''''''''

**File name convention**

.. tab:: 中文

   wheel 文件名为 ``{distribution}-{version}(-{build tag})?-{python tag}-{abi tag}-{platform tag}.whl``。

   distribution
      分发包名称，例如 'django'，'pyramid'。

   version
      分发包版本，例如 1.0。

   build tag
      可选的构建编号。必须以数字开头。当两个 wheel 文件名在其他方面（例如名称、版本和其他标签）相同的时候，构建标签作为区分依据。未指定时，按空元组排序；否则，按两项元组排序，第一项为数字部分（``int`` 类型），第二项为剩余部分（``str`` 类型）。

      构建编号的常见用途是由于构建环境变化（例如使用 manylinux 镜像重新构建分发包）而重建二进制分发包。

      .. warning::

         构建编号不是分发版本的一部分，因此在外部引用时会很困难，尤其是在 Python 生态系统之外的工具和标准中。
         一个常见的需要外部引用的情况是解决安全漏洞。

         由于这一限制，需要外部引用的新分发包 **不应** 在构建时使用构建编号。
         而应为这种情况创建一个 **新的分发版本** 。

   language implementation and version tag
      例如 'py27'，'py2'，'py3'。

   abi tag
      例如 'cp33m'，'abi3'，'none'。

   platform tag
      例如 'linux_x86_64'，'any'。

   例如，``distribution-1.0-1-py27-none-any.whl`` 是名为 'distribution' 的包的第一个构建，并且兼容 Python 2.7（任何 Python 2.7 实现），没有 ABI（纯 Python），适用于任何 CPU 架构。

   文件名中的后三个组件（在扩展名之前）称为 "兼容性标签"。这些兼容性标签表示包的基本解释器要求，详细信息请参见 PEP 425。

.. tab:: 英文

   The wheel filename is ``{distribution}-{version}(-{build
   tag})?-{python tag}-{abi tag}-{platform tag}.whl``.

   distribution
      Distribution name, e.g. 'django', 'pyramid'.

   version
      Distribution version, e.g. 1.0.

   build tag
      Optional build number.  Must start with a digit.  Acts as a
      tie-breaker if two wheel file names are the same in all other
      respects (i.e. name, version, and other tags).  Sort as an
      empty tuple if unspecified, else sort as a two-item tuple with
      the first item being the initial digits as an ``int``, and the
      second item being the remainder of the tag as a ``str``.

      A common use-case for build numbers is rebuilding a binary
      distribution due to a change in the build environment,
      like when using the manylinux image to build
      distributions using pre-release CPython versions.

      .. warning::

         Build numbers are not a part of the distribution version and thus are difficult
         to reference externally, especially so outside the Python ecosystem of tools and standards.
         A common case where a distribution would need to referenced externally is when
         resolving a security vulnerability.

         Due to this limitation, new distributions which need to be referenced externally
         **should not** use build numbers when building the new distribution.
         Instead a **new distribution version** should be created for such cases.


   language implementation and version tag
      E.g. 'py27', 'py2', 'py3'.

   abi tag
      E.g. 'cp33m', 'abi3', 'none'.

   platform tag
      E.g. 'linux_x86_64', 'any'.

   For example, ``distribution-1.0-1-py27-none-any.whl`` is the first
   build of a package called 'distribution', and is compatible with
   Python 2.7 (any Python 2.7 implementation), with no ABI (pure Python),
   on any CPU architecture.

   The last three components of the filename before the extension are
   called "compatibility tags."  The compatibility tags express the
   package's basic interpreter requirements and are detailed in PEP 425.

转义和 Unicode
''''''''''''''''''''

**Escaping and Unicode**

.. tab:: 中文

   由于文件名的各个组件由破折号（``-``，HYPHEN-MINUS）分隔，因此此字符不能出现在任何组件内。对此的处理方法如下：

   - 在分发包名称中，连续的 ``-_.`` 字符（HYPHEN-MINUS，LOW LINE 和 FULL STOP）应替换为 ``_`` （LOW LINE），并且大写字母应替换为相应的小写字母。这相当于常规的 :ref:`名称规范化 <name-normalization>`，然后将 ``-`` 替换为 ``_``。然而，工具在处理 wheel 时必须能够接受 ``.`` （FULL STOP）和大写字母，因为这些字符曾在早期版本的规范中是允许的。
   - 版本号应根据 :ref:`版本规范 <version-specifiers>` 进行规范化。规范化后的版本号不能包含 ``-``。
   - 其他组件不能包含 ``-`` 字符，因此无需进行转义。

   生成 wheel 文件的工具应验证文件名组件中是否包含 ``-``，如果包含，生成的文件可能无法正确处理。

   归档文件名是 Unicode 编码的。虽然更新工具以支持非 ASCII 文件名仍需时间，但此规范已支持这种情况。

   归档中的文件名使用 UTF-8 编码。尽管一些常用的 ZIP 客户端未能正确显示 UTF-8 文件名，但 ZIP 规范和 Python 的 ``zipfile`` 模块都支持此编码。

.. tab:: 英文

   As the components of the filename are separated by a dash (``-``, HYPHEN-MINUS),
   this character cannot appear within any component. This is handled as follows:

   - In distribution names, any run of ``-_.`` characters (HYPHEN-MINUS, LOW LINE
   and FULL STOP) should be replaced with ``_`` (LOW LINE), and uppercase
   characters should be replaced with corresponding lowercase ones. This is
   equivalent to regular :ref:`name normalization <name-normalization>` followed by replacing ``-`` with ``_``.
   Tools consuming wheels must be prepared to accept ``.`` (FULL STOP) and
   uppercase letters, however, as these were allowed by an earlier version of
   this specification.
   - Version numbers should be normalised according to the :ref:`Version specifier
   specification <version-specifiers>`. Normalised version numbers cannot contain ``-``.
   - The remaining components may not contain ``-`` characters, so no escaping
   is necessary.

   Tools producing wheels should verify that the filename components do not contain
   ``-``, as the resulting file may not be processed correctly if they do.

   The archive filename is Unicode.  It will be some time before the tools
   are updated to support non-ASCII filenames, but they are supported in
   this specification.

   The filenames *inside* the archive are encoded as UTF-8.  Although some
   ZIP clients in common use do not properly display UTF-8 filenames,
   the encoding is supported by both the ZIP specification and Python's
   ``zipfile``.

文件内容
'''''''''''''

**File contents**

.. tab:: 中文

   一个 wheel 文件的内容，其中 {distribution} 替换为包的名称，例如 ``beaglevote``，{version} 替换为其版本，例如 ``1.0.0``，包括以下部分：

   #. ``/``，归档的根目录，包含所有要安装到 ``purelib`` 或 ``platlib`` 中的文件，如 ``WHEEL`` 文件中指定的。通常，``purelib`` 和 ``platlib`` 都是 ``site-packages``。
   #. ``{distribution}-{version}.dist-info/`` 包含元数据。
   #. ``{distribution}-{version}.data/`` 包含每个非空安装方案键对应的一个子目录，其中子目录名称是指向安装路径字典（如 ``data``、``scripts``、``headers``、``purelib``、``platlib``）的索引。
   #. Python 脚本必须出现在 ``scripts`` 目录中，并且第一行必须是 ``b'#!python'``，以便在安装时享受脚本包装器生成和 ``#!python`` 重写。它们可以有任何扩展名或没有扩展名。
   #. ``{distribution}-{version}.dist-info/METADATA`` 是格式为版本 1.1 或更高版本的元数据。
   #. ``{distribution}-{version}.dist-info/WHEEL`` 是有关归档本身的元数据，采用相同的基本键：值格式，如下所示::

      Wheel-Version: 1.0
      Generator: bdist_wheel 1.0
      Root-Is-Purelib: true
      Tag: py2-none-any
      Tag: py3-none-any
      Build: 1

   #. ``Wheel-Version`` 是 Wheel 规范的版本号。
   #. ``Generator`` 是生成归档的工具的名称，且可选择包含其版本。
   #. ``Root-Is-Purelib`` 如果归档的顶级目录应该安装到 purelib，则为 true；否则，顶级目录应安装到 platlib。
   #. ``Tag`` 是 wheel 的扩展兼容性标签；例如，文件名将包含 ``py2.py3-none-any``。
   #. ``Build`` 是构建号，如果没有构建号，则省略。
   #. 如果 Wheel-Version 大于安装程序支持的版本，wheel 安装程序应发出警告；如果 Wheel-Version 的主版本大于安装程序支持的版本，则必须失败。
   #. Wheel 作为一个旨在跨多个 Python 版本工作的安装格式，通常不包括 .pyc 文件。
   #. Wheel 不包含 ``setup.py`` 或 ``setup.cfg``。

   这一版本的 wheel 规范基于 distutils 安装方案，并未定义如何将文件安装到其他位置。该布局提供了现有 wininst 和 egg 二进制格式的功能超集。

.. tab:: 英文

   The contents of a wheel file, where {distribution} is replaced with the
   name of the package, e.g. ``beaglevote`` and {version} is replaced with
   its version, e.g. ``1.0.0``, consist of:

   #. ``/``, the root of the archive, contains all files to be installed in
      ``purelib`` or ``platlib`` as specified in ``WHEEL``.  ``purelib`` and
      ``platlib`` are usually both ``site-packages``.
   #. ``{distribution}-{version}.dist-info/`` contains metadata.
   #. ``{distribution}-{version}.data/`` contains one subdirectory
      for each non-empty install scheme key not already covered, where
      the subdirectory name is an index into a dictionary of install paths
      (e.g. ``data``, ``scripts``, ``headers``, ``purelib``, ``platlib``).
   #. Python scripts must appear in ``scripts`` and begin with exactly
      ``b'#!python'`` in order to enjoy script wrapper generation and
      ``#!python`` rewriting at install time.  They may have any or no
      extension.
   #. ``{distribution}-{version}.dist-info/METADATA`` is Metadata version 1.1
      or greater format metadata.
   #. ``{distribution}-{version}.dist-info/WHEEL`` is metadata about the archive
      itself in the same basic key: value format::

         Wheel-Version: 1.0
         Generator: bdist_wheel 1.0
         Root-Is-Purelib: true
         Tag: py2-none-any
         Tag: py3-none-any
         Build: 1

   #. ``Wheel-Version`` is the version number of the Wheel specification.
   #. ``Generator`` is the name and optionally the version of the software
      that produced the archive.
   #. ``Root-Is-Purelib`` is true if the top level directory of the archive
      should be installed into purelib; otherwise the root should be installed
      into platlib.
   #. ``Tag`` is the wheel's expanded compatibility tags; in the example the
      filename would contain ``py2.py3-none-any``.
   #. ``Build`` is the build number and is omitted if there is no build number.
   #. A wheel installer should warn if Wheel-Version is greater than the
      version it supports, and must fail if Wheel-Version has a greater
      major version than the version it supports.
   #. Wheel, being an installation format that is intended to work across
      multiple versions of Python, does not generally include .pyc files.
   #. Wheel does not contain setup.py or setup.cfg.

   This version of the wheel specification is based on the distutils install
   schemes and does not define how to install files to other locations.
   The layout offers a superset of the functionality provided by the existing
   wininst and egg binary formats.


.dist-info 目录
^^^^^^^^^^^^^^^^^^^^^^^^

**The .dist-info directory**

.. tab:: 中文

   #. Wheel 的 .dist-info 目录至少包括 METADATA、WHEEL 和 RECORD 文件。
   #. METADATA 是包的元数据，格式与在源分发包根目录中找到的 PKG-INFO 相同。
   #. WHEEL 是与该包的构建相关的 wheel 元数据。
   #. RECORD 是 wheel 中（几乎）所有文件的列表及其安全哈希值。与 PEP 376 不同，除了 RECORD 文件本身外，其他所有文件都必须包括其哈希值。哈希算法必须是 sha256 或更强；具体来说，不允许使用 md5 和 sha1，因为签名的 wheel 文件依赖于 RECORD 中的强哈希来验证归档的完整性。
   #. PEP 376 中的 INSTALLER 和 REQUESTED 文件不包括在归档中。
   #. RECORD.jws 用于数字签名，但不在 RECORD 中提及。
   #. RECORD.p7s 作为对希望使用 S/MIME 签名来保护其 wheel 文件的用户的照顾，允许存在，但不在 RECORD 中提及。
   #. 在提取过程中，wheel 安装程序会验证 RECORD 中所有哈希值是否与文件内容匹配。除了 RECORD 及其签名外，如果归档中的任何文件未被提及或未正确哈希，安装将失败。

.. tab:: 英文

   #. Wheel .dist-info directories include at a minimum METADATA, WHEEL,
      and RECORD.
   #. METADATA is the package metadata, the same format as PKG-INFO as
      found at the root of sdists.
   #. WHEEL is the wheel metadata specific to a build of the package.
   #. RECORD is a list of (almost) all the files in the wheel and their
      secure hashes.  Unlike PEP 376, every file except RECORD, which
      cannot contain a hash of itself, must include its hash.  The hash
      algorithm must be sha256 or better; specifically, md5 and sha1 are
      not permitted, as signed wheel files rely on the strong hashes in
      RECORD to validate the integrity of the archive.
   #. PEP 376's INSTALLER and REQUESTED are not included in the archive.
   #. RECORD.jws is used for digital signatures.  It is not mentioned in
      RECORD.
   #. RECORD.p7s is allowed as a courtesy to anyone who would prefer to
      use S/MIME signatures to secure their wheel files.  It is not
      mentioned in RECORD.
   #. During extraction, wheel installers verify all the hashes in RECORD
      against the file contents.  Apart from RECORD and its signatures,
      installation will fail if any file in the archive is not both
      mentioned and correctly hashed in RECORD.


.data 目录
^^^^^^^^^^^^^^^^^^^

**The .data directory**

.. tab:: 中文

   任何通常不会安装到 ``site-packages`` 中的文件都放入 ``.data`` 目录，该目录的命名方式与 ``.dist-info`` 目录相同，但以 ``.data/`` 为扩展名::

      distribution-1.0.dist-info/
      distribution-1.0.data/

   ``.data`` 目录包含来自分发包的子目录，如脚本、头文件、文档等。在安装过程中，这些子目录的内容会被移动到它们的目标路径。

.. tab:: 英文

   Any file that is not normally installed inside site-packages goes into
   the .data directory, named as the .dist-info directory but with the
   .data/ extension::

      distribution-1.0.dist-info/

      distribution-1.0.data/

   The .data directory contains subdirectories with the scripts, headers,
   documentation and so forth from the distribution.  During installation the
   contents of these subdirectories are moved onto their destination paths.


签名的 wheel 文件
------------------

**Signed wheel files**

.. tab:: 中文

   Wheel 文件包含一个扩展的 ``RECORD``，以支持数字签名。PEP 376 的 ``RECORD`` 被修改，第二列包含一个安全哈希 ``digestname=urlsafe_b64encode_nopad(digest)`` （不带尾随的 ``=`` 字符的 URL 安全 Base64 编码），代替了原来的 md5sum。所有可能的条目都会进行哈希，包括任何生成的文件（如 ``.pyc`` 文件），但不包括 ``RECORD`` 文件本身，因为它不能包含自己的哈希。例如：

      file.py,sha256=AVTFPZpEKzuHr7OvQZmhaU3LvwKz06AJw8mT_pNh2yI,3144
      distribution-1.0.dist-info/RECORD,,

   签名文件 ``RECORD.jws`` 和 ``RECORD.p7s`` 不会出现在 ``RECORD`` 中，因为它们只能在生成 ``RECORD`` 后添加。存档中的每个其他文件必须在 ``RECORD`` 中有正确的哈希，否则安装将失败。

   如果使用 JSON Web 签名（JWS），一个或多个 JWS JSON 序列化签名会存储在 ``RECORD.jws`` 文件中，紧邻 ``RECORD`` 文件。JWS 用来通过将 ``RECORD`` 的 SHA-256 哈希作为签名的 JSON 有效负载来签署 ``RECORD``:

   .. code-block:: json

      { "hash": "sha256=ADD-r2urObZHcxBW3Cr-vDCu5RJwT4CaRTHiFmbcIYY" }

   （哈希值与 ``RECORD`` 中使用的格式相同。）

   如果使用 ``RECORD.p7s``，它必须包含 ``RECORD`` 的分离 S/MIME 格式签名。

   Wheel 安装程序不需要理解数字签名，但必须验证 ``RECORD`` 中的哈希与提取的文件内容是否一致。当安装程序检查文件哈希与 ``RECORD`` 是否匹配时，一个单独的签名检查器只需要验证 ``RECORD`` 是否与签名匹配。

   参考资料：

   - https://datatracker.ietf.org/doc/html/rfc7515
   - https://datatracker.ietf.org/doc/html/draft-jones-json-web-signature-json-serialization-01
   - https://datatracker.ietf.org/doc/html/rfc7517
   - https://datatracker.ietf.org/doc/html/draft-jones-jose-json-private-key-01

.. tab:: 英文

   Wheel files include an extended RECORD that enables digital
   signatures.  PEP 376's RECORD is altered to include a secure hash
   ``digestname=urlsafe_b64encode_nopad(digest)`` (urlsafe base64
   encoding with no trailing = characters) as the second column instead
   of an md5sum.  All possible entries are hashed, including any
   generated files such as .pyc files, but not RECORD which cannot contain its
   own hash. For example::

      file.py,sha256=AVTFPZpEKzuHr7OvQZmhaU3LvwKz06AJw8mT\_pNh2yI,3144
      distribution-1.0.dist-info/RECORD,,

   The signature file(s) RECORD.jws and RECORD.p7s are not mentioned in
   RECORD at all since they can only be added after RECORD is generated.
   Every other file in the archive must have a correct hash in RECORD
   or the installation will fail.

   If JSON web signatures are used, one or more JSON Web Signature JSON
   Serialization (JWS-JS) signatures is stored in a file RECORD.jws adjacent
   to RECORD.  JWS is used to sign RECORD by including the SHA-256 hash of
   RECORD as the signature's JSON payload:

   .. code-block:: json

      { "hash": "sha256=ADD-r2urObZHcxBW3Cr-vDCu5RJwT4CaRTHiFmbcIYY" }

   (The hash value is the same format used in RECORD.)

   If RECORD.p7s is used, it must contain a detached S/MIME format signature
   of RECORD.

   A wheel installer is not required to understand digital signatures but
   MUST verify the hashes in RECORD against the extracted file contents.
   When the installer checks file hashes against RECORD, a separate signature
   checker only needs to establish that RECORD matches the signature.

   See

   - https://datatracker.ietf.org/doc/html/rfc7515
   - https://datatracker.ietf.org/doc/html/draft-jones-json-web-signature-json-serialization-01
   - https://datatracker.ietf.org/doc/html/rfc7517
   - https://datatracker.ietf.org/doc/html/draft-jones-jose-json-private-key-01


常见问题
=========

**FAQ**

.. tab:: 中文



.. tab:: 英文


Wheel 定义了一个 .data 目录。我应该把所有的数据都放在那里吗？
-----------------------------------------------------------------

**Wheel defines a .data directory.  Should I put all my data there?**

.. tab:: 中文

   本规范并不对如何组织您的代码提供意见。`.data` 目录仅用于存放任何不通常安装在 ``site-packages`` 或 PYTHONPATH 中的文件。换句话说，您可以继续使用 ``pkgutil.get_data(package, resource)``，即使 *这些* 文件通常不会分发到 *wheel* 的 ``.data`` 目录中。

.. tab:: 英文

   This specification does not have an opinion on how you should organize
   your code.  The .data directory is just a place for any files that are
   not normally installed inside ``site-packages`` or on the PYTHONPATH.
   In other words, you may continue to use ``pkgutil.get_data(package,
   resource)`` even though *those* files will usually not be distributed
   in *wheel's* ``.data`` directory.


为什么 wheel 包含附加的签名？
-------------------------------------------

**Why does wheel include attached signatures?**

.. tab:: 中文

   附加签名比独立签名更方便，因为它们与归档文件一起传输。由于只有单个文件被签名，因此可以重新压缩归档文件，而不会使签名失效，或者可以在不需要下载整个归档文件的情况下验证单个文件。

.. tab:: 英文

   Attached signatures are more convenient than detached signatures
   because they travel with the archive.  Since only the individual files
   are signed, the archive can be recompressed without invalidating
   the signature or individual files can be verified without having
   to download the whole archive.


为什么 wheel 允许 JWS 签名？
------------------------------------

**Why does wheel allow JWS signatures?**

.. tab:: 中文

   JWS 是 JOSE 规范的一部分，其设计目标是易于实现，这也是 wheel 的主要设计目标之一。JWS 提供了一个实用、简洁的纯 Python 实现。

.. tab:: 英文

   The JOSE specifications of which JWS is a part are designed to be easy to implement, a feature that is also one of wheel's primary design goals.  JWS yields a useful, concise pure-Python implementation.


为什么 wheel 还允许 S/MIME 签名？
--------------------------------------------

**Why does wheel also allow S/MIME signatures?**

.. tab:: 中文

   对于需要或想要将现有公钥基础设施与 wheel 结合使用的用户，可以使用 S/MIME 签名。
   
   签名软件包只是安全软件包更新系统中的基本构建块。Wheel 仅提供构建块。

.. tab:: 英文

   S/MIME signatures are allowed for users who need or want to use existing public key infrastructure with wheel.

   Signed packages are only a basic building block in a secure package update system.  Wheel only provides the building block.


“purelib”和“platlib”有什么区别？
---------------------------------------------

**What's the deal with "purelib" vs. "platlib"?**

.. tab:: 中文

   Wheel 保留了 "purelib" 与 "platlib" 的区分，这在某些平台上是有意义的。例如，Fedora 将纯 Python 包安装到 '/usr/lib/pythonX.Y/site-packages'，将平台相关的包安装到 '/usr/lib64/pythonX.Y/site-packages'。

   一个 "Root-Is-Purelib: false" 的 wheel，其所有文件都在 ``{name}-{version}.data/purelib`` 中，等效于一个 "Root-Is-Purelib: true" 的 wheel，这两个 wheel 都有相同的文件在根目录下，并且可以同时存在 "purelib" 和 "platlib" 类别中的文件。

   实际上，一个 wheel 应该只有 "purelib" 或 "platlib" 之一，取决于它是否是纯 Python 包，这些文件应位于根目录，并且根据 "Root-Is-Purelib" 设置相应的值。

.. tab:: 英文

   Wheel preserves the "purelib" vs. "platlib" distinction, which is significant on some platforms. For example, Fedorainstalls pure Python packages to '/usr/lib/pythonX.Y/site-packages' and platform dependent packages to '/usr/lib64/pythonX.Y/site-packages'.

   A wheel with "Root-Is-Purelib: false" with all its files in ``{name}-{version}.data/purelib`` is equivalent to a wheelwith "Root-Is-Purelib: true" with those same files in the root, and it is legal to have files in both the "purelib" and "platlib" categories.

   In practice a wheel should have only one of "purelib" or "platlib" depending on whether it is pure Python or not andthose files should be at the root with the appropriate setting given for "Root-is-purelib".


.. _binary-distribution-format-import-wheel:

是否可以直接从 wheel 文件导入 Python 代码？
----------------------------------------------------------------

**Is it possible to import Python code directly from a wheel file?**

.. tab:: 中文

   从技术上讲，由于支持通过简单解压安装并使用与 ``zipimport`` 兼容的归档格式，部分 wheel 文件 *确实* 支持直接放置在 ``sys.path`` 上。然而，虽然这种行为是格式设计的自然结果，但通常不建议依赖它。

   首先，wheel *确实* 主要作为一种分发格式设计，因此跳过安装步骤也意味着故意避免依赖假定完全安装的特性（例如，能够使用像 ``pip`` 和 ``virtualenv`` 这样的标准工具捕获和管理依赖，并以一种可以正确追踪的方式进行审计和安全更新，或通过将头文件发布到适当的位置，完全集成到标准构建机制中以支持 C 扩展）。

   其次，尽管有些 Python 软件被编写为支持直接从 zip 归档运行，但仍然常见的是，代码假设它已经完全安装。当这个假设通过尝试从 zip 归档运行软件被打破时，失败往往是隐晦且难以诊断的（尤其是在第三方库中）。造成此问题的两个最常见原因是：从 zip 归档中导入 C 扩展 *不* 被 CPython 支持（因为这种做法没有得到任何平台上动态加载机制的直接支持），以及从 zip 归档运行时， ``__file__`` 属性不再指向普通的文件系统路径，而是指向一个组合路径，该路径包括文件系统上 zip 归档的位置和归档内模块的相对路径。即使软件在内部正确使用了抽象资源 API，与外部组件的接口仍然可能需要实际的磁盘文件。

   像元类、猴子补丁和元路径导入器一样，如果你不确定自己是否需要利用这个特性，那么你几乎肯定不需要它。如果你 *确实* 决定使用它，务必意识到，许多项目在接受它作为真正的 bug 之前，要求能够用完全安装的包重现失败。

.. tab:: 英文

   Technically, due to the combination of supporting installation via
   simple extraction and using an archive format that is compatible with
   ``zipimport``, a subset of wheel files *do* support being placed directly
   on ``sys.path``. However, while this behaviour is a natural consequence
   of the format design, actually relying on it is generally discouraged.

   Firstly, wheel *is* designed primarily as a distribution format, so
   skipping the installation step also means deliberately avoiding any
   reliance on features that assume full installation (such as being able
   to use standard tools like ``pip`` and ``virtualenv`` to capture and
   manage dependencies in a way that can be properly tracked for auditing
   and security update purposes, or integrating fully with the standard
   build machinery for C extensions by publishing header files in the
   appropriate place).

   Secondly, while some Python software is written to support running
   directly from a zip archive, it is still common for code to be written
   assuming it has been fully installed. When that assumption is broken
   by trying to run the software from a zip archive, the failures can often
   be obscure and hard to diagnose (especially when they occur in third
   party libraries). The two most common sources of problems with this
   are the fact that importing C extensions from a zip archive is *not*
   supported by CPython (since doing so is not supported directly by the
   dynamic loading machinery on any platform) and that when running from
   a zip archive the ``__file__`` attribute no longer refers to an
   ordinary filesystem path, but to a combination path that includes
   both the location of the zip archive on the filesystem and the
   relative path to the module inside the archive. Even when software
   correctly uses the abstract resource APIs internally, interfacing with
   external components may still require the availability of an actual
   on-disk file.

   Like metaclasses, monkeypatching and metapath importers, if you're not
   already sure you need to take advantage of this feature, you almost
   certainly don't need it. If you *do* decide to use it anyway, be
   aware that many projects will require a failure to be reproduced with
   a fully installed package before accepting it as a genuine bug.


历史
=======

**History**

.. tab:: 中文

   - 2013年2月：该规范通过 :pep:`427` 获得批准。
   - 2021年2月：对 wheel 文件名中的转义规则进行了修订，以使其与流行工具的实际做法保持一致。

.. tab:: 英文

   - February 2013: This specification was approved through :pep:`427`.
   - February 2021: The rules on escaping in wheel filenames were revised, to bring them into line with what popular tools actually do.


附录
========

**Appendix**

.. tab:: 中文

   urlsafe-base64-nopad 实现示例::

      # urlsafe-base64-nopad for Python 3
      import base64

      def urlsafe_b64encode_nopad(data):
         return base64.urlsafe_b64encode(data).rstrip(b'=')

      def urlsafe_b64decode_nopad(data):
         pad = b'=' * (4 - (len(data) & 3))
         return base64.urlsafe_b64decode(data + pad)

.. tab:: 英文

   Example urlsafe-base64-nopad implementation::

      # urlsafe-base64-nopad for Python 3
      import base64

      def urlsafe_b64encode_nopad(data):
         return base64.urlsafe_b64encode(data).rstrip(b'=')

      def urlsafe_b64decode_nopad(data):
         pad = b'=' * (4 - (len(data) & 3))
         return base64.urlsafe_b64decode(data + pad)
