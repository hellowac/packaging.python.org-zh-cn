.. highlight:: text

.. _`core-metadata`:

============================
核心元数据规范
============================

**Core metadata specifications**

.. tab:: 中文

    在以下规范中定义的字段应视为有效、完整且不应更改。必填字段包括：

    - ``Metadata-Version``
    - ``Name``
    - ``Version``

    其他所有字段都是可选的。

    元数据的标准文件格式（包括 :doc:`wheels <binary-distribution-format>` 和 :doc:`installed projects <recording-installed-packages>`）基于电子邮件头的格式。然而，电子邮件格式已经经历了多次修订，具体适用于包装元数据的电子邮件RFC并未明确规定。在缺乏精确定义的情况下，实际标准由标准库 :mod:`python:email.parser` 模块根据 :data:`~.python:email.policy.compat32` 策略解析所确定。

    每当元数据被序列化为字节流（例如保存到文件时），字符串必须使用UTF-8编码进行序列化。

    尽管 :pep:`566` 定义了将元数据转换为JSON兼容字典的方法，但目前还没有作为标准交换格式使用。由于需要工具处理多年来存在的包，因此很难转向新格式。

    .. note:: 
        
        *解释旧元数据：* 在 :pep:`566` 中，版本说明符字段的格式规范被放宽，以接受流行发布工具使用的语法（特别是取消了版本说明符必须被括号包围的要求）。元数据消费者可能希望即使是版本号低于2.1的元数据文件，也使用这种更宽松的格式规则。

.. tab:: 英文

    Fields defined in the following specification should be considered valid,
    complete and not subject to change. The required fields are:

    - ``Metadata-Version``
    - ``Name``
    - ``Version``

    All the other fields are optional.

    The standard file format for metadata (including in :doc:`wheels
    <binary-distribution-format>` and :doc:`installed projects
    <recording-installed-packages>`) is based on the format of email headers.
    However, email formats have been revised several times, and exactly which email
    RFC applies to packaging metadata is not specified. In the absence of a precise
    definition, the practical standard is set by what the standard library
    :mod:`python:email.parser` module can parse using the
    :data:`~.python:email.policy.compat32` policy.

    Whenever metadata is serialised to a byte stream (for example, to save
    to a file), strings must be serialised using the UTF-8 encoding.

    Although :pep:`566` defined a way to transform metadata into a JSON-compatible
    dictionary, this is not yet used as a standard interchange format. The need for
    tools to work with years worth of existing packages makes it difficult to shift
    to a new format.

    .. note:: *Interpreting old metadata:* In :pep:`566`, the version specifier
        field format specification was relaxed to accept the syntax used by popular
        publishing tools (namely to remove the requirement that version specifiers
        must be surrounded by parentheses). Metadata consumers may want to use the
        more relaxed formatting rules even for metadata files that are nominally
        less than version 2.1.


.. _core-metadata-metadata-version:

Metadata-Version
================

.. versionadded:: 1.0

.. tab:: 中文

    文件格式的版本；合法值为 "1.0"、"1.1"、"1.2"、"2.1"、"2.2"、"2.3" 和 "2.4"。

    自动化工具在使用元数据时，如果 ``metadata_version`` 大于它们支持的最高版本，应发出警告；如果 ``metadata_version`` 的主版本号大于它们支持的最高版本，必须失败（如 :ref:`版本说明符规范 <version-specifiers>` 中所述，主版本号是第一个点之前的值）。

    为了更广泛的兼容性，构建工具可以选择使用最低的元数据版本来生成分发元数据，该版本包含所有所需的字段。

    示例::

        Metadata-Version: 2.4

.. tab:: 英文

    Version of the file format; legal values are "1.0", "1.1", "1.2", "2.1",
    "2.2", "2.3", and "2.4".

    Automated tools consuming metadata SHOULD warn if ``metadata_version`` is
    greater than the highest version they support, and MUST fail if
    ``metadata_version`` has a greater major version than the highest
    version they support (as described in the
    :ref:`Version specifier specification <version-specifiers>`,
    the major version is the value before the first dot).

    For broader compatibility, build tools MAY choose to produce
    distribution metadata using the lowest metadata version that includes
    all of the needed fields.

    Example::

        Metadata-Version: 2.4


.. _core-metadata-name:

Name
====

.. versionadded:: 1.0

.. tab:: 中文

    .. versionchanged:: 2.1  
        
       从 :ref:`name format <name-format>` 中添加了格式限制。

    分发包的名称。名称字段是分发包的主要标识符。它必须符合 :ref:`name format specification <name-format>`。

    示例::

        Name: BeagleVote

    为了比较的目的，名称在比较之前应该进行 :ref:`normalized <name-normalization>` 规范化。

.. tab:: 英文

    .. versionchanged:: 2.1
    Added restrictions on format from the :ref:`name format <name-format>`.

    The name of the distribution. The name field is the primary identifier for a
    distribution. It must conform to the :ref:`name format specification
    <name-format>`.

    Example::

        Name: BeagleVote

    For comparison purposes, the names should be :ref:`normalized <name-normalization>` before comparing.

.. _core-metadata-version:

Version
=======

.. versionadded:: 1.0

.. tab:: 中文

    一个包含分发包版本号的字符串。此字段必须符合 :ref:`版本说明符规范 <version-specifiers>` 中指定的格式。

    示例::

        Version: 1.0a2

.. tab:: 英文

    A string containing the distribution's version number.  This
    field  must be in the format specified in the
    :ref:`Version specifier specification <version-specifiers>`.

    Example::

        Version: 1.0a2


.. _core-metadata-dynamic:

动态（多次使用）
======================

**Dynamic (multiple use)**

.. versionadded:: 2.2

.. tab:: 中文

    一个包含另一个核心元数据字段名称的字符串。字段名称 ``Name``、 ``Version`` 和 ``Metadata-Version`` 不能在此字段中指定。

    在源分发包的元数据中发现此字段时，以下规则适用：

    1. 如果字段没有标记为 ``Dynamic``，则从该源分发包构建的任何 wheel 中该字段的值 **必须** 与源分发包中的值匹配。如果该字段在源分发包中不存在，并且没有标记为 ``Dynamic``，则该字段 **不得** 出现在 wheel 中。
    2. 如果字段标记为 ``Dynamic``，则它在从源分发包构建的 wheel 中可以包含任何有效的值（包括完全缺失）。

    如果源分发包的元数据版本早于 2.2，则所有字段应被视为已使用 ``Dynamic`` 指定（即，对从源分发包构建的 wheel 的元数据没有特殊限制）。

    在源分发包以外的任何上下文中， ``Dynamic`` 仅供参考，表示该字段的值是在 wheel 构建时计算得出的，可能与源分发包或该项目的其他 wheel 中的值不同。

    ``Dynamic`` 的完整语义详见 :pep:`643`。

.. tab:: 英文

    A string containing the name of another core metadata field. The field
    names ``Name``, ``Version``, and ``Metadata-Version`` may not be specified
    in this field.

    When found in the metadata of a source distribution, the following
    rules apply:

    1. If a field is *not* marked as ``Dynamic``, then the value of the field
       in any wheel built from the sdist MUST match the value in the sdist.
       If the field is not in the sdist, and not marked as ``Dynamic``, then
       it MUST NOT be present in the wheel.
    2. If a field is marked as ``Dynamic``, it may contain any valid value in
       a wheel built from the sdist (including not being present at all).

    If the sdist metadata version is older than version 2.2, then all fields should
    be treated as if they were specified with ``Dynamic`` (i.e. there are no special
    restrictions on the metadata of wheels built from the sdist).

    In any context other than a source distribution, ``Dynamic`` is for information
    only, and indicates that the field value was calculated at wheel build time,
    and may not be the same as the value in the sdist or in other wheels for the
    project.

    Full details of the semantics of ``Dynamic`` are described in :pep:`643`.

.. _core-metadata-platform:

平台（多次使用）
=======================

**Platform (multiple use)**

.. versionadded:: 1.0

.. tab:: 中文

    一个平台规范，描述了分发包支持的操作系统，但该操作系统未列在“操作系统”Trove 分类器中。请参见下面的“分类器”。

    示例::

        Platform: ObscureUnix
        Platform: RareDOS

.. tab:: 英文

    A Platform specification describing an operating system supported by
    the distribution which is not listed in the "Operating System" Trove classifiers.
    See "Classifier" below.

    Examples::

        Platform: ObscureUnix
        Platform: RareDOS

.. _core-metadata-supported-platform:

支持的平台（多次使用）
=================================

**Supported-Platform (multiple use)**

.. versionadded:: 1.1

.. tab:: 中文

    包含 PKG-INFO 文件的二进制分发包将使用其元数据中的 `Supported-Platform` 字段来指定二进制分发包为其编译的操作系统和 CPU。 `Supported-Platform` 字段的语义在本 PEP 中未作详细说明。

    示例::

        Supported-Platform: RedHat 7.2
        Supported-Platform: i386-win32-2791

.. tab:: 英文

    Binary distributions containing a PKG-INFO file will use the
    Supported-Platform field in their metadata to specify the OS and
    CPU for which the binary distribution was compiled.  The semantics of
    the Supported-Platform field are not specified in this PEP.

    Example::

        Supported-Platform: RedHat 7.2
        Supported-Platform: i386-win32-2791


.. _core-metadata-summary:

摘要
=======

**Summary**

.. versionadded:: 1.0

.. tab:: 中文

    关于该分发的作用的一行摘要。

    Example::

        Summary: A module for collecting votes from beagles.



.. tab:: 英文

    A one-line summary of what the distribution does.

    Example::

        Summary: A module for collecting votes from beagles.

.. 其中一些标题过去带有后缀“(可选)”。这成为链接的一部分 (...#description-optional)。我们已更改标题（必填字段现在列在规范的开头），但添加了像这样的明确链接目标，以便指向各个部分的链接不会中断。

.. Some of these headings used to have a suffix "(optional)". This became part of links (...#description-optional). We have changed the headings (required fields are now listed at the start of the specification), but added explicit link targets like this one, so that links to the individual sections are not broken.


.. _description-optional:
.. _core-metadata-description:

说明
===========

**Description**

.. versionadded:: 1.0

.. tab:: 中文

    .. versionchanged:: 2.1  
        此字段现在可以在消息正文中指定。

    这是对分发包的更长描述，可以包括多个段落。处理元数据的软件不应假设该字段的最大大小，尽管人们不应将其说明书作为描述内容。

    此字段的内容可以使用 reStructuredText 标记语言编写 [1]_。对于处理元数据的程序，支持标记语言是可选的；程序也可以直接显示该字段的内容。因此，作者在使用标记语言时应保持谨慎。

    为了支持空行和缩进行的格式，符合 RFC 822 格式的任何 CRLF 字符后必须跟随 7 个空格和一个管道符号 ("|")。因此，Description 字段被编码为一个折叠字段，可以被 RFC822 解析器 [2]_ 解读。

    示例::

        Description: This project provides powerful math functions
                |For example, you can use `sum()` to sum numbers:
                |
                |Example::
                |
                |    >>> sum(1, 2)
                |    3
                |

    这种编码方式意味着，当使用 RFC822 读取器展开字段时，任何出现的 CRLF 后跟 7 个空格和管道符号的情况，都必须被替换为单个 CRLF。

    另外，分发包的描述也可以直接放在消息正文中（即，在头部后跟一个完全空白的行，并且不需要缩进或其他特殊格式）。

.. tab:: 英文

    .. versionchanged:: 2.1
        This field may be specified in the message body instead.

    A longer description of the distribution that can run to several
    paragraphs.  Software that deals with metadata should not assume
    any maximum size for this field, though people shouldn't include
    their instruction manual as the description.

    The contents of this field can be written using reStructuredText
    markup [1]_.  For programs that work with the metadata, supporting
    markup is optional; programs can also display the contents of the
    field as-is.  This means that authors should be conservative in
    the markup they use.

    To support empty lines and lines with indentation with respect to
    the RFC 822 format, any CRLF character has to be suffixed by 7 spaces
    followed by a pipe ("|") char. As a result, the Description field is
    encoded into a folded field that can be interpreted by RFC822
    parser [2]_.

    Example::

        Description: This project provides powerful math functions
                |For example, you can use `sum()` to sum numbers:
                |
                |Example::
                |
                |    >>> sum(1, 2)
                |    3
                |

    This encoding implies that any occurrences of a CRLF followed by 7 spaces
    and a pipe char have to be replaced by a single CRLF when the field is unfolded
    using a RFC822 reader.

    Alternatively, the distribution's description may instead be provided in the
    message body (i.e., after a completely blank line following the headers, with
    no indentation or other special formatting necessary).


.. _description-content-type-optional:
.. _core-metadata-description-content-type:

说明内容类型
========================

**Description-Content-Type**

.. versionadded:: 2.1

.. tab:: 中文

    一个字符串，声明在分发包描述中使用的标记语法（如果有的话），以便工具可以智能地渲染描述。

    历史上，PyPI 支持纯文本和 `reStructuredText (reST) <https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html>`_ 描述，并能够将 reST 渲染为 HTML。然而，很多分发包作者习惯使用 `Markdown <https://daringfireball.net/projects/markdown/>`_ (:rfc:`7763`) 来编写描述，因为许多代码托管网站渲染 Markdown 格式的 README 文件，作者通常会重用该文件作为描述。PyPI 之前无法识别 Markdown 格式，因此无法正确渲染描述。这导致 PyPI 上的许多包显示了渲染不正确的描述，尤其是当 Markdown 作为纯文本上传时，或者更糟糕的是，试图将其作为 reST 渲染时。这个字段允许分发包作者指定描述的格式，开启了 PyPI 和其他工具能够渲染 Markdown 和其他格式的可能性。

    此字段的格式与 HTTP 中的 ``Content-Type`` 头相同（即：`RFC 1341 <https://www.w3.org/Protocols/rfc1341/4_Content-Type.html>`_）。简而言之，它包含 ``type/subtype`` 部分，并可以选择性地包含若干参数：

    格式::

        Description-Content-Type: <type>/<subtype>; charset=<charset>[; <param_name>=<param value> ...]

    ``type/subtype`` 部分有以下合法值：

    - ``text/plain``
    - ``text/x-rst``
    - ``text/markdown``

    ``charset`` 参数可用于指定描述的字符编码。唯一合法的值是 ``UTF-8``。如果省略，则默认为 ``UTF-8``。

    其他参数可能特定于所选的子类型。例如，对于 ``markdown`` 子类型，有一个可选的 ``variant`` 参数，用于指定正在使用的 Markdown 变体（如果未指定，则默认为 ``GFM``）。目前，识别的两种变体是：

    - ``GFM``：用于 :rfc:`GitHub-flavored Markdown <7764#section-3.2>`
    - ``CommonMark``：用于 :rfc:`CommonMark <7764#section-3.5>`

    示例::

        Description-Content-Type: text/plain; charset=UTF-8


    示例::

        Description-Content-Type: text/x-rst; charset=UTF-8


    示例::

        Description-Content-Type: text/markdown; charset=UTF-8; variant=GFM


    示例::

        Description-Content-Type: text/markdown


    如果没有指定 ``Description-Content-Type``，那么应用程序应尝试将其渲染为 ``text/x-rst; charset=UTF-8``，如果无效的 reST 格式，则回退到 ``text/plain``。

    如果 ``Description-Content-Type`` 是未识别的值，则假定内容类型为 ``text/plain`` （虽然 PyPI 可能会拒绝任何未识别的值）。

    如果 ``Description-Content-Type`` 是 ``text/markdown`` 且未指定 ``variant`` 或其值未被识别，则假定 ``variant`` 为 ``GFM``。

    因此，对于上面的最后一个示例， ``charset`` 默认为 ``UTF-8``，``variant`` 默认为 ``GFM``，与前一个示例等效。

.. tab:: 英文

    A string stating the markup syntax (if any) used in the distribution's
    description, so that tools can intelligently render the description.

    Historically, PyPI supported descriptions in plain text and `reStructuredText
    (reST) <https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html>`_,
    and could render reST into HTML. However, it is common for distribution
    authors to write the description in `Markdown
    <https://daringfireball.net/projects/markdown/>`_ (:rfc:`7763`) as many code hosting sites render
    Markdown READMEs, and authors would reuse the file for the description. PyPI
    didn't recognize the format and so could not render the description correctly.
    This resulted in many packages on PyPI with poorly-rendered descriptions when
    Markdown is left as plain text, or worse, was attempted to be rendered as reST.
    This field allows the distribution author to specify the format of their
    description, opening up the possibility for PyPI and other tools to be able to
    render Markdown and other formats.

    The format of this field is the same as the ``Content-Type`` header in HTTP
    (i.e.:
    `RFC 1341 <https://www.w3.org/Protocols/rfc1341/4_Content-Type.html>`_).
    Briefly, this means that it has a ``type/subtype`` part and then it can
    optionally have a number of parameters:

    Format::

        Description-Content-Type: <type>/<subtype>; charset=<charset>[; <param_name>=<param value> ...]

    The ``type/subtype`` part has only a few legal values:

    - ``text/plain``
    - ``text/x-rst``
    - ``text/markdown``

    The ``charset`` parameter can be used to specify the character encoding of
    the description. The only legal value is ``UTF-8``. If omitted, it is assumed to
    be ``UTF-8``.

    Other parameters might be specific to the chosen subtype. For example, for the
    ``markdown`` subtype, there is an optional ``variant`` parameter that allows
    specifying the variant of Markdown in use (defaults to ``GFM`` if not
    specified). Currently, two variants are recognized:

    - ``GFM`` for :rfc:`GitHub-flavored Markdown <7764#section-3.2>`
    - ``CommonMark`` for :rfc:`CommonMark <7764#section-3.5>`

    Example::

        Description-Content-Type: text/plain; charset=UTF-8

    Example::

        Description-Content-Type: text/x-rst; charset=UTF-8

    Example::

        Description-Content-Type: text/markdown; charset=UTF-8; variant=GFM

    Example::

        Description-Content-Type: text/markdown

    If a ``Description-Content-Type`` is not specified, then applications should
    attempt to render it as ``text/x-rst; charset=UTF-8`` and fall back to
    ``text/plain`` if it is not valid rst.

    If a ``Description-Content-Type`` is an unrecognized value, then the assumed
    content type is ``text/plain`` (Although PyPI will probably reject anything
    with an unrecognized value).

    If the ``Description-Content-Type`` is ``text/markdown`` and ``variant`` is not
    specified or is set to an unrecognized value, then the assumed ``variant`` is
    ``GFM``.

    So for the last example above, the ``charset`` defaults to ``UTF-8`` and the
    ``variant`` defaults to ``GFM`` and thus it is equivalent to the example
    before it.


.. _keywords-optional:
.. _core-metadata-keywords:

关键字
========

**Keywords**

.. versionadded:: 1.0

.. tab:: 中文

    一个额外的关键词列表，用逗号分隔，用于帮助在更大的目录中搜索该分发包。

    示例::

        Keywords: dog,puppy,voting,election

    .. note::

        先前的规范显示关键词是用空格分隔的，但 distutils 和 setuptools 实现时使用了逗号。由于这些工具已被广泛使用多年，因此更新规范以匹配事实上的标准更为容易。

.. tab:: 英文

    A list of additional keywords, separated by commas, to be used to assist
    searching for the distribution in a larger catalog.

    Example::

        Keywords: dog,puppy,voting,election

    .. note::

    The specification previously showed keywords separated by spaces,
    but distutils and setuptools implemented it with commas.
    These tools have been very widely used for many years, so it was
    easier to update the specification to match the de facto standard.

.. _author-optional:
.. _core-metadata-author:

作者
======

**Author**

.. versionadded:: 1.0

.. tab:: 中文

    一个包含作者姓名的字符串；可以提供其他联系方式。

    示例::

        Author: C. Schultz, Universal Features Syndicate,
                Los Angeles, CA <cschultz@peanuts.example.com>

.. tab:: 英文

    A string containing the author's name at a minimum; additional
    contact information may be provided.

    Example::

        Author: C. Schultz, Universal Features Syndicate,
                Los Angeles, CA <cschultz@peanuts.example.com>


.. _author-email-optional:
.. _core-metadata-author-email:

作者电子邮件
============

**Author-email**

.. versionadded:: 1.0

.. tab:: 中文

    一个包含作者电子邮件地址的字符串。它可以包含姓名和电子邮件地址，符合 RFC-822 的 ``From:`` 头部合法格式。

    示例::

        Author-email: "C. Schultz" <cschultz@example.com>

    根据 RFC-822，字段中可以包含多个以逗号分隔的电子邮件地址::

        Author-email: cschultz@example.com, snoopy@peanuts.com

.. tab:: 英文

    A string containing the author's e-mail address.  It can contain
    a name and e-mail address in the legal forms for a RFC-822
    ``From:`` header.

    Example::

        Author-email: "C. Schultz" <cschultz@example.com>

    Per RFC-822, this field may contain multiple comma-separated e-mail
    addresses::

        Author-email: cschultz@example.com, snoopy@peanuts.com


.. _maintainer-optional:
.. _core-metadata-maintainer:

维护者
==========

**Maintainer**

.. versionadded:: 1.2

.. tab:: 中文

    一个包含维护者姓名的字符串，至少包括姓名；可以提供其他联系方式。

    请注意，当项目由除原作者以外的其他人维护时，应该使用此字段：如果维护者与 ``Author`` 相同，则应省略此字段。

    示例::

        Maintainer: C. Schultz, Universal Features Syndicate,
                Los Angeles, CA <cschultz@peanuts.example.com>

.. tab:: 英文

    A string containing the maintainer's name at a minimum; additional
    contact information may be provided.

    Note that this field is intended for use when a project is being
    maintained by someone other than the original author:  it should be
    omitted if it is identical to ``Author``.

    Example::

        Maintainer: C. Schultz, Universal Features Syndicate,
                Los Angeles, CA <cschultz@peanuts.example.com>


.. _maintainer-email-optional:
.. _core-metadata-maintainer-email:

维护者电子邮件
================

**Maintainer-email**

.. versionadded:: 1.2

.. tab:: 中文

    一个包含维护者电子邮件地址的字符串。它可以包含一个姓名和电子邮件地址，符合 RFC-822 的合法形式（即 ``From:`` 头格式）。

    请注意，当项目由除原作者以外的其他人维护时，应该使用此字段：如果它与 ``Author-email`` 相同，则应省略此字段。

    示例::

        Maintainer-email: "C. Schultz" <cschultz@example.com>

    根据 RFC-822 规定，此字段可以包含多个由逗号分隔的电子邮件地址::

        Maintainer-email: cschultz@example.com, snoopy@peanuts.com

.. tab:: 英文

    A string containing the maintainer's e-mail address.  It can contain
    a name and e-mail address in the legal forms for a RFC-822
    ``From:`` header.

    Note that this field is intended for use when a project is being
    maintained by someone other than the original author:  it should be
    omitted if it is identical to ``Author-email``.

    Example::

        Maintainer-email: "C. Schultz" <cschultz@example.com>

    Per RFC-822, this field may contain multiple comma-separated e-mail
    addresses::

        Maintainer-email: cschultz@example.com, snoopy@peanuts.com


.. _license-optional:
.. _core-metadata-license:

许可证
=======

**License**

.. versionadded:: 1.0

.. tab:: 中文

    .. deprecated:: 2.4  
        取而代之的是 ``License-Expression``。

    .. warning::  
        从 Metadata 2.4 开始，``License`` 和 ``License-Expression`` 是互斥的。如果同时指定了这两个字段，解析元数据的工具将忽略 ``License`` 字段，并且 PyPI 将拒绝上传。  
        请参阅 `PEP 639 <https://peps.python.org/pep-0639/#deprecate-license-field>`__。

    文本，表示涵盖该分发的许可证，当该许可证不是从 "License" Trove 分类器中选择的项时。请参阅下面的 :ref:`"Classifier" <metadata-classifier>`。

    此字段还可用于指定通过 ``Classifier`` 字段命名的特定版本的许可证，或表示该许可证的变种或例外情况。

    示例::

        License: This software may only be obtained by sending the
                author a postcard, and then the user promises not
                to redistribute it.

        License: GPL version 3, excluding DRM provisions

.. tab:: 英文

    .. deprecated:: 2.4
        in favour of ``License-Expression``.

    .. warning::
        As of Metadata 2.4, ``License`` and ``License-Expression`` are mutually
        exclusive. If both are specified, tools which parse metadata will disregard
        ``License`` and PyPI will reject uploads.
        See `PEP 639 <https://peps.python.org/pep-0639/#deprecate-license-field>`__.

    Text indicating the license covering the distribution where the license
    is not a selection from the "License" Trove classifiers. See
    :ref:`"Classifier" <metadata-classifier>` below.
    This field may also be used to specify a
    particular version of a license which is named via the ``Classifier``
    field, or to indicate a variation or exception to such a license.

    Examples::

        License: This software may only be obtained by sending the
                author a postcard, and then the user promises not
                to redistribute it.

        License: GPL version 3, excluding DRM provisions


.. _license-expression-optional:
.. _core-metadata-license-expression:

许可证表达式
==================

**License-Expression**

.. versionadded:: 2.4

.. tab:: 中文

    文本字符串是有效的 SPDX `许可证表达式 <https://peps.python.org/pep-0639/#term-license-expression>`__，如 `PEP 639 <https://peps.python.org/pep-0639/#spdx>`__ 中所定义。

    示例::

        License-Expression: MIT
        License-Expression: BSD-3-Clause
        License-Expression: MIT AND (Apache-2.0 OR BSD-2-Clause)
        License-Expression: MIT OR GPL-2.0-or-later OR (FSFUL AND BSD-2-Clause)
        License-Expression: GPL-3.0-only WITH Classpath-Exception-2.0 OR BSD-3-Clause
        License-Expression: LicenseRef-Special-License OR CC0-1.0 OR Unlicense
        License-Expression: LicenseRef-Proprietary

.. tab:: 英文

    Text string that is a valid SPDX `license expression <https://peps.python.org/pep-0639/#term-license-expression>`__ as `defined in PEP 639 <https://peps.python.org/pep-0639/#spdx>`__.

    Examples::

        License-Expression: MIT
        License-Expression: BSD-3-Clause
        License-Expression: MIT AND (Apache-2.0 OR BSD-2-Clause)
        License-Expression: MIT OR GPL-2.0-or-later OR (FSFUL AND BSD-2-Clause)
        License-Expression: GPL-3.0-only WITH Classpath-Exception-2.0 OR BSD-3-Clause
        License-Expression: LicenseRef-Special-License OR CC0-1.0 OR Unlicense
        License-Expression: LicenseRef-Proprietary


.. _license-file-optional:
.. _core-metadata-license-file:

许可证文件（多次使用）
===========================

**License-File (multiple use)**

.. versionadded:: 2.4

.. tab:: 中文

    每个条目是一个表示许可证相关文件路径的字符串。  
    该路径位于项目源代码树中，相对于项目根目录。有关详细信息，请参见 :pep:`639`。

    示例::

        License-File: LICENSE
        License-File: AUTHORS
        License-File: LICENSE.txt
        License-File: licenses/LICENSE.MIT
        License-File: licenses/LICENSE.CC0

.. tab:: 英文

    Each entry is a string representation of the path of a license-related file.
    The path is located within the project source tree, relative to the project
    root directory. For details see :pep:`639`.

    Examples::

        License-File: LICENSE
        License-File: AUTHORS
        License-File: LICENSE.txt
        License-File: licenses/LICENSE.MIT
        License-File: licenses/LICENSE.CC0


.. _metadata-classifier:
.. _core-metadata-classifier:

分类器（多次使用）
=========================

**Classifier (multiple use)**

.. versionadded:: 1.1

.. tab:: 中文

    每个条目是一个字符串，提供分发包的单一分类值。  
    分类器在 :pep:`301` 中有描述，且 Python 包索引发布了一个动态的 `当前已定义分类器列表 <https://pypi.org/classifiers/>`__。

    .. note::
        从 Metadata 2.4 开始， ``License ::`` 分类器已弃用，请改用 ``License-Expression``。详见
        `PEP 639 <https://peps.python.org/pep-0639/#deprecate-license-classifiers>`_。

    此字段后可以跟一个环境标记，前面用分号分隔。

    示例::

        Classifier: Development Status :: 4 - Beta
        Classifier: Environment :: Console (Text Based)

.. tab:: 英文

    Each entry is a string giving a single classification value
    for the distribution.  Classifiers are described in :pep:`301`,
    and the Python Package Index publishes a dynamic list of
    `currently defined classifiers <https://pypi.org/classifiers/>`__.

    .. note::
        The use of ``License ::`` classifiers  is deprecated as of Metadata 2.4,
        use ``License-Expression`` instead. See
        `PEP 639 <https://peps.python.org/pep-0639/#deprecate-license-classifiers>`_.

    This field may be followed by an environment marker after a semicolon.

    Examples::

        Classifier: Development Status :: 4 - Beta
        Classifier: Environment :: Console (Text Based)


.. _core-metadata-requires-dist:

Requires-Dist（多次使用）
============================

**Requires-Dist (multiple use)**

.. versionadded:: 1.2

.. tab:: 中文

    .. versionchanged:: 2.1  
       字段格式规范放宽，允许使用流行发布工具使用的语法。

    每个条目包含一个字符串，列出此分发包所需的其他 distutils 项目。

    要求字符串的格式包含一到四个部分：

    * 项目名称，格式与 ``Name:`` 字段相同。这是唯一必需的部分。
    * 以逗号分隔的“extra”名称列表。这些名称由所需项目定义，指的是可能需要额外依赖项的特定功能。名称必须符合 ``Provides-Extra:`` 字段中指定的限制。
    * 版本说明符。解析此格式的工具应接受可选的括号，但生成它的工具不应使用括号。
    * 分号后跟环境标记。这意味着该要求仅在指定条件下才需要。

    有关允许的格式的完整细节，请参阅 :pep:`508`。

    项目名称应与在 `Python 包索引`_ 上找到的名称相对应。

    版本说明符必须遵循 :doc:`version-specifiers` 中描述的规则。

    示例::

        Requires-Dist: pkginfo
        Requires-Dist: PasteDeploy
        Requires-Dist: zope.interface (>3.5.0)
        Requires-Dist: pywin32 >1.0; sys_platform == 'win32'

.. tab:: 英文

    .. versionchanged:: 2.1
       The field format specification was relaxed to accept the syntax used by
       popular publishing tools.

    Each entry contains a string naming some other distutils
    project required by this distribution.

    The format of a requirement string contains from one to four parts:

    * A project name, in the same format as the ``Name:`` field. The only mandatory part.
    * A comma-separated list of 'extra' names. These are defined by
      the required project, referring to specific features which may
      need extra dependencies. The names MUST conform to the restrictions
      specified by the ``Provides-Extra:`` field.
    * A version specifier. Tools parsing the format should accept optional
      parentheses around this, but tools generating it should not use
      parentheses.
    * An environment marker after a semicolon. This means that the
      requirement is only needed in the specified conditions.

    See :pep:`508` for full details of the allowed format.

    The project names should correspond to names as found
    on the `Python Package Index`_.

    Version specifiers must follow the rules described in
    :doc:`version-specifiers`.

    Examples::

        Requires-Dist: pkginfo
        Requires-Dist: PasteDeploy
        Requires-Dist: zope.interface (>3.5.0)
        Requires-Dist: pywin32 >1.0; sys_platform == 'win32'


.. _core-metadata-requires-python:

Requires-Python
===============

.. versionadded:: 1.2

.. tab:: 中文

    此字段指定该分发包与哪些 Python 版本兼容。安装工具在选择要安装的项目版本时，可能会查看此字段。

    该值必须符合 :doc:`version-specifiers` 中指定的格式。

    例如，如果一个分发包使用了 :ref:`f-strings <whatsnew36-pep498>`，则可以通过指定以下内容来防止在 Python 版本低于 3.6 时安装::

        Requires-Python: >=3.6

    此字段后不能跟环境标记。

.. tab:: 英文

    This field specifies the Python version(s) that the distribution is
    compatible with. Installation tools may look at this when
    picking which version of a project to install.

    The value must be in the format specified in :doc:`version-specifiers`.

    For example, if a distribution uses :ref:`f-strings <whatsnew36-pep498>`
    then it may prevent installation on Python < 3.6 by specifying::

        Requires-Python: >=3.6

    This field cannot be followed by an environment marker.

.. _core-metadata-requires-external:

Requires-External（多次使用）
================================

**Requires-External (multiple use)**

.. versionadded:: 1.2

.. tab:: 中文

    .. versionchanged:: 2.1
       字段格式规范被放宽以接受流行发布工具使用的语法。

    此字段包含描述系统中某些依赖项的字符串，该系统将使用该分发包。此字段旨在作为下游项目维护者的提示，对于 ``distutils`` 分发包本身没有意义。

    依赖项字符串的格式为外部依赖项的名称，后面可选地跟着一个在括号中的版本声明。

    此字段后可以跟一个环境标记，紧跟着分号。

    由于它们涉及非 Python 软件的发布，此字段中的版本号 **不** 必须符合 :ref:`Version specifier specification <version-specifiers>` 中指定的格式；它们应该符合外部依赖项使用的版本方案。

    注意，对所使用的字符串没有特定的规则。

    示例::

        Requires-External: C
        Requires-External: libpng (>=1.5)
        Requires-External: make; sys_platform != "win32"

.. tab:: 英文

    .. versionchanged:: 2.1
       The field format specification was relaxed to accept the syntax used by
       popular publishing tools.

    Each entry contains a string describing some dependency in the
    system that the distribution is to be used.  This field is intended to
    serve as a hint to downstream project maintainers, and has no
    semantics which are meaningful to the ``distutils`` distribution.

    The format of a requirement string is a name of an external
    dependency, optionally followed by a version declaration within
    parentheses.

    This field may be followed by an environment marker after a semicolon.

    Because they refer to non-Python software releases, version numbers
    for this field are **not** required to conform to the format
    specified in the :ref:`Version specifier specification <version-specifiers>`:
    they should correspond to the version scheme used by the external dependency.

    Notice that there is no particular rule on the strings to be used.

    Examples::

        Requires-External: C
        Requires-External: libpng (>=1.5)
        Requires-External: make; sys_platform != "win32"


.. _core-metadata-project-url:

Project-URL（多次使用）
==========================

**Project-URL (multiple-use)**

.. versionadded:: 1.2

.. tab:: 中文

    一个字符串，包含项目的可浏览 URL 和一个标签，两者由逗号分隔。

    示例::

        Project-URL: Bug Tracker, http://bitbucket.org/tarek/distribute/issues/

    标签是自由文本，最多 32 个字符。

    从 :pep:`753` 开始，项目元数据的消费方（如 Python 包索引）可以使用标准的规范化过程来发现“著名”标签，然后在呈现给人类时，可以对这些标签进行特别展示。有关详细信息，请参见 :ref:`well-known-project-urls`。

.. tab:: 英文

    A string containing a browsable URL for the project and a label for it,
    separated by a comma.

    Example::

        Project-URL: Bug Tracker, http://bitbucket.org/tarek/distribute/issues/

    The label is free text limited to 32 characters.

    Starting with :pep:`753`, project metadata consumers (such as the Python
    Package Index) can use a standard normalization process to discover "well-known"
    labels, which can then be given special presentations when being rendered
    for human consumption. See :ref:`well-known-project-urls`.

.. _metadata_provides_extra:
.. _core-metadata-provides-extra:
.. _provides-extra-optional-multiple-use:

Provides-Extra（多次使用）
=============================

**Provides-Extra (multiple use)**

.. versionadded:: 2.1

.. tab:: 中文

    一个字符串，包含可选功能的名称。有效的名称仅由小写 ASCII 字母、ASCII 数字和连字符组成。它必须以字母或数字开始并结束。连字符后不能紧跟另一个连字符。名称的格式必须符合以下正则表达式（确保不歧义）::

        ^[a-z0-9]+(-[a-z0-9]+)*$

    指定的名称可用于根据是否请求了该可选功能来使依赖关系具有条件性。

    示例::

        Provides-Extra: pdf
        Requires-Dist: reportlab; extra == 'pdf'

    第二个分发包通过将其放入方括号中来要求可选依赖项，并可以通过用逗号（,）分隔多个功能来请求多个功能。要求会根据每个请求的功能进行评估，并将其添加到分发包的要求集中。

    示例::

        Requires-Dist: beaglevote[pdf]
        Requires-Dist: libexample[test, doc]

    两个功能名称 ``test`` 和 ``doc`` 被保留用于标记分别用于运行自动化测试和生成文档的依赖项。

    可以合法地指定 ``Provides-Extra:``，而不在任何 ``Requires-Dist:`` 中引用它。

    在为旧版本的元数据编写数据时，名称必须遵循与 ``Name:`` 字段比较时相同的规范化规则。如果两个 ``Provides-Extra:`` 条目在规范化后发生冲突，编写元数据的工具必须抛出错误。

    在读取旧版本的元数据时，当该字段的值在新版本的元数据中无效时，工具应该发出警告。如果某个值根据 ``Name:`` 字段的规则在任何核心元数据版本中无效，则应警告用户并忽略该值，以避免歧义。工具可以选择在读取无效名称时为旧版本的元数据抛出错误。

.. tab:: 英文

    .. versionchanged:: 2.3
       :pep:`685` restricted valid values to be unambiguous (i.e. no normalization
       required). For older metadata versions, value restrictions were brought into
       line with ``Name:`` and normalization rules were introduced.

    A string containing the name of an optional feature. A valid name consists only
    of lowercase ASCII letters, ASCII numbers, and hyphen. It must start and end
    with a letter or number. Hyphens cannot be followed by another hyphen. Names are
    limited to those which match the following regex (which guarantees unambiguity)::

        ^[a-z0-9]+(-[a-z0-9]+)*$


    The specified name may be used to make a dependency conditional on whether the
    optional feature has been requested.

    Example::

        Provides-Extra: pdf
        Requires-Dist: reportlab; extra == 'pdf'

    A second distribution requires an optional dependency by placing it
    inside square brackets, and can request multiple features by separating
    them with a comma (,). The requirements are evaluated for each requested
    feature and added to the set of requirements for the distribution.

    Example::

        Requires-Dist: beaglevote[pdf]
        Requires-Dist: libexample[test, doc]

    Two feature names ``test`` and ``doc`` are reserved to mark dependencies that
    are needed for running automated tests and generating documentation,
    respectively.

    It is legal to specify ``Provides-Extra:`` without referencing it in any
    ``Requires-Dist:``.

    When writing data for older metadata versions, names MUST be normalized
    following the same rules used for the ``Name:`` field when performing
    comparisons. Tools writing metadata MUST raise an error if two
    ``Provides-Extra:`` entries would clash after being normalized.

    When reading data for older metadata versions, tools SHOULD warn when values
    for this field would be invalid under newer metadata versions. If a value would
    be invalid following the rules for ``Name:`` in any core metadata version, the
    user SHOULD be warned and the value ignored to avoid ambiguity. Tools MAY choose
    to raise an error when reading an invalid name for older metadata versions.


很少使用的字段
==================

**Rarely Used Fields**

.. tab:: 中文

    本节中的字段目前很少使用，因为它们的设计灵感来自 Linux 包管理系统中的类似机制，但在像 `PyPI <https://pypi.org>`__ 这样的开放索引服务器环境中，工具如何解释它们尚不清楚。

    因此，流行的安装工具完全忽略这些字段，这意味着包发布者没有太大动力去适当地设置它们。然而，这些字段仍保留在元数据规范中，因为它们仍然可能对信息展示有用，并且在与经过策划的包存储库结合使用时，仍然可以用于其最初的目的。

.. tab:: 英文

    The fields in this section are currently rarely used, as their design
    was inspired by comparable mechanisms in Linux package management systems,
    and it isn't at all clear how tools should interpret them in the context
    of an open index server such as `PyPI <https://pypi.org>`__.

    As a result, popular installation tools ignore them completely, which in
    turn means there is little incentive for package publishers to set them
    appropriately. However, they're retained in the metadata specification,
    as they're still potentially useful for informational purposes, and can
    also be used for their originally intended purpose in combination with
    a curated package repository.

.. _core-metadata-provides-dist:

Provides-Dist（多次使用）
----------------------------

**Provides-Dist (multiple use)**

.. versionadded:: 1.2

.. tab:: 中文

    .. versionchanged:: 2.1
       字段格式规范已放宽，以接受流行发布工具使用的语法。

    每个条目包含一个字符串，命名该分发包中包含的 Distutils 项目。此字段 *必须* 包含在 ``Name`` 字段中标识的项目，后跟版本号： `Name (Version)`。

    一个分发包可以提供额外的名称，例如，表示多个项目已被捆绑在一起。例如，``ZODB`` 项目的源分发包历来包含 ``transaction`` 项目，而该项目现在已作为单独的分发包提供。安装这样的源分发包可以满足对 ``ZODB`` 和 ``transaction`` 的依赖要求。

    分发包还可以提供一个“虚拟”项目名称，它不对应任何单独分发的项目：这样的名称可能用来表示一个抽象能力，可能由多个项目中的一个提供。例如，多个项目可能会提供用于给定 ORM 的 RDBMS 绑定：每个项目可能声明它提供 ``ORM-bindings``，允许其他项目仅依赖于安装最多一个该项目。

    可以提供版本声明，且必须遵循 :doc:`version-specifiers` 中描述的规则。如果未指定版本号，则默认使用分发包的版本号。

    此字段后面可以跟环境标记，使用分号分隔。

    示例::

        Provides-Dist: OtherProject
        Provides-Dist: AnotherProject==3.4
        Provides-Dist: virtual_package; python_version >= "3.4"

.. tab:: 英文

    .. versionchanged:: 2.1
       The field format specification was relaxed to accept the syntax used by
       popular publishing tools.

    Each entry contains a string naming a Distutils project which
    is contained within this distribution.  This field *must* include
    the project identified in the ``Name`` field, followed by the
    version : Name (Version).

    A distribution may provide additional names, e.g. to indicate that
    multiple projects have been bundled together.  For instance, source
    distributions of the ``ZODB`` project have historically included
    the ``transaction`` project, which is now available as a separate
    distribution.  Installing such a source distribution satisfies
    requirements for both ``ZODB`` and ``transaction``.

    A distribution may also provide a "virtual" project name, which does
    not correspond to any separately-distributed project:  such a name
    might be used to indicate an abstract capability which could be supplied
    by one of multiple projects.  E.g., multiple projects might supply
    RDBMS bindings for use by a given ORM:  each project might declare
    that it provides ``ORM-bindings``, allowing other projects to depend
    only on having at most one of them installed.

    A version declaration may be supplied and must follow the rules described
    in :doc:`version-specifiers`. The distribution's version number will be implied
    if none is specified.

    This field may be followed by an environment marker after a semicolon.

    Examples::

        Provides-Dist: OtherProject
        Provides-Dist: AnotherProject==3.4
        Provides-Dist: virtual_package; python_version >= "3.4"

.. _core-metadata-obsoletes-dist:

Obsoletes-Dist（多次使用）
-----------------------------

**Obsoletes-Dist (multiple use)**

.. versionadded:: 1.2

.. tab:: 中文

    .. versionchanged:: 2.1
       字段格式规范已放宽，以接受流行发布工具使用的语法。

    每个条目包含一个字符串，描述一个 Distutils 项目的分发包，该分发包已被当前分发包取代，这意味着这两个项目不应同时安装。

    可以提供版本声明。版本号必须符合 :doc:`version-specifiers` 中描述的格式。

    此字段后面可以跟环境标记，使用分号分隔。

    此字段最常见的用途是表示项目名称的更改，例如，Gorgon 2.3 被 Torqued Python 1.0 所取代。当安装 Torqued Python 时，应删除 Gorgon 分发包。

    示例::

        Obsoletes-Dist: Gorgon
        Obsoletes-Dist: OtherProject (<3.0)
        Obsoletes-Dist: Foo; os_name == "posix"

.. tab:: 英文

    .. versionchanged:: 2.1
       The field format specification was relaxed to accept the syntax used by
       popular publishing tools.

    Each entry contains a string describing a distutils project's distribution
    which this distribution renders obsolete, meaning that the two projects
    should not be installed at the same time.

    Version declarations can be supplied.  Version numbers must be in the
    format specified in :doc:`version-specifiers`.

    This field may be followed by an environment marker after a semicolon.

    The most common use of this field will be in case a project name
    changes, e.g. Gorgon 2.3 gets subsumed into Torqued Python 1.0.
    When you install Torqued Python, the Gorgon distribution should be
    removed.

    Examples::

        Obsoletes-Dist: Gorgon
        Obsoletes-Dist: OtherProject (<3.0)
        Obsoletes-Dist: Foo; os_name == "posix"


弃用的字段
=================

**Deprecated Fields**

.. _home-page-optional:
.. _core-metadata-home-page:

主页
---------

**Home-page**

.. versionadded:: 1.0

.. tab:: 中文

    .. deprecated:: 1.2

        根据 :pep:`753`，请改用 :ref:`core-metadata-project-url` 。

    包含分发主页 URL 的字符串。

    示例::

        Home-page: http://www.example.com/~cschultz/bvote/

.. tab:: 英文

    .. deprecated:: 1.2

        Per :pep:`753`, use :ref:`core-metadata-project-url` instead.

    A string containing the URL for the distribution's home page.

    Example::

        Home-page: http://www.example.com/~cschultz/bvote/

.. _core-metadata-download-url:

下载-URL
------------

**Download-URL**

.. versionadded:: 1.1

.. tab:: 中文

    .. deprecated:: 1.2

        根据 :pep:`753`, 请改用 :ref:`core-metadata-project-url` .

    包含可从中下载此发行版的 URL 的字符串。（这意味着 URL 不能是 “``.../BeagleVote-latest.tgz``” 之类的内容，而必须是 “``.../BeagleVote-0.45.tgz``” 。）

.. tab:: 英文

    .. deprecated:: 1.2

        Per :pep:`753`, use :ref:`core-metadata-project-url` instead.

    A string containing the URL from which this version of the distribution
    can be downloaded.  (This means that the URL can't be something like
    "``.../BeagleVote-latest.tgz``", but instead must be
    "``.../BeagleVote-0.45.tgz``".)

需要
--------

**Requires**

.. versionadded:: 1.1

.. tab:: 中文

    .. deprecated:: 1.2
        替换为 ``Requires-Dist``

    每个条目包含一个字符串，描述该包所需的其他模块或包。

    要求字符串的格式与可以与 ``import`` 语句一起使用的模块或包名称相同，后面可选地跟着一个括号内的版本声明。

    版本声明是由条件运算符和版本号组成的系列，条件运算符之间用逗号分隔。条件运算符必须是 "<"、">"、"<="、">="、"==" 和 "!=" 之一。版本号必须采用 ``distutils.version.StrictVersion`` 类接受的格式：由两个或三个点分隔的数字组件组成，后面可以有一个可选的“预发布”标签，该标签由字母 'a' 或 'b' 和一个数字组成。示例版本号有 "1.0"、"2.3a2"、"1.3.99"。

    可以指定任意数量的条件运算符，例如，字符串 ">1.0, !=1.3.4, <2.0" 是一个合法的版本声明。

    所有以下都是有效的要求字符串："rfc822"、"zlib (>=1.1.4)"、"zope"。

    没有标准的字符串列表供选择；Python 社区可以自行选择标准。

    示例::

        Requires: re
        Requires: sys
        Requires: zlib
        Requires: xml.parsers.expat (>1.0)
        Requires: psycopg

.. tab:: 英文

    .. deprecated:: 1.2
       in favour of ``Requires-Dist``

    Each entry contains a string describing some other module or package required
    by this package.

    The format of a requirement string is identical to that of a module or package
    name usable with the ``import`` statement, optionally followed by a version
    declaration within parentheses.

    A version declaration is a series of conditional operators and version numbers,
    separated by commas. Conditional operators must be one of "<", ">"', "<=",
    ">=", "==", and "!=". Version numbers must be in the format accepted by the
    ``distutils.version.StrictVersion`` class: two or three dot-separated numeric
    components, with an optional "pre-release" tag on the end consisting of the
    letter 'a' or 'b' followed by a number. Example version numbers are "1.0",
    "2.3a2", "1.3.99",

    Any number of conditional operators can be specified, e.g. the string ">1.0,
    !=1.3.4, <2.0" is a legal version declaration.

    All of the following are possible requirement strings: "rfc822", "zlib
    (>=1.1.4)", "zope".

    There’s no canonical list of what strings should be used; the Python community
    is left to choose its own standards.

    Examples::

        Requires: re
        Requires: sys
        Requires: zlib
        Requires: xml.parsers.expat (>1.0)
        Requires: psycopg


提供
--------

**Provides**

.. versionadded:: 1.1

.. tab:: 中文

    .. deprecated:: 1.2
       替换为 ``Provides-Dist``

    每个条目包含一个字符串，描述一旦安装此包后将提供的包或模块。这些字符串应与要求字段中使用的字符串匹配。可以提供版本声明（不带比较运算符）；如果未指定版本号，则默认为该包的版本号。

    示例::

        Provides: xml
        Provides: xml.utils
        Provides: xml.utils.iso8601
        Provides: xml.dom
        Provides: xmltools (1.3)

.. tab:: 英文

    .. deprecated:: 1.2
       in favour of ``Provides-Dist``

    Each entry contains a string describing a package or module that will be
    provided by this package once it is installed. These strings should match the
    ones used in Requirements fields. A version declaration may be supplied
    (without a comparison operator); the package’s version number will be implied
    if none is specified.

    Examples::

        Provides: xml
        Provides: xml.utils
        Provides: xml.utils.iso8601
        Provides: xml.dom
        Provides: xmltools (1.3)


过时
---------

**Obsoletes**

.. versionadded:: 1.1

.. tab:: 中文

    .. deprecated:: 1.2
        替换为 ``Obsoletes-Dist``

    每个条目包含一个字符串，描述此包使其过时的包或模块，意味着这两个包不应同时安装。可以提供版本声明。

    此字段的最常见用途是在包名称发生变化时，例如：Gorgon 2.3 被 Torqued Python 1.0 所取代。当安装 Torqued Python 时，应该移除 Gorgon 包。

    示例::

        Obsoletes: Gorgon

.. tab:: 英文

    .. deprecated:: 1.2
        in favour of ``Obsoletes-Dist``

    Each entry contains a string describing a package or module that this package
    renders obsolete, meaning that the two packages should not be installed at the
    same time. Version declarations can be supplied.

    The most common use of this field will be in case a package name changes, e.g.
    Gorgon 2.3 gets subsumed into Torqued Python 1.0. When you install Torqued
    Python, the Gorgon package should be removed.

    Example::

        Obsoletes: Gorgon


历史
=======

**History**

.. tab:: 中文

    - 2001年3月：核心元数据1.0通过 :pep:`241` 被批准。
    - 2003年4月：核心元数据1.1通过 :pep:`314` 被批准。
    - 2010年2月：核心元数据1.2通过 :pep:`345` 被批准。
    - 2018年2月：核心元数据2.1通过 :pep:`566` 被批准。

      - 新增了 ``Description-Content-Type`` 和 ``Provides-Extra`` 字段。
      - 新增了将元数据转换为JSON的标准方法。
      - 限制了 ``Name`` 字段的语法。

    - 2020年10月：核心元数据2.2通过 :pep:`643` 被批准。

      - 新增了 ``Dynamic`` 字段。

    - 2022年3月：核心元数据2.3通过 :pep:`685` 被批准。

      - 限制了 ``extra`` 名称必须进行规范化。

    - 2024年8月：核心元数据2.4通过 :pep:`639` 被批准。

      - 新增了 ``License-Expression`` 字段。
      - 新增了 ``License-File`` 字段。

.. tab:: 英文

    - March 2001: Core metadata 1.0 was approved through :pep:`241`.
    - April 2003: Core metadata 1.1 was approved through :pep:`314`:
    - February 2010: Core metadata 1.2 was approved through :pep:`345`.
    - February 2018: Core metadata 2.1 was approved through :pep:`566`.

      - Added ``Description-Content-Type`` and ``Provides-Extra``.
      - Added canonical method for transforming metadata to JSON.
      - Restricted the grammar of the ``Name`` field.

    - October 2020: Core metadata 2.2 was approved through :pep:`643`.

        - Added the ``Dynamic`` field.

    - March 2022: Core metadata 2.3 was approved through :pep:`685`.

        - Restricted extra names to be normalized.

    - August 2024: Core metadata 2.4 was approved through :pep:`639`.

        - Added the ``License-Expression`` field.
        - Added the ``License-File`` field.

----

.. [1] reStructuredText markup: https://docutils.sourceforge.io/

.. _`Python Package Index`: https://pypi.org/

.. [2] RFC 822 Long Header Fields: :rfc:`822#section-3.1.1`
