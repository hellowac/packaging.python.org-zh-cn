.. |PyPUG| replace:: Python Packaging User Guide

************************
为本指南做出贡献
************************

**Contribute to this guide**

.. tab:: 中文

  |PyPUG| 欢迎贡献者！有很多方式可以帮助我们，包括：

  * 阅读指南并提供反馈
  * 审核新的贡献
  * 修订现有内容
  * 编写新内容
  * 翻译指南

  |PyPUG| 的大部分工作都在 `项目的 GitHub 仓库`__ 上进行。要开始，请查看 `开放问题`__ 和 `拉取请求`__ 列表。如果你计划编写或编辑指南，请阅读 :ref:`style guide <contributing_style_guide>`。

  .. __: https://github.com/pypa/packaging.python.org/
  .. __: https://github.com/pypa/packaging.python.org/issues
  .. __: https://github.com/pypa/packaging.python.org/pulls

  通过为 |PyPUG| 做贡献，你需要遵循 PSF 的 `行为规范`__。

  .. __: https://github.com/pypa/.github/blob/main/CODE_OF_CONDUCT.md

.. tab:: 英文

  The |PyPUG| welcomes contributors! There are lots of ways to help out,
  including:

  * Reading the guide and giving feedback
  * Reviewing new contributions
  * Revising existing content
  * Writing new content
  * Translate the guide

  Most of the work on the |PyPUG| takes place on the
  `project's GitHub repository`__. To get started, check out the list of
  `open issues`__ and `pull requests`__. If you're planning to write or edit
  the guide, please read the :ref:`style guide <contributing_style_guide>`.

  .. __: https://github.com/pypa/packaging.python.org/
  .. __: https://github.com/pypa/packaging.python.org/issues
  .. __: https://github.com/pypa/packaging.python.org/pulls

  By contributing to the |PyPUG|, you're expected to follow the PSF's
  `Code of Conduct`__.

  .. __: https://github.com/pypa/.github/blob/main/CODE_OF_CONDUCT.md


文档类型
===================

**Documentation types**

.. tab:: 中文

  本项目由四种不同类型的文档组成，每种类型有其特定的目的。该项目旨在遵循 `Diátaxis process`_，以创建高质量的文档。在向项目提议新内容时，请选择适当的文档类型。

.. tab:: 英文

  This project consists of four distinct documentation types with specific
  purposes. The project aspires to follow the `Diátaxis process`_
  for creating quality documentation. When proposing new additions to the project please pick the
  appropriate documentation type.

.. _Diátaxis process: https://diataxis.fr/

教程
---------

**Tutorials**

.. tab:: 中文

  教程专注于通过完成一个目标来教授读者新概念。它们是有明确观点的逐步指南，不包含多余的警告或信息。 `示例教程风格文档 <example tutorial-style document_>`_ 。

.. tab:: 英文

  Tutorials are focused on teaching the reader new concepts by accomplishing a
  goal. They are opinionated step-by-step guides. They do not include extraneous
  warnings or information. `example tutorial-style document`_.

.. _example tutorial-style document: https://docs.djangoproject.com/en/dev/intro/

指南
------

**Guides**

.. tab:: 中文

  指南专注于完成特定任务，并可以假设读者具备一定的前提知识。这些内容类似于教程，但具有更窄和明确的焦点，并且可以根据需要提供许多警告和额外信息。它们还可以讨论完成任务的多种方法。:doc:`示例指南风格文档 <guides/packaging-namespace-packages>`。

.. tab:: 英文

  Guides are focused on accomplishing a specific task and can assume some level of
  pre-requisite knowledge. These are similar to tutorials, but have a narrow and
  clear focus and can provide lots of caveats and additional information as
  needed. They may also discuss multiple approaches to accomplishing the task.
  :doc:`example guide-style document <guides/packaging-namespace-packages>`.

讨论
-----------

**Discussions**

.. tab:: 中文

  讨论专注于理解和信息。这些内容探讨特定话题，而没有明确的目标。:doc:`示例讨论风格文档 <discussions/install-requires-vs-requirements>`。

.. tab:: 英文

  Discussions are focused on understanding and information. These explore a
  specific topic without a specific goal in mind. :doc:`example discussion-style
  document <discussions/install-requires-vs-requirements>`.

规范
--------------

**Specifications**

.. tab:: 中文

  规范是参考文档，专注于全面记录包装工具之间约定的互操作接口。:doc:`示例规范风格文档 <specifications/core-metadata>`。

.. tab:: 英文

  Specifications are reference documentation focused on comprehensively documenting
  an agreed-upon interface for interoperability between packaging tools.
  :doc:`example specification-style document <specifications/core-metadata>`.


翻译
============

**Translations**

.. tab:: 中文

  我们使用 `Weblate`_ 来管理该项目的翻译工作。  
  请访问 `packaging.python.org`_ 项目在 Weblate 上进行贡献。

  如果在翻译过程中遇到问题，请在 `GitHub`_ 上提出问题。

  .. 提示::

    本项目的所有翻译应遵循 `reStructuredText syntax`_。

.. tab:: 英文

  We use `Weblate`_ to manage translations of this project.
  Please visit the `packaging.python.org`_ project on Weblate to contribute.

  If you are experiencing issues while you are working on translations,
  please open an issue on `GitHub`_.

  .. tip::

    Any translations of this project should follow `reStructuredText syntax`_.

.. _Weblate: https://weblate.org/
.. _packaging.python.org: https://hosted.weblate.org/projects/pypa/packaging-python-org/
.. _GitHub: https://github.com/pypa/packaging.python.org/issues
.. _reStructuredText syntax: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html

添加语言
-----------------

**Adding a language**

.. tab:: 中文

  如果您的语言没有列在 `packaging.python.org`_ 上，请点击语言列表底部的按钮 :guilabel:`Start new translation`，然后添加您希望翻译的语言。

.. tab:: 英文

  If your language is not listed on `packaging.python.org`_, click the button
  :guilabel:`Start new translation` at the bottom of the language list and add
  the language you want to translate.

遵循 reStructuredText 语法
---------------------------------

**Following reStructuredText syntax**

.. tab:: 中文

  如果您不熟悉 reStructuredText (RST) 语法，请在 Weblate 上翻译之前阅读 `此指南 <this guide_>`_。

  **不要直接翻译引用中的文本**

  在翻译引用中的文本时，请不要直接翻译。

  | 错误: 直接翻译以下文本：

  .. code-block:: rst

      `some ref`_ -> `TRANSLATED TEXT HERE`_

  | 正确: 使用您自己的语言翻译以下文本，并保留原始引用：

  .. code-block:: rst

      `some ref`_ -> `TRANSLATED TEXT HERE <some ref>`_

.. tab:: 英文

  If you are not familiar with reStructuredText (RST) syntax, please read `this guide`_
  before translating on Weblate.

  **Do not translate the text in reference directly**

  When translating the text in reference, please do not translate them directly.

  | Wrong: Translate the following text directly:

  .. code-block:: rst

      `some ref`_ -> `TRANSLATED TEXT HERE`_

  | Right: Translate the following text with your own language and add the original reference:

  .. code-block:: rst

      `some ref`_ -> `TRANSLATED TEXT HERE <some ref>`_

.. _this guide: https://docutils.sourceforge.io/docs/user/rst/quickref.html

本地构建指南
==========================

**Building the guide locally**

.. tab:: 中文

  虽然贡献不是强制性的，但构建本指南的本地版本可能会对测试您的更改有所帮助。为了在本地构建本指南，您需要：

  1. :doc:`Nox <nox:index>`。您可以使用 ``pip`` 安装或升级 nox:

     .. code-block:: bash

        python -m pip install --user nox

  2. Python 3.11。我们的构建脚本通常仅在 Python 3.11 上进行测试。
     请参阅 :doc:`《Python 安装指南》 <python-guide:starting/installation>`
     以便在您的操作系统上安装 Python 3.11。

     要构建本指南，请在项目根文件夹中运行以下 shell 命令：

     .. code-block:: bash

        nox -s build

  在该过程完成后，您可以在 ``./build/html`` 目录中找到 HTML 输出文件。您可以打开 ``index.html`` 文件在 Web 浏览器中查看该指南，但建议通过 HTTP 服务器来提供该指南。

  您可以通过以下命令构建并通过 HTTP 服务器提供该指南：

  .. code-block:: bash

    nox -s preview

  该指南将在 http://localhost:8000 通过浏览器访问。

.. tab:: 英文

  Though not required to contribute, it may be useful to build this guide locally
  in order to test your changes. In order to build this guide locally, you'll
  need:

  1. :doc:`Nox <nox:index>`. You can install or upgrade
    nox using ``pip``:

    .. code-block:: bash

        python -m pip install --user nox

  2. Python 3.11. Our build scripts are usually tested with Python 3.11 only.
     See the :doc:`Hitchhiker's Guide to Python installation instructions <python-guide:starting/installation>`
     to install Python 3.11 on your operating system.

  To build the guide, run the following shell command in the project's root folder:

  .. code-block:: bash

    nox -s build

  After the process has completed you can find the HTML output in the
  ``./build/html`` directory. You can open the ``index.html`` file to view the
  guide in web browser, but it's recommended to serve the guide using an HTTP
  server.

  You can build the guide and serve it via an HTTP server using the following
  command:

  .. code-block:: bash

    nox -s preview

  The guide will be browsable via http://localhost:8000.


指南的部署位置
===========================

**Where the guide is deployed**

.. tab:: 中文

  该指南通过 ReadTheDocs 部署，配置文件位于 https://readthedocs.org/projects/python-packaging-user-guide/。它通过自定义域名进行托管，并由 Fast.ly 提供前端支持。

.. tab:: 英文

  The guide is deployed via ReadTheDocs and the configuration lives at https://readthedocs.org/projects/python-packaging-user-guide/. It's served from a custom domain and fronted by Fast.ly.


.. _contributing_style_guide:

风格指南
===========

**Style guide**

.. tab:: 中文

  这个风格指南提供了有关如何编写 |PyPUG| 的建议。在开始写作之前，请先阅读它。通过遵循该风格指南，您的贡献将有助于构建一个一致的整体，并使您的贡献更容易被项目接受。

.. tab:: 英文

  This style guide has recommendations for how you should write the |PyPUG|.
  Before you start writing, please review it. By following the style guide, your
  contributions will help add to a cohesive whole and make it easier for your
  contributions to be accepted into the project.


目的
-------

**Purpose**

.. tab:: 中文

  |PyPUG| 的目的是成为使用当前工具打包、发布和安装 Python 项目的权威资源。

.. tab:: 英文

  The purpose of the |PyPUG| is to be the authoritative resource on how to
  package, publish, and install Python projects using current tools.


范围
-----

**Scope**

.. tab:: 中文

  本指南旨在通过准确和专注的建议来回答问题并解决问题。

  本指南并不旨在涵盖所有内容，也不打算替代各个项目的文档。例如，pip 有几十个命令、选项和设置。pip 的文档详细描述了每一个，而本指南只描述了完成本指南中具体任务所需的 pip 部分。

.. tab:: 英文

  The guide is meant to answer questions and solve problems with accurate and
  focused recommendations.

  The guide isn't meant to be comprehensive and it's not meant to replace
  individual projects' documentation. For example, pip has dozens of commands,
  options, and settings. The pip documentation describes each of them in detail,
  while this guide describes only the parts of pip that are needed to complete the
  specific tasks described in this guide.


受众
--------

**Audience**

.. tab:: 中文

  本指南的受众是所有使用 Python 和包的用户。

  不要忘记，Python 社区是庞大且包容的。读者可能与您在年龄、性别、教育、文化等方面有所不同，但他们同样值得学习如何进行打包。

  特别需要记住的是，并非所有使用 Python 的人都视自己为程序员。本指南的受众不仅包括专业软件开发人员，还包括天文学家、画家、学生等。

.. tab:: 英文

  The audience of this guide is anyone who uses Python with packages.

  Don't forget that the Python community is big and welcoming. Readers may not
  share your age, gender, education, culture, and more, but they deserve to learn
  about packaging just as much as you do.

  In particular, keep in mind that not all people who use Python see themselves as
  programmers. The audience of this guide includes astronomers or painters or
  students as well as professional software developers.


语音和语调
--------------

**Voice and tone**

.. tab:: 中文

  在编写本指南时，尽量以亲切谦逊的语气写作，即使你知道所有的答案。

  想象一下，你正在与一个你认为聪明且有能力的 Python 项目合作伙伴一起工作。你们彼此愉快地合作，彼此喜欢对方的工作方式。那个人向你提了一个问题，而你知道答案。你会怎么回应？*那就是*你应该用来写本指南的语气。

  这里有个小窍门：试着大声朗读你的文字，感受一下你的语气和调性。它听起来像是你平常会说的话，还是像是在扮演一个角色或发表演讲？可以自由使用缩写，别担心遵循过于严格的语法规则。如果你想以介词结尾，就尽管去做。

  在写作时，根据话题的严肃性和难度调整语气。如果你在写一个入门教程，适当开个玩笑没问题；但如果你在讨论敏感的安全建议，最好避免开玩笑。

.. tab:: 英文

  When writing this guide, strive to write with a voice that's approachable and
  humble, even if you have all the answers.

  Imagine you're working on a Python project with someone you know to be smart and
  skilled. You like working with them and they like working with you. That person
  has asked you a question and you know the answer. How do you respond? *That* is
  how you should write this guide.

  Here's a quick check: try reading aloud to get a sense for your writing's voice
  and tone. Does it sound like something you would say or does it sound like
  you're acting out a part or giving a speech? Feel free to use contractions and
  don't worry about sticking to fussy grammar rules. You are hereby granted
  permission to end a sentence in a preposition, if that's what you want to end it
  with.

  When writing the guide, adjust your tone for the seriousness and difficulty of
  the topic. If you're writing an introductory tutorial, it's OK to make a joke,
  but if you're covering a sensitive security recommendation, you might want to
  avoid jokes altogether.


惯例和机制
-------------------------

**Conventions and mechanics**

.. tab:: 中文

  **面向读者写作**  
    在给出建议或操作步骤时，用 *你* 来称呼读者，或者使用祈使句语气。

    | 错误: 要安装它，用户运行…  
    | 正确: 你可以通过运行来安装它…  
    | 正确: 要安装它，运行…

  **说明假设**  
    避免做未说明的假设。因为读者可能是从网上随意打开某一页面开始阅读，所以该页面可能是他们第一次接触到本指南。如果你做出假设，请明确说明这些假设。

  **充分交叉引用**  
    第一次提到某个工具或实践时，链接到指南中相关部分，或者链接到其他相关文档。帮助读者节省搜索时间。

  **尊重命名规范**  
    在命名工具、网站、人物和其他专有名词时，请使用它们首选的大小写。

    | 错误: Pip 使用…  
    | 正确: pip 使用…  
    |
    | 错误: …托管在 github 上。  
    | 正确: …托管在 GitHub 上。

  **使用性别中立的写作风格**  
    通常情况下，你会直接称呼读者使用 *你*、*你的* 和 *你的*。否则，请使用性别中立的代词 *他们*、*他们的* 和 *他们的*，或完全避免使用代词。

    | 错误: 维护者上传文件后，他…  
    | 正确: 维护者上传文件后，他们…  
    | 正确: 维护者上传文件后，维护者…

  **标题**  
    写标题时使用读者可能正在搜索的词汇。一个好的方式是让标题回答一个隐含的问题。例如，读者可能想知道 *如何安装 MyLibrary？*，因此一个好的标题可能是 *安装 MyLibrary*。

    在部分标题中使用句子式大小写。换句话说，标题应写成你通常写句子的样子。

    | 错误: 你应该了解的 Python 事项  
    | 正确: 你应该了解的 Python 事项

  **数字**  
    在正文中，将一到九的数字写成单词。对于其他数字或表格中的数字，使用阿拉伯数字。

.. tab:: 英文

  **Write to the reader**
    When giving recommendations or steps to take, address the reader as *you*
    or use the imperative mood.

    | Wrong: To install it, the user runs…
    | Right: You can install it by running…
    | Right: To install it, run…

  **State assumptions**
    Avoid making unstated assumptions. Reading on the web means that any page of
    the guide may be the first page of the guide that the reader ever sees.
    If you're going to make assumptions, then say what assumptions that you're
    going to make.

  **Cross-reference generously**
    The first time you mention a tool or practice, link to the part of the
    guide that covers it, or link to a relevant document elsewhere. Save the
    reader a search.

  **Respect naming practices**
    When naming tools, sites, people, and other proper nouns, use their preferred
    capitalization.

    | Wrong: Pip uses…
    | Right: pip uses…
    |
    | Wrong: …hosted on github.
    | Right: …hosted on GitHub.

  **Use a gender-neutral style**
    Often, you'll address the reader directly with *you*, *your* and *yours*.
    Otherwise, use gender-neutral pronouns *they*, *their*, and *theirs* or avoid
    pronouns entirely.

    | Wrong: A maintainer uploads the file. Then he…
    | Right: A maintainer uploads the file. Then they…
    | Right: A maintainer uploads the file. Then the maintainer…

  **Headings**
    Write headings that use words the reader is searching for. A good way to
    do this is to have your heading complete an implied question. For example, a
    reader might want to know *How do I install MyLibrary?* so a good heading
    might be *Install MyLibrary*.

    In section headings, use sentence case. In other words, write headings as you
    would write a typical sentence.

    | Wrong: Things You Should Know About Python
    | Right: Things you should know about Python

  **Numbers**
    In body text, write numbers one through nine as words. For other numbers or
    numbers in tables, use numerals.
