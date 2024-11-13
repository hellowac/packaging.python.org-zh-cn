.. _src-layout-vs-flat-layout:

=========================
src 布局与平面布局
=========================

**src layout vs flat layout**

.. tab:: 中文

  “平面布局”指的是将一个项目的文件组织在一个文件夹或仓库中，使得各种配置文件和 :term:`import packages <Import Package>` 都位于顶层目录。

  ::

      .
      ├── README.md
      ├── noxfile.py
      ├── pyproject.toml
      ├── setup.py
      ├── awesome_package/
      │   ├── __init__.py
      │   └── module.py
      └── tools/
          ├── generate_awesomeness.py
          └── decrease_world_suck.py

  “src 布局”通过将那些打算被导入的代码（即 ``import awesome_package``, 也称为 :term:`import packages <Import Package>`）移动到一个子目录中，从而偏离了平面布局。这个子目录通常命名为 ``src/``, 因此被称为“src 布局”。

  ::

      .
      ├── README.md
      ├── noxfile.py
      ├── pyproject.toml
      ├── setup.py
      ├── src/
      │    └── awesome_package/
      │       ├── __init__.py
      │       └── module.py
      └── tools/
          ├── generate_awesomeness.py
          └── decrease_world_suck.py

  以下是 src 布局与平面布局之间重要的行为差异：

  * src 布局需要安装项目才能运行其代码，而平面布局不需要。

    这意味着，src 布局涉及到项目开发工作流程中的额外步骤（通常使用 :doc:`可编辑安装 <setuptools:userguide/development_mode>` 进行开发，使用常规安装进行测试）。

  * src 布局有助于防止意外使用正在开发中的代码副本。

    这是因为 Python 解释器将当前工作目录作为导入路径中的第一个项。这意味着，如果当前工作目录中存在一个与已安装的 import package 同名的包，当前工作目录中的变体将被使用。这可能导致项目的打包工具配置出现微妙的错误，进而导致某些文件未被包含在分发包中。

    src 布局通过将 import packages 保存在一个与项目根目录分开的目录中，避免了这一点，确保了使用的是已安装的副本。

  * src 布局有助于强制确保一个 :doc:`可编辑安装 <setuptools:userguide/development_mode>` 只能导入那些本应被导入的文件。

    这一点在使用 `路径配置文件 <https://docs.python.org/3/library/site.html#index-2>`_ 的可编辑安装中尤为重要，路径配置文件将目录添加到导入路径中。

    平面布局会将其他项目文件（例如：``README.md``、``tox.ini``）和打包/工具配置文件（例如：``setup.py``、``noxfile.py``）添加到导入路径中。这会使得在可编辑安装中某些导入能够工作，但在常规安装中则无法工作。

.. tab:: 英文

  The "flat layout" refers to organising a project's files in a folder or
  repository, such that the various configuration files and
  :term:`import packages <Import Package>` are all in the top-level directory.

  ::

      .
      ├── README.md
      ├── noxfile.py
      ├── pyproject.toml
      ├── setup.py
      ├── awesome_package/
      │   ├── __init__.py
      │   └── module.py
      └── tools/
          ├── generate_awesomeness.py
          └── decrease_world_suck.py

  The "src layout" deviates from the flat layout by moving the code that is
  intended to be importable (i.e. ``import awesome_package``, also known as
  :term:`import packages <Import Package>`) into a subdirectory. This
  subdirectory is typically named ``src/``, hence "src layout".

  ::

      .
      ├── README.md
      ├── noxfile.py
      ├── pyproject.toml
      ├── setup.py
      ├── src/
      │    └── awesome_package/
      │       ├── __init__.py
      │       └── module.py
      └── tools/
          ├── generate_awesomeness.py
          └── decrease_world_suck.py

  Here's a breakdown of the important behaviour differences between the src
  layout and the flat layout:

  * The src layout requires installation of the project to be able to run its
    code, and the flat layout does not.

    This means that the src layout involves an additional step in the
    development workflow of a project (typically, an
    :doc:`editable installation <setuptools:userguide/development_mode>`
    is used for development and a regular installation is used for testing).

  * The src layout helps prevent accidental usage of the in-development copy of
    the code.

    This is relevant since the Python interpreter includes the current working
    directory as the first item on the import path. This means that if an import
    package exists in the current working directory with the same name as an
    installed import package, the variant from the current working directory will
    be used. This can lead to subtle  misconfiguration of the project's packaging
    tooling, which could result in files not being included in a distribution.

    The src layout helps avoid this by keeping import packages in a directory
    separate from the root directory of the project, ensuring that the installed
    copy is used.

  * The src layout helps enforce that an
    :doc:`editable installation <setuptools:userguide/development_mode>` is only
    able to import files that were meant to be importable.

    This is especially relevant when the editable installation is implemented
    using a `path configuration file <https://docs.python.org/3/library/site.html#index-2>`_
    that adds the directory to the import path.

    The flat layout would add the other project files (eg: ``README.md``,
    ``tox.ini``) and packaging/tooling configuration files (eg: ``setup.py``,
    ``noxfile.py``) on the import path. This would make certain imports work
    in editable installations but not regular installations.

.. _running-cli-from-source-src-layout:

使用 src-layout 从源代码运行命令行界面
============================================================

**Running a command-line interface from source with src-layout**

.. tab:: 中文

  由于前面提到的 src 布局的特殊性，命令行界面不能直接从 :term:`source tree <Project Source Tree>` 运行，而需要将包安装为 :doc:`开发模式 <setuptools:userguide/development_mode>` 以便进行测试。由于在某些情况下这可能不太方便，解决方法是，当通过其 :file:`__main__.py` 文件调用时，可以将包文件夹添加到 Python 的 :py:data:`sys.path` 中：

  .. code-block:: python

      import os
      import sys

      if not __package__:
          # Make CLI runnable from source tree with
          #    python src/package
          package_source_path = os.path.dirname(os.path.dirname(__file__))
          sys.path.insert(0, package_source_path)
          
.. tab:: 英文

  Due to the firstly mentioned specialty of the src layout, a command-line
  interface can not be run directly from the :term:`source tree <Project Source Tree>`,
  but requires installation of the package in
  :doc:`Development Mode <setuptools:userguide/development_mode>`
  for testing purposes. Since this can be unpractical in some situations,
  a workaround could be to prepend the package folder to  Python's
  :py:data:`sys.path` when called via its :file:`__main__.py` file:

  .. code-block:: python

      import os
      import sys

      if not __package__:
          # Make CLI runnable from source tree with
          #    python src/package
          package_source_path = os.path.dirname(os.path.dirname(__file__))
          sys.path.insert(0, package_source_path)
