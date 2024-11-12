===========================
Python打包用户指南
===========================

.. meta::
   :description: Python 打包用户指南 (PyPUG) 是用于打包 Python 软件的教程和指南的集合。
   :keywords: python, packaging, guide, tutorial, 打包, 指南, 教程

.. toctree::
   :maxdepth: 2
   :hidden:

   overview
   flow
   tutorials/index
   guides/index
   discussions/index
   specifications/index
   key_projects
   glossary
   support
   contribute
   news


.. tab:: 中文

   欢迎来到 *Python 打包用户指南*，这是一个包含教程和参考资料的集合，旨在帮助您使用现代工具分发和安装 Python 包。

   本指南由 :doc:`Python 打包机构 <pypa:index>` 在 `GitHub`_ 上维护。我们非常欢迎 :doc:`贡献和反馈 <contribute>`。😊

.. tab:: 英文

   Welcome to the *Python Packaging User Guide*, a collection of tutorials and
   references to help you distribute and install Python packages with modern
   tools.

   This guide is maintained on `GitHub`_ by the :doc:`Python Packaging Authority <pypa:index>`. We
   happily accept :doc:`contributions and feedback <contribute>`. 😊

.. _GitHub: https://github.com/pypa/packaging.python.org


概述和流程
=================

.. tab:: 中文

   .. note::

      构建对 Python 打包的理解是一个逐步的过程。耐心和持续的改进是成功的关键。概述和流程部分为您理解 Python 打包生态系统提供了一个起点。

   :doc:`概述 <overview>` 解释了 Python 打包及其在准备和分发项目中的应用。
   这一部分帮助您了解如何选择最适合您用例的工具和流程。
   它包括什么是打包、它解决了哪些问题以及关键的注意事项。

   要了解用于发布代码的工作流概述，请参见 :doc:`打包流程 <flow>`。

.. tab:: 英文

   .. note::

      Building your understanding of Python packaging is a journey. Patience and
      continuous improvement are key to success. The overview and flow sections
      provide a starting point for understanding the Python packaging ecosystem.

   The :doc:`overview` explains Python packaging
   and its use when preparing and distributing projects.
   This section helps you build understanding about selecting the tools and
   processes that are most suitable for your use case.
   It includes what packaging is, the problems that it solves, and
   key considerations.

   To get an overview of the workflow used to publish your code, see
   :doc:`packaging flow <flow>`.

教程
=========

**Tutorials**

.. tab:: 中文

   教程将引导您完成第一次完成项目所需的步骤。
   教程旨在帮助您成功，并为未来的探索提供起点。
   :doc:`教程/索引 <tutorials/index>` 部分包括：

   * 一个关于安装包的 :doc:`教程 <tutorials/installing-packages>`
   * 一个关于在版本控制项目中管理应用程序依赖项的 :doc:`教程 <tutorials/managing-dependencies>`
   * 一个关于打包和分发项目的 :doc:`教程 <tutorials/packaging-projects>`

.. tab:: 英文

   Tutorials walk through the steps needed to complete a project for the first time.
   Tutorials aim to help you succeed and provide a starting point for future
   exploration.
   The :doc:`tutorials/index` section includes:

   * A :doc:`tutorial on installing packages <tutorials/installing-packages>`
   * A :doc:`tutorial on managing application dependencies <tutorials/managing-dependencies>` in a version controlled project
   * A :doc:`tutorial on packaging and distributing <tutorials/packaging-projects>` your project

指南
======

**Guides**

.. tab:: 中文

   指南提供执行特定任务的步骤。指南更侧重于那些已经熟悉 Python 打包的用户，他们正在寻找具体的信息。

   :doc:`指南/索引 <guides/index>` 部分提供了三个主要领域的“操作指南”：

   * 包的安装
   * 包的构建和分发
   * 其他杂项主题

.. tab:: 英文

   Guides provide steps to perform a specific task. Guides are more focused on
   users who are already familiar with Python packaging and are looking for
   specific information.

   The :doc:`guides/index` section provides "how to" instructions in three major
   areas: package installation; building and distributing packages; miscellaneous
   topics.

解释和讨论
============================

**Explanations and Discussions**

.. tab:: 中文

   :doc:`discussions/index` 部分提供有关以下主题的深入解释和讨论，例如：

.. tab:: 英文

   The :doc:`discussions/index` section for in-depth explanations and discussion
   about topics, such as:

* :doc:`discussions/deploying-python-applications`
* :doc:`discussions/pip-vs-easy-install`

参考
=========

**Reference**

.. tab:: 中文

   * :doc:`specifications/index` 部分包含打包互操作性规范。
   * Python 打包管理局成员维护的 :doc:`其他项目 <key_projects>` 列表。
   * :doc:`glossary` 包含 Python 打包中使用的术语的定义。

.. tab:: 英文

   * The :doc:`specifications/index` section for packaging interoperability specifications.
   * The list of :doc:`other projects <key_projects>` maintained by members of the Python Packaging Authority.
   * The :doc:`glossary` for definitions of terms used in Python packaging.
