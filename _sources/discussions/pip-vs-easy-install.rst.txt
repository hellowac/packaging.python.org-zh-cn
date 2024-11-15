
.. _`pip vs easy_install`:

===================
pip vs easy_install
===================

.. tab:: 中文

    :ref:`easy_install <easy_install>`，现已 `deprecated`_，于 2004 年作为 :ref:`setuptools` 的一部分发布。它当时的一个显著特点是使用需求说明符从 :term:`PyPI <Python Package Index (PyPI)>` 安装 :term:`包 <Distribution Package>`，并自动安装依赖项。

    `:ref:`pip` 于 2008 年发布，作为 `easy_install <easy_install>` 的替代工具，尽管它仍然主要构建在 :ref:`setuptools` 组件之上。pip 当时的一个显著特点是 *不* 以 :term:`Eggs <Egg>` 格式安装包，也不从 :term:`Eggs <Egg>` 安装（而是直接从 :term:`sdists <Source Distribution (or "sdist")>` 安装），并引入了 :ref:`Requirements Files <pip:Requirements Files>` 的概念，赋予用户轻松复制环境的能力。

    以下是 pip 和已弃用的 easy_install 之间的重要差异：

    +------------------------------+--------------------------------------+-------------------------------+
    |                              | **pip**                              | **easy_install**              |
    +------------------------------+--------------------------------------+-------------------------------+
    |Installs from :term:`Wheels   |Yes                                   |No                             |
    |<Wheel>`                      |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |Uninstall Packages            |Yes (``python -m pip uninstall``)     |No                             |
    +------------------------------+--------------------------------------+-------------------------------+
    |Dependency Overrides          |Yes (:ref:`Requirements Files         |No                             |
    |                              |<pip:Requirements Files>`)            |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |List Installed Packages       |Yes (``python -m pip list`` and       |No                             |
    |                              |``python -m pip freeze``)             |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |:pep:`438`                    |Yes                                   |No                             |
    |Support                       |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |Installation format           |'Flat' packages with :file:`egg-info` | Encapsulated Egg format       |
    |                              |metadata.                             |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |sys.path modification         |No                                    |Yes                            |
    |                              |                                      |                               |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |Installs from :term:`Eggs     |No                                    |Yes                            |
    |<Egg>`                        |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |`pylauncher support`_         |No                                    |Yes [1]_                       |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |:ref:`Multi-version Installs` |No                                    |Yes                            |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |Exclude scripts during install|No                                    |Yes                            |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |per project index             |Only in virtualenv                    |Yes, via setup.cfg             |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+

.. tab:: 英文

    :ref:`easy_install <easy_install>`, now `deprecated`_, was released in 2004 as part of :ref:`setuptools`.
    It was notable at the time for installing :term:`packages <Distribution Package>` from
    :term:`PyPI <Python Package Index (PyPI)>` using requirement specifiers, and
    automatically installing dependencies.

    :ref:`pip` came later in 2008, as alternative to :ref:`easy_install <easy_install>`, although still
    largely built on top of :ref:`setuptools` components.  It was notable at the
    time for *not* installing packages as :term:`Eggs <Egg>` or from :term:`Eggs <Egg>` (but
    rather simply as 'flat' packages from :term:`sdists <Source Distribution (or
    "sdist")>`), and introducing the idea of :ref:`Requirements Files
    <pip:Requirements Files>`, which gave users the power to easily replicate
    environments.

    Here's a breakdown of the important differences between pip and the deprecated easy_install:

    +------------------------------+--------------------------------------+-------------------------------+
    |                              | **pip**                              | **easy_install**              |
    +------------------------------+--------------------------------------+-------------------------------+
    |Installs from :term:`Wheels   |Yes                                   |No                             |
    |<Wheel>`                      |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |Uninstall Packages            |Yes (``python -m pip uninstall``)     |No                             |
    +------------------------------+--------------------------------------+-------------------------------+
    |Dependency Overrides          |Yes (:ref:`Requirements Files         |No                             |
    |                              |<pip:Requirements Files>`)            |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |List Installed Packages       |Yes (``python -m pip list`` and       |No                             |
    |                              |``python -m pip freeze``)             |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |:pep:`438`                    |Yes                                   |No                             |
    |Support                       |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |Installation format           |'Flat' packages with :file:`egg-info` | Encapsulated Egg format       |
    |                              |metadata.                             |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |sys.path modification         |No                                    |Yes                            |
    |                              |                                      |                               |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |Installs from :term:`Eggs     |No                                    |Yes                            |
    |<Egg>`                        |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |`pylauncher support`_         |No                                    |Yes [1]_                       |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |:ref:`Multi-version Installs` |No                                    |Yes                            |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |Exclude scripts during install|No                                    |Yes                            |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+
    |per project index             |Only in virtualenv                    |Yes, via setup.cfg             |
    |                              |                                      |                               |
    +------------------------------+--------------------------------------+-------------------------------+

----

.. _deprecated: https://setuptools.readthedocs.io/en/latest/history.html#v42-0-0

.. [1] https://setuptools.readthedocs.io/en/latest/deprecated/easy_install.html#natural-script-launcher


.. _pylauncher support: https://bitbucket.org/vinay.sajip/pylauncher
