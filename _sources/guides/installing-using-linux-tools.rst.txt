.. _`Installing pip/setuptools/wheel with Linux Package Managers`:

===========================================================
使用 Linux 包管理器安装 pip/setuptools/wheel
===========================================================

**Installing pip/setuptools/wheel with Linux Package Managers**

:Page Status: Incomplete
:Last Reviewed: 2021-07-26

.. tab:: 中文

  本节介绍如何使用 Linux 包管理器安装 :ref:`pip` 、:ref:`setuptools` 和 :ref:`wheel` 。

  如果您使用的是从 `python.org <https://www.python.org>`_ 下载的 Python，那么本节内容不适用。请参阅 :ref:`installing_requirements` 部分。

  请注意，特定 Linux 发行版所支持的 :ref:`pip`、:ref:`setuptools` 和 :ref:`wheel` 版本通常会在发布时已经过时，更新通常仅会出于安全原因进行，而不会进行功能更新。对于某些发行版，可以启用额外的仓库来提供更新版本。我们所知道的这些仓库将在下文中解释。

  还需注意，某些发行版通常会为了安全性和标准化应用补丁。这可能会导致与原始未打补丁版本不同的错误或意外行为。如果已知存在这种情况，我们会在下文中说明。

.. tab:: 英文

  This section covers how to install :ref:`pip`, :ref:`setuptools`, and
  :ref:`wheel` using Linux package managers.

  If you're using a Python that was downloaded from `python.org
  <https://www.python.org>`_, then this section does not apply.  See the
  :ref:`installing_requirements` section instead.

  Note that it's common for the versions of :ref:`pip`, :ref:`setuptools`, and
  :ref:`wheel` supported by a specific Linux Distribution to be outdated by the
  time it's released to the public, and updates generally only occur for security
  reasons, not for feature updates.  For certain Distributions, there are
  additional repositories that can be enabled to provide newer versions.  The
  repositories we know about are explained below.

  Also note that it's somewhat common for Distributions to apply patches for the
  sake of security and normalization to their own standards.  In some cases, this
  can lead to bugs or unexpected behaviors that vary from the original unpatched
  versions.  When this is known, we will make note of it below.


Fedora
~~~~~~

.. tab:: 中文

  command:

  .. code-block:: bash

     sudo dnf install python3-pip python3-wheel

  要了解有关 Fedora 中 Python 的更多信息，请访问 `官方 Fedora 文档 <https://developer.fedoraproject.org/tech/languages/python/python-installation.html>`_、 `Python 课堂 <https://labs.fedoraproject.org/en/python-classroom/>`_ 或 `Fedora Loves Python`_。

.. tab:: 英文

  command:

  .. code-block:: bash

    sudo dnf install python3-pip python3-wheel

  To learn more about Python in Fedora, please visit the `official Fedora docs`_,
  `Python Classroom`_ or `Fedora Loves Python`_.

.. _official Fedora docs: https://developer.fedoraproject.org/tech/languages/python/python-installation.html
.. _Python Classroom: https://labs.fedoraproject.org/en/python-classroom/
.. _Fedora Loves Python: https://fedoralovespython.org

CentOS/RHEL
~~~~~~~~~~~

.. tab:: 中文

  CentOS 和 RHEL 在其核心仓库中不提供 :ref:`pip` 或 :ref:`wheel`，虽然 :ref:`setuptools` 是默认安装的。

  要为系统 Python 安装 pip 和 wheel，有两种选择：

  1. 使用 `EPEL 仓库 <https://fedoraproject.org/wiki/EPEL>`_ ，按照 `这些说明 <https://docs.fedoraproject.org/en-US/epel/getting-started/>`__ 启用它。在 EPEL 7 上，您可以通过以下方式安装 pip 和 wheel：

    .. code-block:: bash

      sudo dnf install python3-pip python3-wheel

    由于 EPEL 仅提供额外的不冲突包，因此 EPEL 不提供 setuptools，因为它已经包含在核心仓库中。

  2. 使用 `PyPA Copr Repo <https://copr.fedorainfracloud.org/coprs/pypa/pypa/>`_，按照 `这些说明 <https://fedoraproject.org/wiki/Infrastructure/Fedorahosted-retirement>`__ [1]_ 启用它。然后，您可以通过以下方式安装 pip 和 wheel：

    .. code-block:: bash

      sudo dnf install python3-pip python3-wheel

    要额外升级 setuptools，可以运行：

    .. code-block:: bash

      sudo dnf upgrade python3-setuptools

  要在一个并行的非系统环境中安装 pip、wheel 和 setuptools（使用 yum），有两种选择：

  1. 使用 "软件集合"（Software Collections）功能启用一个包含 pip、setuptools 和 wheel 的并行集合。

     * 对于 Redhat，请参见：https://developers.redhat.com/products/softwarecollections/overview
     * 对于 CentOS，请参见：https://github.com/sclorg
 
     请注意，集合中可能不包含最新版本。

  2. 启用 `IUS 仓库 <https://ius.io/setup>`_，并安装一个 `可并行安装的 <https://ius.io/usage#parallel-installable-packages>`_ Python 版本，同时安装 pip、setuptools 和 wheel，这些版本通常保持较新。

     例如，在 CentOS7/RHEL7 上安装 Python 3.4：

     .. code-block:: bash

        sudo yum install python34u python34u-wheel

.. tab:: 英文

  CentOS and RHEL don't offer :ref:`pip` or :ref:`wheel` in their core repositories,
  although :ref:`setuptools` is installed by default.

  To install pip and wheel for the system Python, there are two options:

  1. Enable the `EPEL repository <https://fedoraproject.org/wiki/EPEL>`_ using
    `these instructions
    <https://docs.fedoraproject.org/en-US/epel/getting-started/>`__.
    On EPEL 7, you can install pip and wheel like so:

    .. code-block:: bash

      sudo dnf install python3-pip python3-wheel

    Since EPEL only offers extra, non-conflicting packages, EPEL does not offer
    setuptools, since it's in the core repository.


  2. Enable the `PyPA Copr Repo
    <https://copr.fedorainfracloud.org/coprs/pypa/pypa/>`_ using `these instructions
    <https://fedoraproject.org/wiki/Infrastructure/Fedorahosted-retirement>`__ [1]_. You can install
    pip and wheel like so:

    .. code-block:: bash

      sudo dnf install python3-pip python3-wheel

    To additionally upgrade setuptools, run:

    .. code-block:: bash

      sudo dnf upgrade python3-setuptools


  To install pip, wheel, and setuptools, in a parallel, non-system environment
  (using yum) then there are two options:


  1. Use the "Software Collections" feature to enable a parallel collection that
    includes pip, setuptools, and wheel.

    * For Redhat, see here:
      https://developers.redhat.com/products/softwarecollections/overview
    * For CentOS, see here: https://github.com/sclorg

    Be aware that collections may not contain the most recent versions.

  2. Enable the `IUS repository <https://ius.io/setup>`_ and
    install one of the `parallel-installable
    <https://ius.io/usage#parallel-installable-packages>`_
    Pythons, along with pip, setuptools, and wheel, which are kept fairly up to
    date.

    For example, for Python 3.4 on CentOS7/RHEL7:

    .. code-block:: bash

      sudo yum install python34u python34u-wheel


openSUSE
~~~~~~~~

.. code-block:: bash

   sudo zypper install python3-pip python3-setuptools python3-wheel


.. _debian-ubuntu:

Debian/Ubuntu and derivatives
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. tab:: 中文

  首先，通过运行以下命令来更新和刷新仓库列表：

  .. code-block:: bash

    sudo apt update
    sudo apt install python3-venv python3-pip

  .. warning::

    最近的 Debian/Ubuntu 版本已修改 pip，默认使用 `"User Scheme" <https://pip.pypa.io/en/stable/user_guide/#user-installs>`_，这是一个重要的行为变化，可能会让一些用户感到惊讶。

.. tab:: 英文

  Firstly, update and refresh repository lists by running this command:

  .. code-block:: bash

     sudo apt update
     sudo apt install python3-venv python3-pip

  .. warning::

    Recent Debian/Ubuntu versions have modified pip to use the `"User Scheme"
    <https://pip.pypa.io/en/stable/user_guide/#user-installs>`_ by default, which
    is a significant behavior change that can be surprising to some users.


Arch Linux
~~~~~~~~~~

.. code-block:: bash

   sudo pacman -S python-pip

----

.. [1] Currently, there is no "copr" yum plugin available for CentOS/RHEL, so
       the only option is to manually place the repo files as described.
