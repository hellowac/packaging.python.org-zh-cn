
.. _simple-repository-api:

=====================
Simple 仓库 API
=====================

**Simple repository API**

.. tab:: 中文

  查询可用包版本和从索引服务器检索包的界面有两种形式：HTML 和 JSON。

.. tab:: 英文

  The interface for querying available package versions and
  retrieving packages from an index server comes in two forms:
  HTML and JSON.

.. _simple-repository-api-base:

基本 HTML API
=============

**Base HTML API**

.. tab:: 中文

  一个实现 Simple API 的仓库通过其基本 URL 定义，这个 URL 是所有附加 URL 所在的顶层 URL。这个 API 被称为 "simple" 仓库，因为 PyPI 的基本 URL 是 ``https://pypi.org/simple/``。

  .. note:: 本文档中后续出现的所有 URL 都将相对于这个基本 URL（因此给定 PyPI 的 URL，``/foo/`` 的 URL 将是 ``https://pypi.org/simple/foo/``）。

  在仓库中，根 URL（本规范中的 ``/`` 代表基本 URL） **必须** 是一个有效的 HTML5 页面，每个项目都应有一个单独的锚点元素。锚点的文本 **必须** 是项目的名称，href 属性 **必须** 链接到该项目的 URL。示例如下::

    <!DOCTYPE html>
    <html>
      <body>
        <a href="/frob/">frob</a>
        <a href="/spamspamspam/">spamspamspam</a>
      </body>
    </html>

  在根 URL 之下，每个仓库中的单独项目将有一个对应的 URL。该 URL 的格式为 ``/<project>/``，其中 ``<project>`` 替换为该项目的规范化名称，因此一个名为 "HolyGrail" 的项目将有一个类似 ``/holygrail/`` 的 URL。该 URL 必须返回一个有效的 HTML5 页面，每个文件都有一个单独的锚点元素。锚点的 href 属性 **必须** 是指向该文件下载位置的 URL，并且锚点文本 **必须** 与该 URL 的最后一个路径组件（即文件名）匹配。该 URL **应该** 包括一个哈希，格式为 URL 片段，语法为：``#<hashname>=<hashvalue>``，其中 ``<hashname>`` 是哈希函数的名称（如 ``sha256``），``<hashvalue>`` 是十六进制编码的摘要。

  除了上述要求外，API 还具有以下约束：

  * 所有返回 HTML5 页面的 URL **必须** 以 ``/`` 结尾，并且仓库 **应该** 将没有 ``/`` 结尾的 URL 重定向到带 ``/`` 的 URL。

  * URL 可以是绝对 URL 或相对 URL，只要它们指向正确的位置。

  * 对文件必须托管在仓库相对位置的地方没有任何约束。

  * API 页面可以有其他 HTML 元素，只要包含所需的锚点元素。

  * 仓库 **可以** 将未规范化的 URL 重定向到规范化的 URL（例如，``/Foobar/`` 可以重定向到 ``/foobar/``），然而客户端 **不得** 依赖此重定向， **必须** 请求规范化的 URL。

  * 仓库 **应该** 选择一个哈希函数，该哈希函数在 Python 标准库的 :py:mod:`hashlib` 模块中可以保证可用（目前为 ``md5``、``sha1``、``sha224``、``sha256``、``sha384``、``sha512``）。当前推荐使用 ``sha256``。

  * 如果某个分发文件有 GPG 签名， **必须** 将其与该文件一起存放，文件名相同，但附加 ``.asc`` 后缀。因此，如果文件 ``/packages/HolyGrail-1.0.tar.gz`` 存在并且有关联的签名，则签名应位于 ``/packages/HolyGrail-1.0.tar.gz.asc``。

  * 仓库 **可以** 在文件链接上包括 ``data-gpg-sig`` 属性，值为 ``true`` 或 ``false``，以指示是否存在 GPG 签名。做此操作的仓库 **应该** 在每个链接上都包括此属性。

  * 仓库 **可以** 在文件链接上包括 ``data-requires-python`` 属性。这将公开相应发布的 :ref:`core-metadata-requires-python` 元数据字段。若该属性存在，安装工具 **应该** 在安装时忽略不满足要求的 Python 版本。例如::

        <a href="..." data-requires-python="&gt;=3">...</a>

    在属性值中，``<`` 和 ``>`` 必须分别 HTML 编码为 ``&lt;`` 和 ``&gt;``。

.. tab:: 英文

  A repository that implements the simple API is defined by its base URL, this is
  the top level URL that all additional URLs are below. The API is named the
  "simple" repository due to the fact that PyPI's base URL is
  ``https://pypi.org/simple/``.

  .. note:: All subsequent URLs in this document will be relative to this base
            URL (so given PyPI's URL, a URL of ``/foo/`` would be
            ``https://pypi.org/simple/foo/``.


  Within a repository, the root URL (``/`` for this spec which represents the base
  URL) **MUST** be a valid HTML5 page with a single anchor element per project in
  the repository. The text of the anchor tag **MUST** be the name of
  the project and the href attribute **MUST** link to the URL for that particular
  project. As an example::

    <!DOCTYPE html>
    <html>
      <body>
        <a href="/frob/">frob</a>
        <a href="/spamspamspam/">spamspamspam</a>
      </body>
    </html>

  Below the root URL is another URL for each individual project contained within
  a repository. The format of this URL is ``/<project>/`` where the ``<project>``
  is replaced by the normalized name for that project, so a project named
  "HolyGrail" would have a URL like ``/holygrail/``. This URL must respond with
  a valid HTML5 page with a single anchor element per file for the project. The
  href attribute **MUST** be a URL that links to the location of the file for
  download, and the text of the anchor tag **MUST** match the final path
  component (the filename) of the URL. The URL **SHOULD** include a hash in the
  form of a URL fragment with the following syntax: ``#<hashname>=<hashvalue>``,
  where ``<hashname>`` is the lowercase name of the hash function (such as
  ``sha256``) and ``<hashvalue>`` is the hex encoded digest.

  In addition to the above, the following constraints are placed on the API:

  * All URLs which respond with an HTML5 page **MUST** end with a ``/`` and the
    repository **SHOULD** redirect the URLs without a ``/`` to add a ``/`` to the
    end.

  * URLs may be either absolute or relative as long as they point to the correct
    location.

  * There are no constraints on where the files must be hosted relative to the
    repository.

  * There may be any other HTML elements on the API pages as long as the required
    anchor elements exist.

  * Repositories **MAY** redirect unnormalized URLs to the canonical normalized
    URL (e.g. ``/Foobar/`` may redirect to ``/foobar/``), however clients
    **MUST NOT** rely on this redirection and **MUST** request the normalized
    URL.

  * Repositories **SHOULD** choose a hash function from one of the ones
    guaranteed to be available via the :py:mod:`hashlib` module in the Python standard
    library (currently ``md5``, ``sha1``, ``sha224``, ``sha256``, ``sha384``,
    ``sha512``). The current recommendation is to use ``sha256``.

  * If there is a GPG signature for a particular distribution file it **MUST**
    live alongside that file with the same name with a ``.asc`` appended to it.
    So if the file ``/packages/HolyGrail-1.0.tar.gz`` existed and had an
    associated signature, the signature would be located at
    ``/packages/HolyGrail-1.0.tar.gz.asc``.

  * A repository **MAY** include a ``data-gpg-sig`` attribute on a file link with
    a value of either ``true`` or ``false`` to indicate whether or not there is a
    GPG signature. Repositories that do this **SHOULD** include it on every link.

  * A repository **MAY** include a ``data-requires-python`` attribute on a file
    link. This exposes the :ref:`core-metadata-requires-python` metadata field
    for the corresponding release. Where this is present, installer tools
    **SHOULD** ignore the download when installing to a Python version that
    doesn't satisfy the requirement. For example::

        <a href="..." data-requires-python="&gt;=3">...</a>

    In the attribute value, < and > have to be HTML encoded as ``&lt;`` and
    ``&gt;``, respectively.

规范化名称
----------------

**Normalized Names**

.. tab:: 中文

  本规范引用了“规范化”项目名称的概念。根据 :ref:`名称规范化规范 <name-normalization>`，名称中唯一有效的字符是 ASCII 字母、ASCII 数字、``.``、``-`` 和 ``_``。名称应该小写，并且所有连续的 ``.``、``-`` 或 ``_`` 字符应替换为单个 ``-`` 字符。这可以通过 Python 中的 ``re`` 模块实现::

    import re

    def normalize(name):
        return re.sub(r"[-_.]+", "-", name).lower()

.. tab:: 英文

  This spec references the concept of a "normalized" project name. As per
  :ref:`the name normalization specification <name-normalization>`
  the only valid characters in a name are the ASCII alphabet, ASCII numbers,
  ``.``, ``-``, and ``_``. The name should be lowercased with all runs of the
  characters ``.``, ``-``, or ``_`` replaced with a single ``-`` character. This
  can be implemented in Python with the ``re`` module::

    import re

    def normalize(name):
        return re.sub(r"[-_.]+", "-", name).lower()

.. _simple-repository-api-yank:

向 Simple API 添加“Yank”支持
=======================================

**Adding "Yank" Support to the Simple API**

.. tab:: 中文

  简单仓库中的链接 **MAY** 包含一个 ``data-yanked`` 属性，该属性可以没有值，也可以有一个任意的字符串值。存在 ``data-yanked`` 属性时， **SHOULD** 解释为指示该链接指向的文件已被“撤回”（"Yanked"），并且通常不应被安装程序选择，除非在特定情况下。

  如果存在， ``data-yanked`` 属性的值是一个任意字符串，表示文件被撤回的原因。处理简单仓库 API 的工具 **MAY** 将此字符串展示给最终用户。

  一旦设置，撤回属性不是不可变的，未来可以被撤销（并且一旦撤销，也可以被重新设置）。因此，API 用户 **MUST** 能够处理被撤回的文件被“恢复” （甚至再次被撤回）的情况。

.. tab:: 英文

  Links in the simple repository **MAY** have a ``data-yanked`` attribute
  which may have no value, or may have an arbitrary string as a value. The
  presence of a ``data-yanked`` attribute **SHOULD** be interpreted as
  indicating that the file pointed to by this particular link has been
  "Yanked", and should not generally be selected by an installer, except
  under specific scenarios.

  The value of the ``data-yanked`` attribute, if present, is an arbitrary
  string that represents the reason for why the file has been yanked. Tools
  that process the simple repository API **MAY** surface this string to
  end users.

  The yanked attribute is not immutable once set, and may be rescinded in
  the future (and once rescinded, may be reset as well). Thus API users
  **MUST** be able to cope with a yanked file being "unyanked" (and even
  yanked again).


安装程序
----------

**Installers**

.. tab:: 中文

  理想的用户体验是，一旦文件被撤回，当用户试图直接安装一个已被撤回的文件时，安装应当失败，就像该文件已被删除一样。然而，当用户在一段时间前进行此操作，而现在计算机只是继续机械地按照原始顺序安装现已被撤回的文件时，安装应当表现得就像文件没有被撤回一样。

  如果选择约束可以通过非撤回版本来满足，安装程序 **MUST** 忽略撤回的版本；如果这意味着无法完全满足请求，安装程序 **MAY** 拒绝使用撤回的版本。实现 **SHOULD** 选择遵循上述意图精神的策略，避免对撤回版本/文件的“新”依赖。

  这意味着的具体做法留给具体的安装程序决定，如何最好地融入其安装程序的整体使用。然而，有两种建议的方法：

  1. 撤回的文件始终被忽略，除非它是唯一符合精确版本指定符的文件，该版本指定符使用 ``==`` （没有任何使其成为范围的修饰符，例如  ``.*`` ）或 ``===``。匹配该版本指定符应按 :ref:`版本指定符规范 <version-specifiers>` 进行，考虑到本地版本、零填充等情况。
  2. 撤回的文件始终被忽略，除非它是唯一符合锁定文件（如 ``Pipfile.lock`` 或 ``poetry.lock``）中指定要安装的文件。在这种情况下，从某个输入文件或命令创建或更新锁定文件时，**SHOULD** 不使用撤回的文件。

  无论安装程序选择何种策略来决定何时安装撤回文件，安装程序 **SHOULD** 在决定安装撤回文件时发出警告。该警告 **MAY** 利用 ``data-yanked`` 属性的值（如果有值）来向用户提供更具体的反馈，解释该文件为何被撤回。

.. tab:: 英文

  The desirable experience for users is that once a file is yanked, when
  a human being is currently trying to directly install a yanked file, that
  it fails as if that file had been deleted. However, when a human did that
  awhile ago, and now a computer is just continuing to mechanically follow
  the original order to install the now yanked file, then it acts as if it
  had not been yanked.

  An installer **MUST** ignore yanked releases, if the selection constraints
  can be satisfied with a non-yanked version, and **MAY** refuse to use a
  yanked release even if it means that the request cannot be satisfied at all.
  An implementation **SHOULD** choose a policy that follows the spirit of the
  intention above, and that prevents "new" dependencies on yanked
  releases/files.

  What this means is left up to the specific installer, to decide how to best
  fit into the overall usage of their installer. However, there are two
  suggested approaches to take:

  1. Yanked files are always ignored, unless they are the only file that
    matches a version specifier that "pins" to an exact version using
    either ``==`` (without any modifiers that make it a range, such as
    ``.*``) or ``===``. Matching this version specifier should otherwise
    be done as per :ref:`the version specifiers specification
    <version-specifiers>` for things like local versions, zero padding,
    etc.
  2. Yanked files are always ignored, unless they are the only file that
    matches what a lock file (such as ``Pipfile.lock`` or ``poetry.lock``)
    specifies to be installed. In this case, a yanked file **SHOULD** not
    be used when creating or updating a lock file from some input file or
    command.

  Regardless of the specific strategy that an installer chooses for deciding
  when to install yanked files, an installer **SHOULD** emit a warning when
  it does decide to install a yanked file. That warning **MAY** utilize the
  value of the ``data-yanked`` attribute (if it has a value) to provide more
  specific feedback to the user about why that file had been yanked.


镜像
-------

**Mirrors**

.. tab:: 中文

  镜像通常可以以两种方式处理撤回的文件：

  1. 它们可以选择完全从其简单仓库 API 中省略撤回的文件，只提供一个显示“活动的”、未撤回文件的仓库视图。
  2. 它们可以选择包含撤回的文件，并且还要镜像 ``data-yanked`` 属性。

  镜像 **不得** 在未镜像 ``data-yanked`` 属性的情况下镜像已撤回的文件。

.. tab:: 英文

  Mirrors can generally treat yanked files one of two ways:

  1. They may choose to omit them from their simple repository API completely,
    providing a view over the repository that shows only "active", unyanked
    files.
  2. They may choose to include yanked files, and additionally mirror the
    ``data-yanked`` attribute as well.

  Mirrors **MUST NOT** mirror a yanked file without also mirroring the ``data-yanked`` attribute for it.

.. _simple-repository-api-versioning:

PyPI Simple API 版本控制
============================

**Versioning PyPI's Simple API**

.. tab:: 中文

  此规范提议在每个成功请求到简单 API 页面时的响应中包含一个 meta 标签，该标签具有名称属性 "pypi:repository-version"，其内容是一个与 :ref:`版本说明符规范 <version-specifiers>` 兼容的版本号，进一步限制为仅支持 Major.Minor 版本，且不包含 :ref:`版本说明符规范 <version-specifiers>` 支持的附加功能。

  最终效果类似于::

    <meta name="pypi:repository-version" content="1.0">

  解释仓库版本时：

  * 增加主版本号用于表示不兼容的变化，这样现有客户端将不再能够有意义地使用该 API。
  * 增加次版本号用于表示向后兼容的变化，这样现有客户端仍然可以有意义地使用该 API。

  具体什么变化构成不兼容或兼容的变化，留给未来规范的裁定，基本的建议是现有客户端将能够“有意义”地继续使用该 API，这可以包括添加、修改或删除现有功能。

  本规范期望主版本号永远不会增加，任何未来的主要 API 进化将采用不同的机制来演化 API。然而，包含主版本号是为了与未来的版本区分开（例如，假设的简单 API v2 位于 /v2/，但如果仓库版本设置为 >= 2，则会造成混淆）。

  本规范将当前 API 版本设置为 "1.0"，并期望未来进一步演化简单 API 的规范将增加次版本号。

.. tab:: 英文

  This spec proposes the inclusion of a meta tag on the responses of every
  successful request to a simple API page, which contains a name attribute
  of "pypi:repository-version", and a content that is a :ref:`version specifiers
  specification <version-specifiers>` compatible
  version number, which is further constrained to ONLY be Major.Minor, and
  none of the additional features supported by :ref:`the version specifiers
  specification <version-specifiers>`.

  This would end up looking like::

    <meta name="pypi:repository-version" content="1.0">

  When interpreting the repository version:

  * Incrementing the major version is used to signal a backwards
    incompatible change such that existing clients would no longer be
    expected to be able to meaningfully use the API.
  * Incrementing the minor version is used to signal a backwards
    compatible change such that existing clients would still be
    expected to be able to meaningfully use the API.

  It is left up to the discretion of any future specs as to what
  specifically constitutes a backwards incompatible vs compatible change
  beyond the broad suggestion that existing clients will be able to
  "meaningfully" continue to use the API, and can include adding,
  modifying, or removing existing features.

  It is expectation of this spec that the major version will never be
  incremented, and any future major API evolutions would utilize a
  different mechanism for API evolution. However the major version
  is included to disambiguate with future versions (e.g. a hypothetical
  simple api v2 that lived at /v2/, but which would be confusing if the
  repository-version was set to a version >= 2).

  This spec sets the current API version to "1.0", and expects that
  future specs that further evolve the simple API will increment the
  minor version number.


客户端
-------

**Clients**

.. tab:: 中文

  与简单 API 交互的客户端 **SHOULD** 检查每个响应中的仓库版本，如果该数据不存在，**MUST** 假定版本为 1.0。

  当遇到大于预期的主版本时，客户端 **MUST** 以适当的错误信息硬性失败，并向用户显示。

  当遇到大于预期的次版本时，客户端 **SHOULD** 向用户发出适当的警告信息。

  客户端 **MAY** 仍然可以继续使用特性检测来确定仓库使用的功能。

.. tab:: 英文

  Clients interacting with the simple API **SHOULD** introspect each
  response for the repository version, and if that data does not exist
  **MUST** assume that it is version 1.0.

  When encountering a major version greater than expected, clients
  **MUST** hard fail with an appropriate error message for the user.

  When encountering a minor version greater than expected, clients
  **SHOULD** warn users with an appropriate message.

  Clients **MAY** still continue to use feature detection in order to
  determine what features a repository uses.

.. _simple-repository-api-metadata-file:

在 Simple Repository API 中提供分发元数据
========================================================

**Serve Distribution Metadata in the Simple Repository API**

.. tab:: 中文

  在简单仓库的项目页面中，每个指向分发包的锚标签 **可能(MAY)** 具有一个 ``data-dist-info-metadata`` 属性。该属性的存在表示，锚标签所指示的分发包 **必须(MUST)** 包含一个核心元数据文件，该文件在分发包处理和/或安装时不会被修改。

  如果存在 ``data-dist-info-metadata`` 属性，则仓库 **必须(MUST)** 将分发包的核心元数据文件与分发包一起提供，并在文件名末尾添加 ``.metadata``。例如，服务于 ``/files/distribution-1.0-py3.none.any.whl`` 的分发包的核心元数据文件将位于 ``/files/distribution-1.0-py3.none.any.whl.metadata``。这类似于 :ref:`基础 HTML API 规范 <simple-repository-api-base>` 中指定 GPG 签名文件的位置。

  仓库 **应该(SHOULD)** 提供核心元数据文件的哈希值，作为 ``data-dist-info-metadata`` 属性的值，采用语法 ``<hashname>=<hashvalue>``，其中 ``<hashname>`` 是所用哈希函数的小写名称，``<hashvalue>`` 是十六进制编码的摘要。如果无法提供哈希，仓库 **可能(MAY)** 使用 ``true`` 作为该属性的值。

.. tab:: 英文

  In a simple repository's project page, each anchor tag pointing to a
  distribution **MAY** have a ``data-dist-info-metadata`` attribute. The
  presence of the attribute indicates the distribution represented by
  the anchor tag **MUST** contain a Core Metadata file that will not be
  modified when the distribution is processed and/or installed.

  If a ``data-dist-info-metadata`` attribute is present, the repository
  **MUST** serve the distribution's Core Metadata file alongside the
  distribution with a ``.metadata`` appended to the distribution's file
  name. For example, the Core Metadata of a distribution served at
  ``/files/distribution-1.0-py3.none.any.whl`` would be located at
  ``/files/distribution-1.0-py3.none.any.whl.metadata``. This is similar
  to how :ref:`the base HTML API specification <simple-repository-api-base>`
  specifies the GPG signature file's location.

  The repository **SHOULD** provide the hash of the Core Metadata file
  as the ``data-dist-info-metadata`` attribute's value using the syntax
  ``<hashname>=<hashvalue>``, where ``<hashname>`` is the lower cased
  name of the hash function used, and ``<hashvalue>`` is the hex encoded
  digest. The repository **MAY** use ``true`` as the attribute's value
  if a hash is unavailable.

向后兼容性
-----------------------

**Backwards Compatibility**

.. tab:: 中文

  如果锚标签缺少 ``data-dist-info-metadata`` 属性，工具应该恢复其当前行为，即下载分发包以检查元数据。

  不支持新 ``data-dist-info-metadata`` 属性的旧版工具应该忽略该属性，并保持其当前行为，即下载分发包以检查元数据。这类似于先前的 ``data-`` 属性添加期望现有工具操作的方式。

.. tab:: 英文

  If an anchor tag lacks the ``data-dist-info-metadata`` attribute,
  tools are expected to revert to their current behaviour of downloading
  the distribution to inspect the metadata.

  Older tools not supporting the new ``data-dist-info-metadata``
  attribute are expected to ignore the attribute and maintain their
  current behaviour of downloading the distribution to inspect the
  metadata. This is similar to how prior ``data-`` attribute additions
  expect existing tools to operate.

.. _simple-repository-api-json:

基于 JSON 的 Python 包索引 Simple API
================================================

**JSON-based Simple API for Python Package Indexes**

.. tab:: 中文

  为了仅使用标准库启用响应解析，本规范规定，所有响应（除了文件本身和 :ref:`基本 HTML API 规范 <simple-repository-api-base>` 中的 HTML 响应）应使用 `JSON <https://www.json.org/>`_ 序列化。

  为了实现零配置发现并最小化额外的 HTTP 请求，本规范扩展了 :ref:`基本 HTML API 规范 <simple-repository-api-base>`，使得所有 API 端点（文件本身除外）将利用 HTTP 内容协商，使客户端和服务器能够选择正确的序列化格式进行传输，即 HTML 或 JSON。

.. tab:: 英文

  To enable response parsing with only the standard library, this spec specifies that
  all responses (besides the files themselves, and the HTML responses from
  :ref:`the base HTML API specification <simple-repository-api-base>`) should be
  serialized using `JSON <https://www.json.org/>`_.

  To enable zero configuration discovery and to minimize the amount of additional HTTP
  requests, this spec extends :ref:`the base HTML API specification
  <simple-repository-api-base>` such that all of the API endpoints (other than the
  files themselves) will utilize HTTP content negotiation to allow client and server to
  select the correct serialization format to serve, i.e. either HTML or JSON.


版本控制
----------

**Versioning**

.. tab:: 中文

  版本控制将遵循 :ref:`API 版本控制规范 <simple-repository-api-versioning>` 格式（``Major.Minor``），该规范已将现有的 HTML 响应定义为 ``1.0``。由于本规范并未为 API 引入新特性，而是描述了现有特性的不同序列化格式，因此本规范不会更改现有的 ``1.0`` 版本，而只是描述如何将其序列化为 JSON。

  类似于 :ref:`API 版本控制规范 <simple-repository-api-versioning>`，如果新的格式变更会导致现有客户端无法继续有效理解该格式，则 **必须(MUST)** 增加主版本号。

  同样，如果格式中添加或移除了特性，但现有客户端仍然能够继续有效理解该格式，则 **必须(MUST)** 增加次版本号。

  不会导致现有客户端无法有效理解格式的变更，并且没有涉及特性添加或移除的，可以在不更改版本号的情况下进行。

  这一点故意模糊，因为本规范认为，最好的做法是由未来对 API 进行更改的规范来调查并决定是否应该增加主版本号或次版本号。

  API 的未来版本可能会添加一些只能在该版本的部分可用序列化方式中表示的内容。所有序列化的版本号，在同一主版本内， **应该(SHOULD)** 保持同步，但如何将特性序列化到每种格式的具体方式可能会有所不同，包括是否该特性在某些格式中完全缺失。

  本规范的意图是将 API 视为返回数据的 URL 端点，其解释由该数据的版本定义，然后序列化为目标序列化格式。

.. tab:: 英文

  Versioning will adhere to :ref:`the API versioning specification
  <simple-repository-api-versioning>` format (``Major.Minor``), which has defined the
  existing HTML responses to be ``1.0``. Since this spec does not introduce new features
  into the API, rather it describes a different serialization format for the existing
  features, this spec does not change the existing ``1.0`` version, and instead just
  describes how to serialize that into JSON.

  Similar to :ref:`the API versioning specification
  <simple-repository-api-versioning>`, the major version number **MUST** be
  incremented if any
  changes to the new format would result in no longer being able to expect existing
  clients to meaningfully understand the format.

  Likewise, the minor version **MUST** be incremented if features are
  added or removed from the format, but existing clients would be expected to continue
  to meaningfully understand the format.

  Changes that would not result in existing clients being unable to meaningfully
  understand the format and which do not represent features being added or removed
  may occur without changing the version number.

  This is intentionally vague, as this spec believes it is best left up to future specs
  that make any changes to the API to investigate and decide whether or not that
  change should increment the major or minor version.

  Future versions of the API may add things that can only be represented in a subset
  of the available serializations of that version. All serializations version numbers,
  within a major version, **SHOULD** be kept in sync, but the specifics of how a
  feature serializes into each format may differ, including whether or not that feature
  is present at all.

  It is the intent of this spec that the API should be thought of as URL endpoints that
  return data, whose interpretation is defined by the version of that data, and then
  serialized into the target serialization format.


.. _json-serialization:

JSON 序列化
------------------

**JSON Serialization**

.. tab:: 中文

  来自 :ref:`基础 HTML API 规范 <simple-repository-api-base>` 的 URL 结构仍然适用，因为本规范仅为已存在的 API 添加了额外的序列化格式。

  以下约束适用于本规范中描述的所有 JSON 序列化响应：

  * 所有 JSON 响应将 *始终* 是一个 JSON 对象，而不是数组或其他类型。

  * 虽然 JSON 原生不支持 URL 类型，但在此 API 中表示 URL 的任何值可以是绝对的或相对的，只要它们指向正确的位置。如果是相对路径，它们是相对于当前 URL 的，就像在 HTML 中一样。

  * 可以向 API 响应中的任何字典对象添加额外的键，且客户端 **必须(MUST)** 忽略它们不理解的键。

  * 所有 JSON 响应将包含一个 ``meta`` 键，该键包含与响应本身相关的信息，而不是响应内容。

  * 所有 JSON 响应将包含一个 ``meta.api-version`` 键，该键是一个字符串，包含 :ref:`API 版本控制规范 <simple-repository-api-versioning>` 中的 ``Major.Minor`` 版本号，并具有与 :ref:`API 版本控制规范 <simple-repository-api-versioning>` 中定义的相同的失败/警告语义。

  * 所有与 :ref:`基础 HTML API 规范 <simple-repository-api-base>` 相关的要求（非 HTML 特定要求）仍然适用。

.. tab:: 英文

  The URL structure from :ref:`the base HTML API specification
  <simple-repository-api-base>` still applies, as this spec only adds an additional
  serialization format for the already existing API.

  The following constraints apply to all JSON serialized responses described in this
  spec:

  * All JSON responses will *always* be a JSON object rather than an array or other
    type.

  * While JSON doesn't natively support a URL type, any value that represents an
    URL in this API may be either absolute or relative as long as they point to
    the correct location. If relative, they are relative to the current URL as if
    it were HTML.

  * Additional keys may be added to any dictionary objects in the API responses
    and clients **MUST** ignore keys that they don't understand.

  * All JSON responses will have a ``meta`` key, which contains information related to
    the response itself, rather than the content of the response.

  * All JSON responses will have a ``meta.api-version`` key, which will be a string that
    contains the :ref:`API versioning specification
    <simple-repository-api-versioning>` ``Major.Minor`` version number, with the
    same fail/warn semantics as defined in :ref:`the API versioning specification
    <simple-repository-api-versioning>`.

  * All requirements of :ref:`the base HTML API specification
    <simple-repository-api-base>` that are not HTML specific still apply.


项目列表
~~~~~~~~~~~~

**Project List**

.. tab:: 中文

  此规范的根 URL ``/`` （代表基础 URL）将是一个 JSON 编码的字典，包含两个键：

  - ``projects``：一个数组，其中每个条目是一个字典，字典中有一个键 ``name``，表示项目名称的字符串。
  - ``meta``：通用响应元数据，如 `前面描述的 <json-serialization_>`__。

  例如：

  .. code-block:: json

      {
        "meta": {
          "api-version": "1.0"
        },
        "projects": [
          {"name": "Frob"},
          {"name": "spamspamspam"}
        ]
      }

  .. note::

    ``name`` 字段与 :ref:`基础 HTML API 规范 <simple-repository-api-base>` 中的字段相同，该规范未明确规定它是未规范化的显示名称还是规范化名称。实际上，这些规范的不同实现对此有不同的选择，因此依赖于它是未规范化还是规范化的名称实际上是依赖于特定仓库的实现细节。

  .. note::

    虽然 ``projects`` 键是一个数组，因此需要按某种顺序排列，但 :ref:`基础 HTML API 规范 <simple-repository-api-base>` 和本规范都没有要求任何特定的排序，也没有要求排序在每次请求之间保持一致。从逻辑上讲，这最好被认为是一个集合，但 JSON 和 HTML 都不支持集合功能。

.. tab:: 英文

  The root URL ``/`` for this spec (which represents the base URL) will be a JSON encoded
  dictionary which has a two keys:

  - ``projects``: An array where each entry is a dictionary with a single key, ``name``, which represents string of the project name.
  - ``meta``: The general response metadata as `described earlier <json-serialization_>`__.

  As an example:

  .. code-block:: json

      {
        "meta": {
          "api-version": "1.0"
        },
        "projects": [
          {"name": "Frob"},
          {"name": "spamspamspam"}
        ]
      }


  .. note::

    The ``name`` field is the same as the one from :ref:`the base HTML API
    specification <simple-repository-api-base>`, which does not specify
    whether it is the non-normalized display name or the normalized name. In practice
    different implementations of these specs are choosing differently here, so relying
    on it being either non-normalized or normalized is relying on an implementation
    detail of the repository in question.


  .. note::

    While the ``projects`` key is an array, and thus is required to be in some kind
    of an order, neither :ref:`the base HTML API specification
    <simple-repository-api-base>` nor this spec requires any specific ordering nor
    that the ordering is consistent from one request to the next. Mentally this is
    best thought of as a set, but both JSON and HTML lack the functionality to have
    sets.


项目详细信息
~~~~~~~~~~~~~~

**Project Detail**

.. tab:: 中文

  该 URL 的格式为 ``/<project>/``，其中 ``<project>`` 被替换为
  :ref:`基础 HTML API 规范 <simple-repository-api-base>` 中该项目的规范化名称，
  例如，名为 "Silly_Walk" 的项目将具有类似 ``/silly-walk/`` 的 URL。

  此 URL 必须响应一个 JSON 编码的字典，包含以下三个键：

  - ``name``：项目的规范化名称。
  - ``files``：一个字典列表，每个字典表示一个单独的文件。
  - ``meta``：通用响应元数据，如 `前面描述的 <json-serialization_>`__。

  每个单独文件的字典包含以下键：

  - ``filename``：表示的文件名。
  - ``url``：文件可以从中下载的 URL。
  - ``hashes``：一个字典，将哈希名称映射到文件的十六进制编码摘要。
    可以包括多个哈希，客户端可以决定如何处理多个哈希（例如，可以验证所有哈希、部分哈希或不验证任何哈希）。这些哈希名称 **SHOULD** 始终规范化为小写。

    ``hashes`` 字典 **MUST** 存在，即使文件没有可用的哈希，然而强烈建议始终至少包括一个安全且保证可用的哈希。

    默认情况下，任何通过 :py:mod:`hashlib` 模块可用的哈希算法（特别是任何可以传递给 :py:func:`hashlib.new()` 且不需要附加参数的算法）都可以作为哈希字典的键。至少应该始终包括 :py:data:`hashlib.algorithms_guaranteed` 中的一个安全算法。目前，推荐使用 ``sha256``。
  - ``requires-python``：一个 **可选** 键，暴露 :ref:`核心元数据-requires-python`
    元数据字段。如果此字段存在，则安装工具 **SHOULD** 在安装到不满足要求的 Python 版本时忽略该下载。

    与 :ref:`基础 HTML API 规范 <simple-repository-api-base>` 中的 ``data-requires-python`` 不同，``requires-python`` 键除了 JSON 自然支持的任何转义外，不需要任何特殊的转义。
  - ``dist-info-metadata``：一个 **可选** 键，表示该文件有可用的元数据，
    该元数据位于与 :ref:`API 元数据文件规范 <simple-repository-api-metadata-file>` 中指定的位置相同（``{file_url}.metadata``）。如果此字段存在，则 **MUST** 是布尔值，表示文件是否有相关的元数据文件，或者是一个字典，将哈希名称映射到元数据哈希的十六进制编码摘要。

    如果此字段是哈希字典而不是布尔值，则与 ``hashes`` 键相同的要求和建议也适用于此键。

    如果此字段缺失，则元数据文件可能存在也可能不存在。如果字段值为真值，则表示元数据文件存在；如果字段值为假值，则表示元数据文件不存在。

    建议服务器在可能的情况下提供元数据文件的哈希。
  - ``gpg-sig``：一个 **可选** 键，作为布尔值，表示文件是否有相关的 GPG 签名。
    签名文件的 URL 跟随 :ref:`基础 HTML API 规范 <simple-repository-api-base>` 中指定的位置（``{file_url}.asc``）。如果此键不存在，则签名文件可能存在也可能不存在。
  - ``yanked``：一个 **可选** 键，可以是布尔值，表示文件是否已被撤销，或是一个非空但任意的字符串，表示文件因特定原因被撤销。如果 ``yanked`` 键存在且值为真，则 **SHOULD** 解释为表示该文件的 ``url`` 字段指向的文件已被 "撤销"，符合 :ref:`API 撤销规范 <simple-repository-api-yank>`。

  例如：

  .. code-block:: json

      {
        "meta": {
          "api-version": "1.0"
        },
        "name": "holygrail",
        "files": [
          {
            "filename": "holygrail-1.0.tar.gz",
            "url": "https://example.com/files/holygrail-1.0.tar.gz",
            "hashes": {"sha256": "...", "blake2b": "..."},
            "requires-python": ">=3.7",
            "yanked": "Had a vulnerability"
          },
          {
            "filename": "holygrail-1.0-py3-none-any.whl",
            "url": "https://example.com/files/holygrail-1.0-py3-none-any.whl",
            "hashes": {"sha256": "...", "blake2b": "..."},
            "requires-python": ">=3.7",
            "dist-info-metadata": true
          }
        ]
      }

  .. note::

    虽然 ``files`` 键是一个数组，因此需要按某种顺序排列，但 :ref:`基础 HTML API 规范
    <simple-repository-api-base>` 和本规范都没有要求任何特定的排序，也没有要求排序在每次请求之间保持一致。从逻辑上讲，这最好被认为是一个集合，但 JSON 和 HTML 都不支持集合功能。

.. tab:: 英文

  The format of this URL is ``/<project>/`` where the ``<project>`` is replaced by the
  :ref:`the base HTML API specification <simple-repository-api-base>` normalized
  name for that project, so a project named "Silly_Walk" would
  have a URL like ``/silly-walk/``.

  This URL must respond with a JSON encoded dictionary that has three keys:

  - ``name``: The normalized name of the project.
  - ``files``: A list of dictionaries, each one representing an individual file.
  - ``meta``: The general response metadata as `described earlier <json-serialization_>`__.

  Each individual file dictionary has the following keys:

  - ``filename``: The filename that is being represented.
  - ``url``: The URL that the file can be fetched from.
  - ``hashes``: A dictionary mapping a hash name to a hex encoded digest of the file.
    Multiple hashes can be included, and it is up to the client to decide what to do
    with multiple hashes (it may validate all of them or a subset of them, or nothing
    at all). These hash names **SHOULD** always be normalized to be lowercase.

    The ``hashes`` dictionary **MUST** be present, even if no hashes are available
    for the file, however it is **HIGHLY** recommended that at least one secure,
    guaranteed-to-be-available hash is always included.

    By default, any hash algorithm available via :py:mod:`hashlib` (specifically any that can
    be passed to :py:func:`hashlib.new()` and do not require additional parameters) can
    be used as a key for the hashes dictionary. At least one secure algorithm from
    :py:data:`hashlib.algorithms_guaranteed` **SHOULD** always be included. At the time
    of this spec, ``sha256`` specifically is recommended.
  - ``requires-python``: An **optional** key that exposes the
    :ref:`core-metadata-requires-python`
    metadata field. Where this is present, installer tools
    **SHOULD** ignore the download when installing to a Python version that
    doesn't satisfy the requirement.

    Unlike ``data-requires-python`` in :ref:`the base HTML API specification
    <simple-repository-api-base>`, the ``requires-python`` key does not
    require any special escaping other than anything JSON does naturally.
  - ``dist-info-metadata``: An **optional** key that indicates
    that metadata for this file is available, via the same location as specified in
    :ref:`the API metadata file specification
    <simple-repository-api-metadata-file>` (``{file_url}.metadata``). Where this
    is present, it **MUST** be
    either a boolean to indicate if the file has an associated metadata file, or a
    dictionary mapping hash names to a hex encoded digest of the metadata's hash.

    When this is a dictionary of hashes instead of a boolean, then all the same
    requirements and recommendations as the ``hashes`` key hold true for this key as
    well.

    If this key is missing then the metadata file may or may not exist. If the key
    value is truthy, then the metadata file is present, and if it is falsey then it
    is not.

    It is recommended that servers make the hashes of the metadata file available if
    possible.
  - ``gpg-sig``: An **optional** key that acts a boolean to indicate if the file has
    an associated GPG signature or not. The URL for the signature file follows what
    is specified in :ref:`the base HTML API specification
    <simple-repository-api-base>` (``{file_url}.asc``). If this key does not exist, then
    the signature may or may not exist.
  - ``yanked``: An **optional** key which may be either a boolean to indicate if the
    file has been yanked, or a non empty, but otherwise arbitrary, string to indicate
    that a file has been yanked with a specific reason. If the ``yanked`` key is present
    and is a truthy value, then it **SHOULD** be interpreted as indicating that the
    file pointed to by the ``url`` field has been "Yanked" as per :ref:`the API
    yank specification <simple-repository-api-yank>`.

  As an example:

  .. code-block:: json

      {
        "meta": {
          "api-version": "1.0"
        },
        "name": "holygrail",
        "files": [
          {
            "filename": "holygrail-1.0.tar.gz",
            "url": "https://example.com/files/holygrail-1.0.tar.gz",
            "hashes": {"sha256": "...", "blake2b": "..."},
            "requires-python": ">=3.7",
            "yanked": "Had a vulnerability"
          },
          {
            "filename": "holygrail-1.0-py3-none-any.whl",
            "url": "https://example.com/files/holygrail-1.0-py3-none-any.whl",
            "hashes": {"sha256": "...", "blake2b": "..."},
            "requires-python": ">=3.7",
            "dist-info-metadata": true
          }
        ]
      }


  .. note::

    While the ``files`` key is an array, and thus is required to be in some kind
    of an order, neither :ref:`the base HTML API specification
    <simple-repository-api-base>` nor this spec requires any specific ordering nor
    that the ordering is consistent from one request to the next. Mentally this is
    best thought of as a set, but both JSON and HTML lack the functionality to have
    sets.


内容类型
-------------

**Content-Types**

.. tab:: 中文

  本规范提议，所有来自 Simple API 的响应将具有一个标准的内容类型，描述响应的类型（Simple API 响应）、所代表的 API 版本，以及使用的序列化格式。

  该内容类型的结构将是：

  .. code-block:: text

      application/vnd.pypi.simple.$version+format

  由于只有主版本可能会对试图理解这些 API 响应的客户端造成破坏性影响，因此内容类型中只会包含主版本，并且会以 ``v`` 为前缀，以澄清它是一个版本号。

  这意味着，对于现有的 1.0 版本 API，内容类型将是：

  - **JSON:** ``application/vnd.pypi.simple.v1+json``
  - **HTML:** ``application/vnd.pypi.simple.v1+html``

  除了上述内容外，还支持一个特殊的 "meta" 版本，命名为 ``latest``，其目的是允许客户端请求最新的版本，而无需事先知道该版本是什么。然而，建议客户端明确指定它们支持的版本。

  为了支持期望现有 :ref:`基础 HTML API 规范 <simple-repository-api-base>` API 响应使用 ``text/html`` 内容类型的现有客户端，本规范进一步定义 ``text/html`` 作为 ``application/vnd.pypi.simple.v1+html`` 内容类型的别名。

.. tab:: 英文

  This spec proposes that all responses from the Simple API will have a standard
  content type that describes what the response is (a Simple API response), what
  version of the API it represents, and what serialization format has been used.

  The structure of this content type will be:

  .. code-block:: text

      application/vnd.pypi.simple.$version+format

  Since only major versions should be disruptive to clients attempting to
  understand one of these API responses, only the major version will be included
  in the content type, and will be prefixed with a ``v`` to clarify that it is a
  version number.

  Which means that for the existing 1.0 API, the content types would be:

  - **JSON:** ``application/vnd.pypi.simple.v1+json``
  - **HTML:** ``application/vnd.pypi.simple.v1+html``

  In addition to the above, a special "meta" version is supported named ``latest``,
  whose purpose is to allow clients to request the absolute latest version, without
  having to know ahead of time what that version is. It is recommended however,
  that clients be explicit about what versions they support.

  To support existing clients which expect the existing :ref:`the base HTML API
  specification <simple-repository-api-base>` API responses to
  use the ``text/html`` content type, this spec further defines ``text/html`` as an alias
  for the ``application/vnd.pypi.simple.v1+html`` content type.


版本 + 格式选择
--------------------------

**Version + Format Selection**

.. tab:: 中文

  现在，由于可能存在多种序列化格式，我们需要一个机制，让客户端能够指示它们能够理解哪些序列化格式。此外，如果将来有新的主版本添加到 API 中，能够避免破坏现有客户端对先前版本的期望将会非常有益。

  为此，本规范标准化了使用 HTTP 的 `服务器驱动内容协商 <https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation>`_。

  虽然本规范不会完全描述服务器驱动内容协商的所有细节，但大致流程如下：

  1. 客户端发出一个包含 ``Accept`` 头的 HTTP 请求，该头列出了它们能够理解的所有版本+格式内容类型。
  2. 服务器检查该头，选择列表中的一个内容类型，然后使用该内容类型返回响应（如果缺少 ``Accept`` 头，则视为 ``Accept: */*``）。
  3. 如果服务器不支持 ``Accept`` 头中列出的任何内容类型，则它可以在以下三种不同选项中选择如何响应：

     a. 选择一个与客户端请求的不同的默认内容类型，并返回该内容类型的响应。
     b. 返回 HTTP ``406 Not Acceptable`` 响应，表示没有可用的请求内容类型，且服务器无法或不愿意选择一个默认的内容类型来响应。
     c. 返回 HTTP ``300 Multiple Choices`` 响应，包含一个所有可能的响应类型的列表，客户端可以选择其中之一。

  4. 客户端解析响应，处理服务器可能响应的不同类型的响应。

  本规范并未规定服务器在处理无法返回的内容类型时做出何种选择，客户端 **SHOULD** 准备好以最合适的方式处理所有可能的响应。

  然而，由于没有标准格式来解释 ``300 Multiple Choices`` 响应，本规范强烈不建议服务器使用该选项，因为客户端无法理解并选择不同的内容类型来请求。此外，客户端 *不太可能* 理解不同的内容类型，因此，最好的情况下，客户端可能会将该响应视为 ``406 Not Acceptable`` 错误。

  本规范 **要求** 如果使用了 ``latest`` 元版本，服务器 **MUST** 使用实际版本的内容类型来响应该请求（例如，一个 ``Accept: application/vnd.pypi.simple.latest+json`` 请求返回一个 ``v1.x`` 响应时，``Content-Type`` 应为 ``application/vnd.pypi.simple.v1+json``）。

  ``Accept`` 头是一个逗号分隔的内容类型列表，表示客户端理解并能够处理的内容类型。它支持三种格式来指定每个请求的内容类型：

  - ``$type/$subtype``
  - ``$type/*``
  - ``*/*``

  对于选择版本+格式，最有用的是 ``$type/$subtype``，因为这是唯一可以实际指定所需版本和格式的方式。

  ``Accept`` 头中列出的内容类型的顺序没有任何特定含义，服务器 **SHOULD** 将它们视为同等有效的响应选项。如果客户端希望指定更倾向于某个特定的内容类型而非其他类型，它们可以使用 ``Accept`` 头的 `质量值 <https://developer.mozilla.org/en-US/docs/Glossary/Quality_values>`_ 语法。

  这允许客户端在 ``Accept`` 头中为某个特定条目指定优先级，方法是附加一个 ``;q=`` 后跟一个介于 ``0`` 和 ``1`` 之间（包括 ``0`` 和 ``1``）的值，最多可以有三位小数。当解释这个值时，质量值更高的条目具有比质量值较低的条目更高的优先级，任何没有质量值的条目默认为质量值 ``1``。

  然而，客户端应记住，服务器可以自由选择它们请求的 **任何** 内容类型，而不考虑其请求的优先级，甚至可能返回一个它们 **没有** 请求的内容类型。

  为了帮助客户端确定它们从 API 请求中收到的响应内容类型，本规范要求服务器始终包括一个 ``Content-Type`` 头，指示响应的内容类型。从技术上讲，这是一项向后不兼容的变更，但实际上 `pip 已经强制执行了这一要求 <https://github.com/pypa/pip/blob/cf3696a81b341925f82f20cb527e656176987565/src/pip/_internal/index/collector.py#L123-L150>`_，因此实际破坏的风险较低。

  客户端操作的示例如下：

  .. code-block:: python

      import email.message
      import requests

      def parse_content_type(header: str) -> str:
          m = email.message.Message()
          m["content-type"] = header
          return m.get_content_type()

      # 构造我们可以接受的内容类型列表，我们希望优先
      # 获取一个使用 JSON 序列化的 v1 响应，但我们也
      # 支持使用 HTML 序列化的 v1 响应。为了兼容性，
      # 我们还请求了 text/html，但我们最不希望收到它，
      # 因为我们不知道它是否真的来自 Simple API 响应，
      # 还是由于配置错误获取的随机 HTML 页面。
      CONTENT_TYPES = [
          "application/vnd.pypi.simple.v1+json",
          "application/vnd.pypi.simple.v1+html;q=0.2",
          "text/html;q=0.01",  # 为了兼容性
      ]
      ACCEPT = ", ".join(CONTENT_TYPES)

      # 实际发送请求到 API，请求所有我们认为可接受的内容类型，
      # 并让服务器从列表中选择其中一个返回。
      resp = requests.get("https://pypi.org/simple/", headers={"Accept": ACCEPT})

      # 如果服务器不支持您请求的任何内容类型，
      # 且它选择返回 HTTP 406 错误而不是默认响应，
      # 那么这将引发关于 406 错误的异常。
      resp.raise_for_status()

      # 确定我们收到的响应类型，以确保它是我们可以支持的类型，
      # 如果是，就分发到能够理解该特定版本+序列化的函数。如果
      # 我们无法理解收到的内容类型，则会引发异常。
      content_type = parse_content_type(resp.headers.get("content-type", ""))
      match content_type:
          case "application/vnd.pypi.simple.v1+json":
              handle_v1_json(resp)
          case "application/vnd.pypi.simple.v1+html" | "text/html":
              handle_v1_html(resp)
          case _:
              raise Exception(f"未知的内容类型: {content_type}")

  如果客户端只希望支持 HTML 或只支持 JSON，那么它们只需
  从 ``Accept`` 头中删除不需要的内容类型，并将接收到它们时
  转化为错误。

.. tab:: 英文

  Now that there is multiple possible serializations, we need a mechanism to allow
  clients to indicate what serialization formats they're able to understand. In
  addition, it would be beneficial if any possible new major version to the API can
  be added without disrupting existing clients expecting the previous API version.

  To enable this, this spec standardizes on the use of HTTP's
  `Server-Driven Content Negotiation <https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation>`_.

  While this spec won't fully describe the entirety of server-driven content
  negotiation, the flow is roughly:

  1. The client makes an HTTP request containing an ``Accept`` header listing all
    of the version+format content types that they are able to understand.
  2. The server inspects that header, selects one of the listed content types,
    then returns a response using that content type (treating the absence of
    an ``Accept`` header as ``Accept: */*``).
  3. If the server does not support any of the content types in the ``Accept``
    header then they are able to choose between 3 different options for how to
    respond:

    a. Select a default content type other than what the client has requested
        and return a response with that.
    b. Return a HTTP ``406 Not Acceptable`` response to indicate that none of
        the requested content types were available, and the server was unable
        or unwilling to select a default content type to respond with.
    c. Return a HTTP ``300 Multiple Choices`` response that contains a list of
        all of the possible responses that could have been chosen.
  4. The client interprets the response, handling the different types of responses
    that the server may have responded with.

  This spec does not specify which choices the server makes in regards to handling
  a content type that it isn't able to return, and clients **SHOULD** be prepared
  to handle all of the possible responses in whatever way makes the most sense for
  that client.

  However, as there is no standard format for how a ``300 Multiple Choices``
  response can be interpreted, this spec highly discourages servers from utilizing
  that option, as clients will have no way to understand and select a different
  content-type to request. In addition, it's unlikely that the client *could*
  understand a different content type anyways, so at best this response would
  likely just be treated the same as a ``406 Not Acceptable`` error.

  This spec **does** require that if the meta version ``latest`` is being used, the
  server **MUST** respond with the content type for the actual version that is
  contained in the response
  (i.e. an ``Accept: application/vnd.pypi.simple.latest+json`` request that returns
  a ``v1.x`` response should have a ``Content-Type`` of
  ``application/vnd.pypi.simple.v1+json``).

  The ``Accept`` header is a comma separated list of content types that the client
  understands and is able to process. It supports three different formats for each
  content type that is being requested:

  - ``$type/$subtype``
  - ``$type/*``
  - ``*/*``

  For the use of selecting a version+format, the most useful of these is
  ``$type/$subtype``, as that is the only way to actually specify the version
  and format you want.

  The order of the content types listed in the ``Accept`` header does not have any
  specific meaning, and the server **SHOULD** consider all of them to be equally
  valid to respond with. If a client wishes to specify that they prefer a specific
  content type over another, they may use the ``Accept`` header's
  `quality value <https://developer.mozilla.org/en-US/docs/Glossary/Quality_values>`_
  syntax.

  This allows a client to specify a priority for a specific entry in their
  ``Accept`` header, by appending a ``;q=`` followed by a value between ``0`` and
  ``1`` inclusive, with up to 3 decimal digits. When interpreting this value,
  an entry with a higher quality has priority over an entry with a lower quality,
  and any entry without a quality present will default to a quality of ``1``.

  However, clients should keep in mind that a server is free to select **any** of
  the content types they've asked for, regardless of their requested priority, and
  it may even return a content type that they did **not** ask for.

  To aid clients in determining the content type of the response that they have
  received from an API request, this spec requires that servers always include a
  ``Content-Type`` header indicating the content type of the response. This is
  technically a backwards incompatible change, however in practice
  `pip has been enforcing this requirement <https://github.com/pypa/pip/blob/cf3696a81b341925f82f20cb527e656176987565/src/pip/_internal/index/collector.py#L123-L150>`_
  so the risks for actual breakages is low.

  An example of how a client can operate would look like:

  .. code-block:: python

      import email.message
      import requests

      def parse_content_type(header: str) -> str:
          m = email.message.Message()
          m["content-type"] = header
          return m.get_content_type()

      # Construct our list of acceptable content types, we want to prefer
      # that we get a v1 response serialized using JSON, however we also
      # can support a v1 response serialized using HTML. For compatibility
      # we also request text/html, but we prefer it least of all since we
      # don't know if it's actually a Simple API response, or just some
      # random HTML page that we've gotten due to a misconfiguration.
      CONTENT_TYPES = [
          "application/vnd.pypi.simple.v1+json",
          "application/vnd.pypi.simple.v1+html;q=0.2",
          "text/html;q=0.01",  # For legacy compatibility
      ]
      ACCEPT = ", ".join(CONTENT_TYPES)


      # Actually make our request to the API, requesting all of the content
      # types that we find acceptable, and letting the server select one of
      # them out of the list.
      resp = requests.get("https://pypi.org/simple/", headers={"Accept": ACCEPT})

      # If the server does not support any of the content types you requested,
      # AND it has chosen to return a HTTP 406 error instead of a default
      # response then this will raise an exception for the 406 error.
      resp.raise_for_status()


      # Determine what kind of response we've gotten to ensure that it is one
      # that we can support, and if it is, dispatch to a function that will
      # understand how to interpret that particular version+serialization. If
      # we don't understand the content type we've gotten, then we'll raise
      # an exception.
      content_type = parse_content_type(resp.headers.get("content-type", ""))
      match content_type:
          case "application/vnd.pypi.simple.v1+json":
              handle_v1_json(resp)
          case "application/vnd.pypi.simple.v1+html" | "text/html":
              handle_v1_html(resp)
          case _:
              raise Exception(f"Unknown content type: {content_type}")

  If a client wishes to only support HTML or only support JSON, then they would
  just remove the content types that they do not want from the ``Accept`` header,
  and turn receiving them into an error.


替代协商机制
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Alternative Negotiation Mechanisms**

.. tab:: 中文

  虽然使用 HTTP 的内容协商被认为是客户端和服务器协调的标准方式，以确保客户端能够理解 HTTP 响应，但在某些情况下，该机制可能不足以满足需求。对于这些情况，本规范提供了可选的替代协商机制，可以 *根据需要* 使用。

.. tab:: 英文

  While using HTTP's Content negotiation is considered the standard way for a client
  and server to coordinate to ensure that the client is getting an HTTP response that
  it is able to understand, there are situations where that mechanism may not be
  sufficient. For those cases this spec has alternative negotiation mechanisms that
  may *optionally* be used instead.


URL 参数
^^^^^^^^^^^^^

**URL Parameter**

.. tab:: 中文

  实现 Simple API 的服务器可以选择支持一个名为 ``format`` 的 URL 参数，允许客户端请求 URL 的特定版本。

  ``format`` 参数的值应为 **一个** 有效的内容类型。传递多个内容类型、通配符、质量值等 **不** 支持。

  支持此参数是可选的，客户端 **不应** 依赖此参数与 API 进行交互。此协商机制旨在简化浏览器中的 API 人机交互探索，或者允许文档或注释链接到特定的版本+格式。

  不支持此参数的服务器可以选择在该参数存在时返回错误，或者直接忽略该参数。

  当服务器实现此参数时， **应** 优先考虑 ``Accept`` 头中的任何值，如果服务器不支持请求的格式，可以选择回退到 ``Accept`` 头，或选择标准服务器驱动内容协商通常会有的任何错误处理方式（例如 ``406 Not Available``、 ``303 Multiple Choices`` 或选择返回的默认类型）。

.. tab:: 英文

  Servers that implement the Simple API may choose to support a URL parameter named
  ``format`` to allow the clients to request a specific version of the URL.

  The value of the ``format`` parameter should be **one** of the valid content types.
  Passing multiple content types, wild cards, quality values, etc... is **not**
  supported.

  Supporting this parameter is optional, and clients **SHOULD NOT** rely on it for
  interacting with the API. This negotiation mechanism is intended to allow for easier
  human based exploration of the API within a browser, or to allow documentation or
  notes to link to a specific version+format.

  Servers that do not support this parameter may choose to return an error when it is
  present, or they may simple ignore its presence.

  When a server does implement this parameter, it **SHOULD** take precedence over any
  values in the client's ``Accept`` header, and if the server does not support the
  requested format, it may choose to fall back to the ``Accept`` header, or choose any
  of the error conditions that standard server-driven content negotiation typically
  has (e.g. ``406 Not Available``, ``303 Multiple Choices``, or selecting a default
  type to return).


端点配置
^^^^^^^^^^^^^^^^^^^^^^

**Endpoint Configuration**

.. tab:: 中文

  这个选项从技术上讲并不是一个特殊选项，它只是使用内容协商并允许服务器选择其默认内容类型的自然结果。

  如果服务器不愿意或无法实现服务器驱动的内容协商，而是希望要求用户明确配置客户端来选择他们想要的版本，那么这是一个支持的配置。

  为了实现这一点，服务器应该为他们希望支持的每个版本+格式创建多个端点（例如 ``/simple/v1+html/`` 和/或 ``/simple/v1+json/``）。在该端点下，他们可以托管只支持一个（或一部分）内容类型的仓库副本。当客户端使用 ``Accept`` 头发出请求时，服务器可以忽略它并返回与该端点对应的内容类型。

  对于希望要求特定配置的客户端，他们可以跟踪特定仓库 URL 被配置为哪个版本+格式，并在向该服务器发出请求时，发送仅包含正确内容类型的 ``Accept`` 头。

.. tab:: 英文

  This option technically is not a special option at all, it is just a natural
  consequence of using content negotiation and allowing servers to select which of the
  available content types is their default.

  If a server is unwilling or unable to implement the server-driven content negotiation,
  and would instead rather require users to explicitly configure their client to select
  the version they want, then that is a supported configuration.

  To enable this, a server should make multiple endpoints (for instance,
  ``/simple/v1+html/`` and/or ``/simple/v1+json/``) for each version+format that they
  wish to support. Under that endpoint, they can host a copy of their repository that
  only supports one (or a subset) of the content-types. When a client makes a request
  using the ``Accept`` header, the server can ignore it and return the content type
  that corresponds to that endpoint.

  For clients that wish to require specific configuration, they can keep track of
  which version+format a specific repository URL was configured for, and when making
  a request to that server, emit an ``Accept`` header that *only* includes the correct
  content type.


TUF 支持 - PEP 458
---------------------

**TUF Support - PEP 458**

.. tab:: 中文

  :pep:`458` 要求所有 API 响应都是可哈希的，并且它们可以通过相对于仓库根目录的路径唯一标识。对于 Simple API 仓库，目标路径是我们的 API 根路径（例如 PyPI 上的 ``/simple/``）。这在使用 TUF 客户端访问 API 时会带来挑战，因为 TUF 客户端无法处理目标可能有多个不同的表示，每个表示的哈希值不同。

  :pep:`458` 并未指定 Simple API 的目标路径应该是什么，但 TUF 要求目标路径必须是“类文件的”，换句话说，路径如 ``simple/PROJECT/`` 是不可接受的，因为它技术上指向一个目录。

  幸运的是，目标路径并不 *必须* 与从 Simple API 获取的 URL 完全匹配，它可以只是一个符号，获取代码知道如何将其转换为实际需要获取的 URL。对于 HTTP 请求的其他方面，如 ``Accept`` 头，情况也是如此。

  最终，如何将目录映射到文件名超出了本规范的范围（但这将属于 :pep:`458` 的范围），本规范将推迟做出如何在 :pep:`458` 元数据中准确表示这一点的决定。

  然而，似乎当前对 pip 进行的 WIP 分支尝试实现 :pep:`458` 时，使用了类似 ``simple/PROJECT/index.html`` 的目标路径。可以通过类似 ``simple/PROJECT/vnd.pypi.simple.vN.FORMAT`` 的方式进行修改，来包括 API 版本和序列化格式。所以 v1 HTML 格式将是 ``simple/PROJECT/vnd.pypi.simple.v1.html``，v1 JSON 格式将是 ``simple/PROJECT/vnd.pypi.simple.v1.json``。

  在这种情况下，由于 ``text/html`` 是与 ``application/vnd.pypi.simple.v1+html`` 的别名，当通过 TUF 进行交互时，规范化为更明确的名称可能是最有意义的。

  同样，``latest`` 元版本不应包含在目标中，只应支持明确声明的版本。

.. tab:: 英文

  :pep:`458` requires that all API responses are hashable and that they can be uniquely
  identified by a path relative to the repository root. For a Simple API repository, the
  target path is the Root of our API (e.g. ``/simple/`` on PyPI). This creates
  challenges when accessing the API using a TUF client instead of directly using a
  standard HTTP client, as the TUF client cannot handle the fact that a target could
  have multiple different representations that all hash differently.

  :pep:`458` does not specify what the target path should be for the Simple API, but
  TUF requires that the target paths be "file-like", in other words, a path like
  ``simple/PROJECT/`` is not acceptable, because it technically points to a
  directory.

  The saving grace is that the target path does not *have* to actually match the URL
  being fetched from the Simple API, and it can just be a sigil that the fetching code
  knows how to transform into the actual URL that needs to be fetched. This same thing
  can hold true for other aspects of the actual HTTP request, such as the ``Accept``
  header.

  Ultimately figuring out how to map a directory to a filename is out of scope for this
  spec (but it would be in scope for :pep:`458`), and this spec defers making a decision
  about how exactly to represent this inside of :pep:`458` metadata.

  However, it appears that the current WIP branch against pip that attempts to implement
  :pep:`458` is using a target path like ``simple/PROJECT/index.html``. This could be
  modified to include the API version and serialization format using something like
  ``simple/PROJECT/vnd.pypi.simple.vN.FORMAT``. So the v1 HTML format would be
  ``simple/PROJECT/vnd.pypi.simple.v1.html`` and the v1 JSON format would be
  ``simple/PROJECT/vnd.pypi.simple.v1.json``.

  In this case, since ``text/html`` is an alias to ``application/vnd.pypi.simple.v1+html``
  when interacting through TUF, it likely will make the most sense to normalize to the
  more explicit name.

  Likewise the ``latest`` metaversion should not be included in the targets, only
  explicitly declared versions should be supported.

建议
---------------

**Recommendations**

.. tab:: 中文

  本节为非规范性内容，代表了规范作者认为在实现此规范时最佳的默认实现决策，但这并不代表必须遵循这些决策。

  这些决策的选择旨在最大限度地将请求迁移到 API 的最新版本，同时保持最大的兼容性。此外，还尝试使得使用 API 的过程中提供一些保护措施，促使客户端做出最佳选择。

  建议服务器：

  - 支持本规范中描述的所有 3 种内容类型，使用服务器驱动的内容协商，只要合理可行，或者至少在接收到使用 HTML 响应的非平凡流量时继续支持。

  - 当遇到不包含任何已知内容类型的 ``Accept`` 头时，服务器不应返回 ``300 Multiple Choice`` 响应，而应返回 ``406 Not Acceptable`` 响应。

    - 然而，如果选择使用端点配置，服务器应尽量返回一个 ``200 OK`` 响应，并使用该端点预期的内容类型。

  - 在选择一个可接受版本时，服务器应选择客户端支持的最高版本，具有最具表现力/功能的序列化格式，同时考虑客户端请求的具体性以及它们表达的质量优先级值，并且只有在最后的手段下才使用 ``text/html`` 内容类型。

  建议客户端：

  - 支持本规范中描述的所有 3 种内容类型，使用服务器驱动的内容协商，只要合理可行。

  - 构造 ``Accept`` 头时，包含所有支持的内容类型。

    除非有特定的实现原因，通常不应为内容类型添加质量优先级值，除非你有实现特定的原因需要服务器考虑（例如，如果你使用的是标准库的 HTML 解析器，并且担心在某些边缘情况下无法解析某些类型的 HTML 响应）。

    唯一的例外是，建议在遗留的 ``text/html`` 内容类型上添加 ``;q=0.01`` 值，除非它是你唯一请求的内容类型。

  - 明确选择要查找的版本，而不是在正常操作中使用 ``latest`` 元版本。

  - 检查响应的 ``Content-Type``，确保它与你预期的内容类型匹配。

.. tab:: 英文

  This section is non-normative, and represents what the spec authors believe to be
  the best default implementation decisions for something implementing this spec, but
  it does **not** represent any sort of requirement to match these decisions.

  These decisions have been chosen to maximize the number of requests that can be
  moved onto the newest version of an API, while maintaining the greatest amount
  of compatibility. In addition, they've also tried to make using the API provide
  guardrails that attempt to push clients into making the best choices it can.

  It is recommended that servers:

  - Support all 3 content types described in this spec, using server-driven
    content negotiation, for as long as they reasonably can, or at least as
    long as they're receiving non trivial traffic that uses the HTML responses.

  - When encountering an ``Accept`` header that does not contain any content types
    that it knows how to work with, the server should not ever return a
    ``300 Multiple Choice`` response, and instead return a ``406 Not Acceptable``
    response.

    - However, if choosing to use the endpoint configuration, you should prefer to
      return a ``200 OK`` response in the expected content type for that endpoint.

  - When selecting an acceptable version, the server should choose the highest version
    that the client supports, with the most expressive/featureful serialization format,
    taking into account the specificity of the client requests as well as any
    quality priority values they have expressed, and it should only use the
    ``text/html`` content type as a last resort.

  It is recommended that clients:

  - Support all 3 content types described in this spec, using server-driven
    content negotiation, for as long as they reasonably can.

  - When constructing an ``Accept`` header, include all of the content types
    that you support.

    You should generally *not* include a quality priority value for your content
    types, unless you have implementation specific reasons that you want the
    server to take into account (for example, if you're using the standard library
    HTML parser and you're worried that there may be some kinds of HTML responses
    that you're unable to parse in some edge cases).

    The one exception to this recommendation is that it is recommended that you
    *should* include a ``;q=0.01`` value on the legacy ``text/html`` content type,
    unless it is the only content type that you are requesting.

  - Explicitly select what versions they are looking for, rather than using the
    ``latest`` meta version during normal operation.

  - Check the ``Content-Type`` of the response and ensure it matches something
    that you were expecting.

用于包索引的 Simple API 的附加字段
========================================================

**Additional Fields for the Simple API for Package Indexes**

.. tab:: 中文

  本规范定义了简单存储库 API 的 1.1 版本。对于 HTML 版本的 API，与 1.0 版本没有变化。对于 JSON 版本的 API，做出了以下更改：

  - ``api-version`` 必须指定版本 1.1 或更高版本。
  - 在顶级新增了一个 ``versions`` 键。
  - 在 ``files`` 数据中，新增了两个 "文件信息" 键：``size`` 和 ``upload-time``。
  - 带有前导下划线的键（在任何级别）被保留为索引服务器的私有键。未来的标准不会为任何此类键分配意义。

  ``versions`` 和 ``size`` 键是必需的。``upload-time`` 键是可选的。

.. tab:: 英文

  This specification defines version 1.1 of the simple repository API. For the
  HTML version of the API, there is no change from version 1.0. For the JSON
  version of the API, the following changes are made:

  - The ``api-version`` must specify version 1.1 or later.
  - A new ``versions`` key is added at the top level.
  - Two new "file information" keys, ``size`` and ``upload-time``, are added to
    the ``files`` data.
  - Keys (at any level) with a leading underscore are reserved as private for
    index server use. No future standard will assign a meaning to any such key.

  The ``versions`` and ``size`` keys are mandatory. The ``upload-time`` key is
  optional.

版本
--------

**Versions**

.. tab:: 中文

  此外， ``versions`` 键必须在顶层存在，除了 :ref:`JSON API 规范 <simple-repository-api-json>` 中定义的 ``name``、 ``files`` 和 ``meta`` 键之外。此键必须包含一个版本字符串的列表，指定该项目上传的所有版本。该值逻辑上是一个集合，因此不能包含重复项，且值的顺序没有意义。

  ``files`` 键中列出的所有文件必须与 ``versions`` 键中的某个版本关联。 ``versions`` 键可以包含没有关联文件的版本（用于表示没有上传文件的版本，如果服务器支持这种概念）。

  请注意，由于服务器可能持有采用 :ref:`版本指定符规范（VSS） <version-specifiers>` 之前的 "遗留" 数据，因此当前不能要求版本字符串必须是有效的 VSS 版本，因此不能假设它们可以使用 VSS 规则进行排序。然而，服务器应尽可能使用规范化的 VSS 版本。

.. tab:: 英文

  An additional key, ``versions`` MUST be present at the top level, in addition to
  the keys ``name``, ``files`` and ``meta`` defined in :ref:`the JSON API
  specification <simple-repository-api-json>`. This key MUST
  contain a list of version strings specifying all of the project versions uploaded
  for this project. The value is logically a set, and as such may not contain
  duplicates, and the order of the values is not significant.

  All of the files listed in the ``files`` key MUST be associated with one of the
  versions in the ``versions`` key. The ``versions`` key MAY contain versions with
  no associated files (to represent versions with no files uploaded, if the server
  has such a concept).

  Note that because servers may hold "legacy" data from before the adoption of
  :ref:`the version specifiers specification (VSS) <version-specifiers>`, version
  strings currently cannot be required to be valid VSS versions, and therefore
  cannot be assumed to be orderable using the VSS rules. However, servers SHOULD
  use normalised VSS versions where
  possible.


附加文件信息
---------------------------

**Additional file information**

.. tab:: 中文

  在 ``files`` 键中添加了两个新键。

  - ``size``：此字段是必需的。它必须包含一个整数，表示文件的字节大小。
  - ``upload-time``：此字段是可选的。如果存在，它必须包含一个有效的 ISO 8601 日期/时间字符串，格式为 ``yyyy-mm-ddThh:mm:ss.ffffffZ``，表示文件上传到索引的时间。如 ``Z`` 后缀所示，上传时间必须使用 UTC 时区。时间戳的秒数部分（ ``.ffffff`` 部分）是可选的，如果存在，最多可包含 6 位精度。如果服务器没有记录文件的上传时间信息，可以省略 ``upload-time`` 键。

.. tab:: 英文

  Two new keys are added to the ``files`` key.

  - ``size``: This field is mandatory. It MUST contain an integer which is the
    file size in bytes.
  - ``upload-time``: This field is optional. If present, it MUST contain a valid
    ISO 8601 date/time string, in the format ``yyyy-mm-ddThh:mm:ss.ffffffZ``,
    which represents the time the file was uploaded to the index. As indicated by
    the ``Z`` suffix, the upload time MUST use the UTC timezone. The fractional
    seconds part of the timestamp (the ``.ffffff`` part) is optional, and if
    present may contain up to 6 digits of precision. If a server does not record
    upload time information for a file, it MAY omit the ``upload-time`` key.

在 Simple API 中重命名 dist-info-metadata
===========================================

**Rename dist-info-metadata in the Simple API**

.. tab:: 中文

  本文档中的关键词 "**MUST**"、"**MUST NOT**"、"**REQUIRED**"、"**SHALL**"、"**SHALL NOT**"、"**SHOULD**"、"**SHOULD NOT**"、"**RECOMMENDED**"、"**MAY**" 和 "**OPTIONAL**" 应按照 :rfc:`RFC 2119 <2119>` 中的描述进行解释。

.. tab:: 英文


  The keywords "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**",
  "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**", "**MAY**",
  and "**OPTIONAL**"" in this document are to be interpreted as described in
  :rfc:`RFC 2119 <2119>`.


服务器
-------

**Servers**

.. tab:: 中文

  当在 Simple API 的 HTML 表示中使用 :ref:`API 元数据文件规范 <simple-repository-api-metadata-file>` 元数据时，**MUST** 使用属性名 ``data-core-metadata`` 输出，且支持的值保持不变。

  当在 :ref:`JSON API 规范 <simple-repository-api-base>` 的 JSON 表示中使用 :ref:`API 元数据文件规范 <simple-repository-api-metadata-file>` 元数据时，**MUST** 使用键 ``core-metadata`` 输出，且支持的值保持不变。

  为了支持使用先前键名的客户端，HTML 表示 **MAY** 也可以使用 ``data-dist-info-metadata`` 输出，如果这样做，**MUST** 与 ``data-core-metadata`` 的值匹配。

.. tab:: 英文

  The :ref:`the API metadata file specification
  <simple-repository-api-metadata-file>` metadata, when used in the HTML
  representation of the Simple API,
  **MUST** be emitted using the attribute name ``data-core-metadata``, with the
  supported values remaining the same.

  The :ref:`the API metadata file specification
  <simple-repository-api-metadata-file>` metadata, when used in the :ref:`the
  JSON API specification <simple-repository-api-base>` JSON representation of the
  Simple API, **MUST** be emitted using the key ``core-metadata``, with the
  supported values remaining the same.

  To support clients that used the previous key names, the HTML representation
  **MAY** also be emitted using the ``data-dist-info-metadata``, and if it does
  so it **MUST** match the value of ``data-core-metadata``.



客户端
-------

**Clients**

.. tab:: 中文

  消费 Simple API 的任何 HTML 表示的客户端 **MUST** 从键 ``data-core-metadata`` 中读取 :ref:`API 元数据文件规范 <simple-repository-api-metadata-file>` 元数据，如果该键存在。如果 ``data-core-metadata`` 不存在，客户端 **MAY** 可选择使用遗留的 ``data-dist-info-metadata``。

  消费 Simple API JSON 表示的客户端 **MUST** 从键 ``core-metadata`` 中读取 :ref:`API 元数据文件规范 <simple-repository-api-metadata-file>` 元数据，如果该键存在。如果 ``core-metadata`` 不存在，客户端 **MAY** 可选择使用遗留的 ``dist-info-metadata`` 键。

.. tab:: 英文

  Clients consuming any of the HTML representations of the Simple API **MUST**
  read the :ref:`the API metadata file specification
  <simple-repository-api-metadata-file>` metadata from the key
  ``data-core-metadata`` if it is
  present. They **MAY** optionally use the legacy ``data-dist-info-metadata`` if
  it is present but ``data-core-metadata`` is not.

  Clients consuming the JSON representation of the Simple API **MUST** read the
  :ref:`the API metadata file specification
  <simple-repository-api-metadata-file>` metadata from the key ``core-metadata``
  if it is present. They
  **MAY** optionally use the legacy ``dist-info-metadata`` key if it is present
  but ``core-metadata`` is not.

历史
=======

**History**

.. tab:: 中文

  * 2015年9月：HTML 格式的初始版本，在 :pep:`503` 中
  * 2016年7月：Requires-Python 元数据，更新至 :pep:`503`
  * 2019年5月：“yank” 支持，在 :pep:`592` 中
  * 2020年7月：API 版本控制约定和元数据，并将 HTML 格式声明为 API v1，在 :pep:`629` 中
  * 2021年5月：提供独立于包的包元数据，在 :pep:`658` 中
  * 2022年5月：JSON 格式的初始版本，提供客户端选择机制，并将两种格式都声明为 API v1，在 :pep:`691` 中
  * 2022年10月：项目版本、文件大小和上传时间在 JSON 格式中的支持，在 :pep:`700` 中
  * 2023年6月：重命名提供独立于包的包元数据的字段，在 :pep:`714` 中

.. tab:: 英文

  * September 2015: initial form of the HTML format, in :pep:`503`
  * July 2016: Requires-Python metadata, in an update to :pep:`503`
  * May 2019: "yank" support, in :pep:`592`
  * July 2020: API versioning convention and metadata, and declaring the HTML
    format as API v1, in :pep:`629`
  * May 2021: providing package metadata independently from a package, in
    :pep:`658`
  * May 2022: initial form of the JSON format, with a mechanism for clients to
    choose between them, and declaring both formats as API v1, in :pep:`691`
  * October 2022: project versions and file size and upload-time in the JSON
    format, in :pep:`700`
  * June 2023: renaming the field which provides package metadata independently
    from a package, in :pep:`714`
