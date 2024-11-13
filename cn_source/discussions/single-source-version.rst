.. _single-source-version:

===================================
单一来源项目版本
===================================

**Single-sourcing the Project Version**

:Page Status: Complete
:Last Reviewed: 2024-10-07

.. tab:: 中文

   许多 Python :term:`分发包 <Distribution Package>` 发布单个 Python :term:`导入包 <Import Package>`，并希望该导入包的运行时 ``__version__`` 属性报告与 :func:`importlib.metadata.version` 所报告的分发包版本号相同（如 :ref:`runtime-version-access` 所述）。

   此外，通常希望这些版本信息来自版本控制系统的 *标签* （例如 ``v1.2.3``），而不是手动更新源代码中的版本号。

   一些项目可能选择容忍这种数据条目重复，并依赖自动化测试确保这些不同的版本值不会发生偏离。

   另一种选择是，项目选择的构建系统可能提供一种方式来定义版本号的唯一“真相源”。

   一般来说，选项如下：

   1) 如果代码在版本控制系统（VCS）中，例如 Git，那么可以从 VCS 提取版本号。

   2) 版本号可以硬编码到 :file:`pyproject.toml` 文件中——然后构建系统可以将其复制到可能需要版本号的其他位置。

   3) 版本号可以硬编码到源代码中——可以放在一个专门的文件中，例如 :file:`_version.txt`（该文件必须作为项目源代码分发包的一部分进行打包），或者作为某个模块中的属性，例如 :file:`__init__.py`。构建系统随后可以在构建时从运行时位置提取版本号。

   请查阅构建系统的文档以了解它们推荐的方式。

   当目标是确保分发包及其关联的导入包共享相同的版本号时，建议项目包括一个自动化测试用例，确保 ``import_name.__version__`` 和 ``importlib.metadata.version("dist-name")`` 报告相同的版本值（注意：对于许多项目， ``import_name`` 和 ``dist-name`` 将是相同的名称）。

.. tab:: 英文

   Many Python :term:`distribution packages <Distribution Package>` publish a single
   Python :term:`import package <Import Package>` where it is desired that the runtime
   ``__version__`` attribute on the import package report the same version specifier
   as :func:`importlib.metadata.version` reports for the distribution package
   (as described in :ref:`runtime-version-access`).

   It is also frequently desired that this version information be derived from a version
   control system *tag* (such as ``v1.2.3``) rather than being manually updated in the
   source code.

   Some projects may choose to simply live with the data entry duplication, and rely
   on automated testing to ensure the different values do not diverge.

   Alternatively, a project's chosen build system may offer a way to define a single
   source of truth for the version number.

   In general, the options are:

   1) If the code is in a version control system (VCS), such as Git, then the version can be extracted from the VCS.

   2) The version can be hard-coded into the :file:`pyproject.toml` file -- and the build system can copy it
      into other locations it may be required.

   3) The version string can be hard-coded into the source code -- either in a special purpose file,
      such as :file:`_version.txt` (which must then be shipped as part of the project's source distribution
      package), or as an attribute in a particular module, such as :file:`__init__.py`. The build
      system can then extract it from the runtime location at build time.

   Consult your build system's documentation for their recommended method.

   When the intention is that a distribution package and its associated import package
   share the same version, it is recommended that the project include an automated test
   case that ensures ``import_name.__version__`` and ``importlib.metadata.version("dist-name")``
   report the same value (note: for many projects, ``import_name`` and ``dist-name`` will
   be the same name).


.. _Build system version handling:

构建系统版本处理
-----------------------------

**Build System Version Handling**

.. tab:: 中文

   以下是一些用于处理版本字符串的构建系统文档的链接。

.. tab:: 英文

   The following are links to some build system's documentation for handling version strings.

* `Flit <https://flit.pypa.io/en/stable/>`_

* `Hatchling <https://hatch.pypa.io/1.9/version/>`_

* `PDM <https://pdm-project.org/en/latest/reference/pep621/#__tabbed_1_2>`_

* `Setuptools <https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html#dynamic-metadata>`_

  -  `setuptools_scm <https://setuptools-scm.readthedocs.io/en/latest/>`_
