.. _`Hosting your Own Simple Repository`:

==================================
托管您自己的简单存储库
==================================

**Hosting your own simple repository**

.. tab:: 中文

  如果你希望托管自己的简单仓库 [1]_, 你可以使用像 :doc:`devpi <devpi:index>` 这样的软件包，或者你可以简单地创建适当的目录结构，并使用任何能够提供静态文件和生成自动索引的 web 服务器。

  无论哪种方式，由于你将托管的仓库很可能不在用户的默认仓库中，你应该在项目描述中指导他们正确配置安装工具。例如，使用 pip：

  .. tab:: Unix/macOS

      .. code-block:: bash

          python3 -m pip install --extra-index-url https://python.example.com/ foobar

  .. tab:: Windows

      .. code-block:: bat

          py -m pip install --extra-index-url https://python.example.com/ foobar

  此外， **强烈** 建议你使用有效的 HTTPS 来提供你的仓库。目前，用户安装的安全性依赖于所有仓库都使用有效的 HTTPS 设置。

.. tab:: 英文

  If you wish to host your own simple repository [2]_, you can either use a
  software package like :doc:`devpi <devpi:index>` or you can simply create the proper
  directory structure and use any web server that can serve static files and
  generate an autoindex.

  In either case, since you'll be hosting a repository that is likely not in
  your user's default repositories, you should instruct them in your project's
  description to configure their installer appropriately. For example with pip:

  .. tab:: Unix/macOS

      .. code-block:: bash

          python3 -m pip install --extra-index-url https://python.example.com/ foobar

  .. tab:: Windows

      .. code-block:: bat

          py -m pip install --extra-index-url https://python.example.com/ foobar

  In addition, it is **highly** recommended that you serve your repository with
  valid HTTPS. At this time, the security of your user's installations depends on
  all repositories using a valid HTTPS setup.


“手动”存储库
===================

**"Manual" repository**

.. tab:: 中文

  目录布局相当简单，在根目录下，你需要为每个项目创建一个目录。这个目录应该是项目的 :ref:`标准化名称 <name-normalization>`。在每个这些目录中，只需放置每个可下载的文件。如果你有项目 "Foo"（版本为 1.0 和 2.0）和 "bar"（版本为 0.1），最终的目录结构应该像这样::

      .
      ├── bar
      │   └── bar-0.1.tar.gz
      └── foo
          ├── Foo-1.0.tar.gz
          └── Foo-2.0.tar.gz

  一旦你有了这样的布局，只需配置你的 web 服务器来提供根目录，并启用自动索引。以使用 `Twisted`_ 内置 Web 服务器为例，你只需运行 ``twistd -n web --path .``，然后指示用户将 URL 添加到他们安装器的配置中。

.. tab:: 英文

  The directory layout is fairly simple, within a root directory you need to
  create a directory for each project. This directory should be the :ref:`normalized name <name-normalization>` of the project. Within each of these directories
  simply place each of the downloadable files. If you have the projects "Foo"
  (with the versions 1.0 and 2.0) and "bar" (with the version 0.1) You should
  end up with a structure that looks like::

      .
      ├── bar
      │   └── bar-0.1.tar.gz
      └── foo
          ├── Foo-1.0.tar.gz
          └── Foo-2.0.tar.gz

  Once you have this layout, simply configure your webserver to serve the root
  directory with autoindex enabled. For an example using the built in Web server
  in `Twisted`_, you would simply run ``twistd -n web --path .`` and then
  instruct users to add the URL to their installer's configuration.


现有项目
=================

**Existing projects**

.. tab:: 中文

  .. list-table::
    :header-rows: 1

    * - 项目
      - 包上传
      - PyPI 失败 [3]_
      - 其他备注

    * - :ref:`devpi`
      - ✔
      - ✔
      - 具有继承、同步、复制、故障转移的多个索引；镜像

    * - :ref:`simpleindex`
      -
      - ✔
      -

    * - :ref:`pypiserver`
      - ✔
      -
      -

    * - :ref:`pypiprivate`
      -
      -
      -

    * - :ref:`pypicloud`
      -
      -
      - 未维护；还有缓存代理；身份验证、授权

    * - :ref:`pywharf`
      -
      -
      - 无人维护； GitHub 中的服务文件

    * - :ref:`pulppython`
      - ✔
      -
      - 还有镜像、代理；Pulp 插件

    * - :ref:`pip2pi`
      -
      -
      - 也可以镜像；手动同步

    * - :ref:`dumb-pypi`
      -
      -
      - 不是服务器，而是静态文件站点生成器

    * - :ref:`httpserver`
      -
      -
      - 标准库

    * - `Apache <https://httpd.apache.org/>`_
      -
      - ✔
      - 使用 `mod_rewrite <https://httpd.apache.org/docs/current/mod/mod_rewrite.html>`_ 和 `mod_cache_disk <https://httpd.apache.org/docs/current/mod/mod_cache_disk.html>`_，你可以通过 Apache 服务器缓存对包索引的请求

.. tab:: 英文

  .. list-table::
    :header-rows: 1

    * - Project
      - Package upload
      - PyPI fall-through [4]_
      - Additional notes

    * - :ref:`devpi`
      - ✔
      - ✔
      - multiple indexes with inheritance, with syncing, replication, fail-over;
        mirroring

    * - :ref:`simpleindex`
      -
      - ✔
      -

    * - :ref:`pypiserver`
      - ✔
      -
      -

    * - :ref:`pypiprivate`
      -
      -
      -

    * - :ref:`pypicloud`
      -
      -
      - unmaintained; also cached proxying; authentication, authorisation

    * - :ref:`pywharf`
      -
      -
      - unmaintained; serve files in GitHub

    * - :ref:`pulppython`
      - ✔
      -
      - also mirroring, proxying; plugin for Pulp

    * - :ref:`pip2pi`
      -
      -
      - also mirroring; manual synchronisation

    * - :ref:`dumb-pypi`
      -
      -
      - not a server, but a static file site generator

    * - :ref:`httpserver`
      -
      -
      - standard-library

    * - `Apache <https://httpd.apache.org/>`_
      -
      - ✔
      - using
        `mod_rewrite
        <https://httpd.apache.org/docs/current/mod/mod_rewrite.html>`_
        and
        `mod_cache_disk
        <https://httpd.apache.org/docs/current/mod/mod_cache_disk.html>`_,
        you can cache requests to package indexes through an Apache server

----

.. [1] For complete documentation of the simple repository protocol, see :ref:`simple repository API <simple-repository-api>`.

.. [2] Can be configured to fall back to PyPI (or another package index) if a requested package is missing.

.. [3] For complete documentation of the simple repository protocol, see :ref:`simple repository API <simple-repository-api>`.

.. [4] Can be configured to fall back to PyPI (or another package index) if a requested package is missing.

.. _Twisted: https://twistedmatrix.com/
