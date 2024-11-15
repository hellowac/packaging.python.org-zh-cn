.. _`well-known-project-urls`:

===================================
元数据中的知名项目 URL
===================================

**Well-known Project URLs in Metadata**

.. tab:: 中文

  .. important::

      本文档主要面向元数据 *消费者*，
      他们应使用下面的标准化规则和常见标签，
      以确保项目 URL 在 Python 生态系统中的展示一致。

      元数据 *生产者*（例如构建工具和单个包维护者）可以继续使用他们喜欢的任何标签，
      但需遵守 ``Project-URL`` 字段长度限制。然而，尽可能地，鼓励用户选择具有意义的标签，
      并使其标准化为常见标签。

  .. note::

      请参见 :ref:`编写你的 pyproject.toml - urls <writing-pyproject-toml-urls>`
      以获取关于如何在包的元数据中选择项目 URL 标签的用户导向指南。

  .. note:: 本规范最初在 :pep:`753` 中定义。

  :pep:`753` 将弃用 :ref:`core-metadata-home-page` 和
  :ref:`core-metadata-download-url` 元数据字段，改为使用
  :ref:`core-metadata-project-url`，并定义了一个标准化和查找程序，
  用于确定 ``Project-URL`` 是否为“常见的”，即是否具有 ``Home-page``、
  ``Download-URL`` 或其他常见项目 URL 的语义。

  这使得索引（如 Python 包索引）和其他下游元数据消费者
  能够以一致的方式展示项目 URL。

.. tab:: 英文

  .. important::

      This document is primarily of interest to metadata *consumers*,
      who should use the normalization rules and well-known list below
      to make their presentation of project URLs consistent across the
      Python ecosystem.

      Metadata *producers* (such as build tools and individual package
      maintainers) may continue to use any labels they please, within the
      overall ``Project-URL`` length restrictions. However, when possible, users are
      *encouraged* to pick meaningful labels that normalize to well-known
      labels.

  .. note::

      See :ref:`Writing your pyproject.toml - urls <writing-pyproject-toml-urls>`
      for user-oriented guidance on choosing project URL labels in your package's
      metadata.

  .. note:: This specification was originally defined in :pep:`753`.

  :pep:`753` deprecates the :ref:`core-metadata-home-page` and
  :ref:`core-metadata-download-url` metadata fields in favor of
  :ref:`core-metadata-project-url`, and defines a normalization and
  lookup procedure for determining whether a ``Project-URL`` is
  "well-known," i.e. has the semantics assigned to ``Home-page``,
  ``Download-URL``, or other common project URLs.

  This allows indices (such as the Python Package Index) and other downstream
  metadata consumers to present project URLs in a
  consistent manner.

.. _project-url-label-normalization:

标签规范化
===================

**Label normalization**

.. tab:: 中文

  .. note::

      标签标准化由元数据 *消费者* 执行，而非元数据
      生产者。

  为了确定一个 ``Project-URL`` 标签是否为“常见的”，元数据消费者
  应在将其与 :ref:`常见标签列表 <well-known-labels>` 进行比较之前，
  对标签进行标准化处理。

  ``Project-URL`` 标签的标准化过程由以下 Python 函数定义：

  .. code-block:: python

      import string

      def normalize_label(label: str) -> str:
          chars_to_remove = string.punctuation + string.whitespace
          removal_map = str.maketrans("", "", chars_to_remove)
          return label.translate(removal_map).lower()

  用简单的语言来说：标签通过删除所有 ASCII 标点符号
  和空白字符，然后将结果转换为小写来进行 *标准化*。

  下表展示了标签标准化前（原始）和标准化后（规范化）的示例：

  .. list-table::
      :header-rows: 1

      * - 原始
        - 规范化
      * - ``Homepage``
        - ``homepage``
      * - ``Home-page``
        - ``homepage``
      * - ``Home page``
        - ``homepage``
      * - ``Change_Log``
        - ``changelog``
      * - ``What's New?``
        - ``whatsnew``
      * - ``github``
        - ``github``

.. tab:: 英文

  .. note::

      Label normalization is performed by metadata *consumers*, not metadata
      producers.

  To determine whether a ``Project-URL`` label is "well-known," metadata
  consumers should normalize the label before comparing it to the
  :ref:`list of well-known labels <well-known-labels>`.

  The normalization procedure for ``Project-URL`` labels is defined
  by the following Python function:

  .. code-block:: python

      import string

      def normalize_label(label: str) -> str:
          chars_to_remove = string.punctuation + string.whitespace
          removal_map = str.maketrans("", "", chars_to_remove)
          return label.translate(removal_map).lower()

  In plain language: a label is *normalized* by deleting all ASCII punctuation
  and whitespace, and then converting the result to lowercase.

  The following table shows examples of labels before (raw) and after
  normalization:

  .. list-table::
      :header-rows: 1

      * - Raw
        - Normalized
      * - ``Homepage``
        - ``homepage``
      * - ``Home-page``
        - ``homepage``
      * - ``Home page``
        - ``homepage``
      * - ``Change_Log``
        - ``changelog``
      * - ``What's New?``
        - ``whatsnew``
      * - ``github``
        - ``github``

.. _well-known-labels:

知名标签
=================

**Well-known labels**

.. tab:: 中文

  .. note::

      常见标签列表是一个持续更新的标准，作为本文档的一部分进行维护。

  以下表格列出了用于专门化 ``Project-URL`` 元数据展示的常见标签：

  .. list-table::
    :header-rows: 1

    * - 标签（人类可读等效）
      - 描述
      - 别名
    * - ``homepage`` (主页)
      - 项目的主页
      - *(无)*
    * - ``source`` (源代码)
      - 项目托管的源代码或代码库
      - ``repository``, ``sourcecode``, ``github``
    * - ``download`` (下载)
      - 当前发行版的下载 URL，等同于 ``Download-URL``
      - *(无)*
    * - ``changelog`` (更新日志)
      - 项目的完整更新日志
      - ``changes``, ``whatsnew``, ``history``
    * - ``releasenotes`` (发布说明)
      - 项目的策划发布说明
      - *(无)*
    * - ``documentation`` (文档)
      - 项目的在线文档
      - ``docs``
    * - ``issues`` (问题追踪)
      - 项目的 bug 跟踪器
      - ``bugs``, ``issue``, ``tracker``, ``issuetracker``, ``bugtracker``
    * - ``funding`` (资助)
      - 资助信息
      - ``sponsor``, ``donate``, ``donation``

  包的元数据消费者可以选择将别名标签与其“父”常见标签相同地呈现，或进一步专门化它们。

.. tab:: 英文

  .. note::

      The list of well-known labels is a living standard, maintained as part of
      this document.

  The following table lists labels that are well-known for the purpose of
  specializing the presentation of ``Project-URL`` metadata:

  .. list-table::
    :header-rows: 1

    * - Label (Human-readable equivalent)
      - Description
      - Aliases
    * - ``homepage`` (Homepage)
      - The project's home page
      - *(none)*
    * - ``source`` (Source Code)
      - The project's hosted source code or repository
      - ``repository``, ``sourcecode``, ``github``
    * - ``download`` (Download)
      - A download URL for the current distribution, equivalent to ``Download-URL``
      - *(none)*
    * - ``changelog`` (Changelog)
      - The project's comprehensive changelog
      - ``changes``, ``whatsnew``, ``history``
    * - ``releasenotes`` (Release Notes)
      - The project's curated release notes
      - *(none)*
    * - ``documentation`` (Documentation)
      - The project's online documentation
      - ``docs``
    * - ``issues`` (Issue Tracker)
      - The project's bug tracker
      - ``bugs``, ``issue``, ``tracker``, ``issuetracker``, ``bugtracker``
    * - ``funding`` (Funding)
      - Funding Information
      - ``sponsor``, ``donate``, ``donation``

  Package metadata consumers may choose to render aliased labels the same as
  their "parent" well known label, or further specialize them.

示例行为
================

**Example behavior**

.. tab:: 中文

  以下展示了项目 URL 元数据从 ``pyproject.toml`` 到核心元数据，再到潜在索引展示的流向：

  .. code-block:: toml
      :caption: 标准配置中的示例项目 URL

      [project.urls]
      "Home Page" = "https://example.com"
      DOCUMENTATION = "https://readthedocs.org"
      Repository = "https://upstream.example.com/me/spam.git"
      GitHub = "https://github.com/example/spam"

  .. code-block:: email
      :caption: 核心元数据表示

      Project-URL: Home page, https://example.com
      Project-URL: DOCUMENTATION, https://readthedocs.org
      Project-URL: Repository, https://upstream.example.com/me/spam.git
      Project-URL: GitHub, https://github.com/example/spam

  .. code-block:: text
      :caption: 潜在展示

      Homepage: https://example.com
      Documentation: https://readthedocs.org
      Source Code: https://upstream.example.com/me/spam.git
      Source Code (GitHub): https://github.com/example/spam

  请注意，核心元数据以用户提供的形式出现
  （因为元数据 *生产者* 不执行标准化），但元数据 *消费者* 会标准化并基于标准化形式
  识别适当的人类可读等效项：

  * ``Home page`` 变为 ``homepage``, 并呈现为 ``Homepage``
  * ``DOCUMENTATION`` 变为 ``documentation``, 并呈现为 ``Documentation``
  * ``Repository`` 变为 ``repository``, 并呈现为 ``Source Code``
  * ``GitHub`` 变为 ``github``, 并呈现为 ``Source Code (GitHub)``
    （作为 ``Source Code`` 的一种专门化）

.. tab:: 英文

  The following shows the flow of project URL metadata from
  ``pyproject.toml`` to core metadata to a potential index presentation:

  .. code-block:: toml
      :caption: Example project URLs in standard configuration

      [project.urls]
      "Home Page" = "https://example.com"
      DOCUMENTATION = "https://readthedocs.org"
      Repository = "https://upstream.example.com/me/spam.git"
      GitHub = "https://github.com/example/spam"

  .. code-block:: email
      :caption: Core metadata representation

      Project-URL: Home page, https://example.com
      Project-URL: DOCUMENTATION, https://readthedocs.org
      Project-URL: Repository, https://upstream.example.com/me/spam.git
      Project-URL: GitHub, https://github.com/example/spam

  .. code-block:: text
      :caption: Potential rendering

      Homepage: https://example.com
      Documentation: https://readthedocs.org
      Source Code: https://upstream.example.com/me/spam.git
      Source Code (GitHub): https://github.com/example/spam

  Observe that the core metadata appears in the form provided by the user
  (since metadata *producers* do not perform normalization), but the
  metadata *consumer* normalizes and identifies appropriate
  human-readable equivalents based on the normalized form:

  * ``Home page`` becomes ``homepage``, which is rendered as ``Homepage``
  * ``DOCUMENTATION`` becomes ``documentation``, which is rendered as ``Documentation``
  * ``Repository`` becomes ``repository``, which is rendered as ``Source Code``
  * ``GitHub`` becomes ``github``, which is rendered as ``Source Code (GitHub)``
    (as a specialization of ``Source Code``)
