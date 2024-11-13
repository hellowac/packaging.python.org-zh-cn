.. highlight:: text

.. _dependency-specifiers:

=====================
依赖说明符
=====================

**Dependency specifiers**

.. tab:: 中文

  本文档描述了依赖规范的格式，最初在 :pep:`508` 中进行了规定。

  依赖的作用是使像 pip [#pip1]_ 这样的工具能够找到正确的包进行安装。有时这非常宽松——仅指定一个名称，而有时则非常具体——指向一个特定的文件进行安装。有时依赖仅在某个平台上相关，或者只有某些版本是可接受的，因此该语言允许描述所有这些情况。

  该语言定义了一种简洁的基于行的格式，已经在 pip 的需求文件中得到了广泛应用，尽管我们并没有规定这些文件允许的命令行选项处理方式。有一个例外——在 :ref:`Versioning specifier specification <version-specifiers>` 中规定的 URL 引用形式，实际上并未在 pip 中实现，但我们使用该格式，而不是 pip 当前的本地格式。

.. tab:: 英文

  This document describes the dependency specifiers format as originally specified
  in :pep:`508`.

  The job of a dependency is to enable tools like pip [#pip]_ to find the right
  package to install. Sometimes this is very loose - just specifying a name, and
  sometimes very specific - referring to a specific file to install. Sometimes
  dependencies are only relevant in one platform, or only some versions are
  acceptable, so the language permits describing all these cases.

  The language defined is a compact line based format which is already in
  widespread use in pip requirements files, though we do not specify the command
  line option handling that those files permit. There is one caveat - the
  URL reference form, specified in :ref:`Versioning specifier specification <version-specifiers>`
  is not actually implemented in pip, but we use that format rather
  than pip's current native format.

规范
=============

**Specification**

示例
--------

**Examples**

.. tab:: 中文

  语言中所有带名称查找的特性如下所示::

    requests [security,tests] >= 2.8.1, == 2.8.* ; python_version < "2.7"

  最小的基于 URL 的查找::

    pip @ https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686

.. tab:: 英文

  All features of the language shown with a name based lookup::

      requests [security,tests] >= 2.8.1, == 2.8.* ; python_version < "2.7"

  A minimal URL based lookup::

      pip @ https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686

概念
--------

**Concepts**

.. tab:: 中文

  依赖规范始终指定一个分发名称。它可以包含额外的功能（extras），通过扩展指定分发的依赖关系来启用可选功能。可以通过版本限制来控制安装的版本，或者指定要安装的特定工件的 URL。最后，依赖关系可以通过环境标记来设定条件。

.. tab:: 英文

  A dependency specification always specifies a distribution name. It may
  include extras, which expand the dependencies of the named distribution to
  enable optional features. The version installed can be controlled using
  version limits, or giving the URL to a specific artifact to install. Finally
  the dependency can be made conditional using environment markers.

语法
-------

**Grammar**

.. tab:: 中文

  我们首先简要介绍语法，然后稍后详细讨论每个部分的语义。

  分发规范以 ASCII 文本形式编写。我们使用 parsley [#parsley1]_ 语法来提供精确的语法。预计该规范将嵌入到一个更大的系统中，该系统提供诸如注释、多行支持（通过续行）或其他类似功能的框架。

  包含用于构建有用解析树的注释的完整语法位于本文档的末尾。

  版本可以按照 :ref:`版本规范 <version-specifiers>` 的规则指定。（注意：URI 在 :rfc:`std-66 <3986>` 中定义）::

      version_cmp   = wsp* '<' | '<=' | '!=' | '==' | '>=' | '>' | '~=' | '==='
      version       = wsp* ( letterOrDigit | '-' | '_' | '.' | '*' | '+' | '!' )+
      version_one   = version_cmp version wsp*
      version_many  = version_one (',' version_one)* (',' wsp*)?
      versionspec   = ( '(' version_many ')' ) | version_many
      urlspec       = '@' wsp* <URI_reference>

  环境标记允许仅在某些环境中生效的规范::

      marker_op     = version_cmp | (wsp+ 'in' wsp+) | (wsp+ 'not' wsp+ 'in' wsp+)
      python_str_c  = (wsp | letter | digit | '(' | ')' | '.' | '{' | '}' |
                      '-' | '_' | '*' | '#' | ':' | ';' | ',' | '/' | '?' |
                      '[' | ']' | '!' | '~' | '`' | '@' | '$' | '%' | '^' |
                      '&' | '=' | '+' | '|' | '<' | '>' )
      dquote        = '"'
      squote        = '\\''
      python_str    = (squote (python_str_c | dquote)* squote |
                      dquote (python_str_c | squote)* dquote)
      env_var       = ('python_version' | 'python_full_version' |
                      'os_name' | 'sys_platform' | 'platform_release' |
                      'platform_system' | 'platform_version' |
                      'platform_machine' | 'platform_python_implementation' |
                      'implementation_name' | 'implementation_version' |
                      'extra' # ONLY when defined by a containing layer
                      )
      marker_var    = wsp* (env_var | python_str)
      marker_expr   = marker_var marker_op marker_var
                    | wsp* '(' marker wsp* ')'
      marker_and    = marker_expr wsp* 'and' marker_expr
                    | marker_expr
      marker_or     = marker_and wsp* 'or' marker_and
                        | marker_and
      marker        = marker_or
      quoted_marker = ';' wsp* marker

  分发的可选组件可以使用 extras 字段指定::

      identifier_end = letterOrDigit | (('-' | '_' | '.' )* letterOrDigit)
      identifier    = letterOrDigit identifier_end*
      name          = identifier
      extras_list   = identifier (wsp* ',' wsp* identifier)*
      extras        = '[' wsp* extras_list? wsp* ']'

  关于 extras 名称的限制在 :pep:`685` 中定义。

  给出名称为基础的要求规则::

      name_req      = name wsp* extras? wsp* versionspec? wsp* quoted_marker?

  以及直接引用规范规则::

      url_req       = name wsp* extras? wsp* urlspec (wsp+ quoted_marker?)?

  最终得出统一规则，指定一个依赖项。::

      specification = wsp* ( url_req | name_req ) wsp*

.. tab:: 英文

  We first cover the grammar briefly and then drill into the semantics of each
  section later.

  A distribution specification is written in ASCII text. We use a parsley
  [#parsley]_ grammar to provide a precise grammar. It is expected that the
  specification will be embedded into a larger system which offers framing such
  as comments, multiple line support via continuations, or other such features.

  The full grammar including annotations to build a useful parse tree is
  included at the end of this document.

  Versions may be specified according to the rules of the
  :ref:`Version specifier specification <version-specifiers>`. (Note:
  URI is defined in :rfc:`std-66 <3986>`)::

      version_cmp   = wsp* '<' | '<=' | '!=' | '==' | '>=' | '>' | '~=' | '==='
      version       = wsp* ( letterOrDigit | '-' | '_' | '.' | '*' | '+' | '!' )+
      version_one   = version_cmp version wsp*
      version_many  = version_one (',' version_one)* (',' wsp*)?
      versionspec   = ( '(' version_many ')' ) | version_many
      urlspec       = '@' wsp* <URI_reference>

  Environment markers allow making a specification only take effect in some
  environments::

      marker_op     = version_cmp | (wsp+ 'in' wsp+) | (wsp+ 'not' wsp+ 'in' wsp+)
      python_str_c  = (wsp | letter | digit | '(' | ')' | '.' | '{' | '}' |
                      '-' | '_' | '*' | '#' | ':' | ';' | ',' | '/' | '?' |
                      '[' | ']' | '!' | '~' | '`' | '@' | '$' | '%' | '^' |
                      '&' | '=' | '+' | '|' | '<' | '>' )
      dquote        = '"'
      squote        = '\\''
      python_str    = (squote (python_str_c | dquote)* squote |
                      dquote (python_str_c | squote)* dquote)
      env_var       = ('python_version' | 'python_full_version' |
                      'os_name' | 'sys_platform' | 'platform_release' |
                      'platform_system' | 'platform_version' |
                      'platform_machine' | 'platform_python_implementation' |
                      'implementation_name' | 'implementation_version' |
                      'extra' # ONLY when defined by a containing layer
                      )
      marker_var    = wsp* (env_var | python_str)
      marker_expr   = marker_var marker_op marker_var
                    | wsp* '(' marker wsp* ')'
      marker_and    = marker_expr wsp* 'and' marker_expr
                    | marker_expr
      marker_or     = marker_and wsp* 'or' marker_and
                        | marker_and
      marker        = marker_or
      quoted_marker = ';' wsp* marker

  Optional components of a distribution may be specified using the extras
  field::

      identifier_end = letterOrDigit | (('-' | '_' | '.' )* letterOrDigit)
      identifier    = letterOrDigit identifier_end*
      name          = identifier
      extras_list   = identifier (wsp* ',' wsp* identifier)*
      extras        = '[' wsp* extras_list? wsp* ']'

  Restrictions on names for extras is defined in :pep:`685`.

  Giving us a rule for name based requirements::

      name_req      = name wsp* extras? wsp* versionspec? wsp* quoted_marker?

  And a rule for direct reference specifications::

      url_req       = name wsp* extras? wsp* urlspec (wsp+ quoted_marker?)?

  Leading to the unified rule that can specify a dependency.::

      specification = wsp* ( url_req | name_req ) wsp*

空格
----------

**Whitespace**

.. tab:: 中文

  非换行空白字符大多是可选的，且没有语义意义。唯一的例外是用于检测 URL 需求的结束。

.. tab:: 英文

  Non line-breaking whitespace is mostly optional with no semantic meaning. The sole exception is detecting the end of a URL requirement.

名称
-----

**Names**

.. tab:: 中文

  Python 分发名称当前在 :pep:`345` 中定义。名称作为分发的主要标识符。它们出现在所有依赖规范中，并且足以作为独立的规范。然而，PyPI 对名称有严格的限制——它们必须匹配一个不区分大小写的正则表达式，否则将不被接受。因此，在本文档中，我们将标识符的可接受值限制为该正则表达式。名称的完整重新定义可能会在未来的元数据 PEP 中进行。该正则表达式（使用 `re.IGNORECASE` 运行）为::

    ^([A-Z0-9]|[A-Z0-9][A-Z0-9._-]*[A-Z0-9])$

.. tab:: 英文

  Python distribution names are currently defined in :pep:`345`. Names
  act as the primary identifier for distributions. They are present in all
  dependency specifications, and are sufficient to be a specification on their
  own. However, PyPI places strict restrictions on names - they must match a
  case insensitive regex or they won't be accepted. Accordingly, in this
  document we limit the acceptable values for identifiers to that regex. A full
  redefinition of name may take place in a future metadata PEP. The regex (run
  with re.IGNORECASE) is::

      ^([A-Z0-9]|[A-Z0-9][A-Z0-9._-]*[A-Z0-9])$

附加内容
------

**Extras**

.. tab:: 中文

  额外（extra）是分发的一个可选部分。分发可以指定任意数量的额外功能，并且每个额外功能都会在 **使用** 该额外功能的依赖规范中声明额外的依赖。例如::

    requests[security,tests]

  额外功能在它们定义的依赖项中与它们所附加的分发的依赖项进行联合。上面的例子将导致安装 `requests` 及其自身的依赖项，并且还会安装 `requests` 的 "security" 额外功能中列出的所有依赖项。

  如果列出了多个额外功能，所有的依赖项都会被联合在一起。

.. tab:: 英文

  An extra is an optional part of a distribution. Distributions can specify as
  many extras as they wish, and each extra results in the declaration of
  additional dependencies of the distribution **when** the extra is used in a
  dependency specification. For instance::

      requests[security,tests]

  Extras union in the dependencies they define with the dependencies of the
  distribution they are attached to. The example above would result in requests
  being installed, and requests own dependencies, and also any dependencies that
  are listed in the "security" extra of requests.

  If multiple extras are listed, all the dependencies are unioned together.

版本
--------

**Versions**

.. tab:: 中文

  有关版本号和版本比较的更多细节，请参见 :ref:`Version specifier specification <version-specifiers>`。版本规范限制了可使用的分发版本。它们仅适用于通过名称查找的分发，而不是通过 URL 查找的分发。版本比较也用于标记（markers）功能。版本周围的可选括号是为了与 :pep:`345` 兼容，但不应生成，只应接受。

.. tab:: 英文

  See the :ref:`Version specifier specification <version-specifiers>` for
  more detail on both version numbers and version comparisons. Version
  specifications limit the versions of a distribution that can be
  used. They only apply to distributions looked up by name, rather than
  via a URL. Version comparison are also used in the markers feature. The
  optional brackets around a version are present for compatibility with
  :pep:`345` but should not be generated, only accepted.

环境标记
-------------------

**Environment Markers**

.. tab:: 中文

  环境标记允许依赖规范提供一个规则，用于描述何时使用该依赖。例如，考虑一个需要 `argparse` 的包。在 Python 2.7 中，`argparse` 始终存在。而在较旧的 Python 版本中，它需要作为一个依赖安装。这可以这样表达::

      argparse;python_version<"2.7"

  一个标记表达式的结果为 `True` 或 `False`。当其结果为 `False` 时，应忽略该依赖规范。

  标记语言的灵感来源于 Python 本身，选择它是因为可以安全地评估它，而无需运行可能成为安全漏洞的任意代码。标记最初在 :pep:`345` 中标准化。本文件修正了在 :pep:`426` 中描述的设计中观察到的一些问题。

  标记表达式中的比较由比较运算符确定类型。<marker_op> 运算符中不在 <version_cmp> 中的运算符与 Python 中的字符串比较方式相同。<version_cmp> 运算符在定义了版本比较规则时，使用 :ref:`版本说明符规范 <version-specifiers>` 中的版本比较规则（即当两边都有有效的版本规范时）。如果该规范没有定义行为，并且该运算符在 Python 中存在，则该运算符回退到 Python 的行为。否则，应引发错误。例如，以下代码将会引发错误::

      "dog" ~= "fred"
      python_version ~= "surprise"

  用户提供的常量始终作为字符串编码，可以使用 ``'`` 或 ``"`` 引号标记。请注意，反斜杠转义并未定义，但现有实现支持它们。由于它们增加了复杂性，并且当前没有明显的需求，因此没有在本规范中包括它们。同样，我们也没有定义非 ASCII 字符支持：我们引用的所有运行时变量预计都是 ASCII 字符。

  标记语法中的变量，如 "os_name"，会解析为在 Python 运行时查找的值。除了 "extra" 外，所有值在今天的所有 Python 版本中都已定义——如果某个值未定义，则视为标记实现中的错误。

  未知变量必须引发错误，而不是导致评估为 `True` 或 `False` 的比较。

  在给定的 Python 实现上无法计算其值的变量，应评估为 ``0`` （对于版本）和空字符串（对于所有其他变量）。

  "extra" 变量是特殊的。它被 wheel 用来指示在 wheel 的 ``METADATA`` 文件中适用于给定额外项的规范，但由于 ``METADATA`` 文件基于 :pep:`426` 的草案版本，因此目前对此没有规范。无论如何，在没有这种特殊处理的上下文中，"extra" 变量应像所有其他未知变量一样引发错误。

  .. list-table::
    :header-rows: 1

    * - Marker
      - Python 等效
      - 示例值
    * - ``os_name``
      - :py:data:`os.name`
      - ``posix``，``java``
    * - ``sys_platform``
      - :py:data:`sys.platform`
      - ``linux``，``linux2``，``darwin``，``java1.8.0_51`` （注意，"linux" 来自 Python3，而 "linux2" 来自 Python2）
    * - ``platform_machine``
      - :py:func:`platform.machine()`
      - ``x86_64``
    * - ``platform_python_implementation``
      - :py:func:`platform.python_implementation()`
      - ``CPython``，``Jython``
    * - ``platform_release``
      - :py:func:`platform.release()`
      - ``3.14.1-x86_64-linode39``，``14.5.0``，``1.8.0_51``
    * - ``platform_system``
      - :py:func:`platform.system()`
      - ``Linux``，``Windows``，``Java``
    * - ``platform_version``
      - :py:func:`platform.version()`
      - ``#1 SMP Fri Apr 25 13:07:35 EDT 2014``
        ``Java HotSpot(TM) 64-Bit Server VM, 25.51-b03, Oracle Corporation``
        ``Darwin Kernel Version 14.5.0: Wed Jul 29 02:18:53 PDT 2015; root:xnu-2782.40.9~2/RELEASE_X86_64``
    * - ``python_version``
      - ``'.'.join(platform.python_version_tuple()[:2])``
      - ``3.4``，``2.7``
    * - ``python_full_version``
      - :py:func:`platform.python_version()`
      - ``3.4.0``，``3.5.0b1``
    * - ``implementation_name``
      - :py:data:`sys.implementation.name <sys.implementation>`
      - ``cpython``
    * - ``implementation_version``
      - 见下定义
      - ``3.4.0``，``3.5.0b1``
    * - ``extra``
      - 除非由解释规范的上下文定义，否则为错误。
      - ``test``

  ``implementation_version`` 标记变量来源于 :py:data:`sys.implementation.version <sys.implementation>`：

  .. code-block:: python

      def format_full_version(info):
          version = '{0.major}.{0.minor}.{0.micro}'.format(info)
          kind = info.releaselevel
          if kind != 'final':
              version += kind[0] + str(info.serial)
          return version

      if hasattr(sys, 'implementation'):
          implementation_version = format_full_version(sys.implementation.version)
      else:
          implementation_version = "0"

  本节的环境标记，最初通过 :pep:`508` 定义，取代了 :pep:`345` 中的环境标记部分。

.. tab:: 英文

  Environment markers allow a dependency specification to provide a rule that
  describes when the dependency should be used. For instance, consider a package
  that needs argparse. In Python 2.7 argparse is always present. On older Python
  versions it has to be installed as a dependency. This can be expressed as so::

      argparse;python_version<"2.7"

  A marker expression evaluates to either True or False. When it evaluates to
  False, the dependency specification should be ignored.

  The marker language is inspired by Python itself, chosen for the ability to
  safely evaluate it without running arbitrary code that could become a security
  vulnerability. Markers were first standardised in :pep:`345`. This document
  fixes some issues that were observed in the design described in :pep:`426`.

  Comparisons in marker expressions are typed by the comparison operator.  The
  <marker_op> operators that are not in <version_cmp> perform the same as they
  do for strings in Python. The <version_cmp> operators use the version comparison
  rules of the :ref:`Version specifier specification <version-specifiers>`
  when those are defined (that is when both sides have a valid
  version specifier). If there is no defined behaviour of this specification
  and the operator exists in Python, then the operator falls back to
  the Python behaviour. Otherwise an error should be raised. e.g. the following
  will result in  errors::

      "dog" ~= "fred"
      python_version ~= "surprise"

  User supplied constants are always encoded as strings with either ``'`` or
  ``"`` quote marks. Note that backslash escapes are not defined, but existing
  implementations do support them. They are not included in this
  specification because they add complexity and there is no observable need for
  them today. Similarly we do not define non-ASCII character support: all the
  runtime variables we are referencing are expected to be ASCII-only.

  The variables in the marker grammar such as "os_name" resolve to values looked
  up in the Python runtime. With the exception of "extra" all values are defined
  on all Python versions today - it is an error in the implementation of markers
  if a value is not defined.

  Unknown variables must raise an error rather than resulting in a comparison
  that evaluates to True or False.

  Variables whose value cannot be calculated on a given Python implementation
  should evaluate to ``0`` for versions, and an empty string for all other
  variables.

  The "extra" variable is special. It is used by wheels to signal which
  specifications apply to a given extra in the wheel ``METADATA`` file, but
  since the ``METADATA`` file is based on a draft version of :pep:`426`, there is
  no current specification for this. Regardless, outside of a context where this
  special handling is taking place, the "extra" variable should result in an
  error like all other unknown variables.

  .. list-table::
    :header-rows: 1

    * - Marker
      - Python equivalent
      - Sample values
    * - ``os_name``
      - :py:data:`os.name`
      - ``posix``, ``java``
    * - ``sys_platform``
      - :py:data:`sys.platform`
      - ``linux``, ``linux2``, ``darwin``, ``java1.8.0_51`` (note that "linux"
        is from Python3 and "linux2" from Python2)
    * - ``platform_machine``
      - :py:func:`platform.machine()`
      - ``x86_64``
    * - ``platform_python_implementation``
      - :py:func:`platform.python_implementation()`
      - ``CPython``, ``Jython``
    * - ``platform_release``
      - :py:func:`platform.release()`
      - ``3.14.1-x86_64-linode39``, ``14.5.0``, ``1.8.0_51``
    * - ``platform_system``
      - :py:func:`platform.system()`
      - ``Linux``, ``Windows``, ``Java``
    * - ``platform_version``
      - :py:func:`platform.version()`
      - ``#1 SMP Fri Apr 25 13:07:35 EDT 2014``
        ``Java HotSpot(TM) 64-Bit Server VM, 25.51-b03, Oracle Corporation``
        ``Darwin Kernel Version 14.5.0: Wed Jul 29 02:18:53 PDT 2015; root:xnu-2782.40.9~2/RELEASE_X86_64``
    * - ``python_version``
      - ``'.'.join(platform.python_version_tuple()[:2])``
      - ``3.4``, ``2.7``
    * - ``python_full_version``
      - :py:func:`platform.python_version()`
      - ``3.4.0``, ``3.5.0b1``
    * - ``implementation_name``
      - :py:data:`sys.implementation.name <sys.implementation>`
      - ``cpython``
    * - ``implementation_version``
      - see definition below
      - ``3.4.0``, ``3.5.0b1``
    * - ``extra``
      - An error except when defined by the context interpreting the
        specification.
      - ``test``

  The ``implementation_version`` marker variable is derived from
  :py:data:`sys.implementation.version <sys.implementation>`:

  .. code-block:: python

      def format_full_version(info):
          version = '{0.major}.{0.minor}.{0.micro}'.format(info)
          kind = info.releaselevel
          if kind != 'final':
              version += kind[0] + str(info.serial)
          return version

      if hasattr(sys, 'implementation'):
          implementation_version = format_full_version(sys.implementation.version)
      else:
          implementation_version = "0"

  This environment markers section, initially defined through :pep:`508`, supersedes the environment markers
  section in :pep:`345`.

完整语法
================

**Complete Grammar**

.. tab:: 中文

  完整的欧芹(parsley)语法::

      wsp           = ' ' | '\t'
      version_cmp   = wsp* <'<=' | '<' | '!=' | '==' | '>=' | '>' | '~=' | '==='>
      version       = wsp* <( letterOrDigit | '-' | '_' | '.' | '*' | '+' | '!' )+>
      version_one   = version_cmp:op version:v wsp* -> (op, v)
      version_many  = version_one:v1 (',' version_one)*:v2 (',' wsp*)? -> [v1] + v2
      versionspec   = ('(' version_many:v ')' ->v) | version_many
      urlspec       = '@' wsp* <URI_reference>
      marker_op     = version_cmp | (wsp* 'in') | (wsp* 'not' wsp+ 'in')
      python_str_c  = (wsp | letter | digit | '(' | ')' | '.' | '{' | '}' |
                      '-' | '_' | '*' | '#' | ':' | ';' | ',' | '/' | '?' |
                      '[' | ']' | '!' | '~' | '`' | '@' | '$' | '%' | '^' |
                      '&' | '=' | '+' | '|' | '<' | '>' )
      dquote        = '"'
      squote        = '\\''
      python_str    = (squote <(python_str_c | dquote)*>:s squote |
                      dquote <(python_str_c | squote)*>:s dquote) -> s
      env_var       = ('python_version' | 'python_full_version' |
                      'os_name' | 'sys_platform' | 'platform_release' |
                      'platform_system' | 'platform_version' |
                      'platform_machine' | 'platform_python_implementation' |
                      'implementation_name' | 'implementation_version' |
                      'extra' # ONLY when defined by a containing layer
                      ):varname -> lookup(varname)
      marker_var    = wsp* (env_var | python_str)
      marker_expr   = marker_var:l marker_op:o marker_var:r -> (o, l, r)
                    | wsp* '(' marker:m wsp* ')' -> m
      marker_and    = marker_expr:l wsp* 'and' marker_expr:r -> ('and', l, r)
                    | marker_expr:m -> m
      marker_or     = marker_and:l wsp* 'or' marker_and:r -> ('or', l, r)
                        | marker_and:m -> m
      marker        = marker_or
      quoted_marker = ';' wsp* marker
      identifier_end = letterOrDigit | (('-' | '_' | '.' )* letterOrDigit)
      identifier    = < letterOrDigit identifier_end* >
      name          = identifier
      extras_list   = identifier:i (wsp* ',' wsp* identifier)*:ids -> [i] + ids
      extras        = '[' wsp* extras_list?:e wsp* ']' -> e
      name_req      = (name:n wsp* extras?:e wsp* versionspec?:v wsp* quoted_marker?:m
                      -> (n, e or [], v or [], m))
      url_req       = (name:n wsp* extras?:e wsp* urlspec:v (wsp+ | end) quoted_marker?:m
                      -> (n, e or [], v, m))
      specification = wsp* ( url_req | name_req ):s wsp* -> s
      # The result is a tuple - name, list-of-extras,
      # list-of-version-constraints-or-a-url, marker-ast or None


      URI_reference = <URI | relative_ref>
      URI           = scheme ':' hier_part ('?' query )? ( '#' fragment)?
      hier_part     = ('//' authority path_abempty) | path_absolute | path_rootless | path_empty
      absolute_URI  = scheme ':' hier_part ( '?' query )?
      relative_ref  = relative_part ( '?' query )? ( '#' fragment )?
      relative_part = '//' authority path_abempty | path_absolute | path_noscheme | path_empty
      scheme        = letter ( letter | digit | '+' | '-' | '.')*
      authority     = ( userinfo '@' )? host ( ':' port )?
      userinfo      = ( unreserved | pct_encoded | sub_delims | ':')*
      host          = IP_literal | IPv4address | reg_name
      port          = digit*
      IP_literal    = '[' ( IPv6address | IPvFuture) ']'
      IPvFuture     = 'v' hexdig+ '.' ( unreserved | sub_delims | ':')+
      IPv6address   = (
                        ( h16 ':'){6} ls32
                        | '::' ( h16 ':'){5} ls32
                        | ( h16 )?  '::' ( h16 ':'){4} ls32
                        | ( ( h16 ':')? h16 )? '::' ( h16 ':'){3} ls32
                        | ( ( h16 ':'){0,2} h16 )? '::' ( h16 ':'){2} ls32
                        | ( ( h16 ':'){0,3} h16 )? '::' h16 ':' ls32
                        | ( ( h16 ':'){0,4} h16 )? '::' ls32
                        | ( ( h16 ':'){0,5} h16 )? '::' h16
                        | ( ( h16 ':'){0,6} h16 )? '::' )
      h16           = hexdig{1,4}
      ls32          = ( h16 ':' h16) | IPv4address
      IPv4address   = dec_octet '.' dec_octet '.' dec_octet '.' dec_octet
      nz            = ~'0' digit
      dec_octet     = (
                        digit # 0-9
                        | nz digit # 10-99
                        | '1' digit{2} # 100-199
                        | '2' ('0' | '1' | '2' | '3' | '4') digit # 200-249
                        | '25' ('0' | '1' | '2' | '3' | '4' | '5') )# %250-255
      reg_name = ( unreserved | pct_encoded | sub_delims)*
      path = (
              path_abempty # begins with '/' or is empty
              | path_absolute # begins with '/' but not '//'
              | path_noscheme # begins with a non-colon segment
              | path_rootless # begins with a segment
              | path_empty ) # zero characters
      path_abempty  = ( '/' segment)*
      path_absolute = '/' ( segment_nz ( '/' segment)* )?
      path_noscheme = segment_nz_nc ( '/' segment)*
      path_rootless = segment_nz ( '/' segment)*
      path_empty    = pchar{0}
      segment       = pchar*
      segment_nz    = pchar+
      segment_nz_nc = ( unreserved | pct_encoded | sub_delims | '@')+
                      # non-zero-length segment without any colon ':'
      pchar         = unreserved | pct_encoded | sub_delims | ':' | '@'
      query         = ( pchar | '/' | '?')*
      fragment      = ( pchar | '/' | '?')*
      pct_encoded   = '%' hexdig
      unreserved    = letter | digit | '-' | '.' | '_' | '~'
      reserved      = gen_delims | sub_delims
      gen_delims    = ':' | '/' | '?' | '#' | '(' | ')?' | '@'
      sub_delims    = '!' | '$' | '&' | '\\'' | '(' | ')' | '*' | '+' | ',' | ';' | '='
      hexdig        = digit | 'a' | 'A' | 'b' | 'B' | 'c' | 'C' | 'd' | 'D' | 'e' | 'E' | 'f' | 'F'

  测试程序——如果语法在字符串 ``grammar`` 中: 

.. tab:: 英文

  The complete parsley grammar::

      wsp           = ' ' | '\t'
      version_cmp   = wsp* <'<=' | '<' | '!=' | '==' | '>=' | '>' | '~=' | '==='>
      version       = wsp* <( letterOrDigit | '-' | '_' | '.' | '*' | '+' | '!' )+>
      version_one   = version_cmp:op version:v wsp* -> (op, v)
      version_many  = version_one:v1 (',' version_one)*:v2 (',' wsp*)? -> [v1] + v2
      versionspec   = ('(' version_many:v ')' ->v) | version_many
      urlspec       = '@' wsp* <URI_reference>
      marker_op     = version_cmp | (wsp* 'in') | (wsp* 'not' wsp+ 'in')
      python_str_c  = (wsp | letter | digit | '(' | ')' | '.' | '{' | '}' |
                      '-' | '_' | '*' | '#' | ':' | ';' | ',' | '/' | '?' |
                      '[' | ']' | '!' | '~' | '`' | '@' | '$' | '%' | '^' |
                      '&' | '=' | '+' | '|' | '<' | '>' )
      dquote        = '"'
      squote        = '\\''
      python_str    = (squote <(python_str_c | dquote)*>:s squote |
                      dquote <(python_str_c | squote)*>:s dquote) -> s
      env_var       = ('python_version' | 'python_full_version' |
                      'os_name' | 'sys_platform' | 'platform_release' |
                      'platform_system' | 'platform_version' |
                      'platform_machine' | 'platform_python_implementation' |
                      'implementation_name' | 'implementation_version' |
                      'extra' # ONLY when defined by a containing layer
                      ):varname -> lookup(varname)
      marker_var    = wsp* (env_var | python_str)
      marker_expr   = marker_var:l marker_op:o marker_var:r -> (o, l, r)
                    | wsp* '(' marker:m wsp* ')' -> m
      marker_and    = marker_expr:l wsp* 'and' marker_expr:r -> ('and', l, r)
                    | marker_expr:m -> m
      marker_or     = marker_and:l wsp* 'or' marker_and:r -> ('or', l, r)
                        | marker_and:m -> m
      marker        = marker_or
      quoted_marker = ';' wsp* marker
      identifier_end = letterOrDigit | (('-' | '_' | '.' )* letterOrDigit)
      identifier    = < letterOrDigit identifier_end* >
      name          = identifier
      extras_list   = identifier:i (wsp* ',' wsp* identifier)*:ids -> [i] + ids
      extras        = '[' wsp* extras_list?:e wsp* ']' -> e
      name_req      = (name:n wsp* extras?:e wsp* versionspec?:v wsp* quoted_marker?:m
                      -> (n, e or [], v or [], m))
      url_req       = (name:n wsp* extras?:e wsp* urlspec:v (wsp+ | end) quoted_marker?:m
                      -> (n, e or [], v, m))
      specification = wsp* ( url_req | name_req ):s wsp* -> s
      # The result is a tuple - name, list-of-extras,
      # list-of-version-constraints-or-a-url, marker-ast or None


      URI_reference = <URI | relative_ref>
      URI           = scheme ':' hier_part ('?' query )? ( '#' fragment)?
      hier_part     = ('//' authority path_abempty) | path_absolute | path_rootless | path_empty
      absolute_URI  = scheme ':' hier_part ( '?' query )?
      relative_ref  = relative_part ( '?' query )? ( '#' fragment )?
      relative_part = '//' authority path_abempty | path_absolute | path_noscheme | path_empty
      scheme        = letter ( letter | digit | '+' | '-' | '.')*
      authority     = ( userinfo '@' )? host ( ':' port )?
      userinfo      = ( unreserved | pct_encoded | sub_delims | ':')*
      host          = IP_literal | IPv4address | reg_name
      port          = digit*
      IP_literal    = '[' ( IPv6address | IPvFuture) ']'
      IPvFuture     = 'v' hexdig+ '.' ( unreserved | sub_delims | ':')+
      IPv6address   = (
                        ( h16 ':'){6} ls32
                        | '::' ( h16 ':'){5} ls32
                        | ( h16 )?  '::' ( h16 ':'){4} ls32
                        | ( ( h16 ':')? h16 )? '::' ( h16 ':'){3} ls32
                        | ( ( h16 ':'){0,2} h16 )? '::' ( h16 ':'){2} ls32
                        | ( ( h16 ':'){0,3} h16 )? '::' h16 ':' ls32
                        | ( ( h16 ':'){0,4} h16 )? '::' ls32
                        | ( ( h16 ':'){0,5} h16 )? '::' h16
                        | ( ( h16 ':'){0,6} h16 )? '::' )
      h16           = hexdig{1,4}
      ls32          = ( h16 ':' h16) | IPv4address
      IPv4address   = dec_octet '.' dec_octet '.' dec_octet '.' dec_octet
      nz            = ~'0' digit
      dec_octet     = (
                        digit # 0-9
                        | nz digit # 10-99
                        | '1' digit{2} # 100-199
                        | '2' ('0' | '1' | '2' | '3' | '4') digit # 200-249
                        | '25' ('0' | '1' | '2' | '3' | '4' | '5') )# %250-255
      reg_name = ( unreserved | pct_encoded | sub_delims)*
      path = (
              path_abempty # begins with '/' or is empty
              | path_absolute # begins with '/' but not '//'
              | path_noscheme # begins with a non-colon segment
              | path_rootless # begins with a segment
              | path_empty ) # zero characters
      path_abempty  = ( '/' segment)*
      path_absolute = '/' ( segment_nz ( '/' segment)* )?
      path_noscheme = segment_nz_nc ( '/' segment)*
      path_rootless = segment_nz ( '/' segment)*
      path_empty    = pchar{0}
      segment       = pchar*
      segment_nz    = pchar+
      segment_nz_nc = ( unreserved | pct_encoded | sub_delims | '@')+
                      # non-zero-length segment without any colon ':'
      pchar         = unreserved | pct_encoded | sub_delims | ':' | '@'
      query         = ( pchar | '/' | '?')*
      fragment      = ( pchar | '/' | '?')*
      pct_encoded   = '%' hexdig
      unreserved    = letter | digit | '-' | '.' | '_' | '~'
      reserved      = gen_delims | sub_delims
      gen_delims    = ':' | '/' | '?' | '#' | '(' | ')?' | '@'
      sub_delims    = '!' | '$' | '&' | '\\'' | '(' | ')' | '*' | '+' | ',' | ';' | '='
      hexdig        = digit | 'a' | 'A' | 'b' | 'B' | 'c' | 'C' | 'd' | 'D' | 'e' | 'E' | 'f' | 'F'

  A test program - if the grammar is in a string ``grammar``:

.. code-block:: python

    import os
    import sys
    import platform

    from parsley import makeGrammar

    grammar = """
        wsp ...
        """
    tests = [
        "A",
        "A.B-C_D",
        "aa",
        "name",
        "name<=1",
        "name>=3",
        "name>=3,",
        "name>=3,<2",
        "name@http://foo.com",
        "name [fred,bar] @ http://foo.com ; python_version=='2.7'",
        "name[quux, strange];python_version<'2.7' and platform_version=='2'",
        "name; os_name=='a' or os_name=='b'",
        # Should parse as (a and b) or c
        "name; os_name=='a' and os_name=='b' or os_name=='c'",
        # Overriding precedence -> a and (b or c)
        "name; os_name=='a' and (os_name=='b' or os_name=='c')",
        # should parse as a or (b and c)
        "name; os_name=='a' or os_name=='b' and os_name=='c'",
        # Overriding precedence -> (a or b) and c
        "name; (os_name=='a' or os_name=='b') and os_name=='c'",
        ]

    def format_full_version(info):
        version = '{0.major}.{0.minor}.{0.micro}'.format(info)
        kind = info.releaselevel
        if kind != 'final':
            version += kind[0] + str(info.serial)
        return version

    if hasattr(sys, 'implementation'):
        implementation_version = format_full_version(sys.implementation.version)
        implementation_name = sys.implementation.name
    else:
        implementation_version = '0'
        implementation_name = ''
    bindings = {
        'implementation_name': implementation_name,
        'implementation_version': implementation_version,
        'os_name': os.name,
        'platform_machine': platform.machine(),
        'platform_python_implementation': platform.python_implementation(),
        'platform_release': platform.release(),
        'platform_system': platform.system(),
        'platform_version': platform.version(),
        'python_full_version': platform.python_version(),
        'python_version': '.'.join(platform.python_version_tuple()[:2]),
        'sys_platform': sys.platform,
    }

    compiled = makeGrammar(grammar, {'lookup': bindings.__getitem__})
    for test in tests:
        parsed = compiled(test).specification()
        print("%s -> %s" % (test, parsed))


历史
=======

**History**

.. tab:: 中文

  - 2015年11月：该规范通过 :pep:`508` 获得批准。
  - 2019年7月： ``python_version`` 的定义被 `更改 <python-version-change_>`_，从 ``platform.python_version()[:3]`` 更改为 ``'.'.join(platform.python_version_tuple()[:2])``，以适应可能的未来 Python 版本，具有两位数的主版本号和次版本号（例如 3.10）。 [#future_versions1]_
  - 2024年6月： ``version_many`` 的定义被更改，允许尾随逗号，以匹配自 2022 年底以来一直使用的 Python 实现的行为。

.. tab:: 英文

  - November 2015: This specification was approved through :pep:`508`.
  - July 2019: The definition of ``python_version`` was `changed <python-version-change_>`_ from ``platform.python_version()[:3]`` to ``'.'.join(platform.python_version_tuple()[:2])``, to accommodate potential future versions of Python with 2-digit major and minor versions (e.g. 3.10). [#future_versions]_
  - June 2024: The definition of ``version_many`` was changed to allow trailing commas, matching with the behavior of the Python implementation that has been in use since late 2022.


参考
==========

**References**

.. tab:: 中文



.. tab:: 英文

.. [#pip1] pip, Python 包的推荐安装程序 (http://pip.readthedocs.org/en/stable/)

.. [#pip] pip, the recommended installer for Python packages (http://pip.readthedocs.org/en/stable/)

.. [#parsley1] parsley PEG 库. (https://pypi.python.org/pypi/parsley/)

.. [#parsley] The parsley PEG library. (https://pypi.python.org/pypi/parsley/)

.. [#future_versions1] 未来的 Python 版本可能会与环境标记变量 ``python_version`` 的定义存在问题（https://github.com/python/peps/issues/560）

.. [#future_versions] Future Python versions might be problematic with the definition of Environment Marker Variable ``python_version`` (https://github.com/python/peps/issues/560)


.. _python-version-change: https://mail.python.org/pipermail/distutils-sig/2018-January/031920.html
