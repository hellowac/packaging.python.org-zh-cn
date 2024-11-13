.. highlight:: text

.. _version-specifiers:

==================
版本标识符
==================

**Version specifiers**

.. tab:: 中文

    该规范描述了一种用于识别 Python 软件发行版的版本并声明对特定版本的依赖关系的方案。

.. tab:: 英文

    This specification describes a scheme for identifying versions of Python software distributions, and declaring dependencies on particular versions.


定义
===========

**Definitions**

.. tab:: 中文

    文中使用的关键字“必须”（MUST）、“不得”（MUST NOT）、“必需”（REQUIRED）、“应”（SHALL）、“不应”（SHALL NOT）、“应该”（SHOULD）、“不应该”（SHOULD NOT）、“推荐”（RECOMMENDED）、“可以”（MAY）和“可选”（OPTIONAL）应按照 :rfc:`2119` 中的描述进行解释。

    “构建工具” 是旨在在开发系统上运行的自动化工具，用于生成源代码和二进制分发档案。构建工具也可以被集成工具调用，以构建作为源代码分发包（sdist）而非预构建二进制档案的软件。

    “索引服务器” 是主动的分发注册中心，发布版本和依赖元数据，并对允许的元数据施加约束。

    “发布工具” 是旨在在开发系统上运行的自动化工具，用于将源代码和二进制分发档案上传到索引服务器。

    “安装工具” 是专门设计用于在部署目标上运行的集成工具，从索引服务器或其他指定位置获取源代码和二进制分发档案，并将它们部署到目标系统。

    “自动化工具” 是一个统称，涵盖构建工具、索引服务器、发布工具、集成工具以及任何其他生成或消耗分发版本和依赖元数据的软件。

.. tab:: 英文

    The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
    "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this
    document are to be interpreted as described in :rfc:`2119`.

    "Build tools" are automated tools intended to run on development systems,
    producing source and binary distribution archives. Build tools may also be
    invoked by integration tools in order to build software distributed as
    sdists rather than prebuilt binary archives.

    "Index servers" are active distribution registries which publish version and
    dependency metadata and place constraints on the permitted metadata.

    "Publication tools" are automated tools intended to run on development
    systems and upload source and binary distribution archives to index servers.

    "Installation tools" are integration tools specifically intended to run on
    deployment targets, consuming source and binary distribution archives from
    an index server or other designated location and deploying them to the target
    system.

    "Automated tools" is a collective term covering build tools, index servers,
    publication tools, integration tools and any other software that produces
    or consumes distribution version and dependency metadata.

.. _Version scheme:

版本方案
==============

**Version scheme**

.. tab:: 中文

    发行版由公共版本标识符标识，该标识符支持所有定义的版本比较操作

    版本方案既用于描述特定发行版档案提供的发行版版本，也用于对构建或运行软件所需的依赖项版本进行限制。

.. tab:: 英文

    Distributions are identified by a public version identifier which supports all defined version comparison operations

    The version scheme is used both to describe the distribution version provided by a particular distribution archive, as well as to place constraints on the version of dependencies needed in order to build or run the software.


.. _public-version-identifiers:

公共版本标识符
--------------------------

**Public version identifiers**

.. tab:: 中文

    规范的公共版本标识符必须遵循以下方案::

        [N!]N(.N)*[{a|b|rc}N][.postN][.devN]

    公共版本标识符不得包含前导或尾随空格。

    公共版本标识符在给定的分发包中必须是唯一的。

    安装工具应忽略任何不符合该方案的公共版本，但必须包括以下规定的标准化步骤。当检测到不符合规范或模糊不清的版本时，安装工具可以发出警告。

    另请参见 :ref:`version-specifiers-regex`，该部分提供了一个正则表达式，用于检查是否严格符合规范格式，以及一个更宽松的正则表达式，接受可能需要后续标准化的输入。

    公共版本标识符被分为最多五个部分：

    * **Epoch 部分**: ``N!``
    * **Release 部分**: ``N(.N)*``
    * **Pre-release 部分**: ``{a|b|rc}N``
    * **Post-release 部分**: ``.postN``
    * **Development release 部分**: ``.devN``

    每个给定的版本都将是“最终版本”、“预发布版本”、“后发布版本”或“开发版本”，这些定义在以下各节中。

    所有数字组件必须是非负整数，以 ASCII 数字序列表示。

    所有数字组件必须根据其数字值来解释和排序，而不是作为文本字符串。

    所有数字组件可以是零。除了“Release 部分”下述描述的情况外，零作为数字组件没有特别的意义，仅作为版本排序中的最低可能值。

    .. note::

        该方案允许一些难以阅读的版本标识符，以更好地适应现有公共和私人 Python 项目中广泛使用的版本控制实践。

        因此，尽管规范技术上允许某些版本控制实践，但强烈不推荐新项目采用这些做法。在这种情况下，相关细节将在以下各节中指出。

.. tab:: 英文

    The canonical public version identifiers MUST comply with the following
    scheme::

        [N!]N(.N)*[{a|b|rc}N][.postN][.devN]

    Public version identifiers MUST NOT include leading or trailing whitespace.

    Public version identifiers MUST be unique within a given distribution.

    Installation tools SHOULD ignore any public versions which do not comply with
    this scheme but MUST also include the normalizations specified below.
    Installation tools MAY warn the user when non-compliant or ambiguous versions
    are detected.

    See also :ref:`version-specifiers-regex` which provides a regular
    expression to check strict conformance with the canonical format, as
    well as a more permissive regular expression accepting inputs that may
    require subsequent normalization.

    Public version identifiers are separated into up to five segments:

    * Epoch segment: ``N!``
    * Release segment: ``N(.N)*``
    * Pre-release segment: ``{a|b|rc}N``
    * Post-release segment: ``.postN``
    * Development release segment: ``.devN``

    Any given release will be a "final release", "pre-release", "post-release" or
    "developmental release" as defined in the following sections.

    All numeric components MUST be non-negative integers represented as sequences
    of ASCII digits.

    All numeric components MUST be interpreted and ordered according to their
    numeric value, not as text strings.

    All numeric components MAY be zero. Except as described below for the
    release segment, a numeric component of zero has no special significance
    aside from always being the lowest possible value in the version ordering.

    .. note::

        Some hard to read version identifiers are permitted by this scheme in
        order to better accommodate the wide range of versioning practices
        across existing public and private Python projects.

        Accordingly, some of the versioning practices which are technically
        permitted by the specification are strongly discouraged for new projects. Where
        this is the case, the relevant details are noted in the following
        sections.


.. _local-version-identifiers:

本地版本标识符
-------------------------

**Local version identifiers**

.. tab:: 中文

    本地版本标识符必须遵循以下方案::

        <公共版本标识符>[+<本地版本标签>]

    它们由一个正常的公共版本标识符（如前一节所定义）和一个任意的“本地版本标签”组成，二者通过加号连接。本地版本标签没有指定的语义，但会有一些语法限制。

    本地版本标识符用于表示与上游项目完全兼容的修补版本（如果适用，亦包括 ABI 兼容）。例如，这些标识符可以由应用程序开发人员和系统集成商创建，通常是在升级到新的上游版本会对应用程序或其他集成系统（如 Linux 发行版）造成破坏时，应用特定的回溯修复来实现。

    本地版本标签的引入使得可以区分上游版本和下游集成商可能修改过的重构版本。使用本地版本标识符不会影响发布类型，但在源代码分发中，它表示该版本的代码可能与对应的上游发布版本不同。

    为了确保本地版本标识符能够方便地作为文件名和 URL 的一部分，并避免在十六进制哈希表示中出现格式不一致，本地版本标签必须仅限于以下字符集：

    * ASCII 字母（``[a-zA-Z]``）
    * ASCII 数字（``[0-9]``）
    * 句点（``.``）

    本地版本标签必须以 ASCII 字母或数字开头和结尾。

    本地版本的比较和排序将分别考虑本地版本的每个部分（通过句点“.”分隔）。如果一个部分完全由 ASCII 数字组成，则该部分在比较时应视为整数；如果一个部分包含任何 ASCII 字母，则该部分应按字典顺序（不区分大小写）进行比较。当比较数字部分和字母部分时，数字部分总是被认为大于字母部分。此外，拥有更多部分的本地版本总是会被认为大于部分较少的版本，只要较短版本的部分与较长版本的开始部分完全匹配。

    “上游项目” 是指定义自己公共版本的项目。“下游项目”是指跟踪并重新分发上游项目的项目，可能会从上游项目的后续版本中回溯安全和错误修复。

    当发布上游项目到公共索引服务器时，本地版本标识符不应使用，但可以用于标识直接从项目源代码创建的私有构建。在发布与上游项目的公共版本标识符所标识的版本兼容的版本时，下游项目应使用本地版本标识符，但该版本包含额外的修改（如修复 bug）。由于 Python 包索引（PyPI）仅用于索引和托管上游项目，因此它必须不允许使用本地版本标识符。

    使用本地版本标识符的源代码分发应提供 ``python.integrator`` 扩展元数据（如 :pep:`459` 所定义）。

.. tab:: 英文

    Local version identifiers MUST comply with the following scheme::

        <public version identifier>[+<local version label>]

    They consist of a normal public version identifier (as defined in the
    previous section), along with an arbitrary "local version label", separated
    from the public version identifier by a plus. Local version labels have
    no specific semantics assigned, but some syntactic restrictions are imposed.

    Local version identifiers are used to denote fully API (and, if applicable,
    ABI) compatible patched versions of upstream projects. For example, these
    may be created by application developers and system integrators by applying
    specific backported bug fixes when upgrading to a new upstream release would
    be too disruptive to the application or other integrated system (such as a
    Linux distribution).

    The inclusion of the local version label makes it possible to differentiate
    upstream releases from potentially altered rebuilds by downstream
    integrators. The use of a local version identifier does not affect the kind
    of a release but, when applied to a source distribution, does indicate that
    it may not contain the exact same code as the corresponding upstream release.

    To ensure local version identifiers can be readily incorporated as part of
    filenames and URLs, and to avoid formatting inconsistencies in hexadecimal
    hash representations, local version labels MUST be limited to the following
    set of permitted characters:

    * ASCII letters (``[a-zA-Z]``)
    * ASCII digits (``[0-9]``)
    * periods (``.``)

    Local version labels MUST start and end with an ASCII letter or digit.

    Comparison and ordering of local versions considers each segment of the local
    version (divided by a ``.``) separately. If a segment consists entirely of
    ASCII digits then that section should be considered an integer for comparison
    purposes and if a segment contains any ASCII letters then that segment is
    compared lexicographically with case insensitivity. When comparing a numeric
    and lexicographic segment, the numeric section always compares as greater than
    the lexicographic segment. Additionally a local version with a great number of
    segments will always compare as greater than a local version with fewer
    segments, as long as the shorter local version's segments match the beginning
    of the longer local version's segments exactly.

    An "upstream project" is a project that defines its own public versions. A
    "downstream project" is one which tracks and redistributes an upstream project,
    potentially backporting security and bug fixes from later versions of the
    upstream project.

    Local version identifiers SHOULD NOT be used when publishing upstream
    projects to a public index server, but MAY be used to identify private
    builds created directly from the project source. Local
    version identifiers SHOULD be used by downstream projects when releasing a
    version that is API compatible with the version of the upstream project
    identified by the public version identifier, but contains additional changes
    (such as bug fixes). As the Python Package Index is intended solely for
    indexing and hosting upstream projects, it MUST NOT allow the use of local
    version identifiers.

    Source distributions using a local version identifier SHOULD provide the
    ``python.integrator`` extension metadata (as defined in :pep:`459`).


最终版本
--------------

**Final releases**

.. tab:: 中文

    仅由发布段和可选的纪元标识符组成的版本标识符称为“最终版本”。

    发布段由一个或多个非负整数值组成，值之间用句点分隔::

        N(.N)*

    项目中的最终版本必须以一致递增的方式编号，否则自动化工具将无法正确地进行升级。

    发布段的比较和排序依次考虑发布段中每个组成部分的数值。在比较具有不同组成部分数目的发布段时，较短的段将根据需要使用额外的零进行填充。

    虽然在此方案下允许在第一个部分后面添加任何数量的额外组成部分，但最常见的变体是使用两个组成部分（"major.minor"）或三个组成部分（"major.minor.micro"）。

    例如::

        0.9
        0.9.1
        0.9.2
        ...
        0.9.10
        0.9.11
        1.0
        1.0.1
        1.1
        2.0
        2.0.1
        ...

    一个发布系列是指任何一组具有共同前缀的最终发布版本号。例如，`3.3.1`、`3.3.5` 和 `3.3.9.45` 都属于 `3.3` 发布系列。

    .. note::

        ``X.Y`` 和 ``X.Y.0`` 并不被视为不同的版本号，因为发布段比较规则隐式地将两个组件形式的版本扩展为 ``X.Y.0``，当与包含三个组件的任何版本进行比较时。

    也允许基于日期的发布段。以下是一个使用发布日期的年份和月份的日期型版本方案示例::

        2012.4
        2012.7
        2012.10
        2013.1
        2013.6
        ...

.. tab:: 英文

    A version identifier that consists solely of a release segment and optionally
    an epoch identifier is termed a "final release".

    The release segment consists of one or more non-negative integer
    values, separated by dots::

        N(.N)*

    Final releases within a project MUST be numbered in a consistently
    increasing fashion, otherwise automated tools will not be able to upgrade
    them correctly.

    Comparison and ordering of release segments considers the numeric value
    of each component of the release segment in turn. When comparing release
    segments with different numbers of components, the shorter segment is
    padded out with additional zeros as necessary.

    While any number of additional components after the first are permitted
    under this scheme, the most common variants are to use two components
    ("major.minor") or three components ("major.minor.micro").

    For example::

        0.9
        0.9.1
        0.9.2
        ...
        0.9.10
        0.9.11
        1.0
        1.0.1
        1.1
        2.0
        2.0.1
        ...

    A release series is any set of final release numbers that start with a
    common prefix. For example, ``3.3.1``, ``3.3.5`` and ``3.3.9.45`` are all
    part of the ``3.3`` release series.

    .. note::

        ``X.Y`` and ``X.Y.0`` are not considered distinct release numbers, as
        the release segment comparison rules implicit expand the two component
        form to ``X.Y.0`` when comparing it to any release segment that includes
        three components.

    Date based release segments are also permitted. An example of a date based
    release scheme using the year and month of the release::

        2012.4
        2012.7
        2012.10
        2013.1
        2013.6
        ...


.. _pre-release-versions:

预发布
------------

**Pre-releases**

.. tab:: 中文

    一些项目使用“alpha、beta、发布候选”预发布周期，在最终发布之前支持用户进行测试。

    如果作为项目开发周期的一部分使用这些预发布版本，它们通过在版本标识符中包含预发布段来表示::

        X.YaN   # Alpha 版本
        X.YbN   # Beta 版本
        X.YrcN  # 发布候选版本
        X.Y     # 最终发布版本

    仅由发布段和预发布段组成的版本标识符称为“预发布版本”。

    预发布段由预发布阶段的字母标识符以及一个非负整数值组成。给定发布的预发布版本按阶段（alpha、beta、发布候选）顺序排序，然后在该阶段内按数字成分排序。

    安装工具可以接受相同发布段的 ``c`` 和 ``rc`` 版本，以处理一些现有的遗留版本。

    安装工具应将 ``c`` 版本解释为与 ``rc`` 版本等效（即，``c1`` 表示与 ``rc1`` 相同的版本）。

    构建工具、发布工具和索引服务器应不允许为相同的发布段创建 ``rc`` 和 ``c`` 版本。

.. tab:: 英文

    Some projects use an "alpha, beta, release candidate" pre-release cycle to
    support testing by their users prior to a final release.

    If used as part of a project's development cycle, these pre-releases are
    indicated by including a pre-release segment in the version identifier::

        X.YaN   # Alpha release
        X.YbN   # Beta release
        X.YrcN  # Release Candidate
        X.Y     # Final release

    A version identifier that consists solely of a release segment and a
    pre-release segment is termed a "pre-release".

    The pre-release segment consists of an alphabetical identifier for the
    pre-release phase, along with a non-negative integer value. Pre-releases for
    a given release are ordered first by phase (alpha, beta, release candidate)
    and then by the numerical component within that phase.

    Installation tools MAY accept both ``c`` and ``rc`` releases for a common
    release segment in order to handle some existing legacy distributions.

    Installation tools SHOULD interpret ``c`` versions as being equivalent to
    ``rc`` versions (that is, ``c1`` indicates the same version as ``rc1``).

    Build tools, publication tools and index servers SHOULD disallow the creation
    of both ``rc`` and ``c`` releases for a common release segment.


发布后
-------------

**Post-releases**

.. tab:: 中文

    一些项目使用后发布版本来解决最终发布中的轻微错误，这些错误不会影响分发的软件（例如，修正发布说明中的错误）。

    如果作为项目开发周期的一部分使用这些后发布版本，它们通过在版本标识符中包含后发布段来表示::

        X.Y.postN    # 后发布版本

    包含后发布段但不包含开发版本段的版本标识符称为“后发布版本”。

    后发布段由字符串 ``.post`` 和一个非负整数值组成。后发布版本按其数字成分排序，紧跟相应的发布版本之后，并排在任何后续版本之前。

    .. note::

        强烈不建议使用后发布版本发布包含实际 bug 修复的维护版本。通常，更好的做法是使用更长的版本号，并为每个维护版本递增最后一个组件。

    后发布版本也可以用于预发布版本::

        X.YaN.postM   # Alpha 版本的后发布
        X.YbN.postM   # Beta 版本的后发布
        X.YrcN.postM  # 发布候选版本的后发布

    .. note::

        强烈不建议创建预发布版本的后发布版本，因为这使得版本标识符对人类读者而言难以解析。通常，创建一个新的预发布版本，通过递增数字组件来表示，会更加清晰。

.. tab:: 英文

    Some projects use post-releases to address minor errors in a final release
    that do not affect the distributed software (for example, correcting an error
    in the release notes).

    If used as part of a project's development cycle, these post-releases are
    indicated by including a post-release segment in the version identifier::

        X.Y.postN    # Post-release

    A version identifier that includes a post-release segment without a
    developmental release segment is termed a "post-release".

    The post-release segment consists of the string ``.post``, followed by a
    non-negative integer value. Post-releases are ordered by their
    numerical component, immediately following the corresponding release,
    and ahead of any subsequent release.

    .. note::

        The use of post-releases to publish maintenance releases containing
        actual bug fixes is strongly discouraged. In general, it is better
        to use a longer release number and increment the final component
        for each maintenance release.

    Post-releases are also permitted for pre-releases::

        X.YaN.postM   # Post-release of an alpha release
        X.YbN.postM   # Post-release of a beta release
        X.YrcN.postM  # Post-release of a release candidate

    .. note::

        Creating post-releases of pre-releases is strongly discouraged, as
        it makes the version identifier difficult to parse for human readers.
        In general, it is substantially clearer to simply create a new
        pre-release by incrementing the numeric component.


开发版本
----------------------

**Developmental releases**

.. tab:: 中文

    一些项目会定期进行开发版本发布，系统打包者（尤其是 Linux 发行版的打包者）可能希望直接从源代码控制中创建早期版本，这些版本不会与后续的项目版本冲突。

    如果作为项目开发周期的一部分使用这些开发版本，它们通过在版本标识符中包含开发版本段来表示::

        X.Y.devN    # 开发版本

    包含开发版本段的版本标识符称为“开发版本”。

    开发版本段由字符串 ``.dev`` 和一个非负整数值组成。开发版本按其数字成分排序，紧跟相应的发布版本之前（并且排在任何具有相同发布段的预发布版本之前），并排在任何之前发布的版本（包括任何后发布版本）之后。

    开发版本也可以用于预发布和后发布版本::

        X.YaN.devM       # Alpha 版本的开发版本
        X.YbN.devM       # Beta 版本的开发版本
        X.YrcN.devM      # 发布候选版本的开发版本
        X.Y.postN.devM   # 后发布版本的开发版本

    .. note::

        虽然它们可能对持续集成（CI）有用，但强烈不建议将预发布版本的开发版本发布到通用的公共索引服务器，因为这使得版本标识符对人类读者难以解析。如果需要发布此类版本，创建一个新的预发布版本，通过递增数字组件表示，会更加清晰。

        预发布版本的开发版本也强烈不推荐，但对于那些使用后发布标记进行完整维护发布的项目（这些发布可能包含代码更改）而言，使用开发版本可能是适当的。

.. tab:: 英文

    Some projects make regular developmental releases, and system packagers
    (especially for Linux distributions) may wish to create early releases
    directly from source control which do not conflict with later project
    releases.

    If used as part of a project's development cycle, these developmental
    releases are indicated by including a developmental release segment in the
    version identifier::

        X.Y.devN    # Developmental release

    A version identifier that includes a developmental release segment is
    termed a "developmental release".

    The developmental release segment consists of the string ``.dev``,
    followed by a non-negative integer value. Developmental releases are ordered
    by their numerical component, immediately before the corresponding release
    (and before any pre-releases with the same release segment), and following
    any previous release (including any post-releases).

    Developmental releases are also permitted for pre-releases and
    post-releases::

        X.YaN.devM       # Developmental release of an alpha release
        X.YbN.devM       # Developmental release of a beta release
        X.YrcN.devM      # Developmental release of a release candidate
        X.Y.postN.devM   # Developmental release of a post-release

    .. note::

        While they may be useful for continuous integration purposes, publishing
        developmental releases of pre-releases to general purpose public index
        servers is strongly discouraged, as it makes the version identifier
        difficult to parse for human readers. If such a release needs to be
        published, it is substantially clearer to instead create a new
        pre-release by incrementing the numeric component.

        Developmental releases of post-releases are also strongly discouraged,
        but they may be appropriate for projects which use the post-release
        notation for full maintenance releases which may include code changes.


版本时代
--------------

**Version epochs**

.. tab:: 中文

    如果包含在版本标识符中，纪元（epoch）出现在所有其他组件之前，并通过感叹号与发布段分隔::

        E!X.Y  # 带有纪元的版本标识符

    如果未显式给出纪元，则隐式纪元为 ``0``。

    大多数版本标识符不会包含纪元，因为只有当项目 *改变* 了其版本编号的处理方式，以至于正常的版本排序规则会得出错误的结果时，才需要显式指定纪元。例如，如果一个项目使用基于日期的版本号（如 ``2014.04``），并且想切换到语义化版本（如 ``1.0``），那么使用正常的排序方案时，新版本会被识别为 *早于* 基于日期的版本::

        1.0
        1.1
        2.0
        2013.10
        2014.04

    然而，通过显式指定纪元，可以适当更改排序顺序，因为来自较晚纪元的所有版本会排在较早纪元的版本之后::

        2013.10
        2014.04
        1!1.0
        1!1.1
        1!2.0

.. tab:: 英文

    If included in a version identifier, the epoch appears before all other
    components, separated from the release segment by an exclamation mark::

        E!X.Y  # Version identifier with epoch

    If no explicit epoch is given, the implicit epoch is ``0``.

    Most version identifiers will not include an epoch, as an explicit epoch is
    only needed if a project *changes* the way it handles version numbering in
    a way that means the normal version ordering rules will give the wrong
    answer. For example, if a project is using date based versions like
    ``2014.04`` and would like to switch to semantic versions like ``1.0``, then
    the new releases would be identified as *older* than the date based releases
    when using the normal sorting scheme::

        1.0
        1.1
        2.0
        2013.10
        2014.04

    However, by specifying an explicit epoch, the sort order can be changed
    appropriately, as all versions from a later epoch are sorted after versions
    from an earlier epoch::

        2013.10
        2014.04
        1!1.0
        1!1.1
        1!2.0


.. _version-specifiers-normalization:

规范化
-------------

**Normalization**

.. tab:: 中文

    为了保持与现有版本的更好兼容性，解析版本时必须考虑一些“替代(alternative)”语法。这些语法在解析版本时必须被考虑，但它们应该“标准化”为上述定义的标准语法。

.. tab:: 英文

    In order to maintain better compatibility with existing versions there are a
    number of "alternative" syntaxes that MUST be taken into account when parsing
    versions. These syntaxes MUST be considered when parsing a version, however
    they should be "normalized" to the standard syntax defined above.


区分大小写
~~~~~~~~~~~~~~~~

**Case sensitivity**

.. tab:: 中文

    所有 ASCII 字母在版本中应不区分大小写，且标准形式为小写。这允许像 ``1.1RC1`` 这样的版本，它将被标准化为 ``1.1rc1``。

.. tab:: 英文

    All ascii letters should be interpreted case insensitively within a version and the normal form is lowercase. This allows versions such as ``1.1RC1`` which would be normalized to ``1.1rc1``.


整数规范化
~~~~~~~~~~~~~~~~~~~~~

**Integer Normalization**

.. tab:: 中文

    所有整数通过内置的 `int()` 进行解释，并标准化为输出的字符串形式。这意味着整数版本 ``00`` 会标准化为 ``0``，而 ``09000`` 会标准化为 ``9000``。但是，这对于本地版本中的字母数字段内的整数不适用，例如 ``1.0+foo0100``，因为该版本已经是标准化形式。

.. tab:: 英文

    All integers are interpreted via the ``int()`` built in and normalize to the string form of the output. This means that an integer version of ``00`` would normalize to ``0`` while ``09000`` would normalize to ``9000``. This does not hold true for integers inside of an alphanumeric segment of a local version such as ``1.0+foo0100`` which is already in its normalized form.


预发布分隔符
~~~~~~~~~~~~~~~~~~~~~~

**Pre-release separators**

.. tab:: 中文

    预发行版应允许在发布段和预发行段之间使用 ``.``、 ``-`` 或 ``_`` 作为分隔符。其标准形式是没有分隔符的。这允许诸如 ``1.1.a1`` 或 ``1.1-a1`` 的版本，它们会被标准化为 ``1.1a1``。还应允许在预发行符号和数字之间使用分隔符。这允许诸如 ``1.0a.1`` 的版本，它会被标准化为 ``1.0a1``。

.. tab:: 英文

    Pre-releases should allow a ``.``, ``-``, or ``_`` separator between the release segment and the pre-release segment. The normal form for this is without a separator. This allows versions such as ``1.1.a1`` or ``1.1-a1`` which would be normalized to ``1.1a1``. It should also allow a separator to be used between the pre-release signifier and the numeral. This allows versions such as ``1.0a.1`` which would be normalized to ``1.0a1``.


预发布拼写
~~~~~~~~~~~~~~~~~~~~

**Pre-release spelling**

.. tab:: 中文

    预发行版允许额外的拼写形式，例如将 ``alpha``、``beta``、``c``、``pre`` 和 ``preview`` 分别替换为 ``a``、``b``、``rc``、``rc`` 和 ``rc``。这允许诸如 ``1.1alpha1``、``1.1beta2`` 或 ``1.1c3`` 的版本，它们会被标准化为 ``1.1a1``、``1.1b2`` 和 ``1.1rc3``。在每种情况下，额外的拼写形式应视为与其标准形式等效。

.. tab:: 英文

    Pre-releases allow the additional spellings of ``alpha``, ``beta``, ``c``, ``pre``, and ``preview`` for ``a``, ``b``, ``rc``, ``rc``, and ``rc`` respectively. This allows versions such as ``1.1alpha1``, ``1.1beta2``, or ``1.1c3`` which normalize to ``1.1a1``, ``1.1b2``, and ``1.1rc3``. In every case the additional spelling should be considered equivalent to their normal forms.


隐式预发布编号
~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Implicit pre-release number**

.. tab:: 中文

    预发布版本允许省略数字，在这种情况下，数字隐式地被假定为``0``。其标准形式是显式地包含``0``。这允许版本如``1.2a``，它将标准化为``1.2a0``。

.. tab:: 英文

    Pre releases allow omitting the numeral in which case it is implicitly assumed to be ``0``. The normal form for this is to include the ``0`` explicitly. This allows versions such as ``1.2a`` which is normalized to ``1.2a0``.


发布后分隔符
~~~~~~~~~~~~~~~~~~~~~~~

**Post release separators**

.. tab:: 中文

    后发布版本允许使用 ``.``、 ``-`` 或 ``_`` 作为分隔符，也允许完全省略分隔符。其标准形式是使用 ``.`` 分隔符。这允许版本如 ``1.2-post2`` 或 ``1.2post2`` ，它们将标准化为 ``1.2.post2`` 。与预发布分隔符类似，这也允许在后发布标志符和数字之间使用可选的分隔符。这允许版本如 ``1.2.post-2`` ，它将标准化为 ``1.2.post2`` 。

.. tab:: 英文

    Post releases allow a ``.``, ``-``, or ``_`` separator as well as omitting the separator all together. The normal form of this is with the ``.`` separator. This allows versions such as ``1.2-post2`` or ``1.2post2`` which normalize to ``1.2.post2``. Like the pre-release separator this also allows an optional separator between the post release signifier and the numeral. This allows versions like ``1.2.post-2`` which would normalize to ``1.2.post2``.


发布后拼写
~~~~~~~~~~~~~~~~~~~~~

**Post release spelling**

.. tab:: 中文

    后发布版本允许使用``rev``和``r``的额外拼写。这允许版本如``1.0-r4``，它将标准化为``1.0.post4``。与预发布版本类似，这些额外的拼写应被视为与其标准形式等效。

.. tab:: 英文

    Post-releases allow the additional spellings of ``rev`` and ``r``. This allows versions such as ``1.0-r4`` which normalizes to ``1.0.post4``. As with the pre-releases the additional spellings should be considered equivalent to their normal forms.


隐式发布后编号
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Implicit post release number**

.. tab:: 中文

    后发布版本允许省略数字，在这种情况下，默认假设数字为 ``0`` 。其标准形式是显式包含 ``0`` 。这允许版本如 ``1.2.post`` ，它将标准化为 ``1.2.post0`` 。

.. tab:: 英文

    Post releases allow omitting the numeral in which case it is implicitly assumed to be ``0``. The normal form for this is to include the ``0`` explicitly. This allows versions such as ``1.2.post`` which is normalized to ``1.2.post0``.


隐式发布后
~~~~~~~~~~~~~~~~~~~~~~

**Implicit post releases**

.. tab:: 中文

    后发布版本允许完全省略 ``post`` 标识符。在使用这种形式时，分隔符必须是 ``-`` ，且不允许使用其他形式。这允许版本如 ``1.0-1`` ，它将标准化为 ``1.0.post1`` 。此特定标准化形式不得与隐式后发布版本数字规则一起使用。换句话说， ``1.0-`` *不是* 一个有效版本，并且它 *不会* 标准化为 ``1.0.post0`` 。

.. tab:: 英文

    Post releases allow omitting the ``post`` signifier all together. When using this form the separator MUST be ``-`` and no other form is allowed. This allows versions such as ``1.0-1`` to be normalized to ``1.0.post1``. This particular normalization MUST NOT be used in conjunction with the implicit post release number rule. In other words, ``1.0-`` is *not* a valid version and it does *not* normalize to ``1.0.post0``.


开发发布分隔符
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Development release separators**

.. tab:: 中文

    开发版本允许使用 ``.`` 、 ``-`` 或 ``_`` 分隔符，也允许完全省略分隔符。标准形式是使用 ``.`` 分隔符。这允许版本如 ``1.2-dev2`` 或 ``1.2dev2`` ，它们会标准化为 ``1.2.dev2`` 。

.. tab:: 英文

    Development releases allow a ``.``, ``-``, or a ``_`` separator as well as omitting the separator all together. The normal form of this is with the ``.`` separator. This allows versions such as ``1.2-dev2`` or ``1.2dev2`` which normalize to ``1.2.dev2``.


隐式开发发布编号
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Implicit development release number**

.. tab:: 中文

    开发版本允许省略数字，在这种情况下，数字隐式地假定为 ``0`` 。标准形式是显式地包含 ``0`` 。这允许版本如 ``1.2.dev`` ，它会标准化为 ``1.2.dev0`` 。

.. tab:: 英文

    Development releases allow omitting the numeral in which case it is implicitly assumed to be ``0``. The normal form for this is to include the ``0`` explicitly. This allows versions such as ``1.2.dev`` which is normalized to ``1.2.dev0``.


本地版本段
~~~~~~~~~~~~~~~~~~~~~~

**Local version segments**

.. tab:: 中文

    在本地版本中，除了使用 ``.`` 作为段落分隔符外， ``-`` 和 ``_`` 也都是可以接受的。正常形式是使用 ``.`` 字符。这允许像 ``1.0+ubuntu-1`` 这样的版本被规范化为 ``1.0+ubuntu.1``。

.. tab:: 英文

    With a local version, in addition to the use of ``.`` as a separator of segments, the use of ``-`` and ``_`` is also acceptable. The normal form is using the ``.`` character. This allows versions such as ``1.0+ubuntu-1`` to be normalized to ``1.0+ubuntu.1``.


前导 v 字符
~~~~~~~~~~~~~~~~~~~~~

**Preceding v character**

.. tab:: 中文

    为了支持常见的版本表示法 ``v1.0``，版本号可以前面加上一个字面上的 ``v`` 字符。该字符必须在所有情况下被忽略，并且应该从版本的所有规范化形式中省略。带有和不带有 ``v`` 的相同版本被视为等效。

.. tab:: 英文

    In order to support the common version notation of ``v1.0`` versions may be preceded by a single literal ``v`` character. This character MUST be ignored for all purposes and should be omitted from all normalized forms of the version. The same version with and without the ``v`` is considered equivalent.


前导和尾随空格
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Leading and Trailing Whitespace**

.. tab:: 中文

    所有规范化版本的形式必须默默地忽略并去除前导和尾随的空白字符。这包括 ``" "``, ``\t``, ``\n``, ``\r``, ``\f``, 和 ``\v``。这允许意外的空白字符被合理处理，例如像 ``1.0\n`` 这样的版本会规范化为 ``1.0``。

.. tab:: 英文

    Leading and trailing whitespace must be silently ignored and removed from all normalized forms of a version. This includes ``" "``, ``\t``, ``\n``, ``\r``, ``\f``, and ``\v``. This allows accidental whitespace to be handled sensibly, such as a version like ``1.0\n`` which normalizes to ``1.0``.


兼容版本方案的示例
-------------------------------------

**Examples of compliant version schemes**

.. tab:: 中文

    标准版本方案旨在涵盖公共和私有 Python 项目中的各种标识实践。实际上，一个试图使用该方案提供的全部灵活性的单一项目，可能会导致用户在确定版本的相对顺序时遇到困难，尽管上述规则确保所有符合规范的工具将一致地对其进行排序。

    以下示例展示了一小部分项目可能选择的不同方式来标识其发布版本，同时确保“最新发布版本”和“最新稳定发布版本”能够被人类用户和自动化工具轻松确定。

    简单的“major.minor”版本管理::

        0.1
        0.2
        0.3
        1.0
        1.1
        ...

    简单的“major.minor.micro”版本管理::

        1.1.0
        1.1.1
        1.1.2
        1.2.0
        ...

    带有 alpha、beta 和候选预发布版本的“major.minor”版本管理::

        0.9
        1.0a1
        1.0a2
        1.0b1
        1.0rc1
        1.0
        1.1a1
        ...

    带有开发版本、发布候选版本以及用于小幅修正的后发布版本的“major.minor”版本管理::

        0.9
        1.0.dev1
        1.0.dev2
        1.0.dev3
        1.0.dev4
        1.0c1
        1.0c2
        1.0
        1.0.post1
        1.1.dev1
        ...

    基于日期的发布版本，使用每年内递增的序列号，跳过零::

        2012.1
        2012.2
        2012.3
        ...
        2012.15
        2013.1
        2013.2
        ...

.. tab:: 英文

    The standard version scheme is designed to encompass a wide range of identification practices across public and private Python projects. In practice, a single project attempting to use the full flexibility offered by the scheme would create a situation where human users had difficulty figuring out the relative order of versions, even though the rules above ensure all compliant tools will order them consistently.

    The following examples illustrate a small selection of the different approaches projects may choose to identify their releases, while still ensuring that the "latest release" and the "latest stable release" can be easily determined, both by human users and automated tools.

    Simple "major.minor" versioning::

        0.1
        0.2
        0.3
        1.0
        1.1
        ...

    Simple "major.minor.micro" versioning::

        1.1.0
        1.1.1
        1.1.2
        1.2.0
        ...

    "major.minor" versioning with alpha, beta and candidate pre-releases::

        0.9
        1.0a1
        1.0a2
        1.0b1
        1.0rc1
        1.0
        1.1a1
        ...

    "major.minor" versioning with developmental releases, release candidates and post-releases for minor corrections::

        0.9
        1.0.dev1
        1.0.dev2
        1.0.dev3
        1.0.dev4
        1.0c1
        1.0c2
        1.0
        1.0.post1
        1.1.dev1
        ...

    Date based releases, using an incrementing serial within each year, skipping zero::

        2012.1
        2012.2
        2012.3
        ...
        2012.15
        2013.1
        2013.2
        ...


允许后缀和相对顺序的摘要
---------------------------------------------------

**Summary of permitted suffixes and relative ordering**

.. tab:: 中文

    .. note::

        本节主要针对自动处理分发元数据的工具的作者，而非决定版本方案的 Python 分发开发者。

    版本标识符的 epoch 段必须根据给定 epoch 的数值进行排序。如果没有 epoch 段，则隐式的数值为 ``0``。

    版本标识符的发布段必须按照 Python 的元组排序方式进行排序，当规范化的发布段按以下方式解析时::

        tuple(map(int, release_segment.split(".")))

    所有参与比较的发布段必须通过在必要时用零填充较短的段来转换为一致的长度。

    在数字发布（``1.0``，``2.7.3``）中，允许使用以下后缀，并且必须按所示顺序排序::

        .devN, aN, bN, rcN, <无后缀>, .postN

    请注意，``c`` 被视为语义上等同于 ``rc``，并且必须像 ``rc`` 一样排序。工具可以拒绝在同一发布段中同时使用相同的 ``N`` 值的 ``c`` 和 ``rc``，将其视为模糊不清，并且仍然符合规范。

    在 alpha（``1.0a1``）、beta（``1.0b1``）或发布候选（``1.0rc1``，``1.0c1``）中，允许使用以下后缀，并且必须按所示顺序排序::

        .devN, <无后缀>, .postN

    在后发布（``1.0.post1``）中，允许使用以下后缀，并且必须按所示顺序排序::

        .devN, <无后缀>

    请注意， ``devN`` 和 ``postN`` 必须始终以点号前缀，即使它们紧随数字版本后（例如 ``1.0.dev456``，``1.0.post1``）。

    在具有共享前缀的预发布、后发布或开发发布段中，排序必须按数字组件的值进行。

    以下示例涵盖了许多可能的组合::

        1.dev0
        1.0.dev456
        1.0a1
        1.0a2.dev456
        1.0a12.dev456
        1.0a12
        1.0b1.dev456
        1.0b2
        1.0b2.post345.dev456
        1.0b2.post345
        1.0rc1.dev456
        1.0rc1
        1.0
        1.0+abc.5
        1.0+abc.7
        1.0+5
        1.0.post456.dev34
        1.0.post456
        1.0.15
        1.1.dev1

.. tab:: 英文

    .. note::

        This section is intended primarily for authors of tools that
        automatically process distribution metadata, rather than developers
        of Python distributions deciding on a versioning scheme.

    The epoch segment of version identifiers MUST be sorted according to the
    numeric value of the given epoch. If no epoch segment is present, the
    implicit numeric value is ``0``.

    The release segment of version identifiers MUST be sorted in
    the same order as Python's tuple sorting when the normalized release segment is
    parsed as follows::

        tuple(map(int, release_segment.split(".")))

    All release segments involved in the comparison MUST be converted to a
    consistent length by padding shorter segments with zeros as needed.

    Within a numeric release (``1.0``, ``2.7.3``), the following suffixes
    are permitted and MUST be ordered as shown::

        .devN, aN, bN, rcN, <no suffix>, .postN

    Note that ``c`` is considered to be semantically equivalent to ``rc`` and must
    be sorted as if it were ``rc``. Tools MAY reject the case of having the same
    ``N`` for both a ``c`` and a ``rc`` in the same release segment as ambiguous
    and remain in compliance with the specification.

    Within an alpha (``1.0a1``), beta (``1.0b1``), or release candidate
    (``1.0rc1``, ``1.0c1``), the following suffixes are permitted and MUST be
    ordered as shown::

        .devN, <no suffix>, .postN

    Within a post-release (``1.0.post1``), the following suffixes are permitted
    and MUST be ordered as shown::

        .devN, <no suffix>

    Note that ``devN`` and ``postN`` MUST always be preceded by a dot, even
    when used immediately following a numeric version (e.g. ``1.0.dev456``,
    ``1.0.post1``).

    Within a pre-release, post-release or development release segment with a
    shared prefix, ordering MUST be by the value of the numeric component.

    The following example covers many of the possible combinations::

        1.dev0
        1.0.dev456
        1.0a1
        1.0a2.dev456
        1.0a12.dev456
        1.0a12
        1.0b1.dev456
        1.0b2
        1.0b2.post345.dev456
        1.0b2.post345
        1.0rc1.dev456
        1.0rc1
        1.0
        1.0+abc.5
        1.0+abc.7
        1.0+5
        1.0.post456.dev34
        1.0.post456
        1.0.15
        1.1.dev1


不同元数据版本的版本排序
---------------------------------------------------

**Version ordering across different metadata versions**

.. tab:: 中文

    元数据 v1.0 (:pep:`241`) 和元数据 v1.1 (:pep:`314`) 没有指定标准的版本标识或排序方案。然而，元数据 v1.2 (:pep:`345`) 确实指定了一个方案，该方案在 :pep:`386` 中进行了定义。

    由于简单安装程序 API 的性质，安装程序无法知道特定分发版本所使用的元数据版本。此外，安装程序需要能够创建一个合理优先级的列表，包含所有或尽可能多的项目版本，以确定应该安装哪些版本。这些要求迫使我们对所有版本的项目使用统一的解析机制进行标准化。

    基于上述原因，本规范必须用于所有版本的元数据，并且即使是元数据 v1.2，也会取代 :pep:`386`。工具应忽略任何无法根据本规范中的规则解析的版本，但如果没有符合本规范的版本可用，工具可以回退到实现定义的版本解析和排序方案。

    分发用户可能希望显式地从他们控制的任何私有包索引中移除不符合规范的版本。

.. tab:: 英文

    Metadata v1.0 (:pep:`241`) and metadata v1.1 (:pep:`314`) do not specify a standard
    version identification or ordering scheme. However metadata v1.2 (:pep:`345`)
    does specify a scheme which is defined in :pep:`386`.

    Due to the nature of the simple installer API it is not possible for an
    installer to be aware of which metadata version a particular distribution was
    using. Additionally installers required the ability to create a reasonably
    prioritized list that includes all, or as many as possible, versions of
    a project to determine which versions it should install. These requirements
    necessitate a standardization across one parsing mechanism to be used for all
    versions of a project.

    Due to the above, this specification MUST be used for all versions of metadata and
    supersedes :pep:`386` even for metadata v1.2. Tools SHOULD ignore any versions
    which cannot be parsed by the rules in this specification, but MAY fall back to
    implementation defined version parsing and ordering schemes if no versions
    complying with this specification are available.

    Distribution users may wish to explicitly remove non-compliant versions from
    any private package indexes they control.


与其他版本方案的兼容性
----------------------------------------

**Compatibility with other version schemes**

.. tab:: 中文

    一些项目可能选择使用一种版本方案，该方案需要进行转换以符合本规范中定义的公共版本方案。在这种情况下，项目特定的版本可以存储在元数据中，而转换后的公共版本则发布在版本字段中。

    这允许自动化分发工具提供一致正确的发布版本排序，同时仍允许开发者为他们的项目使用他们偏好的内部版本管理方案。

.. tab:: 英文

    Some projects may choose to use a version scheme which requires
    translation in order to comply with the public version scheme defined in
    this specification. In such cases, the project specific version can be stored in the
    metadata while the translated public version is published in the version field.

    This allows automated distribution tools to provide consistently correct
    ordering of published releases, while still allowing developers to use
    the internal versioning scheme they prefer for their projects.


语义版本控制
~~~~~~~~~~~~~~~~~~~

**Semantic versioning**

.. tab:: 中文

    `语义版本控制 <Semantic versioning_>`_ 是一种流行的版本标识方案，它在发布版本号的不同元素的意义上比本规范更具规定性。即使一个项目选择不遵循语义版本控制的细节，该方案仍然值得了解，因为它涵盖了在依赖其他分发版本以及发布他人依赖的分发版本时可能出现的许多问题。

    语义版本控制中的“Major.Minor.Patch”（在本规范中描述为“major.minor.micro”）部分（2.0.0 规范中的第 1-8 条款）与本规范中定义的版本方案完全兼容，建议遵循这些部分。

    包含连字符（预发布 - 第 10 条）或加号（构建 - 第 11 条）的语义版本与本规范*不*兼容，且不允许出现在公共版本字段中。

    将此类基于语义版本控制的源标签转换为兼容的公共版本的一种可能机制是使用 ``.devN`` 后缀来指定适当的版本顺序。

    特定的构建信息也可以包含在本地版本标签中。

.. tab:: 英文

    `Semantic versioning`_ is a popular version identification scheme that is
    more prescriptive than this specification regarding the significance of different
    elements of a release number. Even if a project chooses not to abide by
    the details of semantic versioning, the scheme is worth understanding as
    it covers many of the issues that can arise when depending on other
    distributions, and when publishing a distribution that others rely on.

    The "Major.Minor.Patch" (described in this specification as "major.minor.micro")
    aspects of semantic versioning (clauses 1-8 in the 2.0.0 specification)
    are fully compatible with the version scheme defined in this specification, and abiding
    by these aspects is encouraged.

    Semantic versions containing a hyphen (pre-releases - clause 10) or a
    plus sign (builds - clause 11) are *not* compatible with this specification
    and are not permitted in the public version field.

    One possible mechanism to translate such semantic versioning based source
    labels to compatible public versions is to use the ``.devN`` suffix to
    specify the appropriate version order.

    Specific build information may also be included in local version labels.

.. _Semantic versioning: https://semver.org/


基于 DVCS 的版本标签
~~~~~~~~~~~~~~~~~~~~~~~~~

**DVCS based version labels**

.. tab:: 中文

    许多构建工具与分布式版本控制系统（如 Git 和 Mercurial）集成，以便在版本标识符中添加标识哈希值。由于哈希值无法可靠地排序，因此此类版本不允许出现在公共版本字段中。

    与语义版本控制类似，公共的 ``.devN`` 后缀可以用于唯一标识此类发布版本以供发布，同时原始的基于 DVCS 的标签可以存储在项目的元数据中。

    标识哈希信息也可以包含在本地版本标签中。

.. tab:: 英文

    Many build tools integrate with distributed version control systems like
    Git and Mercurial in order to add an identifying hash to the version
    identifier. As hashes cannot be ordered reliably such versions are not
    permitted in the public version field.

    As with semantic versioning, the public ``.devN`` suffix may be used to
    uniquely identify such releases for publication, while the original DVCS based
    label can be stored in the project metadata.

    Identifying hash information may also be included in local version labels.


Olson 数据库版本控制
~~~~~~~~~~~~~~~~~~~~~~~~~

**Olson database versioning**

.. tab:: 中文

    ``pytz`` 项目继承了其版本管理方案，来源于相应的 Olson 时区数据库版本管理方案：年份后跟一个小写字母，用于指示该年份内数据库的版本。

    这可以转换为符合规范的公共版本标识符，格式为 ``<year>.<serial>``，其中 serial 从零或一开始（对于 '<year>a' 发布），并随着该年内每次数据库更新递增。

    与其他转换后的版本标识符一样，相应的 Olson 数据库版本可以记录在项目的元数据中。

.. tab:: 英文

    The ``pytz`` project inherits its versioning scheme from the corresponding
    Olson timezone database versioning scheme: the year followed by a lowercase
    character indicating the version of the database within that year.

    This can be translated to a compliant public version identifier as
    ``<year>.<serial>``, where the serial starts at zero or one (for the
    '<year>a' release) and is incremented with each subsequent database
    update within the year.

    As with other translated version identifiers, the corresponding Olson
    database version could be recorded in the project metadata.


版本说明符
==================

**Version specifiers**

.. tab:: 中文

    版本说明符由一系列版本条款组成，条款之间用逗号分隔。例如::

        ~= 0.9, >= 1.0, != 1.3.4.*, < 2.0

    比较运算符决定了版本条款的类型：

    * ``~=``: `兼容发布 <Compatible release_>`_ 条款
    * ``==``: `版本匹配 <Version matching_>`_ 条款
    * ``!=``: `版本排除 <Version exclusion_>`_ 条款
    * ``<=``, ``>=``: `包含性排序比较 <Inclusive ordered comparison_>`_ 条款
    * ``<``, ``>``: `排他性排序比较 <Exclusive ordered comparison_>`_ 条款
    * ``===``: `任意相等 <Arbitrary equality_>`_ 条款。

    逗号（","）相当于逻辑**与**运算符：候选版本必须匹配所有给定的版本条款，才能与整个说明符匹配。

    条件运算符和后续版本标识符之间的空白是可选的，逗号周围的空白也是如此。

    当多个候选版本与版本说明符匹配时，首选版本应为通过标准 `版本方案 <Version scheme_>`_ 定义的一致排序确定的最新版本。是否将预发布版本视为候选版本，应按照 `预发布版本的处理 <Handling of pre-releases_>`_ 中描述的方式处理。

    除非特别说明，本地版本标识符不得在版本说明符中使用，检查候选版本是否匹配给定版本说明符时，必须完全忽略本地版本标签。

.. tab:: 英文

    A version specifier consists of a series of version clauses, separated by
    commas. For example::

        ~= 0.9, >= 1.0, != 1.3.4.*, < 2.0

    The comparison operator determines the kind of version clause:

    * ``~=``: `Compatible release`_ clause
    * ``==``: `Version matching`_ clause
    * ``!=``: `Version exclusion`_ clause
    * ``<=``, ``>=``: `Inclusive ordered comparison`_ clause
    * ``<``, ``>``: `Exclusive ordered comparison`_ clause
    * ``===``: `Arbitrary equality`_ clause.

    The comma (",") is equivalent to a logical **and** operator: a candidate
    version must match all given version clauses in order to match the
    specifier as a whole.

    Whitespace between a conditional operator and the following version
    identifier is optional, as is the whitespace around the commas.

    When multiple candidate versions match a version specifier, the preferred
    version SHOULD be the latest version as determined by the consistent
    ordering defined by the standard `Version scheme`_. Whether or not
    pre-releases are considered as candidate versions SHOULD be handled as
    described in `Handling of pre-releases`_.

    Except where specifically noted below, local version identifiers MUST NOT be
    permitted in version specifiers, and local version labels MUST be ignored
    entirely when checking if candidate versions match a given version
    specifier.


.. _version-specifiers-compatible-release:
.. _Compatible release:

兼容版本
------------------

**Compatible release**

.. tab:: 中文

    兼容发布条款由兼容发布运算符 ``~=``
    和一个版本标识符组成。它匹配任何预计与指定版本兼容的候选版本。

    指定的版本标识符必须遵循 `版本方案`_ 中描述的标准格式。此版本说明符中不允许使用本地版本标识符。

    对于给定的发布标识符 ``V.N``，兼容发布条款大致等价于以下一对比较条款::

        >= V.N, == V.*

    此运算符不得与单段版本号（例如 ``~=1``）一起使用。

    例如，以下版本条款组是等效的::

        ~= 2.2
        >= 2.2, == 2.*

        ~= 1.4.5
        >= 1.4.5, == 1.4.*

    如果在兼容发布条款中指定了一个预发布、后发布或开发发布，如 ``V.N.suffix``，则在确定所需的前缀匹配时，会忽略后缀::

        ~= 2.2.post3
        >= 2.2.post3, == 2.*

        ~= 1.4.5a4
        >= 1.4.5a4, == 1.4.*

    发布段比较的填充规则意味着，可以通过在版本说明符中附加额外的零来控制兼容发布条款中的假定前向兼容性程度::

        ~= 2.2.0
        >= 2.2.0, == 2.2.*

        ~= 1.4.5.0
        >= 1.4.5.0, == 1.4.5.*

.. tab:: 英文

    A compatible release clause consists of the compatible release operator ``~=``
    and a version identifier. It matches any candidate version that is expected
    to be compatible with the specified version.

    The specified version identifier must be in the standard format described in
    `Version scheme`_. Local version identifiers are NOT permitted in this
    version specifier.

    For a given release identifier ``V.N``, the compatible release clause is
    approximately equivalent to the pair of comparison clauses::

        >= V.N, == V.*

    This operator MUST NOT be used with a single segment version number such as
    ``~=1``.

    For example, the following groups of version clauses are equivalent::

        ~= 2.2
        >= 2.2, == 2.*

        ~= 1.4.5
        >= 1.4.5, == 1.4.*

    If a pre-release, post-release or developmental release is named in a
    compatible release clause as ``V.N.suffix``, then the suffix is ignored
    when determining the required prefix match::

        ~= 2.2.post3
        >= 2.2.post3, == 2.*

        ~= 1.4.5a4
        >= 1.4.5a4, == 1.4.*

    The padding rules for release segment comparisons means that the assumed
    degree of forward compatibility in a compatible release clause can be
    controlled by appending additional zeros to the version specifier::

        ~= 2.2.0
        >= 2.2.0, == 2.2.*

        ~= 1.4.5.0
        >= 1.4.5.0, == 1.4.5.*

.. _Version matching:

版本匹配
----------------

**Version matching**

.. tab:: 中文

    版本匹配条款包括版本匹配运算符 ``==``
    和一个版本标识符。

    指定的版本标识符必须遵循 `版本方案 <Version scheme_>`_ 中描述的标准格式，但公共版本标识符后缀 ``.*`` 是允许的，如下所述。

    默认情况下，版本匹配运算符基于严格的相等比较：指定的版本必须与请求的版本完全相同。执行的*唯一*替代操作是对发布段进行零填充，以确保发布段按相同长度进行比较。

    是否使用严格的版本匹配取决于版本说明符的具体使用场景。自动化工具应至少发出警告，并且如果严格版本匹配使用不当，可以完全拒绝使用。

    可以通过在版本匹配条款中的版本标识符后附加 ``.*`` 后缀来请求前缀匹配，而不是严格比较。这意味着在确定版本标识符是否匹配条款时，会忽略附加的尾随段。如果指定的版本仅包含发布段，则发布段中的尾随组件（或缺少尾随组件）也会被忽略。

    例如，给定版本 ``1.1.post1``，以下条款将匹配或不匹配，如下所示::

        == 1.1        # 不相等，因此 1.1.post1 不匹配该条款
        == 1.1.post1  # 相等，因此 1.1.post1 匹配该条款
        == 1.1.*      # 相同前缀，因此 1.1.post1 匹配该条款

    对于前缀匹配，预发布段被视为有一个隐含的前导 ``.``，因此，给定版本 ``1.1a1``，以下条款将匹配或不匹配，如下所示::

        == 1.1        # 不相等，因此 1.1a1 不匹配该条款
        == 1.1a1      # 相等，因此 1.1a1 匹配该条款
        == 1.1.*      # 相同前缀，因此如果请求预发布版本，则 1.1a1 匹配该条款

    精确匹配也被视为前缀匹配（这种解释由版本标识符发布段的常规零填充规则所隐含）。给定版本 ``1.1``，以下条款将匹配或不匹配，如下所示::

        == 1.1        # 相等，因此 1.1 匹配该条款
        == 1.1.0      # 零填充将 1.1 扩展为 1.1.0，因此匹配该条款
        == 1.1.dev1   # 不相等（开发版本），因此 1.1 不匹配该条款
        == 1.1a1      # 不相等（预发布版本），因此 1.1 不匹配该条款
        == 1.1.post1  # 不相等（后发布版本），因此 1.1 不匹配该条款
        == 1.1.*      # 相同前缀，因此 1.1 匹配该条款

    包含开发版或本地发布版本的前缀匹配（如 ``1.0.dev1.*`` 或 ``1.0+foo1.*``）是无效的。如果存在，开发发布段始终是公共版本中的最后一个段，本地版本在比较时会被忽略，因此在前缀匹配中使用它们没有意义。

    在定义发布分发的依赖关系时，强烈不推荐使用 ``==``（没有至少带有通配符后缀），因为这会大大复杂化安全修复的部署。严格的版本比较运算符主要用于在定义应用程序的可重复*部署*时，使用共享分发索引。

    如果指定的版本标识符是公共版本标识符（没有本地版本标签），则在匹配版本时，候选版本的本地版本标签必须被忽略。

    如果指定的版本标识符是本地版本标识符，则在匹配版本时，必须考虑候选版本的本地版本标签，公共版本标识符按照上述方式进行匹配，本地版本标签则使用严格的字符串相等比较进行检查。

.. tab:: 英文

    A version matching clause includes the version matching operator ``==``
    and a version identifier.

    The specified version identifier must be in the standard format described in
    `Version scheme`_, but a trailing ``.*`` is permitted on public version
    identifiers as described below.

    By default, the version matching operator is based on a strict equality
    comparison: the specified version must be exactly the same as the requested
    version. The *only* substitution performed is the zero padding of the
    release segment to ensure the release segments are compared with the same
    length.

    Whether or not strict version matching is appropriate depends on the specific
    use case for the version specifier. Automated tools SHOULD at least issue
    warnings and MAY reject them entirely when strict version matches are used
    inappropriately.

    Prefix matching may be requested instead of strict comparison, by appending
    a trailing ``.*`` to the version identifier in the version matching clause.
    This means that additional trailing segments will be ignored when
    determining whether or not a version identifier matches the clause. If the
    specified version includes only a release segment, then trailing components
    (or the lack thereof) in the release segment are also ignored.

    For example, given the version ``1.1.post1``, the following clauses would
    match or not as shown::

        == 1.1        # Not equal, so 1.1.post1 does not match clause
        == 1.1.post1  # Equal, so 1.1.post1 matches clause
        == 1.1.*      # Same prefix, so 1.1.post1 matches clause

    For purposes of prefix matching, the pre-release segment is considered to
    have an implied preceding ``.``, so given the version ``1.1a1``, the
    following clauses would match or not as shown::

        == 1.1        # Not equal, so 1.1a1 does not match clause
        == 1.1a1      # Equal, so 1.1a1 matches clause
        == 1.1.*      # Same prefix, so 1.1a1 matches clause if pre-releases are requested

    An exact match is also considered a prefix match (this interpretation is
    implied by the usual zero padding rules for the release segment of version
    identifiers). Given the version ``1.1``, the following clauses would
    match or not as shown::

        == 1.1        # Equal, so 1.1 matches clause
        == 1.1.0      # Zero padding expands 1.1 to 1.1.0, so it matches clause
        == 1.1.dev1   # Not equal (dev-release), so 1.1 does not match clause
        == 1.1a1      # Not equal (pre-release), so 1.1 does not match clause
        == 1.1.post1  # Not equal (post-release), so 1.1 does not match clause
        == 1.1.*      # Same prefix, so 1.1 matches clause

    It is invalid to have a prefix match containing a development or local release
    such as ``1.0.dev1.*`` or ``1.0+foo1.*``. If present, the development release
    segment is always the final segment in the public version, and the local version
    is ignored for comparison purposes, so using either in a prefix match wouldn't
    make any sense.

    The use of ``==`` (without at least the wildcard suffix) when defining
    dependencies for published distributions is strongly discouraged as it
    greatly complicates the deployment of security fixes. The strict version
    comparison operator is intended primarily for use when defining
    dependencies for repeatable *deployments of applications* while using
    a shared distribution index.

    If the specified version identifier is a public version identifier (no
    local version label), then the local version label of any candidate versions
    MUST be ignored when matching versions.

    If the specified version identifier is a local version identifier, then the
    local version labels of candidate versions MUST be considered when matching
    versions, with the public version identifier being matched as described
    above, and the local version label being checked for equivalence using a
    strict string equality comparison.

.. _Version exclusion:

版本排除
-----------------

**Version exclusion**

.. tab:: 中文

    版本排除条款包括版本排除运算符 ``!=``
    和一个版本标识符。

    允许的版本标识符和比较语义与 `版本匹配 <Version matching_>`_ 运算符相同，只不过匹配的意义是相反的。

    例如，给定版本 ``1.1.post1``，以下条款将匹配或不匹配，如下所示::

        != 1.1        # 不相等，因此 1.1.post1 匹配该条款
        != 1.1.post1  # 相等，因此 1.1.post1 不匹配该条款
        != 1.1.*      # 相同前缀，因此 1.1.post1 不匹配该条款

.. tab:: 英文

    A version exclusion clause includes the version exclusion operator ``!=``
    and a version identifier.

    The allowed version identifiers and comparison semantics are the same as
    those of the `Version matching`_ operator, except that the sense of any
    match is inverted.

    For example, given the version ``1.1.post1``, the following clauses would
    match or not as shown::

        != 1.1        # Not equal, so 1.1.post1 matches clause
        != 1.1.post1  # Equal, so 1.1.post1 does not match clause
        != 1.1.*      # Same prefix, so 1.1.post1 does not match clause

.. _Inclusive ordered comparison:

包含有序比较
----------------------------

**Inclusive ordered comparison**

.. tab:: 中文

    包含比较运算符的有序比较条款包括一个比较运算符和一个版本标识符，并将匹配任何版本，在该版本中，基于候选版本和指定版本的相对位置，比较是正确的，比较依据是标准 `版本方案 <Version scheme_>`_ 所定义的一致排序。

    包含的有序比较运算符是 ``<=`` 和 ``>=``。

    与版本匹配一样，发布段会根据需要进行零填充，以确保发布段按相同的长度进行比较。

    本地版本标识符在此版本说明符中不允许使用。

.. tab:: 英文

    An inclusive ordered comparison clause includes a comparison operator and a
    version identifier, and will match any version where the comparison is correct
    based on the relative position of the candidate version and the specified
    version given the consistent ordering defined by the standard
    `Version scheme`_.

    The inclusive ordered comparison operators are ``<=`` and ``>=``.

    As with version matching, the release segment is zero padded as necessary to
    ensure the release segments are compared with the same length.

    Local version identifiers are NOT permitted in this version specifier.

.. _Exclusive ordered comparison:

排除有序比较
----------------------------

**Exclusive ordered comparison**

.. tab:: 中文

    排他性有序比较 ``>`` 和 ``<`` 与包含有序比较类似，它们依赖于候选版本和指定版本的相对位置，比较依据是标准 `版本方案 <Version scheme_>`_ 所定义的一致排序。然而，它们特别排除了指定版本的预发布、后发布和本地版本。

    排他性有序比较 ``>V`` **不得** 允许给定版本的后发布，除非 ``V`` 本身是后发布版本。您可以通过使用 ``>V.postN`` 强制要求发布晚于特定的后发布版本，包括额外的后发布版本。例如，``>1.7`` 将允许 ``1.7.1`` 但不允许 ``1.7.0.post1``，而 ``>1.7.post2`` 将允许 ``1.7.1`` 和 ``1.7.0.post3``，但不允许 ``1.7.0``。

    排他性有序比较 ``>V`` **不得** 匹配指定版本的本地版本。

    排他性有序比较 ``<V`` **不得** 允许指定版本的预发布，除非指定版本本身就是预发布版本。通过使用 ``<V.rc1`` 或类似的形式，可以允许早于特定预发布版本的预发布，但不等于该特定版本的预发布。

    与版本匹配一样，发布段会根据需要进行零填充，以确保发布段按相同的长度进行比较。

    本地版本标识符在此版本说明符中不允许使用。

.. tab:: 英文

    The exclusive ordered comparisons ``>`` and ``<`` are similar to the inclusive
    ordered comparisons in that they rely on the relative position of the candidate
    version and the specified version given the consistent ordering defined by the
    standard `Version scheme`_. However, they specifically exclude pre-releases,
    post-releases, and local versions of the specified version.

    The exclusive ordered comparison ``>V`` **MUST NOT** allow a post-release
    of the given version unless ``V`` itself is a post release. You may mandate
    that releases are later than a particular post release, including additional
    post releases, by using ``>V.postN``. For example, ``>1.7`` will allow
    ``1.7.1`` but not ``1.7.0.post1`` and ``>1.7.post2`` will allow ``1.7.1``
    and ``1.7.0.post3`` but not ``1.7.0``.

    The exclusive ordered comparison ``>V`` **MUST NOT** match a local version of
    the specified version.

    The exclusive ordered comparison ``<V`` **MUST NOT** allow a pre-release of
    the specified version unless the specified version is itself a pre-release.
    Allowing pre-releases that are earlier than, but not equal to a specific
    pre-release may be accomplished by using ``<V.rc1`` or similar.

    As with version matching, the release segment is zero padded as necessary to
    ensure the release segments are compared with the same length.

    Local version identifiers are NOT permitted in this version specifier.

.. _Arbitrary equality:

任意相等
------------------

**Arbitrary equality**

.. tab:: 中文

    任意相等比较是简单的字符串相等操作，它不考虑任何语义信息，如零填充或本地版本。该运算符也不支持像 ``==`` 运算符那样的前缀匹配。

    任意相等的主要用例是允许指定一个版本，该版本无法通过本规范表示。这个运算符是特殊的，它充当逃生阀，允许使用实现此规范的工具的人安装一个否则与本规范不兼容的遗留版本。

    一个例子是 ``===foobar``，它将匹配 ``foobar`` 的版本。

    该运算符也可以用来显式要求一个未打补丁的项目版本，例如 ``===1.0``，它不会匹配版本 ``1.0+downstream1``。

    强烈不建议使用此运算符，工具在使用时 **可能** 会显示警告。

.. tab:: 英文

    Arbitrary equality comparisons are simple string equality operations which do
    not take into account any of the semantic information such as zero padding or
    local versions. This operator also does not support prefix matching as the
    ``==`` operator does.

    The primary use case for arbitrary equality is to allow for specifying a
    version which cannot otherwise be represented by this specification. This operator is
    special and acts as an escape hatch to allow someone using a tool which
    implements this specification to still install a legacy version which is otherwise
    incompatible with this specification.

    An example would be ``===foobar`` which would match a version of ``foobar``.

    This operator may also be used to explicitly require an unpatched version
    of a project such as ``===1.0`` which would not match for a version
    ``1.0+downstream1``.

    Use of this operator is heavily discouraged and tooling MAY display a warning
    when it is used.

.. _Handling of pre-releases:

处理预发布版本
------------------------

**Handling of pre-releases**

.. tab:: 中文

    任何类型的预发布版本，包括开发版本，默认情况下都从所有版本说明符中排除， *除非* 它们已经安装在系统中、被用户显式请求，或如果满足版本说明符的唯一可用版本是预发布版本。

    默认情况下，依赖解析工具 **应** ：

    * 接受所有版本说明符中已经安装的预发布版本
    * 对于没有满足版本说明符的最终版本或后发布版本的版本说明符，接受远程可用的预发布版本
    * 排除所有其他预发布版本的考虑

    如果需要预发布版本来满足版本说明符，依赖解析工具 **可能** 会发出警告。

    依赖解析工具 **应** 允许用户请求以下替代行为：

    * 接受所有版本说明符的预发布版本
    * 排除所有版本说明符的预发布版本（如果预发布版本已在本地安装，或如果预发布版本是满足特定说明符的唯一方式，则报告错误或警告）

    依赖解析工具 **可能** 还允许基于每个发行版来控制上述行为。

    后发布版本和最终版本在版本说明符中没有特殊处理 —— 它们总是包含在内，除非被显式排除。

.. tab:: 英文

    Pre-releases of any kind, including developmental releases, are implicitly
    excluded from all version specifiers, *unless* they are already present
    on the system, explicitly requested by the user, or if the only available
    version that satisfies the version specifier is a pre-release.

    By default, dependency resolution tools SHOULD:

    * accept already installed pre-releases for all version specifiers
    * accept remotely available pre-releases for version specifiers where
      there is no final or post release that satisfies the version specifier
    * exclude all other pre-releases from consideration

    Dependency resolution tools MAY issue a warning if a pre-release is needed
    to satisfy a version specifier.

    Dependency resolution tools SHOULD also allow users to request the
    following alternative behaviours:

    * accepting pre-releases for all version specifiers
    * excluding pre-releases for all version specifiers (reporting an error or
      warning if a pre-release is already installed locally, or if a
      pre-release is the only way to satisfy a particular specifier)

    Dependency resolution tools MAY also allow the above behaviour to be
    controlled on a per-distribution basis.

    Post-releases and final releases receive no special treatment in version
    specifiers - they are always included unless explicitly excluded.


示例
--------

**Examples**

.. tab:: 中文

    * ``~=3.1``：版本 3.1 或更高版本，但不包括版本 4.0 或更高版本。
    * ``~=3.1.2``：版本 3.1.2 或更高版本，但不包括版本 3.2.0 或更高版本。
    * ``~=3.1a1``：版本 3.1a1 或更高版本，但不包括版本 4.0 或更高版本。
    * ``== 3.1``：特定版本 3.1（或 3.1.0），排除所有预发布版本、后发布版本、开发版本以及任何 3.1.x 的维护版本。
    * ``== 3.1.*``：任何以 3.1 开头的版本。等同于 ``~=3.1.0`` 兼容发布条款。
    * ``~=3.1.0, != 3.1.3``：版本 3.1.0 或更高版本，但不包括版本 3.1.3，也不包括版本 3.2.0 或更高版本。

.. tab:: 英文

    * ``~=3.1``: version 3.1 or later, but not version 4.0 or later.
    * ``~=3.1.2``: version 3.1.2 or later, but not version 3.2.0 or later.
    * ``~=3.1a1``: version 3.1a1 or later, but not version 4.0 or later.
    * ``== 3.1``: specifically version 3.1 (or 3.1.0), excludes all pre-releases,
      post releases, developmental releases and any 3.1.x maintenance releases.
    * ``== 3.1.*``: any version that starts with 3.1. Equivalent to the
    ``~=3.1.0`` compatible release clause.
    * ``~=3.1.0, != 3.1.3``: version 3.1.0 or later, but not version 3.1.3 and
      not version 3.2.0 or later.


直接引用
=================

**Direct references**

.. tab:: 中文

    一些自动化工具可能允许使用直接引用作为普通版本说明符的替代方案。直接引用由符号 ``@`` 和一个明确的 URL 组成。

    是否使用直接引用取决于版本说明符的具体使用场景。自动化工具 **应至少发出警告** ，并 **可能完全拒绝** 在不合适的情况下使用直接引用。

    公共索引服务器 **不应** 允许在上传的分发包中使用直接引用。直接引用主要作为软件集成者的工具，而非发布者的工具。

    根据使用场景，直接 URL 引用的合适目标可能是源代码包（sdist）或轮子（wheel）二进制归档文件。具体支持的 URL 和目标将取决于工具。

    例如，可以直接引用本地源代码归档文件::

        pip @ file:///localbuilds/pip-1.3.1.zip

    或者，也可以引用预构建的归档文件::

        pip @ file:///localbuilds/pip-1.3.1-py33-none-any.whl

    所有不指向本地文件 URL 的直接引用 **应** 指定安全的传输机制（如 ``https``），并在 URL 中包含预期的哈希值以供验证。如果直接引用未指定任何哈希信息，或者提供了工具无法理解的哈希信息，或使用了工具认为不可信的哈希算法，自动化工具 **应至少发出警告** ，并 **可能拒绝** 依赖该 URL。如果该直接引用还使用了不安全的传输方式，自动化工具 **不应** 依赖该 URL。

    **推荐** 仅使用标准库 `hashlib` 模块最新版本无条件提供的哈希算法来进行源代码归档哈希验证。在本文写作时，这些哈希算法包括 ``'md5'``、``'sha1'``、``'sha224'``、``'sha256'``、``'sha384'`` 和 ``'sha512'``。

    对于源代码归档和轮子文件引用，可以通过在 URL 片段中包含 ``<hash-algorithm>=<expected-hash>`` 条目来指定预期的哈希值。

    对于版本控制引用，应使用 ``VCS+protocol`` 方案来标识版本控制系统和安全的传输方式，并且应使用支持基于哈希的提交标识符的版本控制系统。自动化工具 **可以省略** 对于不提供基于哈希的提交标识符的版本控制系统的缺少哈希警告。

    对于不支持在 URL 中直接包含提交或标签引用的版本控制系统，可以使用 ``@<commit-hash>`` 或 ``@<tag>#<commit-hash>`` 语法将这些信息附加到 URL 的末尾。

    .. note::

        这与现有的 pip 支持的 VCS 引用语法 **不完全** 相同。首先，分发包名称被移到 URL 前面，而不是嵌入在 URL 中。其次，即使是基于标签获取的，也会包括提交哈希，以满足上述要求： *每个* 链接都应包含哈希值，以增加伪造的难度（创建一个具有特定标签的恶意仓库比较容易，而创建一个具有特定 *哈希* 的仓库则不容易）。

    远程 URL 示例::

        pip @ https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686
        pip @ git+https://github.com/pypa/pip.git@7921be1537eac1e97bc40179a57f0349c2aee67d
        pip @ git+https://github.com/pypa/pip.git@1.3.1#7921be1537eac1e97bc40179a57f0349c2aee67d

.. tab:: 英文

    Some automated tools may permit the use of a direct reference as an
    alternative to a normal version specifier. A direct reference consists of
    the specifier ``@`` and an explicit URL.

    Whether or not direct references are appropriate depends on the specific
    use case for the version specifier. Automated tools SHOULD at least issue
    warnings and MAY reject them entirely when direct references are used
    inappropriately.

    Public index servers SHOULD NOT allow the use of direct references in
    uploaded distributions. Direct references are intended as a tool for
    software integrators rather than publishers.

    Depending on the use case, some appropriate targets for a direct URL
    reference may be an sdist or a wheel binary archive. The exact URLs and
    targets supported will be tool dependent.

    For example, a local source archive may be referenced directly::

        pip @ file:///localbuilds/pip-1.3.1.zip

    Alternatively, a prebuilt archive may also be referenced::

        pip @ file:///localbuilds/pip-1.3.1-py33-none-any.whl

    All direct references that do not refer to a local file URL SHOULD specify
    a secure transport mechanism (such as ``https``) AND include an expected
    hash value in the URL for verification purposes. If a direct reference is
    specified without any hash information, with hash information that the
    tool doesn't understand, or with a selected hash algorithm that the tool
    considers too weak to trust, automated tools SHOULD at least emit a warning
    and MAY refuse to rely on the URL. If such a direct reference also uses an
    insecure transport, automated tools SHOULD NOT rely on the URL.

    It is RECOMMENDED that only hashes which are unconditionally provided by
    the latest version of the standard library's :py:mod:`hashlib` module be used
    for source archive hashes. At time of writing, that list consists of
    ``'md5'``, ``'sha1'``, ``'sha224'``, ``'sha256'``, ``'sha384'``, and
    ``'sha512'``.

    For source archive and wheel references, an expected hash value may be
    specified by including a ``<hash-algorithm>=<expected-hash>`` entry as
    part of the URL fragment.

    For version control references, the ``VCS+protocol`` scheme SHOULD be
    used to identify both the version control system and the secure transport,
    and a version control system with hash based commit identifiers SHOULD be
    used. Automated tools MAY omit warnings about missing hashes for version
    control systems that do not provide hash based commit identifiers.

    To handle version control systems that do not support including commit or
    tag references directly in the URL, that information may be appended to the
    end of the URL using the ``@<commit-hash>`` or the ``@<tag>#<commit-hash>``
    notation.

    .. note::

        This isn't *quite* the same as the existing VCS reference notation
        supported by pip. Firstly, the distribution name is moved in front rather
        than embedded as part of the URL. Secondly, the commit hash is included
        even when retrieving based on a tag, in order to meet the requirement
        above that *every* link should include a hash to make things harder to
        forge (creating a malicious repo with a particular tag is easy, creating
        one with a specific *hash*, less so).

    Remote URL examples::

        pip @ https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686
        pip @ git+https://github.com/pypa/pip.git@7921be1537eac1e97bc40179a57f0349c2aee67d
        pip @ git+https://github.com/pypa/pip.git@1.3.1#7921be1537eac1e97bc40179a57f0349c2aee67d


文件 URL
---------

**File URLs**

.. tab:: 中文

    文件 URL 的形式为 ``file://<host>/<path>``。如果省略了 ``<host>``，则默认认为它是 ``localhost``，即使省略了 ``<host>``，第三个斜杠 **仍然必须** 存在。 ``<path>`` 定义了要访问的文件系统路径。

    在各种 \*nix 操作系统中， ``<host>`` 的唯一允许值是省略、``localhost``，或者当前计算机认为与其自身主机匹配的其他完全限定域名（FQDN）。换句话说，在 \*nix 系统中， ``file://`` 方案只能用于访问本地计算机上的路径。

    在 Windows 系统中，如果适用，文件格式应包括驱动器字母，作为 ``<path>`` 的一部分（例如 ``file:///c:/path/to/a/file``）。与 \*nix 不同，在 Windows 中， ``<host>`` 参数可以用于指定位于网络共享上的文件。换句话说，为了将 ``\\machine\volume\file`` 转换为 ``file://`` URL，它将变成 ``file://machine/volume/file``。有关 Windows 上 ``file://`` URL 的更多信息，请参见 `MSDN <https://web.archive.org/web/20130321051043/http://blogs.msdn.com/b/ie/archive/2006/12/06/file-uris-in-windows.aspx>`_。

.. tab:: 英文

    File URLs take the form of ``file://<host>/<path>``. If the ``<host>`` is
    omitted it is assumed to be ``localhost`` and even if the ``<host>`` is omitted
    the third slash MUST still exist. The ``<path>`` defines what the file path on
    the filesystem that is to be accessed.

    On the various \*nix operating systems the only allowed values for ``<host>``
    is for it to be omitted, ``localhost``, or another FQDN that the current
    machine believes matches its own host. In other words, on \*nix the ``file://``
    scheme can only be used to access paths on the local machine.

    On Windows the file format should include the drive letter if applicable as
    part of the ``<path>`` (e.g. ``file:///c:/path/to/a/file``). Unlike \*nix on
    Windows the ``<host>`` parameter may be used to specify a file residing on a
    network share. In other words, in order to translate ``\\machine\volume\file``
    to a ``file://`` url, it would end up as ``file://machine/volume/file``. For
    more information on ``file://`` URLs on Windows see
    `MSDN <https://web.archive.org/web/20130321051043/http://blogs.msdn.com/b/ie/archive/2006/12/06/file-uris-in-windows.aspx>`_.



与 pkg_resources.parse_version 的差异总结
=======================================================

**Summary of differences from pkg_resources.parse_version**

.. tab:: 中文

    * 注意：此比较是针对 ``pkg_resources.parse_version`` 在 :pep:`440` 编写时的实现。PEP 被接受后，setuptools 6.0 及更高版本采纳了此处描述的行为。

    * 本地版本的排序方式不同，本规范要求本地版本排序为大于没有本地版本的相同版本，而 ``pkg_resources.parse_version`` 将其视为预发布标记。

    * 本规范故意限制了有效版本的语法，而 ``pkg_resources.parse_version`` 尝试从 *任何* 任意字符串中提供某种意义。

    * ``pkg_resources.parse_version`` 允许任意深度嵌套的版本标识符，如 ``1.0.dev1.post1.dev5``。然而，本规范仅允许每种类型的标识符使用一次，并且它们必须按照特定的顺序存在。

.. tab:: 英文

    * Note: this comparison is to ``pkg_resources.parse_version`` as it existed at the time :pep:`440` was written. After the PEP was accepted, setuptools 6.0 and later versions adopted the behaviour described here.

    * Local versions sort differently, this specification requires that they sort as greater than the same version without a local version, whereas ``pkg_resources.parse_version`` considers it a pre-release marker.

    * This specification purposely restricts the syntax which constitutes a valid version while ``pkg_resources.parse_version`` attempts to provide some meaning from *any* arbitrary string.

    * ``pkg_resources.parse_version`` allows arbitrarily deeply nested version signifiers like ``1.0.dev1.post1.dev5``. This specification however allows only a single use of each type and they must exist in a certain order.



.. _version-specifiers-regex:

附录：使用正则表达式解析版本字符串
==========================================================

**Appendix: Parsing version strings with regular expressions**

.. tab:: 中文

    如前所述，在 :ref:`public-version-identifiers` 部分，发布的版本标识符应该使用规范格式。本节提供了可以用来测试版本是否已经是该格式的正则表达式，如果不是，则提取各个组件以便后续标准化。

    要测试版本标识符是否符合规范格式，可以使用以下函数：

    .. code-block:: python

        import re
        def is_canonical(version):
            return re.match(r'^([1-9][0-9]*!)?(0|[1-9][0-9]*)(\.(0|[1-9][0-9]*))*((a|b|rc)(0|[1-9][0-9]*))?(\.post(0|[1-9][0-9]*))?(\.dev(0|[1-9][0-9]*))?$', version) is not None

    要提取版本标识符的组件，使用以下正则表达式（由 `packaging <https://github.com/pypa/packaging>`_ 项目定义）：

.. tab:: 英文

    As noted earlier in the :ref:`public-version-identifiers` section,
    published version identifiers SHOULD use the canonical format. This
    section provides regular expressions that can be used to test whether a
    version is already in that form, and if it's not, extract the various
    components for subsequent normalization.

    To test whether a version identifier is in the canonical format, you can use
    the following function:

    .. code-block:: python

        import re
        def is_canonical(version):
            return re.match(r'^([1-9][0-9]*!)?(0|[1-9][0-9]*)(\.(0|[1-9][0-9]*))*((a|b|rc)(0|[1-9][0-9]*))?(\.post(0|[1-9][0-9]*))?(\.dev(0|[1-9][0-9]*))?$', version) is not None

    To extract the components of a version identifier, use the following regular
    expression (as defined by the `packaging <https://github.com/pypa/packaging>`_
    project):

.. code-block:: python

    VERSION_PATTERN = r"""
        v?
        (?:
            (?:(?P<epoch>[0-9]+)!)?                           # epoch
            (?P<release>[0-9]+(?:\.[0-9]+)*)                  # release segment
            (?P<pre>                                          # pre-release
                [-_\.]?
                (?P<pre_l>(a|b|c|rc|alpha|beta|pre|preview))
                [-_\.]?
                (?P<pre_n>[0-9]+)?
            )?
            (?P<post>                                         # post release
                (?:-(?P<post_n1>[0-9]+))
                |
                (?:
                    [-_\.]?
                    (?P<post_l>post|rev|r)
                    [-_\.]?
                    (?P<post_n2>[0-9]+)?
                )
            )?
            (?P<dev>                                          # dev release
                [-_\.]?
                (?P<dev_l>dev)
                [-_\.]?
                (?P<dev_n>[0-9]+)?
            )?
        )
        (?:\+(?P<local>[a-z0-9]+(?:[-_\.][a-z0-9]+)*))?       # local version
    """

    _regex = re.compile(
        r"^\s*" + VERSION_PATTERN + r"\s*$",
        re.VERBOSE | re.IGNORECASE,
    )



历史
=======

**History**

.. tab:: 中文

    - 2014 年 8 月: 该规范通过 :pep:`440` 批准。

.. tab:: 英文

    - August 2014: This specification was approved through :pep:`440`.
