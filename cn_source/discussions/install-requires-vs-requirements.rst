.. _`install_requires vs requirements files`:

======================================
install_requires 与需求文件
======================================

**install_requires vs requirements files**


install_requires
----------------

.. tab:: 中文

   ``install_requires`` 是一个 :ref:`setuptools` 的 :file:`setup.py` 关键字，应当用于指定项目 **最基本** 的运行依赖。当项目通过 :ref:`pip` 安装时，pip 会使用这个规范来安装其依赖项。

   例如，如果项目依赖于 A 和 B，``install_requires`` 应该如下所示：

   ::

      install_requires=[
         'A',
         'B'
      ]

   此外，最佳实践是标明已知的下限或上限版本。

   例如，已知项目至少需要 'A' 的 v1 版本和 'B' 的 v2 版本，则应该这样指定：

   ::

      install_requires=[
         'A>=1',
         'B>=2'
      ]

   还可能已知项目 'A' 在其 v2 版本中引入了一个变更，这个变更使得项目与 'A' 的 v2 版本及之后的版本不兼容，因此不应允许使用 v2：

   ::

      install_requires=[
         'A>=1,<2',
         'B>=2'
      ]

   不建议在 ``install_requires`` 中指定依赖项的确切版本，或者指定子依赖项（即依赖项的依赖项）。这种做法过于严格，阻止了用户从依赖项升级中受益。

   最后，重要的是要理解，``install_requires`` 列出了“抽象”的需求，即仅包含名称和版本限制，这些并不决定依赖项将从哪里获取（即从哪个索引或源）。具体的获取方式（即如何将其转化为“具体”依赖）将在安装时通过 :ref:`pip` 的选项来确定。 [1]_

.. tab:: 英文

   ``install_requires`` is a :ref:`setuptools` :file:`setup.py` keyword that
   should be used to specify what a project **minimally** needs to run correctly.
   When the project is installed by :ref:`pip`, this is the specification that is
   used to install its dependencies.

   For example, if the project requires A and B, your ``install_requires`` would be
   like so:

   ::

      install_requires=[
         'A',
         'B'
      ]

   Additionally, it's best practice to indicate any known lower or upper bounds.

   For example, it may be known, that your project requires at least v1 of 'A', and
   v2 of 'B', so it would be like so:

   ::

      install_requires=[
         'A>=1',
         'B>=2'
      ]

   It may also be known that project 'A' introduced a change in its v2
   that breaks the compatibility of your project with v2 of 'A' and later,
   so it makes sense to not allow v2:

   ::

      install_requires=[
         'A>=1,<2',
         'B>=2'
      ]

   It is not considered best practice to use ``install_requires`` to pin
   dependencies to specific versions, or to specify sub-dependencies
   (i.e. dependencies of your dependencies).  This is overly-restrictive, and
   prevents the user from gaining the benefit of dependency upgrades.

   Lastly, it's important to understand that ``install_requires`` is a listing of
   "Abstract" requirements, i.e just names and version restrictions that don't
   determine where the dependencies will be fulfilled from (i.e. from what
   index or source).  The where (i.e. how they are to be made "Concrete") is to
   be determined at install time using :ref:`pip` options. [2]_


需求文件
------------------

**Requirements files**

.. tab:: 中文

   :ref:`需求文件 <pip:Requirements Files>` 简单来说，就是将 :ref:`pip:pip install` 的参数列表放入一个文件中。

   而 ``install_requires`` 定义的是单个项目的依赖项，:ref:`需求文件 <pip:Requirements Files>` 通常用于定义一个完整 Python 环境的依赖。

   ``install_requires`` 中的依赖是最基本的要求，而依赖文件通常包含详细列出的固定版本，以实现 :ref:`可重复安装 <pip:Repeatability>` 一个完整环境的目的。

   ``install_requires`` 中的依赖是“抽象”的，即与特定索引无关，而依赖文件通常包含像 ``--index-url`` 或 ``--find-links`` 这样的 pip 选项，使得依赖项变得“具体”，即与特定索引或包目录相关联。[1]_

   虽然 ``install_requires`` 的元数据在安装时会被 pip 自动分析，依赖文件则不会，只有当用户明确使用 ``python -m pip install -r`` 安装时，依赖文件才会被使用。

.. tab:: 英文

   :ref:`Requirements Files <pip:Requirements Files>` described most simply, are
   just a list of :ref:`pip:pip install` arguments placed into a file.

   Whereas ``install_requires`` defines the dependencies for a single project,
   :ref:`Requirements Files <pip:Requirements Files>` are often used to define
   the requirements for a complete Python environment.

   Whereas ``install_requires`` requirements are minimal, requirements files
   often contain an exhaustive listing of pinned versions for the purpose of
   achieving :ref:`repeatable installations <pip:Repeatability>` of a complete
   environment.

   Whereas ``install_requires`` requirements are "Abstract", i.e. not associated
   with any particular index, requirements files often contain pip
   options like ``--index-url`` or ``--find-links`` to make requirements
   "Concrete", i.e. associated with a particular index or directory of
   packages. [2]_

   Whereas ``install_requires`` metadata is automatically analyzed by pip during an
   install, requirements files are not, and only are used when a user specifically
   installs them using ``python -m pip install -r``.

----

.. [1] 有关“抽象(Abstract)”与“具体(Concrete)”要求的更多信息, 见 https://caremad.io/posts/2013/07/setup-vs-requirement/.

.. [2] For more on "Abstract" vs "Concrete" requirements, see https://caremad.io/posts/2013/07/setup-vs-requirement/.
