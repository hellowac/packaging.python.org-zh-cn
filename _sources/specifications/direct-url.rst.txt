
.. _direct-url:

==========================================================
记录已安装发行版的直接 URL 来源
==========================================================

**Recording the Direct URL Origin of installed distributions**

.. tab:: 中文

  本文档指定了一个位于已安装发行版的 ``*.dist-info`` 目录中的 :file:`direct_url.json` 文件，用于记录发行版的直接 URL 来源。``*.dist-info`` 目录的一般结构和使用方式在 :ref:`recording-installed-packages` 中描述。

.. tab:: 英文

  This document specifies a :file:`direct_url.json` file in the
  ``*.dist-info`` directory of an installed distribution, to record the
  Direct URL Origin of the distribution. The general structure and usage of
  ``*.dist-info`` directories is described in :ref:`recording-installed-packages`.


规范
=============

**Specification**

.. tab:: 中文

  当从指定直接 URL 引用（包括 VCS URL）的要求安装发行版时，安装工具 **必须** 在 ``*.dist-info`` 目录中创建 :file:`direct_url.json` 文件。

  当从其他类型的要求（即名称加版本说明符）安装发行版时，**不得** 创建此文件。

  此 JSON 文件 **必须** 是一个 UTF-8 编码的、符合 :rfc:`8259` 的序列化版本，表示 :doc:`direct-url-data-structure`。

  .. note::

    当请求的 URL 使用 file:// 协议并指向一个恰好包含 VCS 检出内容的本地目录时，安装工具 **不得** 尝试推断任何 VCS 信息，因此 **不得** 在 :file:`direct_url.json` 文件中输出任何与 VCS 相关的信息（如 ``vcs_info``）。

  .. note::

    一般来说，安装工具应尽可能保留请求的 URL 中提供的信息，生成 :file:`direct_url.json` 时。例如，环境变量中的用户名:密码应被保留，而 ``requested_revision`` 应尽可能忠实地反映请求 URL 中提供的修订版本。然而，这些信息应通过更精确的数据进行 **补充**，如 ``commit_id``。

.. tab:: 英文

  The :file:`direct_url.json` file MUST be created in the :file:`*.dist-info`
  directory by installers when installing a distribution from a requirement
  specifying a direct URL reference (including a VCS URL).

  This file MUST NOT be created when installing a distribution from an other
  type of requirement (i.e. name plus version specifier).

  This JSON file MUST be a UTF-8 encoded, :rfc:`8259` compliant, serialization of the
  :doc:`direct-url-data-structure`.

  .. note::

    When the requested URL has the file:// scheme and points to a local directory that happens to contain a
    VCS checkout, installers MUST NOT attempt to infer any VCS information and
    therefore MUST NOT output any VCS related information (such as ``vcs_info``)
    in :file:`direct_url.json`.

  .. note::

    As a general rule, installers should as much as possible preserve the
    information that was provided in the requested URL when generating
    :file:`direct_url.json`. For example user:password environment variables
    should be preserved and ``requested_revision`` should reflect the revision that was
    provided in the requested URL as faithfully as possible. This information is
    however *enriched* with more precise data, such as ``commit_id``.


示例 pip 命令及其对 direct_url.json 的影响
========================================================

**Example pip commands and their effect on direct_url.json**

.. tab:: 中文

  生成 ``direct_url.json`` 的命令：

  * ``pip install https://example.com/app-1.0.tgz``
  * ``pip install https://example.com/app-1.0.whl``
  * ``pip install "app @ git+https://example.com/repo/app.git#subdirectory=setup"``
  * ``pip install ./app``
  * ``pip install file:///home/user/app``
  * ``pip install --editable "app @ git+https://example.com/repo/app.git#subdirectory=setup"``
    （在这种情况下，``url`` 将是 git 仓库克隆到的本地目录，``dir_info`` 将包含 ``"editable": true``，且不会设置 ``vcs_info``）
  * ``pip install -e ./app``

  *不生成 ``direct_url.json`` 的命令*：

  * ``pip install app``
  * ``pip install app --no-index --find-links https://example.com/``

.. tab:: 英文

  Commands that generate a ``direct_url.json``:

  * ``pip install https://example.com/app-1.0.tgz``
  * ``pip install https://example.com/app-1.0.whl``
  * ``pip install "app @ git+https://example.com/repo/app.git#subdirectory=setup"``
  * ``pip install ./app``
  * ``pip install file:///home/user/app``
  * ``pip install --editable "app @ git+https://example.com/repo/app.git#subdirectory=setup"``
    (in which case, ``url`` will be the local directory where the git repository has been
    cloned to, and ``dir_info`` will be present with ``"editable": true`` and no
    ``vcs_info`` will be set)
  * ``pip install -e ./app``

  Commands that *do not* generate a ``direct_url.json``

  * ``pip install app``
  * ``pip install app --no-index --find-links https://example.com/``


历史记录
=======

**History**

.. tab:: 中文

  - 2020 年 3 月：该规范已通过 :pep:`610` 批准。

.. tab:: 英文

  - March 2020: This specification was approved through :pep:`610`.
