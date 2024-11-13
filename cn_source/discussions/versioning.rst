.. _versioning:
.. _`Choosing a versioning scheme`:

==========
版本控制
==========

**Versioning**

.. tab:: 中文

   本讨论涵盖了 Python 包版本控制的所有方面。

.. tab:: 英文

   This discussion covers all aspects of versioning Python packages.


有效版本号
=====================

**Valid version numbers**

.. tab:: 中文

   不同的 Python 项目可能会根据特定项目的需求使用不同的版本控制方案，但为了与像 :ref:`pip` 这样的工具兼容，它们都需要遵循一个灵活的版本标识符格式，其权威参考为 :ref:`版本规范说明 <version-specifiers>` 。以下是一些版本号的示例 [#version-examples1]_ :

   - 一个简单的版本（正式发布版）：``1.2.0``
   - 一个开发版本：``1.2.0.dev1``
   - 一个 alpha 版本：``1.2.0a1``
   - 一个 beta 版本：``1.2.0b1``
   - 一个发布候选版本：``1.2.0rc1``
   - 一个后发布版本：``1.2.0.post1``
   - 一个 alpha 版本的后发布版本（可能，但不推荐）：``1.2.0a1.post1``
   - 只有两个组件的简单版本：``23.12``
   - 只有一个组件的简单版本：``42``
   - 带有纪元（epoch）的版本：``1!1.0``

   项目可以使用预发布周期，在最终发布之前支持用户进行测试。顺序为：alpha 版本、beta 版本、发布候选版本、最终发布。Pip 和其他现代 Python 包管理工具在决定安装哪个版本的依赖时默认忽略预发布版本，除非明确请求（例如，使用 ``pip install pkg==1.1a3`` 或 ``pip install --pre pkg``）。

   开发版本的目的是支持在开发周期初期发布的版本，例如，夜间构建或来自 Linux 发行版的最新源代码构建。

   后发布版本用于解决正式发布中的一些轻微错误，这些错误不会影响分发的软件，例如，修正发布说明中的错误。后发布版本不应用于修复 bug；这些应通过新的正式版本来解决（例如，在使用语义版本控制时递增第三个组件）。

   最后，纪元（epoch）是一个很少使用的功能，用于在更改版本控制方案时修正排序顺序。例如，如果一个项目使用日历版本控制，版本号为 23.12，并且切换到语义版本控制，版本号为 1.0，则比较 1.0 和 23.12 时可能会出现错误的排序。为了解决这个问题，新的版本号应该有一个显式的纪元，例如 "1!1.0"，以便被视为比旧版本号更新。

.. tab:: 英文

   Different Python projects may use different versioning schemes based on the
   needs of that particular project, but in order to be compatible with tools like
   :ref:`pip`, all of them are required to comply with a flexible format for
   version identifiers, for which the authoritative reference is the
   :ref:`specification of version specifiers <version-specifiers>`. Here are some
   examples of version numbers [#version-examples2]_:

   - A simple version (final release): ``1.2.0``
   - A development release: ``1.2.0.dev1``
   - An alpha release: ``1.2.0a1``
   - A beta release: ``1.2.0b1``
   - A release candidate: ``1.2.0rc1``
   - A post-release: ``1.2.0.post1``
   - A post-release of an alpha release (possible, but discouraged): ``1.2.0a1.post1``
   - A simple version with only two components: ``23.12``
   - A simple version with just one component: ``42``
   - A version with an epoch: ``1!1.0``

   Projects can use a cycle of pre-releases to support testing by their users
   before a final release. In order, the steps are: alpha releases, beta releases,
   release candidates, final release. Pip and other modern Python package
   installers ignore pre-releases by default when deciding which versions of
   dependencies to install, unless explicitly requested (e.g., with
   ``pip install pkg==1.1a3`` or ``pip install --pre pkg``).

   The purpose of development releases is to support releases made early during a
   development cycle, for example, a nightly build, or a build from the latest
   source in a Linux distribution.

   Post-releases are used to address minor errors in a final release that do not
   affect the distributed software, such as correcting an error in the release
   notes. They should not be used for bug fixes; these should be done with a new
   final release (e.g., incrementing the third component when using semantic
   versioning).

   Finally, epochs, a rarely used feature, serve to fix the sorting order when
   changing the versioning scheme. For example, if a project is using calendar
   versioning, with versions like 23.12, and switches to semantic versioning, with
   versions like 1.0, the comparison between 1.0 and 23.12 will go the wrong way.
   To correct this, the new version numbers should have an explicit epoch, as in
   "1!1.0", in order to be treated as more recent than the old version numbers.



语义版本控制与日历版本控制
===========================================

**Semantic versioning vs. calendar versioning**

.. tab:: 中文

   版本控制方案是对版本号各个部分进行解释的规范化方式，并决定在发布新版本时应该选择哪个版本号。Python 包常用的两种版本控制方案是语义版本控制和日历版本控制。

   .. caution::

      选择哪个版本号由项目的维护者决定。这实际上意味着版本号的提升反映了维护者的观点。这个观点可能与最终用户对该版本控制方案的期望有所不同。

      在选择下一个版本号时，有已知的例外情况。维护者可能会故意打破最后一个版本段仅包含向后兼容性更改的假设。一个这样的例外情况是当需要解决安全漏洞时。安全发布通常以修补版本发布，但不可避免地会包含破坏性更改。

.. tab:: 英文

   A versioning scheme is a formalized way to interpret the segments of a version
   number, and to decide which should be the next version number for a new release
   of a package. Two versioning schemes are commonly used for Python packages,
   semantic versioning and calendar versioning.

   .. caution::

      The decision which version number to choose is up to a
      project's maintainer. This effectively means that version
      bumps reflect the maintainer's view. That view may differ
      from the end-users' perception of what said formalized
      versioning scheme promises them.

      There are known exceptions for selecting the next version
      number. The maintainers may consciously choose to break the
      assumption that the last version segment only contains
      backwards-compatible changes.
      One such case is when security vulnerability needs to be
      addressed. Security releases often come in patch versions
      but contain breaking changes inevitably.


语义版本控制
-------------------

**Semantic versioning**

.. tab:: 中文

   *语义版本控制* （或称 SemVer）的理念是使用三部分版本号，*major.minor.patch*，其中项目作者根据以下规则递增：

   - *major*：当他们做出不兼容的 API 更改时，
   - *minor*：当他们以向后兼容的方式增加功能时，
   - *patch*：当他们进行向后兼容的错误修复时。

   大多数 Python 项目使用类似于语义版本控制的方案。然而，大多数项目，尤其是大型项目，并不严格遵循语义版本控制，因为许多更改在技术上是破坏性的，但只影响少数用户。这些项目通常在不兼容性较大时递增主版本号，或者用于标志项目的重大变更，而不是为了任何微小的不兼容性 [#semver-strictness1]_ 。相反，主版本号的递增有时用于表示显著的但向后兼容的新特性。

   对于那些严格遵循语义版本控制的项目，这种方式允许用户使用 :ref:`兼容的发布版本说明符 <version-specifiers-compatible-release>`，通过 ``~=`` 操作符。例如， ``name ~= X.Y`` 大致等同于 ``name >= X.Y, == X.*``，即它要求至少是 X.Y 版本，并允许任何后续版本，只要 X 保持不变，Y 增大。类似地，``name ~= X.Y.Z`` 大致等同于 ``name >= X.Y.Z, == X.Y.*``，即要求至少是 X.Y.Z 版本，并允许同样的 X 和 Y 版本，但 Z 可以更大。

   采用语义版本控制的 Python 项目应遵循 `Semantic Versioning 2.0.0 规范 <semver_>`_ 的第 1-8 条。

   流行的 :doc:`Sphinx <sphinx:index>` 文档生成器是一个使用严格语义版本控制的示例（:doc:`Sphinx 版本管理政策 <sphinx:internals/release-process>`）。著名的 :doc:`NumPy <numpy:index>` 科学计算包明确使用“宽松”语义版本控制，其中递增次版本号的发布可能包含向后不兼容的 API 更改（:doc:`NumPy 版本管理政策 <numpy:dev/depending_on_numpy>`）。

.. tab:: 英文

   The idea of *semantic versioning* (or SemVer) is to use 3-part version numbers,
   *major.minor.patch*, where the project author increments:

   - *major* when they make incompatible API changes,
   - *minor* when they add functionality in a backwards-compatible manner, and
   - *patch*, when they make backwards-compatible bug fixes.

   A majority of Python projects use a scheme that resembles semantic
   versioning. However, most projects, especially larger ones, do not strictly
   adhere to semantic versioning, since many changes are technically breaking
   changes but affect only a small fraction of users. Such projects tend to
   increment the major number when the incompatibility is high, or to signal a
   shift in the project, rather than for any tiny incompatibility
   [#semver-strictness2]_. Conversely, a bump of the major version number
   is sometimes used to signal significant but backwards-compatible new
   features.

   For those projects that do use strict semantic versioning, this approach allows
   users to make use of :ref:`compatible release version specifiers
   <version-specifiers-compatible-release>`, with the ``~=`` operator.  For
   example, ``name ~= X.Y`` is roughly equivalent to ``name >= X.Y, == X.*``, i.e.,
   it requires at least release X.Y, and allows any later release with greater Y as
   long as X is the same. Likewise, ``name ~= X.Y.Z`` is roughly equivalent to
   ``name >= X.Y.Z, == X.Y.*``, i.e., it requires at least X.Y.Z and allows a later
   release with same X and Y but higher Z.

   Python projects adopting semantic versioning should abide by clauses 1-8 of the
   `Semantic Versioning 2.0.0 specification <semver_>`_.

   The popular :doc:`Sphinx <sphinx:index>` documentation generator is an example
   project that uses strict semantic versioning (:doc:`Sphinx versioning policy
   <sphinx:internals/release-process>`). The famous :doc:`NumPy <numpy:index>`
   scientific computing package explicitly uses "loose" semantic versioning, where
   releases incrementing the minor version can contain backwards-incompatible API
   changes (:doc:`NumPy versioning policy <numpy:dev/depending_on_numpy>`).


日历版本控制
-------------------

**Calendar versioning**

.. tab:: 中文

   语义版本控制并不适合所有项目，尤其是那些有规律的基于时间的发布周期，并且具有提供多次发布前警告的弃用过程的项目。

   基于日期的版本控制，或称为 `日历版本控制 <calver_>`_ （CalVer），的一个关键优势是，只需版本号即可直观地了解某个特定发布的基本功能集有多旧。

   日历版本号通常采用 *年.月* 的形式（例如，23.12 表示 2023 年 12 月）。

   标准的 Python 包安装器 :doc:`Pip <pip:index>` 就采用了日历版本控制。

.. tab:: 英文

   Semantic versioning is not a suitable choice for all projects, such as those
   with a regular time based release cadence and a deprecation process that
   provides warnings for a number of releases prior to removal of a feature.

   A key advantage of date-based versioning, or `calendar versioning <calver_>`_
   (CalVer), is that it is straightforward to tell how old the base feature set of
   a particular release is given just the version number.

   Calendar version numbers typically take the form *year.month* (for example,
   23.12 for December 2023).

   :doc:`Pip <pip:index>`, the standard Python package installer, uses calendar
   versioning.


其他方案
-------------

**Other schemes**

.. tab:: 中文

   顺序版本控制指的是最简单的版本控制方案，它由一个单一的数字组成，每次发布时递增。虽然顺序版本控制对开发者来说非常容易管理，但对最终用户来说却是最难追踪的，因为顺序版本号几乎不提供关于 API 向后兼容性的信息。

   上述版本控制方案的组合是可能的。例如，一个项目可能会将基于日期的版本控制与顺序版本控制结合起来，创建一种 *年.顺序* 的版本号方案，这种方案可以直观地传达发布的大致年龄，但不会在该年度内承诺特定的发布周期。

.. tab:: 英文

   Serial versioning refers to the simplest possible versioning scheme, which
   consists of a single number incremented every release. While serial versioning
   is very easy to manage as a developer, it is the hardest to track as an end
   user, as serial version numbers convey little or no information regarding API
   backwards compatibility.

   Combinations of the above schemes are possible. For example, a project may
   combine date based versioning with serial versioning to create a *year.serial*
   numbering scheme that readily conveys the approximate age of a release, but
   doesn't otherwise commit to a particular release cadence within the year.


本地版本标识符
=========================

**Local version identifiers**

.. tab:: 中文

   公共版本标识符旨在支持通过 :term:`PyPI <Python 包索引 (PyPI)>` 进行分发。Python 打包工具还支持 :ref:`本地版本标识符 <local-version-identifiers>` 的概念，可以用来标识不打算发布的本地开发构建，或者由重新分发者维护的修改版发布。

   本地版本标识符的形式是一个公共版本标识符，后跟 "+" 和一个本地版本标签。例如，应用了 Fedora 特定补丁的包可以具有版本 "1.2.1+fedora.4"。另一个例子是由 setuptools-scm_ 计算的版本，setuptools-scm 是一个读取 Git 数据中版本的 setuptools 插件。在一个 Git 仓库中，如果自上次发布以来有提交，setuptools-scm 会生成类似 "0.5.dev1+gd00980f" 的版本；如果仓库中有未跟踪的更改，则生成类似 "0.5.dev1+gd00980f.d20231217" 的版本。

.. tab:: 英文

   Public version identifiers are designed to support distribution via :term:`PyPI
   <Python Package Index (PyPI)>`. Python packaging tools also support the notion
   of a :ref:`local version identifier <local-version-identifiers>`, which can be
   used to identify local development builds not intended for publication, or
   modified variants of a release maintained by a redistributor.

   A local version identifier takes the form of a public version identifier,
   followed by "+" and a local version label. For example, a package with
   Fedora-specific patches applied could have the version "1.2.1+fedora.4".
   Another example is versions computed by setuptools-scm_, a setuptools plugin
   that reads the version from Git data. In a Git repository with some commits
   since the latest release, setuptools-scm generates a version like
   "0.5.dev1+gd00980f", or if the repository has untracked changes, like
   "0.5.dev1+gd00980f.d20231217".

.. _runtime-version-access:

在运行时访问版本信息
========================================

**Accessing version information at runtime**

.. tab:: 中文

   可以使用标准库的 :func:`importlib.metadata.version` 函数在运行时获取当前环境中所有 :term:`分发包 <Distribution Package>` 的版本信息：

      >>> importlib.metadata.version("cryptography")  
      '41.0.7'

   许多项目还选择通过提供包级别的 ``__version__`` 属性来为其顶级 :term:`导入包 <Import Package>` 版本化：

      >>> import cryptography  
      >>> cryptography.__version__  
      '41.0.7'

   这种技术对于希望确保版本查询调用（例如 ``pip -V``）尽可能快速运行的 CLI 应用程序尤为有价值。

   希望确保其报告的分发包和导入包版本相一致的包发布者可以查看 :ref:`single-source-version` 讨论，以了解实现这一点的潜在方法。

   由于导入包和模块并不是 *必须* 以这种方式发布运行时版本信息（请参见 :pep:`PEP 396 <396>` 中撤回的提案），因此 ``__version__`` 属性应仅通过已知提供该属性的接口进行查询（例如，项目查询其自身版本或其直接依赖项的版本），或者查询代码应设计为能够处理缺少该属性的情况  [#fallback-to-dist-version1]_ 。

   一些项目可能需要发布外部 API 的版本信息，而这些 API 并非模块本身的版本。这些项目应定义自己的特定项目方式来在运行时获取相关信息。例如，标准库的 :mod:`ssl` 模块提供了多种访问底层 OpenSSL 库版本的方式：

      >>> ssl.OPENSSL_VERSION  
      'OpenSSL 3.2.2 4 Jun 2024'  
      >>> ssl.OPENSSL_VERSION_INFO  
      (3, 2, 0, 2, 0)  
      >>> hex(ssl.OPENSSL_VERSION_NUMBER)  
      '0x30200020'

.. tab:: 英文

   Version information for all :term:`distribution packages <Distribution Package>`
   that are locally available in the current environment can be obtained at runtime
   using the standard library's :func:`importlib.metadata.version` function::

      >>> importlib.metadata.version("cryptography")
      '41.0.7'

   Many projects also choose to version their top level
   :term:`import packages <Import Package>` by providing a package level
   ``__version__`` attribute::

      >>> import cryptography
      >>> cryptography.__version__
      '41.0.7'

   This technique can be particularly valuable for CLI applications which want
   to ensure that version query invocations (such as ``pip -V``) run as quickly
   as possible.

   Package publishers wishing to ensure their reported distribution package and
   import package versions are consistent with each other can review the
   :ref:`single-source-version` discussion for potential approaches to doing so.

   As import packages and modules are not *required* to publish runtime
   version information in this way (see the withdrawn proposal in
   :pep:`PEP 396 <396>`), the ``__version__`` attribute should either only be
   queried with interfaces that are known to provide it (such as a project
   querying its own version or the version of one of its direct dependencies),
   or else the querying code should be designed to handle the case where the
   attribute is missing [#fallback-to-dist-version2]_.

   Some projects may need to publish version information for external APIs
   that aren't the version of the module itself. Such projects should
   define their own project-specific ways of obtaining the relevant information
   at runtime. For example, the standard library's :mod:`ssl` module offers
   multiple ways to access the underlying OpenSSL library version::

      >>> ssl.OPENSSL_VERSION
      'OpenSSL 3.2.2 4 Jun 2024'
      >>> ssl.OPENSSL_VERSION_INFO
      (3, 2, 0, 2, 0)
      >>> hex(ssl.OPENSSL_VERSION_NUMBER)
      '0x30200020'

--------------------------------------------------------------------------------

.. [#version-examples1] Seth Larson 在 `博客文章 <versions-seth-larson_>`_ 中给出了更多不寻常版本号的示例。

.. [#version-examples2] Some more examples of unusual version numbers are given in a `blog post <versions-seth-larson_>`_ by Seth Larson.

.. [#semver-strictness1] 关于此问题的一些个人观点，请参见以下博客文章：`由 Hynek Schlawak 撰写 <semver-hynek-schlawack_>`_，`由 Donald Stufft 撰写 <semver-donald-stufft_>`_，`由 Bernát Gábor 撰写 <semver-bernat-gabor_>`_，`由 Brett Cannon 撰写 <semver-brett-cannon_>`_。若想幽默地了解，请阅读关于 ZeroVer_ 的文章。

.. [#semver-strictness2] For some personal viewpoints on this issue, see these blog posts: `by Hynek Schlawak <semver-hynek-schlawack_>`_, `by Donald Stufft <semver-donald-stufft_>`_, `by Bernát Gábor <semver-bernat-gabor_>`_, `by Brett Cannon <semver-brett-cannon_>`_. For a humoristic take, read about ZeroVer_.

.. [#fallback-to-dist-version1] A full list mapping the top level names available for import to the distribution packages that provide those import packages and modules may be obtained through the standard library's :func:`importlib.metadata.packages_distributions` function. This means that even code that is attempting to infer a version to report for all importable top-level names has a means to fall back to reporting the distribution version information if no ``__version__`` attribute is defined. Only standard library modules, and modules added via means other than Python package installation would fail to have version information reported in that case.

.. [#fallback-to-dist-version2] 通过标准库的 :func:`importlib.metadata.packages_distributions` 函数，可以获得将顶级名称映射到提供这些导入包和模块的分发包的完整列表。这意味着，即使是尝试推断所有可导入顶级名称的版本并进行报告的代码，也可以在没有定义 ``__version__`` 属性的情况下回退到报告分发版本信息。如果没有版本信息被报告的情况，仅有标准库模块和通过非 Python 包安装方式添加的模块会失败。


.. _zerover: https://0ver.org
.. _calver: https://calver.org/overview_zhcn.html
.. _semver: https://semver.org
.. _semver-bernat-gabor: https://bernat.tech/posts/version-numbers/
.. _semver-brett-cannon: https://snarky.ca/why-i-dont-like-semver/
.. _semver-donald-stufft: https://caremad.io/posts/2016/02/versioning-software/
.. _semver-hynek-schlawack: https://hynek.me/articles/semver-will-not-save-you/
.. _setuptools-scm: https://setuptools-scm.readthedocs.io
.. _versions-seth-larson: https://sethmlarson.dev/pep-440
