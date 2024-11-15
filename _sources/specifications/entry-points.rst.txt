.. _entry-points:

==========================
入口点规范
==========================

**Entry points specification**

.. tab:: 中文

  *入口点* 是一种机制，用于已安装的分发包宣传其提供的组件，以供其他代码发现和使用。例如：

  - 分发包可以指定 ``console_scripts`` 入口点，每个入口点指向一个函数。当 *pip*（或其他支持 console_scripts 的安装程序）安装该分发包时，它将为每个入口点创建一个命令行包装器。
  - 应用程序可以使用入口点加载插件；例如，Pygments（一个语法高亮工具）可以使用来自单独安装包的额外词法分析器和样式。有关更多信息，请参见 :doc:`/guides/creating-and-discovering-plugins`。

  入口点文件格式最初是为了允许使用 setuptools 构建的包提供集成点元数据，这些元数据将在运行时通过 :py:mod:`importlib.metadata` 读取。现在，它作为 PyPA 互操作性规范定义，以允许除了 ``setuptools`` 之外的构建工具发布与 :py:mod:`importlib.metadata` 兼容的入口点元数据，并允许除了 :py:mod:`importlib.metadata` 之外的运行时库便捷地读取已发布的入口点元数据（可能采用不同的缓存和冲突解决策略）。

.. tab:: 英文

  *Entry points* are a mechanism for an installed distribution to advertise
  components it provides to be discovered and used by other code. For
  example:

  - Distributions can specify ``console_scripts`` entry points, each referring to
    a function. When *pip* (or another console_scripts aware installer) installs
    the distribution, it will create a command-line wrapper for each entry point.
  - Applications can use entry points to load plugins; e.g. Pygments (a syntax
    highlighting tool) can use additional lexers and styles from separately
    installed packages. For more about this, see
    :doc:`/guides/creating-and-discovering-plugins`.

  The entry point file format was originally developed to allow packages built
  with setuptools to provide integration point metadata that would be read at
  runtime with :py:mod:`importlib.metadata`. It is now defined as a PyPA interoperability
  specification in order to allow build tools other than ``setuptools`` to publish
  :py:mod:`importlib.metadata` compatible entry point metadata, and runtime libraries other
  than :py:mod:`importlib.metadata` to portably read published entry point metadata
  (potentially with different caching and conflict resolution strategies).

数据模型
==========

**Data model**

.. tab:: 中文

  从概念上讲，入口点由三个必需的属性定义：

  - **group**（组）：入口点所属的组指示它提供的对象类型。例如，``console_scripts`` 组是用于指向可以作为命令使用的函数的入口点，而 ``pygments.styles`` 是定义 pygments 样式的类的组。消费者通常定义预期的接口。为了避免冲突，定义新组的消费者应该使用以消费者项目所拥有的 PyPI 名称开头，并接上 ``.`` 的名称。组名必须是由字母、数字和下划线组成的一个或多个组，通过点（`.`）分隔（正则表达式 ``^\w+(\.\w+)*$``）。

  - **name**（名称）：标识该入口点在其组中的名称。其精确定义由消费者决定。例如，对于控制台脚本，入口点的名称就是启动它的命令。在一个分发包中，入口点名称应唯一。如果不同的分发包提供相同的名称，消费者决定如何处理这种冲突。名称可以包含除 ``=`` 之外的任何字符，但不能以任何空白字符开始或结束，也不能以 ``[`` 开头。对于新的入口点，推荐仅使用字母、数字、下划线、点和破折号（正则表达式 ``[\w.-]+``）。

  - **object reference**（对象引用）：指向一个 Python 对象。它的形式为 ``importable.module`` 或 ``importable.module:object.attr``。由点和冒号分隔的每个部分都是有效的 Python 标识符。它的查找方式如下所示::

      import importlib
      modname, qualname_separator, qualname = object_ref.partition(':')
      obj = importlib.import_module(modname)
      if qualname_separator:
          for attr in qualname.split('.'):
              obj = getattr(obj, attr)

  .. note::
    一些工具将这种对象引用本身称为 '入口点'，以便于表述，特别是当它指向一个启动程序的函数时。

  此外，还有一个可选属性：**extras**（额外特性）是一个字符串集合，标识提供入口点的分发包的可选功能。如果指定了这些，入口点就需要这些 'extras' 的依赖项。请参见元数据字段 :ref:`metadata_provides_extra`。

  不再推荐为入口点使用 extras。消费者应支持从现有分发包中解析它们，但可以忽略它们。新的发布工具不需要支持指定 extras。处理 extras 的功能与 setuptools 管理 'egg' 包的模型相关，但像 pip 和 virtualenv 这样的新工具使用的是不同的模型。

.. tab:: 英文

  Conceptually, an entry point is defined by three required properties:

  - The **group** that an entry point belongs to indicates what sort of object it
    provides. For instance, the group ``console_scripts`` is for entry points
    referring to functions which can be used as a command, while
    ``pygments.styles`` is the group for classes defining pygments styles.
    The consumer typically defines the expected interface. To avoid clashes,
    consumers defining a new group should use names starting with a PyPI name
    owned by the consumer project, followed by ``.``. Group names must be one or
    more groups of letters, numbers and underscores, separated by dots (regex
    ``^\w+(\.\w+)*$``).

  - The **name** identifies this entry point within its group. The precise meaning
    of this is up to the consumer. For console scripts, the name of the entry point
    is the command that will be used to launch it. Within a distribution, entry
    point names should be unique. If different distributions provide the same
    name, the consumer decides how to handle such conflicts. The name may contain
    any characters except ``=``, but it cannot start or end with any whitespace
    character, or start with ``[``. For new entry points, it is recommended to
    use only letters, numbers, underscores, dots and dashes (regex ``[\w.-]+``).

  - The **object reference** points to a Python object. It is either in the form
    ``importable.module``, or ``importable.module:object.attr``. Each of the parts
    delimited by dots and the colon is a valid Python identifier.
    It is intended to be looked up like this::

      import importlib
      modname, qualname_separator, qualname = object_ref.partition(':')
      obj = importlib.import_module(modname)
      if qualname_separator:
          for attr in qualname.split('.'):
              obj = getattr(obj, attr)

  .. note::
    Some tools call this kind of object reference by itself an 'entry point', for
    want of a better term, especially where it points to a function to launch a
    program.

  There is also an optional property: the **extras** are a set of strings
  identifying optional features of the distribution providing the entry point.
  If these are specified, the entry point requires the dependencies of those
  'extras'. See the metadata field :ref:`metadata_provides_extra`.

  Using extras for an entry point is no longer recommended. Consumers should
  support parsing them from existing distributions, but may then ignore them.
  New publishing tools need not support specifying extras. The functionality of
  handling extras was tied to setuptools' model of managing 'egg' packages, but
  newer tools such as pip and virtualenv use a different model.

文件格式
===========

**File format**

.. tab:: 中文

  入口点在一个名为 :file:`entry_points.txt` 的文件中定义，该文件位于分发包的 :file:`*.dist-info` 目录下。这个目录在 :ref:`recording-installed-packages` 中描述了已安装的分发包，在 :ref:`binary-distribution-format` 中描述了 wheel 包。该文件使用 UTF-8 字符编码。

  文件内容采用 INI 格式，由 Python 的 :mod:`configparser` 模块读取。然而，configparser 默认将名称视为不区分大小写，而入口点名称是区分大小写的。可以通过如下方式创建一个区分大小写的配置解析器::

      import configparser

      class CaseSensitiveConfigParser(configparser.ConfigParser):
          optionxform = staticmethod(str)

  入口点文件必须始终使用 ``=`` 来分隔名称和值（而 configparser 也允许使用 ``:``）。

  配置文件的各个部分表示入口点组，名称表示入口点名称，值则编码了对象引用和可选的 extras。如果使用 extras，它们将以逗号分隔的列表形式出现在方括号内。

  在值中，读取器必须接受并忽略冒号前后、多余的空格（包括多个连续的空格）、对象引用和左方括号之间的空格、额外特性名称与方括号及冒号之间的空格，以及右方括号后的空格。extras 的语法已在 :pep:`508`（作为 ``extras``）中正式规定，而对值的限制在 :pep:`685` 中指定。

  对于编写文件的工具，建议仅在对象引用和左方括号之间插入一个空格。

  例如：

  .. code-block:: ini

      [console_scripts]
      foo = foomod:main
      # 一个依赖于 extras 的例子：
      foobar = foomod:main_bar [bar,baz]

      # pytest 插件引用一个模块，因此没有 ':obj'
      [pytest11]
      nbval = nbval.plugin

.. tab:: 英文

  Entry points are defined in a file called :file:`entry_points.txt` in the
  :file:`*.dist-info` directory of the distribution. This is the directory
  described in :ref:`recording-installed-packages` for installed distributions,
  and in :ref:`binary-distribution-format` for wheels.
  The file uses the UTF-8 character encoding.

  The file contents are in INI format, as read by Python's :mod:`configparser`
  module. However, configparser treats names as case-insensitive by default,
  whereas entry point names are case sensitive. A case-sensitive config parser
  can be made like this::

      import configparser

      class CaseSensitiveConfigParser(configparser.ConfigParser):
          optionxform = staticmethod(str)

  The entry points file must always use ``=`` to delimit names from values
  (whereas configparser also allows using ``:``).

  The sections of the config file represent entry point groups, the names are
  names, and the values encode both the object reference and the optional extras.
  If extras are used, they are a comma-separated list inside square brackets.

  Within a value, readers must accept and ignore spaces (including multiple
  consecutive spaces) before or after the colon, between the object reference and
  the left square bracket, between the extra names and the square brackets and
  colons delimiting them, and after the right square bracket. The syntax for
  extras is formally specified as part of :pep:`508` (as ``extras``) and
  restrictions on values specified in :pep:`685`.
  For tools writing the file, it is recommended only to insert a space between the
  object reference and the left square bracket.

  For example:

  .. code-block:: ini

      [console_scripts]
      foo = foomod:main
      # One which depends on extras:
      foobar = foomod:main_bar [bar,baz]

      # pytest plugins refer to a module, so there is no ':obj'
      [pytest11]
      nbval = nbval.plugin

用于脚本
===============

**Use for scripts**

.. tab:: 中文

  在包装中，两个入口点组具有特殊意义：``console_scripts`` 和 ``gui_scripts``。在这两个组中，入口点的名称应当在包安装后能够作为系统 shell 中的命令使用。对象引用指向一个函数，当运行该命令时，该函数将被调用，并且该函数无需参数。该函数可以返回一个整数作为进程的退出代码，返回 ``None`` 相当于返回 ``0``。

  例如，入口点 ``mycmd = mymod:main`` 将创建一个命令 ``mycmd``，该命令启动一个如下脚本::

      import sys
      from mymod import main
      sys.exit(main())

  ``console_scripts`` 和 ``gui_scripts`` 之间的区别仅在 Windows 系统中有效。``console_scripts`` 会被包装成一个控制台可执行文件，因此它们会附加到一个控制台，并可以使用 :py:data:`sys.stdin`、:py:data:`sys.stdout` 和 :py:data:`sys.stderr` 进行输入和输出。``gui_scripts`` 会被包装成一个图形界面可执行文件，因此它们可以在没有控制台的情况下启动，但除非应用代码进行重定向，否则无法使用标准流。在其他平台上没有这样的区分。

  安装工具应该在安装方案的脚本目录中为 ``console_scripts`` 和 ``gui_scripts`` 设置包装器。它们不负责将该目录添加到定义命令行工具查找位置的 ``PATH`` 环境变量中。

  由于文件是从名称创建的，并且一些文件系统不区分大小写，包应该避免在这些组中使用仅在大小写上有所不同的名称。当名称仅在大小写上有所差异时，安装工具的行为是未定义的。

.. tab:: 英文

  Two groups of entry points have special significance in packaging:
  ``console_scripts`` and ``gui_scripts``. In both groups, the name of the entry
  point should be usable as a command in a system shell after the package is
  installed. The object reference points to a function which will be called with
  no arguments when this command is run. The function may return an integer to be
  used as a process exit code, and returning ``None`` is equivalent to returning
  ``0``.

  For instance, the entry point ``mycmd = mymod:main`` would create a command
  ``mycmd`` launching a script like this::

      import sys
      from mymod import main
      sys.exit(main())

  The difference between ``console_scripts`` and ``gui_scripts`` only affects
  Windows systems. ``console_scripts`` are wrapped in a console executable,
  so they are attached to a console and can use :py:data:`sys.stdin`,
  :py:data:`sys.stdout` and :py:data:`sys.stderr` for input and output.
  ``gui_scripts`` are wrapped in a GUI executable, so they can be started without
  a console, but cannot use standard streams unless application code redirects them.
  Other platforms do not have the same distinction.

  Install tools are expected to set up wrappers for both ``console_scripts`` and
  ``gui_scripts`` in the scripts directory of the install scheme. They are not
  responsible for putting this directory in the ``PATH`` environment variable
  which defines where command-line tools are found.

  As files are created from the names, and some filesystems are case-insensitive,
  packages should avoid using names in these groups which differ only in case.
  The behaviour of install tools when names differ only in case is undefined.


历史记录
=======

**History**

.. tab:: 中文

  - 2017年10月：该规范的编写旨在规范化 setuptools 中现有的入口点功能 (discussion_)。

.. tab:: 英文

  - October 2017: This specification was written to formalize the existing entry points feature of setuptools (discussion_).



.. _discussion: https://mail.python.org/pipermail/distutils-sig/2017-October/031585.html
