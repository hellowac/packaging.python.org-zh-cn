.. _declaring-project-metadata:
.. _pyproject-toml-spec:

================================
``pyproject.toml`` 规范
================================

``pyproject.toml`` **specification**

.. tab:: 中文

  .. warning::

    这是一个 **技术性、正式的规范** 。如需获取关于 ``pyproject.toml`` 的温和、用户友好的指南，请参见 :ref:`writing-pyproject-toml`。

  ``pyproject.toml`` 文件作为与打包相关工具（以及其他工具）的配置文件。

  .. note:: 此规范最初定义于 :pep:`518` 和 :pep:`621` 。

  ``pyproject.toml`` 文件采用 `TOML <https://toml.io>`_ 格式编写。目前指定了三个表格，分别是
  :ref:`[build-system] <pyproject-build-system-table>`，
  :ref:`[project] <pyproject-project-table>` 和
  :ref:`[tool] <pyproject-tool-table>`。其他表格保留以供未来使用（工具特定的配置应使用 ``[tool]`` 表格）。

.. tab:: 英文

  .. warning::

    This is a **technical, formal specification**. For a gentle, user-friendly guide to ``pyproject.toml``, see :ref:`writing-pyproject-toml`.

  The ``pyproject.toml`` file acts as a configuration file for packaging-related
  tools (as well as other tools).

  .. note:: This specification was originally defined in :pep:`518` and :pep:`621`.

  The ``pyproject.toml`` file is written in `TOML <https://toml.io>`_. Three
  tables are currently specified, namely
  :ref:`[build-system] <pyproject-build-system-table>`,
  :ref:`[project] <pyproject-project-table>` and
  :ref:`[tool] <pyproject-tool-table>`. Other tables are reserved for future
  use (tool-specific configuration should use the ``[tool]`` table).

.. _pyproject-build-system-table:

声明构建系统依赖项： ``[build-system]`` 表
=================================================================

**Declaring build system dependencies: the** ``[build-system]`` **table**

.. tab:: 中文

  ``[build-system]`` 表格声明了运行项目构建系统所需安装的任何 Python 级别的依赖项。

  .. TODO: 与 PEP 517 合并

  ``[build-system]`` 表格用于存储与构建相关的数据。最初，表格中只有一个有效且强制的键：``requires``。该键必须包含一个字符串列表，表示执行构建系统所需的依赖项。列表中的字符串遵循 :ref:`version specifier specification <version-specifiers>`。

  以下是一个使用 ``setuptools`` 构建的项目的 ``[build-system]`` 表格示例：

  .. code-block:: toml

    [build-system]
    # 执行构建系统所需的最小要求。
    requires = ["setuptools"]

  构建工具在没有 ``pyproject.toml`` 文件时，应默认使用上述示例配置文件作为其默认语义。

  工具不应要求存在 ``[build-system]`` 表格。``pyproject.toml`` 文件可以用来存储与构建相关的数据以外的配置详情，因此合法地缺少 ``[build-system]`` 表格。如果文件存在但缺少 ``[build-system]`` 表格，则应使用上述指定的默认值。如果表格已指定但缺少必需的字段，则工具应将其视为错误。

  为了仅用于说明目的，提供一个类型特定的数据表示形式，以下 `JSON Schema <https://json-schema.org>`_ 可匹配数据格式：

  .. code-block:: json

    {
        "$schema": "http://json-schema.org/schema#",

        "type": "object",
        "additionalProperties": false,

        "properties": {
            "build-system": {
                "type": "object",
                "additionalProperties": false,

                "properties": {
                    "requires": {
                        "type": "array",
                        "items": {
                            "type": "string"
                        }
                    }
                },
                "required": ["requires"]
            },

            "tool": {
                "type": "object"
            }
        }
    }

.. tab:: 英文

  The ``[build-system]`` table declares any Python level dependencies that
  must be installed in order to run the project's build system
  successfully.

  .. TODO: merge with PEP 517

  The ``[build-system]`` table is used to store build-related data.
  Initially, only one key of the table is valid and is mandatory
  for the table: ``requires``. This key must have a value of a list
  of strings representing dependencies required to execute the
  build system. The strings in this list follow the :ref:`version specifier
  specification <version-specifiers>`.

  An example ``[build-system]`` table for a project built with
  ``setuptools`` is:

  .. code-block:: toml

    [build-system]
    # Minimum requirements for the build system to execute.
    requires = ["setuptools"]

  Build tools are expected to use the example configuration file above as
  their default semantics when a ``pyproject.toml`` file is not present.

  Tools should not require the existence of the ``[build-system]`` table.
  A ``pyproject.toml`` file may be used to store configuration details
  other than build-related data and thus lack a ``[build-system]`` table
  legitimately. If the file exists but is lacking the ``[build-system]``
  table then the default values as specified above should be used.
  If the table is specified but is missing required fields then the tool
  should consider it an error.


  To provide a type-specific representation of the resulting data from
  the TOML file for illustrative purposes only, the following
  `JSON Schema <https://json-schema.org>`_ would match the data format:

  .. code-block:: json

    {
        "$schema": "http://json-schema.org/schema#",

        "type": "object",
        "additionalProperties": false,

        "properties": {
            "build-system": {
                "type": "object",
                "additionalProperties": false,

                "properties": {
                    "requires": {
                        "type": "array",
                        "items": {
                            "type": "string"
                        }
                    }
                },
                "required": ["requires"]
            },

            "tool": {
                "type": "object"
            }
        }
    }


.. _pyproject-project-table:

声明项目元数据：``[project]`` 表
===================================================

**Declaring project metadata: the** ``[project]`` **table**

.. tab:: 中文

  ``[project]`` 表格指定了项目的 :ref:`核心元数据 <core-metadata>`。

  元数据分为两种类型： *静态* 和 *动态*。静态元数据直接在 ``pyproject.toml`` 文件中指定，并且不能由工具指定或更改（这包括元数据 *引用* 的内容，例如元数据引用的文件内容）。动态元数据通过 ``dynamic`` 键（在本规范后面定义）列出，表示工具稍后将提供的元数据。

  缺少 ``[project]`` 表格隐含意味着 :term:`构建后端 <Build Backend>` 将动态提供所有键。

  必须静态定义的唯一键是：

  - ``name``

  必需的键可以 *静态* 指定，或者列为动态键：

  - ``version``

  所有其他键都被视为可选的，可以静态指定、列为动态，或者不指定。

  ``[project]`` 表格中允许的完整键列表是：

  - ``authors``
  - ``classifiers``
  - ``dependencies``
  - ``description``
  - ``dynamic``
  - ``entry-points``
  - ``gui-scripts``
  - ``keywords``
  - ``license``
  - ``maintainers``
  - ``name``
  - ``optional-dependencies``
  - ``readme``
  - ``requires-python``
  - ``scripts``
  - ``urls``
  - ``version``

.. tab:: 英文

  The ``[project]`` table specifies the project's :ref:`core metadata <core-metadata>`.

  There are two kinds of metadata: *static* and *dynamic*. Static
  metadata is specified in the ``pyproject.toml`` file directly and
  cannot be specified or changed by a tool (this includes data
  *referred* to by the metadata, e.g. the contents of files referenced
  by the metadata). Dynamic metadata is listed via the ``dynamic`` key
  (defined later in this specification) and represents metadata that a
  tool will later provide.

  The lack of a ``[project]`` table implicitly means the :term:`build backend <Build Backend>`
  will dynamically provide all keys.

  The only keys required to be statically defined are:

  - ``name``

  The keys which are required but may be specified *either* statically
  or listed as dynamic are:

  - ``version``

  All other keys are considered optional and may be specified
  statically, listed as dynamic, or left unspecified.

  The complete list of keys allowed in the ``[project]`` table are:

  - ``authors``
  - ``classifiers``
  - ``dependencies``
  - ``description``
  - ``dynamic``
  - ``entry-points``
  - ``gui-scripts``
  - ``keywords``
  - ``license``
  - ``maintainers``
  - ``name``
  - ``optional-dependencies``
  - ``readme``
  - ``requires-python``
  - ``scripts``
  - ``urls``
  - ``version``


``name``
--------

.. tab:: 中文

  - TOML_ 类型: string  
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段: :ref:`Name <core-metadata-name>`

  项目的名称。

  工具在读取该名称时， **应该** :ref:`规范化 <name-normalization>` 该名称，以确保内部的一致性。

.. tab:: 英文

  - TOML_ type: string
  - Corresponding :ref:`core metadata <core-metadata>` field: :ref:`Name <core-metadata-name>`

  The name of the project.

  Tools SHOULD :ref:`normalize <name-normalization>` this name, as soon as it is read for internal consistency.

``version``
-----------

.. tab:: 中文

  - TOML_ 类型: string  
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段: :ref:`Version <core-metadata-version>`

  项目的版本，如 :ref:`版本标识符规范 <version-specifiers>` 中定义的。

  用户 **应该** 优先指定已经规范化的版本。

.. tab:: 英文

  - TOML_ type: string
  - Corresponding :ref:`core metadata <core-metadata>` field: :ref:`Version <core-metadata-version>`

  The version of the project, as defined in the :ref:`Version specifier specification <version-specifiers>`.

  Users SHOULD prefer to specify already-normalized versions.


``description``
---------------

.. tab:: 中文

  - TOML_ 类型: string
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段: :ref:`Summary <core-metadata-summary>`

  一行即可完成项目的摘要描述。如果包含多行，工具可能会出错。

.. tab:: 英文

  - TOML_ type: string
  - Corresponding :ref:`core metadata <core-metadata>` field: :ref:`Summary <core-metadata-summary>`

  The summary description of the project in one line. Tools MAY error if this includes multiple lines.


``readme``
----------

.. tab:: 中文

  - TOML_ 类型: string 或 table  
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段:  
    :ref:`Description <core-metadata-description>` 和  
    :ref:`Description-Content-Type <core-metadata-description-content-type>`

  项目的完整描述（即 README）。

  该键接受一个字符串或一个表。如果是字符串，则为相对于 ``pyproject.toml`` 的路径，指向包含完整描述的文本文件。工具 **必须** 假定该文件的编码为 UTF-8。如果文件路径以不区分大小写的 ``.md`` 后缀结尾，则工具 **必须** 假定内容类型为 ``text/markdown``。如果文件路径以不区分大小写的 ``.rst`` 结尾，则工具 **必须** 假定内容类型为 ``text/x-rst``。如果工具识别比本 PEP 更多的扩展名，它们 **可以** 为用户推断内容类型，而不需要将此键标记为 ``dynamic``。对于所有未识别的后缀，当未提供内容类型时，工具 **必须** 引发错误。

  ``readme`` 键也可以接受一个表。该表的 ``file`` 键的值是一个字符串，表示相对于 ``pyproject.toml`` 的路径，指向包含完整描述的文件。``text`` 键的值是完整描述的字符串。这两个键是互斥的，因此如果元数据同时指定了这两个键，工具 **必须** 引发错误。

  在 ``readme`` 键指定的表中，还应包含一个 ``content-type`` 键，该键的值是一个字符串，指定完整描述的内容类型。如果元数据未在表中指定此键，工具 **必须** 引发错误。如果元数据未指定 ``charset`` 参数，则假定为 UTF-8。如果工具选择支持其他编码，它们 **可以** 支持其他编码。工具 **可以** 支持其他内容类型，并将其转换为 :ref:`核心元数据 <core-metadata>` 支持的内容类型。如果不支持的内容类型，则工具 **必须** 引发错误。

.. tab:: 英文

  - TOML_ type: string or table
  - Corresponding :ref:`core metadata <core-metadata>` field:
    :ref:`Description <core-metadata-description>` and
    :ref:`Description-Content-Type <core-metadata-description-content-type>`

  The full description of the project (i.e. the README).

  The key accepts either a string or a table. If it is a string then
  it is a path relative to ``pyproject.toml`` to a text file containing
  the full description. Tools MUST assume the file's encoding is UTF-8.
  If the file path ends in a case-insensitive ``.md`` suffix, then tools
  MUST assume the content-type is ``text/markdown``. If the file path
  ends in a case-insensitive ``.rst``, then tools MUST assume the
  content-type is ``text/x-rst``. If a tool recognizes more extensions
  than this PEP, they MAY infer the content-type for the user without
  specifying this key as ``dynamic``. For all unrecognized suffixes
  when a content-type is not provided, tools MUST raise an error.

  The ``readme`` key may also take a table. The ``file`` key has a
  string value representing a path relative to ``pyproject.toml`` to a
  file containing the full description. The ``text`` key has a string
  value which is the full description. These keys are
  mutually-exclusive, thus tools MUST raise an error if the metadata
  specifies both keys.

  A table specified in the ``readme`` key also has a ``content-type``
  key which takes a string specifying the content-type of the full
  description. A tool MUST raise an error if the metadata does not
  specify this key in the table. If the metadata does not specify the
  ``charset`` parameter, then it is assumed to be UTF-8. Tools MAY
  support other encodings if they choose to. Tools MAY support
  alternative content-types which they can transform to a content-type
  as supported by the :ref:`core metadata <core-metadata>`. Otherwise
  tools MUST raise an error for unsupported content-types.


``requires-python``
-------------------

.. tab:: 中文

  - TOML_ 类型: string
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段:
    :ref:`Requires-Python <core-metadata-requires-python>`

  项目的Python版本要求。

.. tab:: 英文

  - TOML_ type: string
  - Corresponding :ref:`core metadata <core-metadata>` field:
    :ref:`Requires-Python <core-metadata-requires-python>`

  The Python version requirements of the project.


``license``
-----------

.. tab:: 中文

  - TOML_ 类型: table
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段:
    :ref:`License <core-metadata-license>`

  该表可以有两个键中的一个。 ``file`` 键的值是一个字符串，表示相对于 ``pyproject.toml`` 的路径，指向包含项目许可证的文件。工具 **必须** 假定该文件的编码为 UTF-8。 ``text`` 键的值是一个字符串，表示项目的许可证。这两个键是互斥的，因此如果元数据同时指定了这两个键，工具 **必须** 引发错误。

.. tab:: 英文

  - TOML_ type: table
  - Corresponding :ref:`core metadata <core-metadata>` field:
    :ref:`License <core-metadata-license>`

  The table may have one of two keys. The ``file`` key has a string
  value that is a file path relative to ``pyproject.toml`` to the file
  which contains the license for the project. Tools MUST assume the
  file's encoding is UTF-8. The ``text`` key has a string value which is
  the license of the project.  These keys are mutually exclusive, so a
  tool MUST raise an error if the metadata specifies both keys.


``authors``/``maintainers``
---------------------------

.. tab:: 中文

  - TOML_ 类型: 具有字符串键和值的内联表的数组
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段:
    :ref:`作者 <core-metadata-author>`、 
    :ref:`作者邮箱 <core-metadata-author-email>`、
    :ref:`维护者 <core-metadata-maintainer>`，以及
    :ref:`维护者邮箱 <core-metadata-maintainer-email>`

  被认为是项目“作者”的人员或组织。其确切含义可以根据解释而有所不同——它可以列出原始或主要作者、当前维护者或包的所有者。

  “维护者”键与“作者”键类似，其确切含义同样可以根据解释而有所不同。

  这些键接受一个表的数组，每个表有两个键：``name`` 和 ``email``。这两个值必须是字符串。``name`` 值必须是有效的电子邮件名称（即：可以作为电子邮件的名称部分，参见 :rfc:`822`），且不能包含逗号。``email`` 值必须是有效的电子邮件地址。这两个键是可选的，但至少必须在表中指定其中一个键。

  使用这些数据填充 :ref:`核心元数据 <core-metadata>` 的方式如下：

  1. 如果只提供了 ``name``，该值将填写到 :ref:`作者 <core-metadata-author>` 或 :ref:`维护者 <core-metadata-maintainer>` 中，视情况而定。
  2. 如果只提供了 ``email``，该值将填写到 :ref:`作者邮箱 <core-metadata-author-email>` 或 :ref:`维护者邮箱 <core-metadata-maintainer-email>` 中，视情况而定。
  3. 如果同时提供了 ``name`` 和 ``email``，该值将填写到 :ref:`作者邮箱 <core-metadata-author-email>` 或 :ref:`维护者邮箱 <core-metadata-maintainer-email>` 中，格式为 ``{name} <{email}>``。
  4. 多个值应该用逗号分隔。

.. tab:: 英文

  - TOML_ type: Array of inline tables with string keys and values
  - Corresponding :ref:`core metadata <core-metadata>` field:
    :ref:`Author <core-metadata-author>`,
    :ref:`Author-email <core-metadata-author-email>`,
    :ref:`Maintainer <core-metadata-maintainer>`, and
    :ref:`Maintainer-email <core-metadata-maintainer-email>`

  The people or organizations considered to be the "authors" of the
  project. The exact meaning is open to interpretation — it may list the
  original or primary authors, current maintainers, or owners of the
  package.

  The "maintainers" key is similar to "authors" in that its exact
  meaning is open to interpretation.

  These keys accept an array of tables with 2 keys: ``name`` and
  ``email``. Both values must be strings. The ``name`` value MUST be a
  valid email name (i.e. whatever can be put as a name, before an email,
  in :rfc:`822`) and not contain commas. The ``email`` value MUST be a
  valid email address. Both keys are optional, but at least one of the
  keys must be specified in the table.

  Using the data to fill in :ref:`core metadata <core-metadata>` is as
  follows:

  1. If only ``name`` is provided, the value goes in
    :ref:`Author <core-metadata-author>` or
    :ref:`Maintainer <core-metadata-maintainer>` as appropriate.
  2. If only ``email`` is provided, the value goes in
    :ref:`Author-email <core-metadata-author-email>` or
    :ref:`Maintainer-email <core-metadata-maintainer-email>`
    as appropriate.
  3. If both ``email`` and ``name`` are provided, the value goes in
    :ref:`Author-email <core-metadata-author-email>` or
    :ref:`Maintainer-email <core-metadata-maintainer-email>`
    as appropriate, with the format ``{name} <{email}>``.
  4. Multiple values should be separated by commas.


``keywords``
------------

.. tab:: 中文

  - TOML_ 类型: array of strings
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段:
    :ref:`Keywords <core-metadata-keywords>`

  项目的关键词。

.. tab:: 英文

  - TOML_ type: array of strings
  - Corresponding :ref:`core metadata <core-metadata>` field:
    :ref:`Keywords <core-metadata-keywords>`

  The keywords for the project.


``classifiers``
---------------

.. tab:: 中文

  - TOML_ 类型: array of strings
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段:
    :ref:`Classifier <core-metadata-classifier>`

  适用于该项目的 Trove 分类器。

.. tab:: 英文

  - TOML_ type: array of strings
  - Corresponding :ref:`core metadata <core-metadata>` field:
    :ref:`Classifier <core-metadata-classifier>`

  Trove classifiers which apply to the project.


``urls``
--------

.. tab:: 中文

  - TOML_ 类型: 包含字符串键和值的表
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段:
    :ref:`Project-URL <core-metadata-project-url>`

  一个包含 URL 的表格，其中键是 URL 标签，值是 URL 本身。有关规范化规则和处理元数据时展示的常见规则，请参阅 :ref:`well-known-project-urls`。

.. tab:: 英文

  - TOML_ type: table with keys and values of strings
  - Corresponding :ref:`core metadata <core-metadata>` field:
    :ref:`Project-URL <core-metadata-project-url>`

  A table of URLs where the key is the URL label and the value is the
  URL itself. See :ref:`well-known-project-urls` for normalization rules
  and well-known rules when processing metadata for presentation.


入口点
------------

**Entry points**

.. tab:: 中文

  - TOML_ 类型: 表格（ ``[project.scripts]``、 ``[project.gui-scripts]`` 和 ``[project.entry-points]``）
  - :ref:`入口点规范 <entry-points>`

  有三个与入口点相关的表格。``[project.scripts]`` 表格对应于 :ref:`入口点规范 <entry-points>` 中的 ``console_scripts`` 组。表格的键是入口点的名称，值是对象引用。

  ``[project.gui-scripts]`` 表格对应于 :ref:`入口点规范 <entry-points>` 中的 ``gui_scripts`` 组。其格式与 ``[project.scripts]`` 相同。

  ``[project.entry-points]`` 表格是一个表格集合。每个子表的名称是一个入口点组。键值语义与 ``[project.scripts]`` 相同。用户不得创建嵌套子表格，而应保持入口点组仅为一级。

  如果元数据定义了 ``[project.entry-points.console_scripts]`` 或 ``[project.entry-points.gui_scripts]`` 表格，构建后端必须引发错误，因为在面对 ``[project.scripts]`` 和 ``[project.gui-scripts]`` 时，这些表格会产生歧义。

.. tab:: 英文

  - TOML_ type: table (``[project.scripts]``, ``[project.gui-scripts]``,
    and ``[project.entry-points]``)
  - :ref:`Entry points specification <entry-points>`

  There are three tables related to entry points. The
  ``[project.scripts]`` table corresponds to the ``console_scripts``
  group in the :ref:`entry points specification <entry-points>`. The key
  of the table is the name of the entry point and the value is the
  object reference.

  The ``[project.gui-scripts]`` table corresponds to the ``gui_scripts``
  group in the :ref:`entry points specification <entry-points>`. Its
  format is the same as ``[project.scripts]``.

  The ``[project.entry-points]`` table is a collection of tables. Each
  sub-table's name is an entry point group. The key and value semantics
  are the same as ``[project.scripts]``. Users MUST NOT create
  nested sub-tables but instead keep the entry point groups to only one
  level deep.

  Build back-ends MUST raise an error if the metadata defines a
  ``[project.entry-points.console_scripts]`` or
  ``[project.entry-points.gui_scripts]`` table, as they would
  be ambiguous in the face of ``[project.scripts]`` and
  ``[project.gui-scripts]``, respectively.


``dependencies``/``optional-dependencies``
------------------------------------------

.. tab:: 中文

  - TOML_ 类型: :pep:`508` 字符串的数组（``dependencies``），以及具有 :pep:`508` 字符串数组值的表格（``optional-dependencies``）
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段:
    :ref:`Requires-Dist <core-metadata-requires-dist>` 和
    :ref:`Provides-Extra <core-metadata-provides-extra>`

  项目的（可选）依赖项。

  对于 ``dependencies``，它是一个键，其值是一个字符串数组。每个字符串表示项目的一个依赖项，必须按照有效的 :pep:`508` 字符串格式进行格式化。每个字符串直接映射到一个 :ref:`Requires-Dist <core-metadata-requires-dist>` 条目。

  对于 ``optional-dependencies``，它是一个表格，其中每个键指定一个额外的依赖项（extra），其值是一个字符串数组。数组中的字符串必须是有效的 :pep:`508` 字符串。每个键必须是 :ref:`Provides-Extra <core-metadata-provides-extra>` 的有效值。数组中的每个值因此会成为与相应的 :ref:`Provides-Extra <core-metadata-provides-extra>` 元数据匹配的 :ref:`Requires-Dist <core-metadata-requires-dist>` 条目。

.. tab:: 英文

  - TOML_ type: Array of :pep:`508` strings (``dependencies``), and a
    table with values of arrays of :pep:`508` strings
    (``optional-dependencies``)
  - Corresponding :ref:`core metadata <core-metadata>` field:
    :ref:`Requires-Dist <core-metadata-requires-dist>` and
    :ref:`Provides-Extra <core-metadata-provides-extra>`

  The (optional) dependencies of the project.

  For ``dependencies``, it is a key whose value is an array of strings.
  Each string represents a dependency of the project and MUST be
  formatted as a valid :pep:`508` string. Each string maps directly to
  a :ref:`Requires-Dist <core-metadata-requires-dist>` entry.

  For ``optional-dependencies``, it is a table where each key specifies
  an extra and whose value is an array of strings. The strings of the
  arrays must be valid :pep:`508` strings. The keys MUST be valid values
  for :ref:`Provides-Extra <core-metadata-provides-extra>`. Each value
  in the array thus becomes a corresponding
  :ref:`Requires-Dist <core-metadata-requires-dist>` entry for the
  matching :ref:`Provides-Extra <core-metadata-provides-extra>`
  metadata.

.. _declaring-project-metadata-dynamic:

``dynamic``
-----------

.. tab:: 中文

  - TOML_ 类型: 字符串数组
  - 对应的 :ref:`核心元数据 <core-metadata>` 字段:
    :ref:`Dynamic <core-metadata-dynamic>`

  指定此 PEP 列出的哪些键是故意未指定的，以便其他工具可以/将动态提供这些元数据。这清楚地区分了哪些元数据是故意未指定并且预计保持未指定，而不是由工具后续提供。

  - 构建后端必须遵守静态指定的元数据（这意味着元数据没有在 ``dynamic`` 中列出该键）。
  - 如果元数据在 ``dynamic`` 中指定了 ``name``，则构建后端必须抛出错误。
  - 如果 :ref:`核心元数据 <core-metadata>` 规范将某个字段列为 "Required"（必需），则元数据必须静态指定该键或将其列在 ``dynamic`` 中（否则构建后端必须抛出错误，即不应允许必需的键不以某种方式列出在 ``[project]`` 表中）。
  - 如果 :ref:`核心元数据 <core-metadata>` 规范将某个字段列为 "Optional"（可选），则元数据可以将其列入 ``dynamic``，如果期望构建后端稍后提供该键的数据。
  - 如果元数据静态指定了某个键，同时又将其列在 ``dynamic`` 中，则构建后端必须抛出错误。
  - 如果元数据未在 ``dynamic`` 中列出某个键，则构建后端不能代表用户填充所需的元数据（即 ``dynamic`` 是唯一允许工具填充元数据的方式，用户必须同意填充）。
  - 如果元数据在 ``dynamic`` 中列出了某个键，但构建后端无法确定其数据，则构建后端必须抛出错误（如果确定该数据为准确值，则可以省略数据）。

.. tab:: 英文

  - TOML_ type: array of string
  - Corresponding :ref:`core metadata <core-metadata>` field:
    :ref:`Dynamic <core-metadata-dynamic>`

  Specifies which keys listed by this PEP were intentionally
  unspecified so another tool can/will provide such metadata
  dynamically. This clearly delineates which metadata is purposefully
  unspecified and expected to stay unspecified compared to being
  provided via tooling later on.

  - A build back-end MUST honour statically-specified metadata (which
    means the metadata did not list the key in ``dynamic``).
  - A build back-end MUST raise an error if the metadata specifies
    ``name`` in ``dynamic``.
  - If the :ref:`core metadata <core-metadata>` specification lists a
    field as "Required", then the metadata MUST specify the key
    statically or list it in ``dynamic`` (build back-ends MUST raise an
    error otherwise, i.e. it should not be possible for a required key
    to not be listed somehow in the ``[project]`` table).
  - If the :ref:`core metadata <core-metadata>` specification lists a
    field as "Optional", the metadata MAY list it in ``dynamic`` if the
    expectation is a build back-end will provide the data for the key
    later.
  - Build back-ends MUST raise an error if the metadata specifies a
    key statically as well as being listed in ``dynamic``.
  - If the metadata does not list a key in ``dynamic``, then a build
    back-end CANNOT fill in the requisite metadata on behalf of the user
    (i.e. ``dynamic`` is the only way to allow a tool to fill in
    metadata and the user must opt into the filling in).
  - Build back-ends MUST raise an error if the metadata specifies a
    key in ``dynamic`` but the build back-end was unable to determine
    the data for it (omitting the data, if determined to be the accurate
    value, is acceptable).



.. _pyproject-tool-table:

任意工具配置： ``[tool]`` 表
==================================================

**Arbitrary tool configuration: the** ``[tool]`` **table**

.. tab:: 中文

  ``[tool]`` 表是任何与 Python 项目相关的工具的配置存储位置，不仅限于构建工具，只要工具使用 ``[tool]`` 内的子表格，用户就可以指定配置数据。例如，`flit <https://pypi.python.org/pypi/flit>`_ 工具会将其配置存储在 ``[tool.flit]`` 中。

  需要一种机制来分配 ``tool.*`` 命名空间中的名称，以确保不同的项目不会尝试使用相同的子表并发生冲突。我们的规则是，项目只有在其在 Cheeseshop/PyPI 上拥有 ``$NAME`` 的条目时，才能使用子表 ``tool.$NAME``。

.. tab:: 英文

  The ``[tool]`` table is where any tool related to your Python
  project, not just build tools, can have users specify configuration
  data as long as they use a sub-table within ``[tool]``, e.g. the
  `flit <https://pypi.python.org/pypi/flit>`_ tool would store its
  configuration in ``[tool.flit]``.

  A mechanism is needed to allocate names within the ``tool.*``
  namespace, to make sure that different projects do not attempt to use
  the same sub-table and collide. Our rule is that a project can use
  the subtable ``tool.$NAME`` if, and only if, they own the entry for
  ``$NAME`` in the Cheeseshop/PyPI.



历史记录
=======

**History**

.. tab:: 中文

  - 2016年5月: 最初的 ``pyproject.toml`` 文件规范，包含一个 ``[build-system]`` 表格，其中有一个 ``requires`` 键和一个 ``[tool]`` 表格，通过了 :pep:`518` 的批准。

  - 2020年11月: ``[project]`` 表格的规范通过 :pep:`621` 得到了批准。

.. tab:: 英文

  - May 2016: The initial specification of the ``pyproject.toml`` file, with just
    a ``[build-system]`` containing a ``requires`` key and a ``[tool]`` table, was
    approved through :pep:`518`.

  - November 2020: The specification of the ``[project]`` table was approved
    through :pep:`621`.



.. _TOML: https://toml.io
