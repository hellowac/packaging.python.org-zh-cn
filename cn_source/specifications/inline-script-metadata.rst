.. _inline-script-metadata:

======================
内联脚本元数据
======================

**Inline script metadata**

.. tab:: 中文

    该规范定义了一种元数据格式，可以嵌入到单文件 Python 脚本中，以帮助启动器、集成开发环境（IDE）和其他可能需要与此类脚本交互的外部工具。

.. tab:: 英文

    This specification defines a metadata format that can be embedded in single-file
    Python scripts to assist launchers, IDEs and other external tools which may need
    to interact with such scripts.


规范
=============

**Specification**

.. tab:: 中文

    此规范定义了一种元数据注释块格式（在某种程度上受 `reStructuredText Directives`__ 的启发）。

    __ https://docutils.sourceforge.io/docs/ref/rst/directives.html

    任何 Python 脚本都可以包含顶级注释块，其 **必须** 以 ``# /// TYPE`` 行开头，其中 ``TYPE`` 确定如何处理内容。具体格式为：一个 ``#``，后跟一个空格，再后跟三个正斜杠，再后跟一个空格，最后是元数据类型。块 **必须** 以 ``# ///`` 行结束，即：一个 ``#``，后跟一个空格，再后跟三个正斜杠。``TYPE`` 只能由 ASCII 字母、数字和连字符组成。

    在这两行（``# /// TYPE`` 和 ``# ///``）之间的每一行 **必须** 是以 ``#`` 开头的注释。如果在 ``#`` 之后还有其他字符，则第一个字符 **必须** 是空格。嵌入内容是通过删除每行的前两个字符形成的，如果第二个字符是空格，则删除这两个字符，否则仅删除第一个字符（即该行仅由一个 ``#`` 组成）。

    当下一行不是有效的嵌入内容行时，将该行作为结束行 ``# ///``。例如，以下是一个完全有效的单个块：

    .. code:: python

        # /// some-toml
        # embedded-csharp = """
        # /// <summary>
        # /// text
        # ///
        # /// </summary>
        # public class MyClass { }
        # """
        # ///

    在起始行和结束行之间 **不得** 插入另一个起始行。在这种情况下，工具可以产生错误。未闭合的块 **必须** 被忽略。

    当定义了多个相同 ``TYPE`` 的注释块时，工具 **必须** 产生错误。

    读取嵌入元数据的工具可以遵循标准的 Python 编码声明。如果工具选择不这样做，它们 **必须** 将文件按 UTF-8 处理。

    以下是可以用于解析元数据的规范正则表达式：

    .. code:: text

        (?m)^# /// (?P<type>[a-zA-Z0-9-]+)$\s(?P<content>(^#(| .*)$\s)+)^# ///$

    在文本规范与正则表达式之间存在差异的情况下，以文本规范为准。

    工具 **不得** 从未由本规范标准化的类型的元数据块中读取数据。

.. tab:: 英文

    This specification defines a metadata comment block format (loosely inspired by
    `reStructuredText Directives`__).

    __ https://docutils.sourceforge.io/docs/ref/rst/directives.html

    Any Python script may have top-level comment blocks that MUST start with the
    line ``# /// TYPE`` where ``TYPE`` determines how to process the content. That
    is: a single ``#``, followed by a single space, followed by three forward
    slashes, followed by a single space, followed by the type of metadata. Block
    MUST end with the line ``# ///``. That is: a single ``#``, followed by a single
    space, followed by three forward slashes. The ``TYPE`` MUST only consist of
    ASCII letters, numbers and hyphens.

    Every line between these two lines (``# /// TYPE`` and ``# ///``) MUST be a
    comment starting with ``#``. If there are characters after the ``#`` then the
    first character MUST be a space. The embedded content is formed by taking away
    the first two characters of each line if the second character is a space,
    otherwise just the first character (which means the line consists of only a
    single ``#``).

    Precedence for an ending line ``# ///`` is given when the next line is not
    a valid embedded content line as described above. For example, the following
    is a single fully valid block:

    .. code:: python

        # /// some-toml
        # embedded-csharp = """
        # /// <summary>
        # /// text
        # ///
        # /// </summary>
        # public class MyClass { }
        # """
        # ///

    A starting line MUST NOT be placed between another starting line and its ending
    line. In such cases tools MAY produce an error. Unclosed blocks MUST be ignored.

    When there are multiple comment blocks of the same ``TYPE`` defined, tools MUST
    produce an error.

    Tools reading embedded metadata MAY respect the standard Python encoding
    declaration. If they choose not to do so, they MUST process the file as UTF-8.

    This is the canonical regular expression that MAY be used to parse the
    metadata:

    .. code:: text

        (?m)^# /// (?P<type>[a-zA-Z0-9-]+)$\s(?P<content>(^#(| .*)$\s)+)^# ///$

    In circumstances where there is a discrepancy between the text specification
    and the regular expression, the text specification takes precedence.

    Tools MUST NOT read from metadata blocks with types that have not been
    standardized by this specification.

脚本类型
-----------

**script type**

.. tab:: 中文

    第一个类型的元数据块名为 ``script``，其中包含脚本元数据（依赖数据和工具配置）。

    本文档 **可以** 包含顶级字段 ``dependencies`` 和 ``requires-python``，并 **可以** 选择性地包含一个 ``[tool]`` 表。

    ``[tool]`` 表可由任何工具、脚本运行器或其他配置用于指定行为。它具有与 :ref:`pyproject.toml 中的 [tool] 表 <pyproject-tool-table>` 相同的语义。

    顶级字段如下：

    * ``dependencies``：字符串列表，用于指定脚本的运行时依赖项。每个条目 **必须** 是一个有效的 :ref:`依赖项指定符 <dependency-specifiers>`。
    * ``requires-python``：用于指定脚本兼容的 Python 版本的字符串。该字段的值 **必须** 是一个有效的 :ref:`版本指定符 <version-specifiers>`。

    如果无法提供指定的 ``dependencies``，脚本运行器 **必须** 产生错误；如果无法提供满足指定 ``requires-python`` 的任何 Python 版本，脚本运行器 **应当** 产生错误。

.. tab:: 英文

    The first type of metadata block is named ``script``, which contains
    script metadata (dependency data and tool configuration).

    This document MAY include the top-level fields ``dependencies`` and ``requires-python``,
    and MAY optionally include a ``[tool]`` table.

    The ``[tool]`` MAY be used by any tool, script runner or otherwise, to configure
    behavior. It has the same semantics as the :ref:`[tool] table in pyproject.toml
    <pyproject-tool-table>`.

    The top-level fields are:

    * ``dependencies``: A list of strings that specifies the runtime dependencies of the script. Each entry MUST be a valid :ref:`dependency specifier <dependency-specifiers>`.
    * ``requires-python``: A string that specifies the Python version(s) with which the script is compatible. The value of this field MUST be a valid :ref:`version specifier <version-specifiers>`.

    Script runners MUST error if the specified ``dependencies`` cannot be provided.
    Script runners SHOULD error if no version of Python that satisfies the specified
    ``requires-python`` can be provided.

示例
-------

**Example**

.. tab:: 中文

    以下是嵌入元数据的脚本示例：

    .. code:: python

        # /// script
        # requires-python = ">=3.11"
        # dependencies = [
        #   "requests<3",
        #   "rich",
        # ]
        # ///

        import requests
        from rich.pretty import pprint

        resp = requests.get("https://peps.python.org/api/peps.json")
        data = resp.json()
        pprint([(k, v["title"]) for k, v in data.items()][:10])

.. tab:: 英文

    The following is an example of a script with embedded metadata:

    .. code:: python

        # /// script
        # requires-python = ">=3.11"
        # dependencies = [
        #   "requests<3",
        #   "rich",
        # ]
        # ///

        import requests
        from rich.pretty import pprint

        resp = requests.get("https://peps.python.org/api/peps.json")
        data = resp.json()
        pprint([(k, v["title"]) for k, v in data.items()][:10])


参考实现
========================

**Reference Implementation**

.. tab:: 中文

    以下是一个在 Python 3.11 或更高版本中读取元数据的示例。

    .. code:: python

        import re
        import tomllib

        REGEX = r'(?m)^# /// (?P<type>[a-zA-Z0-9-]+)$\s(?P<content>(^#(| .*)$\s)+)^# ///$'

        def read(script: str) -> dict | None:
            name = 'script'
            matches = list(
                filter(lambda m: m.group('type') == name, re.finditer(REGEX, script))
            )
            if len(matches) > 1:
                raise ValueError(f'Multiple {name} blocks found')
            elif len(matches) == 1:
                content = ''.join(
                    line[2:] if line.startswith('# ') else line[1:]
                    for line in matches[0].group('content').splitlines(keepends=True)
                )
                return tomllib.loads(content)
            else:
                return None

    通常，工具会编辑依赖项，例如包管理器或 CI 中的依赖项更新自动化。以下是使用 ``tomlkit`` 库__ 修改内容的一个简单示例。

    __ https://tomlkit.readthedocs.io/en/latest/

    .. code:: python

        import re
        import tomlkit

        REGEX = r'(?m)^# /// (?P<type>[a-zA-Z0-9-]+)$\s(?P<content>(^#(| .*)$\s)+)^# ///$'

        def add(script: str, dependency: str) -> str:
            match = re.search(REGEX, script)
            content = ''.join(
                line[2:] if line.startswith('# ') else line[1:]
                for line in match.group('content').splitlines(keepends=True)
            )

            config = tomlkit.parse(content)
            config['dependencies'].append(dependency)
            new_content = ''.join(
                f'# {line}' if line.strip() else f'#{line}'
                for line in tomlkit.dumps(config).splitlines(keepends=True)
            )

            start, end = match.span('content')
            return script[:start] + new_content + script[end:]

    请注意，此示例使用了一个可以保留 TOML 格式的库。这并不是编辑的必备条件，而是一种“锦上添花”的功能。

    以下是如何读取任意元数据块流的示例。

    .. code:: python

        import re
        from typing import Iterator

        REGEX = r'(?m)^# /// (?P<type>[a-zA-Z0-9-]+)$\s(?P<content>(^#(| .*)$\s)+)^# ///$'

        def stream(script: str) -> Iterator[tuple[str, str]]:
            for match in re.finditer(REGEX, script):
                yield match.group('type'), ''.join(
                    line[2:] if line.startswith('# ') else line[1:]
                    for line in match.group('content').splitlines(keepends=True)
                )

.. tab:: 英文

    The following is an example of how to read the metadata on Python 3.11 or
    higher.

    .. code:: python

        import re
        import tomllib

        REGEX = r'(?m)^# /// (?P<type>[a-zA-Z0-9-]+)$\s(?P<content>(^#(| .*)$\s)+)^# ///$'

        def read(script: str) -> dict | None:
            name = 'script'
            matches = list(
                filter(lambda m: m.group('type') == name, re.finditer(REGEX, script))
            )
            if len(matches) > 1:
                raise ValueError(f'Multiple {name} blocks found')
            elif len(matches) == 1:
                content = ''.join(
                    line[2:] if line.startswith('# ') else line[1:]
                    for line in matches[0].group('content').splitlines(keepends=True)
                )
                return tomllib.loads(content)
            else:
                return None

    Often tools will edit dependencies like package managers or dependency update
    automation in CI. The following is a crude example of modifying the content
    using the ``tomlkit`` library__.

    __ https://tomlkit.readthedocs.io/en/latest/

    .. code:: python

        import re

        import tomlkit

        REGEX = r'(?m)^# /// (?P<type>[a-zA-Z0-9-]+)$\s(?P<content>(^#(| .*)$\s)+)^# ///$'

        def add(script: str, dependency: str) -> str:
            match = re.search(REGEX, script)
            content = ''.join(
                line[2:] if line.startswith('# ') else line[1:]
                for line in match.group('content').splitlines(keepends=True)
            )

            config = tomlkit.parse(content)
            config['dependencies'].append(dependency)
            new_content = ''.join(
                f'# {line}' if line.strip() else f'#{line}'
                for line in tomlkit.dumps(config).splitlines(keepends=True)
            )

            start, end = match.span('content')
            return script[:start] + new_content + script[end:]

    Note that this example used a library that preserves TOML formatting. This is
    not a requirement for editing by any means but rather is a "nice to have"
    feature.

    The following is an example of how to read a stream of arbitrary metadata
    blocks.

    .. code:: python

        import re
        from typing import Iterator

        REGEX = r'(?m)^# /// (?P<type>[a-zA-Z0-9-]+)$\s(?P<content>(^#(| .*)$\s)+)^# ///$'

        def stream(script: str) -> Iterator[tuple[str, str]]:
            for match in re.finditer(REGEX, script):
                yield match.group('type'), ''.join(
                    line[2:] if line.startswith('# ') else line[1:]
                    for line in match.group('content').splitlines(keepends=True)
            )


建议
===============

**Recommendations**

.. tab:: 中文

    支持管理不同 Python 版本的工具应尝试使用与脚本的 ``requires-python`` 元数据兼容的最高可用 Python 版本（如果已定义）。

.. tab:: 英文

    Tools that support managing different versions of Python should attempt to use
    the highest available version of Python that is compatible with the script's
    ``requires-python`` metadata, if defined.


历史
=======

**History**

.. tab:: 中文

    - 2023年10月：该规范已通过 :pep:`723` 有条件批准。
    - 2024年1月：通过对 :pep:`723` 的修订， ``pyproject`` 元数据块类型被重命名为 ``script``，并移除了 ``[run]`` 表，使 ``dependencies`` 和 ``requires-python`` 成为顶级键。此外，该规范不再是临时性的。

.. tab:: 英文

    - October 2023: This specification was conditionally approved through :pep:`723`.
    - January 2024: Through amendments to :pep:`723`, the ``pyproject`` metadata block type was renamed to ``script``, and the ``[run]`` table was dropped, making the ``dependencies`` and ``requires-python`` keys top-level. Additionally, the specification is no longer provisional.
