.. highlight:: json

.. _direct-url-data-structure:

=========================
直接 URL 数据结构
=========================

**Direct URL Data Structure**

.. tab:: 中文

  本文档指定了一种可 JSON 序列化的抽象数据结构，用于表示指向 Python 项目和分发工件的 URL，例如 VCS 源树、本地源树、源分发和轮子文件。

  在撰写时，尚未正式指定如何将该数据结构的各部分合并为一个可以传递给工具的单一 URL。一种常见的表示方式是 pip URL 格式（ `VCS 支持 <pip-vcs-support_>`_ ），其他示例见 :ref:`版本说明符规范 <version-specifiers>`。

.. tab:: 英文

  This document specifies a JSON-serializable abstract data structure that can represent
  URLs to python projects and distribution artifacts such as VCS source trees, local
  source trees, source distributions and wheels.

  At time of writing, it is not formally specified how to merge the parts of this
  data structure into a single URL that can be passed to tools. A common representation is the
  pip URL format (`VCS Support <pip-vcs-support_>`_), other examples are provided in the
  :ref:`Version specifier specification <version-specifiers>`.

规范
=============

**Specification**

.. tab:: 中文

  Direct URL 数据结构必须是一个字典，能够根据 :rfc:`8259` 序列化为 JSON。

  它必须包含至少两个字段。第一个字段是 ``url`` ，类型为 ``string``。其内容必须是一个有效的 URL，符合 `WHATWG URL 标准 <whatwg-url-standard_>`_。

  根据 ``url`` 所指向的内容，第二个字段必须是以下之一：

  - ``vcs_info`` （如果 ``url`` 是 VCS 引用）
  - ``archive_info`` （如果 ``url`` 是源归档或轮子文件）
  - ``dir_info`` （如果 ``url`` 是本地目录）

  这些信息字段的值是一个（可能为空的）子字典，其中的可能键在下文定义。

.. tab:: 英文

  The Direct URL Data Structure MUST be a dictionary, serializable to JSON according to
  :rfc:`8259`.

  It MUST contain at least two fields. The first one is ``url``, with
  type ``string``. Its content must be a valid URL according to the
  `WHATWG URL Standard <whatwg-url-standard_>`_.

  Depending on what ``url`` refers to, the second field MUST be one of ``vcs_info``
  (if ``url`` is a VCS reference), ``archive_info`` (if
  ``url`` is a source archive or a wheel), or ``dir_info`` (if ``url``  is a
  local directory). These info fields have a (possibly empty) subdictionary as
  value, with the possible keys defined below.

安全注意事项
-----------------------

**Security Considerations**

.. tab:: 中文

  出于安全原因，持久化时，``url`` 必须去除任何敏感的认证信息。

  然而，URL 的 ``user:password`` 部分可以由环境变量组成，符合以下正则表达式：

  .. code-block:: text

      \$\{[A-Za-z0-9-_]+\}(:\$\{[A-Za-z0-9-_]+\})?

  此外，URL 的 ``user:password`` 部分也可以是一个众所周知的、非敏感的字符串。典型的例子是对于 URL 如 ``ssh://git@gitlab.com/user/repo``，``git`` 就是一个非敏感的字符串。

.. tab:: 英文

  When persisted, ``url`` MUST be stripped of any sensitive authentication information,
  for security reasons.

  The user:password section of the URL MAY however
  be composed of environment variables, matching the following regular
  expression:

  .. code-block:: text

      \$\{[A-Za-z0-9-_]+\}(:\$\{[A-Za-z0-9-_]+\})?

  Additionally, the user:password section of the URL MAY be a
  well-known, non security sensitive string. A typical example is ``git``
  in the case of a URL such as ``ssh://git@gitlab.com/user/repo``.

VCS URL
--------

**VCS URLs**

.. tab:: 中文

  当 ``url`` 指向一个 VCS 仓库时，``vcs_info`` 键必须作为一个字典存在，并包含以下键：

  - 一个 ``vcs`` 键（类型为 ``string``）必须存在，包含 VCS 的名称（即 ``git``、``hg``、``bzr``、``svn`` 之一）。其他 VCS 应该通过撰写 PEP 来注册，以修订此规范。
    ``url`` 的值必须与相应的 VCS 兼容，以便安装工具可以将其直接传递给 VCS 的检出/下载命令，无需转换。
  - 一个可选的 ``requested_revision`` 键（类型为 ``string``），用于命名一个分支/标签/引用/提交/修订等（格式必须与 VCS 兼容）。此字段必须与用户请求的修订匹配，并且在用户没有选择特定修订时不得存在。
  - 一个 ``commit_id`` 键（类型为 ``string``）必须存在，包含要安装的精确提交/修订号。
    如果 VCS 支持基于提交哈希的修订标识符，则应使用该提交哈希作为 ``commit_id``，以引用源代码的不可变版本。

.. tab:: 英文

  When ``url`` refers to a VCS repository, the ``vcs_info`` key MUST be present
  as a dictionary with the following keys:

  - A ``vcs`` key (type ``string``) MUST be present, containing the name of the VCS
    (i.e. one of ``git``, ``hg``, ``bzr``, ``svn``). Other VCS's SHOULD be registered by
    writing a PEP to amend this specification.
    The ``url`` value MUST be compatible with the corresponding VCS,
    so an installer can hand it off without transformation to a
    checkout/download command of the VCS.
  - A ``requested_revision`` key (type ``string``) MAY be present naming a
    branch/tag/ref/commit/revision/etc (in a format compatible with the VCS). This field
    MUST match the revision requested by the user and MUST NOT exist when the user did
    not select a specific revision.
  - A ``commit_id`` key (type ``string``) MUST be present, containing the
    exact commit/revision number that was/is to be installed.
    If the VCS supports commit-hash
    based revision identifiers, such commit-hash MUST be used as
    ``commit_id`` in order to reference an immutable
    version of the source code.

存档 URL
------------

**Archive URLs**

.. tab:: 中文

  当 ``url`` 指向一个源归档或 wheel 文件时，``archive_info`` 键必须作为一个字典存在，并包含以下键：

  - 一个 ``hashes`` 键，应该作为一个字典，映射哈希名称到文件的十六进制编码摘要。

    可以包括多个哈希，消费者可以自行决定如何处理多个哈希（它可以验证所有哈希，或其中的一部分，或者完全不验证）。

    这些哈希名称应该始终标准化为小写。

    可以使用通过 :py:mod:`hashlib` 提供的任何哈希算法（特别是任何可以传递给 :py:func:`hashlib.new()` 且不需要额外参数的算法）作为哈希字典的键。应该始终包括至少一个来自 :py:data:`hashlib.algorithms_guaranteed` 的安全算法。撰写时，推荐使用 ``sha256``。

  - 一个已弃用的 ``hash`` 键（类型为 ``string``）可以存在，用于向后兼容，值为 ``<hash-algorithm>=<expected-hash>``。

  数据结构的生产者应当在有一个或多个哈希可用时发出 ``hashes`` 键。生产者应当继续在之前使用 ``hash`` 键的上下文中发出 ``hash`` 键，以保持对现有客户端的向后兼容性。

  当 ``hash`` 和 ``hashes`` 键都存在时，``hash`` 键表示的哈希也必须存在于 ``hashes`` 字典中，以便消费者可以仅在 ``hashes`` 键存在时考虑使用它，若 ``hashes`` 键不存在，则回退使用 ``hash`` 键。

.. tab:: 英文

  When ``url`` refers to a source archive or a wheel, the ``archive_info`` key
  MUST be present as a dictionary with the following keys:

  - A ``hashes`` key SHOULD be present as a dictionary mapping a hash name to a hex
    encoded digest of the file.

    Multiple hashes can be included, and it is up to the consumer to decide what to do
    with multiple hashes (it may validate all of them or a subset of them, or nothing at
    all).

    These hash names SHOULD always be normalized to be lowercase.

    Any hash algorithm available via :py:mod:`hashlib` (specifically any that can be passed to
    :py:func:`hashlib.new()` and do not require additional parameters) can be used as a key for
    the hashes dictionary. At least one secure algorithm from
    :py:data:`hashlib.algorithms_guaranteed` SHOULD always be included. At time of writing,
    ``sha256`` specifically is recommended.

  - A deprecated ``hash`` key (type ``string``) MAY be present for backwards compatibility
    purposes, with value ``<hash-algorithm>=<expected-hash>``.

  Producers of the data structure SHOULD emit the ``hashes`` key whether one or multiple
  hashes are available. Producers SHOULD continue to emit the ``hash`` key in contexts
  where they did so before, so as to keep backwards compatibility for existing clients.

  When both the ``hash`` and ``hashes`` keys are present, the hash represented in the
  ``hash`` key MUST also be present in the ``hashes`` dictionary, so consumers can
  consider the ``hashes`` key only if it is present, and fall back to ``hash`` otherwise.

本地目录
-----------------

**Local directories**

.. tab:: 中文

  当 ``url`` 指向一个本地目录时， ``dir_info`` 键必须作为一个字典存在，并包含以下键：

  - ``editable`` （类型： ``boolean`` ）：如果分发版是在可编辑模式下安装的，则为 ``true``，否则为 ``false``。如果该键不存在，则默认为 ``false``。

  当 ``url`` 指向本地目录时，必须使用 ``file`` 协议并符合 :rfc:`8089`。特别是，路径部分必须是绝对路径。创建绝对路径时，应当保留符号链接。

.. tab:: 英文

  When ``url`` refers to a local directory, the ``dir_info`` key MUST be
  present as a dictionary with the following key:

  - ``editable`` (type: ``boolean``): ``true`` if the distribution was/is to be installed
    in editable mode, ``false`` otherwise. If absent, default to ``false``.

  When ``url`` refers to a local directory, it MUST have the ``file`` scheme and
  be compliant with :rfc:`8089`. In
  particular, the path component must be absolute. Symbolic links SHOULD be
  preserved when making relative paths absolute.

子目录中的项目
--------------------------

**Projects in subdirectories**

.. tab:: 中文

  一个顶层的 ``subdirectory`` 字段可以存在，该字段包含相对于 VCS 仓库、源档案或本地目录根目录的目录路径，用于指定 ``pyproject.toml`` 或 ``setup.py`` 文件的位置。

.. tab:: 英文

  A top-level ``subdirectory`` field MAY be present containing a directory path,
  relative to the root of the VCS repository, source archive or local directory,
  to specify where ``pyproject.toml`` or ``setup.py`` is located.

已注册的 VCS
==============

**Registered VCS**

.. tab:: 中文

  本节列出了已注册的 VCS（版本控制系统）；扩展的 VCS 特定信息，说明如何使用 ``vcs``、``requested_revision`` 和 ``vcs_info`` 的其他字段；以及在某些情况下需要额外的 VCS 特定字段。

  工具可以支持其他 VCS，尽管建议通过编写 PEP 来修改此规范以注册这些 VCS。``vcs`` 字段应该是命令名称（小写）。为了支持此类 VCS，必要的额外字段应以 VCS 命令名称为前缀。

.. tab:: 英文

  This section lists the registered VCS's; expanded, VCS-specific information
  on how to use the ``vcs``, ``requested_revision``, and other fields of
  ``vcs_info``; and in
  some cases additional VCS-specific fields.
  Tools MAY support other VCS's although it is RECOMMENDED to register
  them by writing a PEP to amend this specification. The ``vcs`` field SHOULD be the command name
  (lowercased). Additional fields that would be necessary to
  support such VCS SHOULD be prefixed with the VCS command name.

Git
---

**Git**

.. tab:: 中文

  主页
    https://git-scm.com/

  vcs 命令
    git

  ``vcs`` 字段
    git

  ``requested_revision`` 字段
    标签名、分支名、Git 引用、提交哈希、简短的提交哈希或其他提交类型。

  ``commit_id`` 字段
    提交哈希（40 个十六进制字符的 sha1）。

  .. note::

    工具可以使用 ``git show-ref`` 和 ``git symbolic-ref`` 命令来确定 ``requested_revision`` 是否对应 Git 引用。
    反过来，以 ``refs/tags/`` 开头的引用对应于标签，以 ``refs/remotes/origin/`` 开头的引用则对应于克隆后的分支。

.. tab:: 英文

  Home page
    https://git-scm.com/

  vcs command
    git

  ``vcs`` field
    git

  ``requested_revision`` field
    A tag name, branch name, Git ref, commit hash, shortened commit hash,
    or other commit-ish.

  ``commit_id`` field
    A commit hash (40 hexadecimal characters sha1).

  .. note::

    Tools can use the ``git show-ref`` and ``git symbolic-ref`` commands
    to determine if the ``requested_revision`` corresponds to a Git ref.
    In turn, a ref beginning with ``refs/tags/`` corresponds to a tag, and
    a ref beginning with ``refs/remotes/origin/`` after cloning corresponds
    to a branch.

Mercurial
---------

**Mercurial**

.. tab:: 中文

  主页
    https://www.mercurial-scm.org/

  vcs 命令
    hg

  ``vcs`` 字段
    hg

  ``requested_revision`` 字段
    标签名、分支名、变更集 ID、简短的变更集 ID。

  ``commit_id`` 字段
    变更集 ID（40 个十六进制字符）。

.. tab:: 英文

  Home page
    https://www.mercurial-scm.org/

  vcs command
    hg

  ``vcs`` field
    hg

  ``requested_revision`` field
    A tag name, branch name, changeset ID, shortened changeset ID.

  ``commit_id`` field
    A changeset ID (40 hexadecimal characters).

Bazaar
------

**Bazaar**

.. tab:: 中文

  主页
    https://www.breezy-vcs.org/

  vcs 命令
    bzr

  ``vcs`` 字段
    bzr

  ``requested_revision`` 字段
    标签名、分支名、修订 ID。

  ``commit_id`` 字段
    修订 ID。

.. tab:: 英文

  Home page
    https://www.breezy-vcs.org/

  vcs command
    bzr

  ``vcs`` field
    bzr

  ``requested_revision`` field
    A tag name, branch name, revision id.

  ``commit_id`` field
    A revision id.

Subversion
----------

**Subversion**

.. tab:: 中文

  主页
    https://subversion.apache.org/

  vcs 命令
    svn

  ``vcs`` 字段
    svn

  ``requested_revision`` 字段
    ``requested_revision`` 必须与 ``svn checkout`` ``--revision`` 选项兼容。
    在 Subversion 中，分支或标签是 URL 的一部分。

  ``commit_id`` 字段
    由于 Subversion 不支持全局唯一标识符，
    因此此字段是对应仓库中的 Subversion 修订号。

.. tab:: 英文

  Home page
    https://subversion.apache.org/

  vcs command
    svn

  ``vcs`` field
    svn

  ``requested_revision`` field
    ``requested_revision`` must be compatible with ``svn checkout`` ``--revision`` option.
    In Subversion, branch or tag is part of ``url``.

  ``commit_id`` field
    Since Subversion does not support globally unique identifiers,
    this field is the Subversion revision number in the corresponding
    repository.

JSON 架构
===========

**JSON Schema**

.. tab:: 中文

  以下 JSON Schema 可用于验证 ``direct_url.json`` 的内容：

.. tab:: 英文

  The following JSON Schema can be used to validate the contents of ``direct_url.json``:

.. code-block::

     {
       "$schema": "https://json-schema.org/draft/2019-09/schema",
       "title": "Direct URL Data",
       "description": "Data structure that can represent URLs to python projects and distribution artifacts such as VCS source trees, local source trees, source distributions and wheels.",
       "definitions": {
         "URL": {
           "type": "string",
           "format": "uri"
         },
         "DirInfo": {
           "type": "object",
           "properties": {
             "editable": {
               "type": ["boolean", "null"]
             }
           }
         },
         "VCSInfo": {
           "type": "object",
           "properties": {
             "vcs": {
               "type": "string",
               "enum": [
                 "git",
                 "hg",
                 "bzr",
                 "svn"
               ]
             },
             "requested_revision": {
               "type": "string"
             },
             "commit_id": {
               "type": "string"
             },
             "resolved_revision": {
               "type": "string"
             }
           },
           "required": [
             "vcs",
             "commit_id"
           ]
         },
         "ArchiveInfo": {
           "type": "object",
           "properties": {
             "hash": {
               "type": "string",
               "pattern": "^\\w+=[a-f0-9]+$",
               "deprecated": true
             },
             "hashes": {
               "type": "object",
               "patternProperties": {
                 "^[a-f0-9]+$": {
                   "type": "string"
                 }
               }
             }
           }
         }
       },
       "allOf": [
         {
           "type": "object",
           "properties": {
             "url": {
               "$ref": "#/definitions/URL"
             }
           },
           "required": [
             "url"
           ]
         },
         {
           "anyOf": [
             {
               "type": "object",
               "properties": {
                 "dir_info": {
                   "$ref": "#/definitions/DirInfo"
                 }
               },
               "required": [
                 "dir_info"
               ]
             },
             {
               "type": "object",
               "properties": {
                 "vcs_info": {
                   "$ref": "#/definitions/VCSInfo"
                 }
               },
               "required": [
                 "vcs_info"
               ]
             },
             {
               "type": "object",
               "properties": {
                 "archive_info": {
                   "$ref": "#/definitions/ArchiveInfo"
                 }
               },
               "required": [
                 "archive_info"
               ]
             }
           ]
         }
       ]
     }

示例
========

**Examples**

.. tab:: 中文

  源档案:

  .. code::

      {
          "url": "https://github.com/pypa/pip/archive/1.3.1.zip",
          "archive_info": {
              "hashes": {
                  "sha256": "2dc6b5a470a1bde68946f263f1af1515a2574a150a30d6ce02c6ff742fcc0db8"
              }
          }
      }

  带有标签和提交哈希的 Git URL:

  .. code::

      {
          "url": "https://github.com/pypa/pip.git",
          "vcs_info": {
              "vcs": "git",
              "requested_revision": "1.3.1",
              "commit_id": "7921be1537eac1e97bc40179a57f0349c2aee67d"
          }
      }

  本地目录:

  .. code::

    {
        "url": "file:///home/user/project",
        "dir_info": {}
    }

  可编辑模式下的本地目录:

  .. code::

    {
        "url": "file:///home/user/project",
        "dir_info": {
            "editable": true
        }
    }

.. tab:: 英文

  Source archive:

  .. code::

      {
          "url": "https://github.com/pypa/pip/archive/1.3.1.zip",
          "archive_info": {
              "hashes": {
                  "sha256": "2dc6b5a470a1bde68946f263f1af1515a2574a150a30d6ce02c6ff742fcc0db8"
              }
          }
      }

  Git URL with tag and commit-hash:

  .. code::

      {
          "url": "https://github.com/pypa/pip.git",
          "vcs_info": {
              "vcs": "git",
              "requested_revision": "1.3.1",
              "commit_id": "7921be1537eac1e97bc40179a57f0349c2aee67d"
          }
      }

  Local directory:

  .. code::

    {
        "url": "file:///home/user/project",
        "dir_info": {}
    }

  Local directory in editable mode:

  .. code::

    {
        "url": "file:///home/user/project",
        "dir_info": {
            "editable": true
        }
    }


历史
=======

**History**

.. tab:: 中文

  - 2020年3月：该规范通过 :pep:`610` 获得批准，定义了 ``direct_url.json`` 元数据文件。
  - 2023年1月：添加了 ``archive_info.hashes`` 键（ `讨论 <archive-info-hashes_>`_ ）。

.. tab:: 英文

  - March 2020: This specification was approved through :pep:`610`, defining the ``direct_url.json`` metadata file.
  - January 2023: Added the ``archive_info.hashes`` key (`discussion <archive-info-hashes_>`_).



.. _archive-info-hashes: https://discuss.python.org/t/22299
.. _pip-vcs-support: https://pip.pypa.io/en/stable/topics/vcs-support/
.. _whatwg-url-standard: https://url.spec.whatwg.org/
