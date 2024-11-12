.. _managing-dependencies:

管理依赖项
=================================

**Managing Application Dependencies**

.. tab:: 中文

  在 :ref:`包安装教程 <installing-packages>` 中，我们已经涵盖了安装和更新 Python 包的基本步骤。

  然而，即使是个人项目，交互式运行这些命令也可能变得乏味，而且当需要为多个贡献者的项目自动设置开发环境时，事情会变得更加复杂。

  本教程将带您了解如何使用 :ref:`Pipenv` 来管理应用程序的依赖项。它将向您展示如何安装和使用所需的工具，并给出最佳实践的强烈建议。

  请记住，Python 被用于许多不同的用途，您管理依赖项的方式可能会根据您决定如何发布软件而有所不同。这里提供的指导最直接适用于网络服务（包括 Web 应用）的开发和部署，但也非常适合用于管理任何类型项目的开发和测试环境。

  有关其他工具的替代方案，请参阅 :ref:`其他应用依赖管理工具 <other-dependency-management-tools>`。

.. tab:: 英文

  The :ref:`package installation tutorial <installing-packages>`
  covered the basics of getting set up to install and update Python packages.

  However, running these commands interactively can get tedious even for your
  own personal projects, and things get even more difficult when trying to set up
  development environments automatically for projects with multiple contributors.

  This tutorial walks you through the use of :ref:`Pipenv` to manage dependencies
  for an application. It will show you how to install and use the necessary tools
  and make strong recommendations on best practices.

  Keep in mind that Python is used for a great many different purposes, and
  precisely how you want to manage your dependencies may change based on how you
  decide to publish your software. The guidance presented here is most directly
  applicable to the development and deployment of network services (including
  web applications), but is also very well suited to managing development and
  testing environments for any kind of project.

  For alternatives, see :ref:`Other Tools for Application Dependency Management <other-dependency-management-tools>`.

安装 Pipenv
-----------------

**Installing Pipenv**

.. _pipenv-user-base:

.. tab:: 中文

  :ref:`Pipenv` 是一个用于管理 Python 项目依赖的工具。如果你熟悉 Node.js 的 `npm`_ 或 Ruby 的 `bundler`_，你会发现它们在理念上有相似之处。虽然 :ref:`pip` 通常足够满足个人使用需求，但对于协作项目，Pipenv 是推荐的工具，因为它是一个更高层次的工具，可以简化常见用例的依赖管理。

  使用 ``pip`` 安装 Pipenv：

  .. tab:: Unix/macOS

      .. code-block:: bash

          python3 -m pip install --user pipenv

  .. tab:: Windows

      .. code-block:: bat

          py -m pip install --user pipenv


  .. note:: 这会执行一个 `用户安装 <user installation>`_ 以防止破坏任何系统范围的包。如果安装后 ``pipenv`` 在你的 shell 中不可用，你需要将 :py:data:`user base <python:site.USER_BASE>` 的二进制目录添加到你的 ``PATH`` 中。
      有关更多信息，请参见 :ref:`安装到用户站点 <Installing to the User Site>`。

.. tab:: 英文

  :ref:`Pipenv` is a dependency manager for Python projects. If you're familiar
  with Node.js' `npm`_ or Ruby's `bundler`_, it is similar in spirit to those
  tools. While :ref:`pip` alone is often sufficient for personal use, Pipenv is
  recommended for collaborative projects as it's a higher-level tool that
  simplifies dependency management for common use cases.

  Use ``pip`` to install Pipenv:

  .. tab:: Unix/macOS

      .. code-block:: bash

          python3 -m pip install --user pipenv

  .. tab:: Windows

      .. code-block:: bat

          py -m pip install --user pipenv

  .. Note:: This does a `user installation`_ to prevent breaking any system-wide
      packages. If ``pipenv`` isn't available in your shell after installation,
      you'll need to add the :py:data:`user base <python:site.USER_BASE>`'s
      binary directory to your ``PATH``.
      See :ref:`Installing to the User Site <Installing to the User Site>` for more information.

.. _npm: https://www.npmjs.com/
.. _bundler: https://bundler.io/
.. _user installation: https://pip.pypa.io/en/stable/user_guide/#user-installs

为您的项目安装软件包
------------------------------------

**Installing packages for your project**

.. tab:: 中文

  Pipenv 以项目为单位管理依赖关系。要安装包，进入你的项目目录（或仅为本教程创建一个空目录），然后运行：

  .. code-block:: bash

      cd myproject
      pipenv install requests

  Pipenv 将安装 `Requests`_ 库并在你的项目目录中创建一个 ``Pipfile``。这个 :ref:`Pipfile` 用于记录项目需要哪些依赖包，以便你在需要重新安装时，比如与他人共享项目时，可以方便地恢复这些依赖。你应该会看到类似以下输出（尽管显示的路径会有所不同）：

  .. code-block:: text

      Creating a Pipfile for this project...
      Creating a virtualenv for this project...
      Using base prefix '/usr/local/Cellar/python3/3.6.2/Frameworks/Python.framework/Versions/3.6'
      New python executable in ~/.local/share/virtualenvs/tmp-agwWamBd/bin/python3.6
      Also creating executable in ~/.local/share/virtualenvs/tmp-agwWamBd/bin/python
      Installing setuptools, pip, wheel...done.

      Virtualenv location: ~/.local/share/virtualenvs/tmp-agwWamBd
      Installing requests...
      Collecting requests
        Using cached requests-2.18.4-py2.py3-none-any.whl
      Collecting idna<2.7,>=2.5 (from requests)
        Using cached idna-2.6-py2.py3-none-any.whl
      Collecting urllib3<1.23,>=1.21.1 (from requests)
        Using cached urllib3-1.22-py2.py3-none-any.whl
      Collecting chardet<3.1.0,>=3.0.2 (from requests)
        Using cached chardet-3.0.4-py2.py3-none-any.whl
      Collecting certifi>=2017.4.17 (from requests)
        Using cached certifi-2017.7.27.1-py2.py3-none-any.whl
      Installing collected packages: idna, urllib3, chardet, certifi, requests
      Successfully installed certifi-2017.7.27.1 chardet-3.0.4 idna-2.6 requests-2.18.4 urllib3-1.22

.. tab:: 英文

  Pipenv manages dependencies on a per-project basis. To install packages,
  change into your project's directory (or just an empty directory for this
  tutorial) and run:

  .. code-block:: bash

      cd myproject
      pipenv install requests

  Pipenv will install the `Requests`_ library and create a ``Pipfile``
  for you in your project's directory. The :ref:`Pipfile` is used to track which
  dependencies your project needs in case you need to re-install them, such as
  when you share your project with others. You should get output similar to this
  (although the exact paths shown will vary):

  .. code-block:: text

      Creating a Pipfile for this project...
      Creating a virtualenv for this project...
      Using base prefix '/usr/local/Cellar/python3/3.6.2/Frameworks/Python.framework/Versions/3.6'
      New python executable in ~/.local/share/virtualenvs/tmp-agwWamBd/bin/python3.6
      Also creating executable in ~/.local/share/virtualenvs/tmp-agwWamBd/bin/python
      Installing setuptools, pip, wheel...done.

      Virtualenv location: ~/.local/share/virtualenvs/tmp-agwWamBd
      Installing requests...
      Collecting requests
        Using cached requests-2.18.4-py2.py3-none-any.whl
      Collecting idna<2.7,>=2.5 (from requests)
        Using cached idna-2.6-py2.py3-none-any.whl
      Collecting urllib3<1.23,>=1.21.1 (from requests)
        Using cached urllib3-1.22-py2.py3-none-any.whl
      Collecting chardet<3.1.0,>=3.0.2 (from requests)
        Using cached chardet-3.0.4-py2.py3-none-any.whl
      Collecting certifi>=2017.4.17 (from requests)
        Using cached certifi-2017.7.27.1-py2.py3-none-any.whl
      Installing collected packages: idna, urllib3, chardet, certifi, requests
      Successfully installed certifi-2017.7.27.1 chardet-3.0.4 idna-2.6 requests-2.18.4 urllib3-1.22

      Adding requests to Pipfile's [packages]...

.. _Requests: https://pypi.org/project/requests/


使用已安装的软件包
------------------------

**Using installed packages**

.. tab:: 中文

  现在， `Requests` 已经安装，你可以创建一个简单的 :file:`main.py` 文件来使用它：

  .. code-block:: python

      import requests

      response = requests.get('https://httpbin.org/ip')

      print('Your IP is {0}'.format(response.json()['origin']))

  然后，你可以使用 ``pipenv run`` 运行这个脚本：

  .. code-block:: bash

      pipenv run python main.py

  你应该看到类似下面的输出：

  .. code-block:: text

      Your IP is 8.8.8.8

  使用 ``pipenv run`` 可以确保你的安装包在脚本中可用。你也可以通过使用 ``pipenv shell`` 启动一个新的 shell，这样所有命令都能访问到你安装的包。

.. tab:: 英文

  Now that Requests is installed you can create a simple :file:`main.py` file
  to use it:

  .. code-block:: python

      import requests

      response = requests.get('https://httpbin.org/ip')

      print('Your IP is {0}'.format(response.json()['origin']))

  Then you can run this script using ``pipenv run``:

  .. code-block:: bash

      pipenv run python main.py

  You should get output similar to this:

  .. code-block:: text

      Your IP is 8.8.8.8

  Using ``pipenv run`` ensures that your installed packages are available to
  your script. It's also possible to spawn a new shell that ensures all commands
  have access to your installed packages with ``pipenv shell``.


后续步骤
----------

**Next steps**

.. tab:: 中文

  恭喜你，现在你已经掌握了如何在协作型 Python 项目中有效地管理依赖和开发环境！✨ 🍰 ✨

  如果你有兴趣创建并分发你自己的 Python 包，可以参考 :ref:`打包和分发包的教程 <distributing-packages>`。

  请注意，当你的应用程序包含 Python 源包定义时，你可以通过 ``pipenv install -e <相对路径到源目录>`` 将它们（及其依赖）添加到你的 ``pipenv`` 环境中（例如 ``pipenv install -e .`` 或 ``pipenv install -e src``）。

.. tab:: 英文

  Congratulations, you now know how to effectively manage dependencies and
  development environments on a collaborative Python project! ✨ 🍰 ✨

  If you're interested in creating and distributing your own Python packages, see
  the :ref:`tutorial on packaging and distributing packages <distributing-packages>`.

  Note that when your application includes definitions of Python source packages,
  they (and their dependencies) can be added to your ``pipenv`` environment with
  ``pipenv install -e <relative-path-to-source-directory>`` (e.g.
  ``pipenv install -e .`` or ``pipenv install -e src``).


.. _other-dependency-management-tools:

其他应用程序依赖项管理工具
-------------------------------------------------

**Other Tools for Application Dependency Management**

.. tab:: 中文

  如果你发现这种管理应用程序依赖的方法不适合你或你的使用场景，你可以探索以下其他工具和技术，按字母顺序列出，看看其中是否有更适合的工具：

  * `hatch <https://github.com/pypa/hatch>`_：一个偏向于项目管理工作流更多步骤的工具，包括版本递增和从项目模板创建新项目骨架。
  * `micropipenv <https://github.com/thoth-station/micropipenv>`_：一个轻量级的 pip 包装器，支持 ``requirements.txt``、Pipenv 和 Poetry 锁定文件，或将其转换为兼容 pip-tools 的输出。专为容器化 Python 应用设计，但不限于此。
  * `PDM <https://github.com/pdm-project/pdm>`_：一种依赖于 :pep:`517` 和 :pep:`621` 等标准的现代 Python 包管理工具。
  * `pip-tools <https://github.com/jazzband/pip-tools>`_：从项目中直接使用的包列表创建所有依赖项的锁定文件，并确保只安装那些依赖项。
  * `Poetry <https://github.com/python-poetry/poetry>`_：与 Pipenv 相似的工具，更多地关注于结构化为可分发 Python 包的项目，要求有有效的 ``pyproject.toml`` 文件。与之相对，Pipenv 明确避免假设正在处理的应用程序将支持作为 ``pip`` 可安装的 Python 包进行分发。

.. tab:: 英文

  If you find this particular approach to managing application dependencies isn't
  working well for you or your use case, you may want to explore these other tools
  and techniques, listed in alphabetical order, to see if one of them is a better fit:

  * `hatch <https://github.com/pypa/hatch>`_ for opinionated coverage of even
    more steps in the project management workflow, such as incrementing versions
    and creating new skeleton projects from project templates.
  * `micropipenv <https://github.com/thoth-station/micropipenv>`_ for a lightweight
    wrapper around pip that supports ``requirements.txt``, Pipenv and Poetry lock files,
    or converting them to pip-tools compatible output. Designed for containerized
    Python applications, but not limited to them.
  * `PDM <https://github.com/pdm-project/pdm>`_ for a modern Python package
    management relying on standards such as :pep:`517` and :pep:`621`.
  * `pip-tools <https://github.com/jazzband/pip-tools>`_ for creating a lock file of all
    dependencies from a list of packages directly used in a project, and ensuring that
    only those dependencies are installed.
  * `Poetry <https://github.com/python-poetry/poetry>`__ for a tool comparable in scope
    to Pipenv that focuses more directly on use cases where the project being managed is
    structured as a distributable Python package with a valid ``pyproject.toml`` file.
    By contrast, Pipenv explicitly avoids making the assumption that the application
    being worked on will support distribution as a ``pip``-installable Python package.
