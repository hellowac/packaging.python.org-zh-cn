.. _installing-stand-alone-command-line-tools:

安装独立的命令行工具
=========================================

**Installing stand alone command line tools**

.. tab:: 中文

  许多包提供命令行应用程序。例如， `mypy <https://github.com/python/mypy>`_ 、`flake8 <https://github.com/PyCQA/flake8>`_ 、 `black <https://github.com/psf/black>`_ 和 :ref:`pipenv` 都是这样的包。

  通常，您希望能够在系统的任何位置访问这些应用程序，但将包及其依赖项安装到同一个全局环境中可能会导致版本冲突，并破坏操作系统对 Python 包的依赖。

  :ref:`pipx` 通过为每个包创建一个虚拟环境来解决这个问题，同时确保其应用程序可以通过一个在 ``$PATH`` 中的目录进行访问。这使得每个包都可以升级或卸载，而不会与其他包发生冲突，并允许您从任何地方安全地运行这些应用程序。

  .. note:: pipx 仅适用于 Python 3.6 及更高版本。

  您可以使用 pip 安装 pipx：

  .. tab:: Unix/macOS

    .. code-block:: bash

        python3 -m pip install --user pipx
        python3 -m pipx ensurepath

  .. tab:: Windows

    .. code-block:: bat

        py -m pip install --user pipx
        py -m pipx ensurepath

  .. note::

    ``ensurepath`` 确保应用程序目录位于您的 ``$PATH`` 中。
    您可能需要重新启动终端才能使此更新生效。

  现在，您可以使用 ``pipx install`` 安装包，并从任何地方运行该包的应用程序。

  .. code-block:: console

    $ pipx install PACKAGE
    $ PACKAGE_APPLICATION [ARGS]

  例如：

  .. code-block:: console

    $ pipx install cowsay
      installed package cowsay 6.1, installed using Python 3.12.2
      These apps are now globally available
        - cowsay
    done! ✨ 🌟 ✨
    $ cowsay -t moo
      ___
    < moo >
      ===
            \
            \
              ^__^
              (oo)\_______
              (__)\       )\/
                  ||     ||
                  ||----w |

  要查看通过 pipx 安装的包以及可用的应用程序列表，可以使用 ``pipx list``：

  .. code-block:: console

    $ pipx list
    venvs are in /Users/user/Library/Application Support/pipx/venvs
    apps are exposed on your $PATH at /Users/user/.local/bin
    manual pages are exposed at /Users/user/.local/share/man
      package black 24.2.0, installed using Python 3.12.2
        - black
        - blackd
      package cowsay 6.1, installed using Python 3.12.2
        - cowsay
      package mypy 1.9.0, installed using Python 3.12.2
        - dmypy
        - mypy
        - mypyc
        - stubgen
        - stubtest
      package nox 2024.3.2, installed using Python 3.12.2
        - nox
        - tox-to-nox

  要升级或卸载包：

  .. code-block:: bash

    pipx upgrade PACKAGE
    pipx uninstall PACKAGE

  您可以使用 pip 升级或卸载 pipx：

  .. tab:: Unix/macOS

    .. code-block:: bash

        python3 -m pip install --upgrade pipx
        python3 -m pip uninstall pipx

  .. tab:: Windows

    .. code-block:: bat

        py -m pip install --upgrade pipx
        py -m pip uninstall pipx

  pipx 还允许您在一个临时、短命的环境中安装并运行应用程序的最新版本。例如：

  .. code-block:: bash

    pipx run cowsay -t moooo

  要查看 pipx 提供的所有命令，可以运行：

  .. code-block:: bash

    pipx --help

  您可以在 https://pipx.pypa.io/ 上了解有关 pipx 的更多信息。

.. tab:: 英文

  Many packages provide command line applications. Examples of such packages are
  `mypy <https://github.com/python/mypy>`_,
  `flake8 <https://github.com/PyCQA/flake8>`_,
  `black <https://github.com/psf/black>`_, and
  :ref:`pipenv`.

  Usually you want to be able to access these applications from anywhere on your
  system, but installing packages and their dependencies to the same global
  environment can cause version conflicts and break dependencies the operating
  system has on Python packages.

  :ref:`pipx` solves this by creating a virtual environment for each package,
  while also ensuring that its applications are accessible through a directory
  that is on your ``$PATH``. This allows each package to be upgraded or
  uninstalled without causing conflicts with other packages, and allows you to
  safely run the applications from anywhere.

  .. note:: pipx only works with Python 3.6+.

  pipx is installed with pip:

  .. tab:: Unix/macOS

    .. code-block:: bash

        python3 -m pip install --user pipx
        python3 -m pipx ensurepath

  .. tab:: Windows

    .. code-block:: bat

        py -m pip install --user pipx
        py -m pipx ensurepath

  .. note::

    ``ensurepath`` ensures that the application directory is on your ``$PATH``.
    You may need to restart your terminal for this update to take effect.

  Now you can install packages with ``pipx install`` and run the package's
  applications(s) from anywhere.

  .. code-block:: console

    $ pipx install PACKAGE
    $ PACKAGE_APPLICATION [ARGS]

  For example:

  .. code-block:: console

    $ pipx install cowsay
      installed package cowsay 6.1, installed using Python 3.12.2
      These apps are now globally available
        - cowsay
    done! ✨ 🌟 ✨
    $ cowsay -t moo
      ___
    < moo >
      ===
            \
            \
              ^__^
              (oo)\_______
              (__)\       )\/
                  ||     ||
                  ||----w |


  To see a list of packages installed with pipx and which applications are
  available, use ``pipx list``:

  .. code-block:: console

    $ pipx list
    venvs are in /Users/user/Library/Application Support/pipx/venvs
    apps are exposed on your $PATH at /Users/user/.local/bin
    manual pages are exposed at /Users/user/.local/share/man
      package black 24.2.0, installed using Python 3.12.2
        - black
        - blackd
      package cowsay 6.1, installed using Python 3.12.2
        - cowsay
      package mypy 1.9.0, installed using Python 3.12.2
        - dmypy
        - mypy
        - mypyc
        - stubgen
        - stubtest
      package nox 2024.3.2, installed using Python 3.12.2
        - nox
        - tox-to-nox

  To upgrade or uninstall a package:

  .. code-block:: bash

    pipx upgrade PACKAGE
    pipx uninstall PACKAGE

  pipx can be upgraded or uninstalled with pip:

  .. tab:: Unix/macOS

    .. code-block:: bash

        python3 -m pip install --upgrade pipx
        python3 -m pip uninstall pipx

  .. tab:: Windows

    .. code-block:: bat

        py -m pip install --upgrade pipx
        py -m pip uninstall pipx

  pipx also allows you to install and run the latest version of an application
  in a temporary, ephemeral environment. For example:

  .. code-block:: bash

    pipx run cowsay -t moooo

  To see the full list of commands pipx offers, run:

  .. code-block:: bash

    pipx --help

  You can learn more about pipx at https://pipx.pypa.io/.
