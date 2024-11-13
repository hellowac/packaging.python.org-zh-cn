.. _creating-command-line-tools:

=========================================
创建和打包命令行工具
=========================================

**Creating and packaging command-line tools**

.. tab:: 中文

    本指南将引导你创建和打包一个独立的命令行应用程序，该应用程序可以使用 :ref:`pipx` 安装。pipx 是一个工具，用于创建和管理 :term:`Python 虚拟环境 <Virtual Environment>`，并暴露包的可执行脚本（以及可用的手册页）供命令行使用。

.. tab:: 英文

    This guide will walk you through creating and packaging a standalone command-line application
    that can be installed with :ref:`pipx`, a tool creating and managing :term:`Python Virtual Environments <Virtual Environment>`
    and exposing the executable scripts of packages (and available manual pages) for use on the command-line.

创建包
====================

**Creating the package**

.. tab:: 中文

    首先，为 :term:`项目 <Project>` 创建一个源代码目录结构。为了举例说明，我们将构建一个简单的工具，该工具根据命令行中给出的参数输出一个问候语（一个字符串）。

    .. todo:: 在另一个指南或讨论中建议 Python 包的最佳结构，并在此处提供链接。

    该项目将遵循 :ref:`src-layout <src-layout-vs-flat-layout>` 结构，最终将呈现以下文件树，顶级文件夹和包名为 ``greetings``：

    ::

        .
        ├── pyproject.toml
        └── src
            └── greetings
                ├── cli.py
                ├── greet.py
                ├── __init__.py
                └── __main__.py

    负责工具功能的实际代码将存储在 :file:`greet.py` 文件中，文件名与主模块相同：

    .. code-block:: python

        import typer
        from typing_extensions import Annotated


        def greet(
            name: Annotated[str, typer.Argument(help="The (last, if --gender is given) name of the person to greet")] = "",
            gender: Annotated[str, typer.Option(help="The gender of the person to greet")] = "",
            knight: Annotated[bool, typer.Option(help="Whether the person is a knight")] = False,
            count: Annotated[int, typer.Option(help="Number of times to greet the person")] = 1
        ):
            greeting = "Greetings, dear "
            masculine = gender == "masculine"
            feminine = gender == "feminine"
            if gender or knight:
                salutation = ""
                if knight:
                    salutation = "Sir "
                elif masculine:
                    salutation = "Mr. "
                elif feminine:
                    salutation = "Ms. "
                greeting += salutation
                if name:
                    greeting += f"{name}!"
                else:
                    pronoun = "her" if feminine else "his" if masculine or knight else "its"
                    greeting += f"what's-{pronoun}-name"
            else:
                if name:
                    greeting += f"{name}!"
                elif not gender:
                    greeting += "friend!"
            for i in range(0, count):
                print(greeting)

    上述函数接受多个关键字参数，用于确定如何构建输出的问候语。接下来，构建命令行接口以提供这些功能，这将在 :file:`cli.py` 中完成：

    .. code-block:: python

        import typer

        from .greet import greet


        app = typer.Typer()
        app.command()(greet)


        if __name__ == "__main__":
            app()

    命令行接口是使用 typer_ 构建的，它是一个基于 Python 类型提示的易用 CLI 解析器。它提供了自动补全和漂亮的命令行帮助。另一种选择是 :py:mod:`argparse`，这是 Python 标准库中包含的命令行解析器，适用于大多数需求，但通常需要在 ``cli.py`` 中编写大量代码以确保正常工作。另一个选择是 docopt_，它使得仅基于文档字符串创建 CLI 接口成为可能；高级用户可以使用 click_ （``typer`` 就是基于 click 构建的）。

    现在，添加一个空的 :file:`__init__.py` 文件，以将项目定义为常规的 :term:`导入包 <Import Package>`。

    文件 :file:`__main__.py` 标志着通过 :mod:`runpy` 运行应用程序时的主要入口点（即 ``python -m greetings``，在平面布局中可以立即使用，但需要安装具有 src 布局的包），因此在这里初始化命令行接口：

    .. code-block:: python

        if __name__ == "__main__":
            from greetings.cli import app
            app()

    .. note::

        为了支持直接从 :term:`源代码目录 <Project Source Tree>` 调用命令行接口，
        即作为 ``python src/greetings``，可以在此文件中加入某些 hack；更多信息请阅读
        :ref:`running-cli-from-source-src-layout` 。

.. tab:: 英文

    First of all, create a source tree for the :term:`project <Project>`. For the sake of an example, we'll
    build a simple tool outputting a greeting (a string) for a person based on arguments given on the command-line.

    .. todo:: Advise on the optimal structure of a Python package in another guide or discussion and link to it here.

    This project will adhere to :ref:`src-layout <src-layout-vs-flat-layout>` and in the end be alike this file tree,
    with the top-level folder and package name ``greetings``:

    ::

        .
        ├── pyproject.toml
        └── src
            └── greetings
                ├── cli.py
                ├── greet.py
                ├── __init__.py
                └── __main__.py

    The actual code responsible for the tool's functionality will be stored in the file :file:`greet.py`,
    named after the main module:

    .. code-block:: python

        import typer
        from typing_extensions import Annotated


        def greet(
            name: Annotated[str, typer.Argument(help="The (last, if --gender is given) name of the person to greet")] = "",
            gender: Annotated[str, typer.Option(help="The gender of the person to greet")] = "",
            knight: Annotated[bool, typer.Option(help="Whether the person is a knight")] = False,
            count: Annotated[int, typer.Option(help="Number of times to greet the person")] = 1
        ):
            greeting = "Greetings, dear "
            masculine = gender == "masculine"
            feminine = gender == "feminine"
            if gender or knight:
                salutation = ""
                if knight:
                    salutation = "Sir "
                elif masculine:
                    salutation = "Mr. "
                elif feminine:
                    salutation = "Ms. "
                greeting += salutation
                if name:
                    greeting += f"{name}!"
                else:
                    pronoun = "her" if feminine else "his" if masculine or knight else "its"
                    greeting += f"what's-{pronoun}-name"
            else:
                if name:
                    greeting += f"{name}!"
                elif not gender:
                    greeting += "friend!"
            for i in range(0, count):
                print(greeting)

    The above function receives several keyword arguments that determine how the greeting to output is constructed.
    Now, construct the command-line interface to provision it with the same, which is done
    in :file:`cli.py`:

    .. code-block:: python

        import typer

        from .greet import greet


        app = typer.Typer()
        app.command()(greet)


        if __name__ == "__main__":
            app()

    The command-line interface is built with typer_, an easy-to-use CLI parser based on Python type hints. It provides
    auto-completion and nicely styled command-line help out of the box. Another option would be :py:mod:`argparse`,
    a command-line parser which is included in Python's standard library. It is sufficient for most needs, but requires
    a lot of code, usually in ``cli.py``, to function properly. Alternatively, docopt_ makes it possible to create CLI
    interfaces based solely on docstrings; advanced users are encouraged to make use of click_ (on which ``typer`` is based).

    Now, add an empty :file:`__init__.py` file, to define the project as a regular :term:`import package <Import Package>`.

    The file :file:`__main__.py` marks the main entry point for the application when running it via :mod:`runpy`
    (i.e. ``python -m greetings``, which works immediately with flat layout, but requires installation of the package with src layout),
    so initizalize the command-line interface here:

    .. code-block:: python

        if __name__ == "__main__":
            from greetings.cli import app
            app()

    .. note::

        In order to enable calling the command-line interface directly from the :term:`source tree <Project Source Tree>`,
        i.e. as ``python src/greetings``, a certain hack could be placed in this file; read more at
        :ref:`running-cli-from-source-src-layout`.


``pyproject.toml``
------------------

.. tab:: 中文

    项目的 :term:`元数据 <Pyproject Metadata>` 放在 :term:`pyproject.toml` 文件中。可以按照 :ref:`writing-pyproject-toml` 中的说明填写 :term:`pyproject 元数据键 <Pyproject Metadata Key>` 和 ``[build-system]`` 表格，并添加对 ``typer`` 的依赖（本教程使用版本 *0.12.3*）。

    为了使该项目被识别为一个命令行工具，还需要添加一个 ``console_scripts`` :ref:`入口点 <entry-points>` （参见 :ref:`console_scripts`），作为一个 :term:`子键 <Pyproject Metadata Subkey>` :

    .. code-block:: toml

        [project.scripts]
        greet = "greetings.cli:app"

    现在，项目的源代码目录已准备好转化为一个 :term:`分发包 <Distribution Package>`，
    使其能够被安装。

.. tab:: 英文

    The project's :term:`metadata <Pyproject Metadata>` is placed in :term:`pyproject.toml`. The :term:`pyproject metadata keys <Pyproject Metadata Key>` and the ``[build-system]`` table may be filled in as described in :ref:`writing-pyproject-toml`, adding a dependency
    on ``typer`` (this tutorial uses version *0.12.3*).

    For the project to be recognised as a command-line tool, additionally a ``console_scripts`` :ref:`entry point <entry-points>` (see :ref:`console_scripts`) needs to be added as a :term:`subkey <Pyproject Metadata Subkey>`:

    .. code-block:: toml

        [project.scripts]
        greet = "greetings.cli:app"

    Now, the project's source tree is ready to be transformed into a :term:`distribution package <Distribution Package>`,
    which makes it installable.


使用 ``pipx`` 安装包
====================================

**Installing the package with** ``pipx``

.. tab:: 中文

    按照 :ref:`installing-stand-alone-command-line-tools` 中的说明安装 ``pipx`` 后，安装你的项目：

    .. code-block:: console

        $ cd path/to/greetings/
        $ pipx install .

    这将暴露我们定义的作为入口点的可执行脚本，并使得 ``greet`` 命令可用。我们来测试一下：

    .. code-block:: console

        $ greet --knight Lancelot
        Greetings, dear Sir Lancelot!
        $ greet --gender feminine Parks
        Greetings, dear Ms. Parks!
        $ greet --gender masculine
        Greetings, dear Mr. what's-his-name!

    由于这个示例使用了 ``typer``，你现在也可以通过添加 ``--help`` 选项来查看程序的使用概览，或者通过 ``--install-completion`` 选项配置自动补全。

    如果你只想运行程序而不永久安装，可以使用 ``pipx run``，它将为该程序创建一个临时（但会缓存的）虚拟环境：

    .. code-block:: console

        $ pipx run --spec . greet --knight

    然而，这种语法有点不方便；由于我们上面定义的入口点名称与包名不匹配，因此我们需要明确指定要运行的可执行脚本（即使目前只有一个脚本）。

    不过，解决这个问题有一个更实用的办法，通过为 ``pipx run`` 定义特定的入口点。可以在 :file:`pyproject.toml` 中如下定义：

    .. code-block:: toml

        [project.entry-points."pipx.run"]
        greetings = "greetings.cli:app"

    由于这个入口点（*必须* 与包名匹配），``pipx`` 将把可执行脚本作为默认脚本来运行，这样就可以执行以下命令：

    .. code-block:: console

        $ pipx run . --knight

.. tab:: 英文

    After installing ``pipx`` as described in :ref:`installing-stand-alone-command-line-tools`, install your project:

    .. code-block:: console

        $ cd path/to/greetings/
        $ pipx install .

    This will expose the executable script we defined as an entry point and make the command ``greet`` available.
    Let's test it:

    .. code-block:: console

        $ greet --knight Lancelot
        Greetings, dear Sir Lancelot!
        $ greet --gender feminine Parks
        Greetings, dear Ms. Parks!
        $ greet --gender masculine
        Greetings, dear Mr. what's-his-name!

    Since this example uses ``typer``, you could now also get an overview of the program's usage by calling it with
    the ``--help`` option, or configure completions via the ``--install-completion`` option.

    To just run the program without installing it permanently, use ``pipx run``, which will create a temporary
    (but cached) virtual environment for it:

    .. code-block:: console

        $ pipx run --spec . greet --knight

    This syntax is a bit unpractical, however; as the name of the entry point we defined above does not match the package name,
    we need to state explicitly which executable script to run (even though there is only on in existence).

    There is, however, a more practical solution to this problem, in the form of an entry point specific to ``pipx run``.
    The same can be defined as follows in :file:`pyproject.toml`:

    .. code-block:: toml

        [project.entry-points."pipx.run"]
        greetings = "greetings.cli:app"


    Thanks to this entry point (which *must* match the package name), ``pipx`` will pick up the executable script as the
    default one and run it, which makes this command possible:

    .. code-block:: console

        $ pipx run . --knight

结论
==========

**Conclusion**

.. tab:: 中文

    到现在为止，你已经知道如何打包一个用 Python 编写的命令行应用程序。下一步可以是将你的包发布出去，即将其上传到 :term:`包索引 <Package Index>`，最常见的是 :term:`PyPI <Python Package Index (PyPI)>`。为此，按照 :ref:`Packaging your project` 中的说明进行操作。完成后，别忘了 :ref:`做一些研究 <analyzing-pypi-package-downloads>`，了解你的包是如何被接受的！

.. tab:: 英文

    You know by now how to package a command-line application written in Python. A further step could be to distribute you package,
    meaning uploading it to a :term:`package index <Package Index>`, most commonly :term:`PyPI <Python Package Index (PyPI)>`. To do that, follow the instructions at :ref:`Packaging your project`. And once you're done, don't forget to :ref:`do some research <analyzing-pypi-package-downloads>` on how your package is received!

.. _click: https://click.palletsprojects.com/
.. _docopt: https://docopt.readthedocs.io/en/latest/
.. _typer: https://typer.tiangolo.com/
