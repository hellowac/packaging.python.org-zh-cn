制作适合 PyPI 的 README
=============================

**Making a PyPI-friendly README**

.. tab:: 中文

   README 文件可以帮助用户了解你的项目，并且可以用来设置项目在 PyPI 上的描述。本文指南帮助你创建一个符合 PyPI 格式的 README，并将其包含在你的包中，以便它能够在 PyPI 上显示。

.. tab:: 英文

   README files can help your users understand your project and can be used to set your project's description on PyPI.
   This guide helps you create a README in a PyPI-friendly format and include your README in your package so it appears on PyPI.


创建 README 文件
----------------------

**Creating a README file**

.. tab:: 中文

   Python 项目的 README 文件通常命名为 ``README``、 ``README.txt``、 ``README.rst`` 或 ``README.md``。

   为了确保 README 在 PyPI 上正确显示，请选择 PyPI 支持的标记语言。
   `PyPI 的 README 渲染器 <https://github.com/pypa/readme_renderer>`_ 支持以下格式：

   * 普通文本
   * `reStructuredText <https://docutils.sourceforge.io/rst.html>`_ （不包含 Sphinx 扩展）
   * Markdown（默认使用 `GitHub Flavored Markdown <https://github.github.com/gfm/>`_， 或 `CommonMark <https://commonmark.org/>`_）

   通常，将 README 文件保存在项目的根目录中，与 :file:`setup.py` 文件位于同一目录下。

.. tab:: 英文

   README files for Python projects are often named ``README``, ``README.txt``, ``README.rst``, or ``README.md``.

   For your README to display properly on PyPI, choose a markup language supported by PyPI.
   Formats supported by `PyPI's README renderer <https://github.com/pypa/readme_renderer>`_ are:

   * plain text
   * `reStructuredText <https://docutils.sourceforge.io/rst.html>`_ (without Sphinx extensions)
   * Markdown (`GitHub Flavored Markdown <https://github.github.com/gfm/>`_ by default,
     or `CommonMark <https://commonmark.org/>`_)

   It's customary to save your README file in the root of your project, in the same directory as your :file:`setup.py` file.


将 README 包含在包的元数据中
------------------------------------------------

**Including your README in your package's metadata**

.. tab:: 中文

   要将 README 的内容作为包的描述，设置项目的 ``Description`` 和 ``Description-Content-Type`` 元数据，通常在项目的 :file:`setup.py` 文件中进行设置。

   .. seealso::

      * :ref:`description-optional`
      * :ref:`description-content-type-optional`

   例如，要在包的 :file:`setup.py` 文件中设置这些值，可以使用 ``setup()`` 函数的 ``long_description`` 和 ``long_description_content_type`` 参数。

   将 ``long_description`` 的值设置为 README 文件本身的内容（而不是路径）。
   将 ``long_description_content_type`` 设置为 README 文件标记语言的有效 ``Content-Type`` 值，例如 ``text/plain``、 ``text/x-rst`` （用于 reStructuredText）或 ``text/markdown``。

   .. note::

      如果你使用 GitHub 风格的 Markdown 编写项目描述，请确保升级以下工具：

      .. tab:: Unix/macOS

         .. code-block:: bash

            python3 -m pip install --user --upgrade setuptools wheel twine

      .. tab:: Windows

         .. code-block:: bat

            py -m pip install --user --upgrade setuptools wheel twine

      各工具的最低要求版本为：

      - ``setuptools >= 38.6.0``
      - ``wheel >= 0.31.0``
      - ``twine >= 1.11.0``

      推荐使用 ``twine`` 上传项目的分发包：

      .. code-block:: bash

         twine upload dist/*

   例如，参考这个 :file:`setup.py` 文件， 它将 :file:`README.md` 的内容读取为 ``long_description`` 并将标记语言识别为 GitHub 风格的 Markdown :

   .. code-block:: python

      from setuptools import setup

      # 读取 README 文件的内容
      from pathlib import Path
      this_directory = Path(__file__).parent
      long_description = (this_directory / "README.md").read_text()

      setup(
         name='an_example_package',
         # 其他参数省略
         long_description=long_description,
         long_description_content_type='text/markdown'
      )

.. tab:: 英文

   To include your README's contents as your package description,
   set your project's ``Description`` and ``Description-Content-Type`` metadata,
   typically in your project's :file:`setup.py` file.

   .. seealso::

      * :ref:`description-optional`
      * :ref:`description-content-type-optional`

   For example, to set these values in a package's :file:`setup.py` file,
   use ``setup()``'s ``long_description`` and ``long_description_content_type``.

   Set the value of ``long_description`` to the contents (not the path) of the README file itself.
   Set the ``long_description_content_type`` to an accepted ``Content-Type``-style value for your README file's markup,
   such as ``text/plain``, ``text/x-rst`` (for reStructuredText), or ``text/markdown``.

   .. note::

      If you're using GitHub-flavored Markdown to write a project's description, ensure you upgrade
      the following tools:

      .. tab:: Unix/macOS

         .. code-block:: bash

            python3 -m pip install --user --upgrade setuptools wheel twine

      .. tab:: Windows

         .. code-block:: bat

            py -m pip install --user --upgrade setuptools wheel twine

      The minimum required versions of the respective tools are:

      - ``setuptools >= 38.6.0``
      - ``wheel >= 0.31.0``
      - ``twine >= 1.11.0``

      It's recommended that you use ``twine`` to upload the project's distribution packages:

      .. code-block:: bash

         twine upload dist/*

   For example, see this :file:`setup.py` file,
   which reads the contents of :file:`README.md` as ``long_description``
   and identifies the markup as GitHub-flavored Markdown:

   .. code-block:: python

      from setuptools import setup

      # read the contents of your README file
      from pathlib import Path
      this_directory = Path(__file__).parent
      long_description = (this_directory / "README.md").read_text()

      setup(
         name='an_example_package',
         # other arguments omitted
         long_description=long_description,
         long_description_content_type='text/markdown'
      )


验证 reStructuredText 标记
----------------------------------

**Validating reStructuredText markup**

.. tab:: 中文

   如果你的 README 是用 reStructuredText 编写的，任何无效的标记都会阻止其正确渲染，导致 PyPI 只显示 README 的原始源代码。

   请注意，docstring 中使用的 Sphinx 扩展，如
   :doc:`指令 <sphinx:usage/restructuredtext/directives>` 和 :doc:`角色 <sphinx:usage/restructuredtext/roles>` （例如，“``:py:func:`getattr```”或“``:ref:`my-reference-label```”），在这里是不允许的，且会导致类似于“``Error: Unknown interpreted text role "py:func".``”的错误信息。

   在上传之前，你可以按照以下步骤检查你的 README 是否存在标记错误：

   1. 安装最新版本的 `twine <https://github.com/pypa/twine>`_ ；要求版本为 1.12.0 或更高：

      .. tab:: Unix/macOS

         .. code-block:: bash

               python3 -m pip install --upgrade twine

      .. tab:: Windows

         .. code-block:: bat

               py -m pip install --upgrade twine

   2. 按照 :ref:`打包你的项目 <Packaging Your Project>` 中的描述，构建项目的 sdist 和 wheel 文件。

   3. 在 sdist 和 wheel 上运行 ``twine check``：

      .. code-block:: bash

         twine check dist/*

      该命令将报告任何 README 渲染问题。如果标记渲染正常，命令将输出 ``Checking distribution FILENAME: Passed``。

.. tab:: 英文

   If your README is written in reStructuredText, any invalid markup will prevent
   it from rendering, causing PyPI to instead just show the README's raw source.

   Note that Sphinx extensions used in docstrings, such as
   :doc:`directives <sphinx:usage/restructuredtext/directives>` and :doc:`roles <sphinx:usage/restructuredtext/roles>`
   (e.g., "``:py:func:`getattr```" or "``:ref:`my-reference-label```"), are not allowed here and will result in error
   messages like "``Error: Unknown interpreted text role "py:func".``".

   You can check your README for markup errors before uploading as follows:

   1. Install the latest version of `twine <https://github.com/pypa/twine>`_; version 1.12.0 or higher is required:

      .. tab:: Unix/macOS

         .. code-block:: bash

               python3 -m pip install --upgrade twine

      .. tab:: Windows

         .. code-block:: bat

               py -m pip install --upgrade twine

   2. Build the sdist and wheel for your project as described under :ref:`Packaging Your Project <Packaging Your Project>`.

   3. Run ``twine check`` on the sdist and wheel:

      .. code-block:: bash

         twine check dist/*

      This command will report any problems rendering your README.  If your markup renders fine, the command will output ``Checking distribution FILENAME: Passed``.
