.. _`PyPI mirrors and caches`:

================================
包索引镜像和缓存
================================

**Package index mirrors and caches**

.. tab:: 中文

  :页面状态: 不完整(Incomplete)
  :最后更新: 2023-11-08

  PyPI（以及其他 :term:`包索引 <Package Index>`）的镜像或缓存可以用来加速本地包的安装，
  支持离线工作，处理公司防火墙问题或应对网络不稳定。

  这一领域有多种选择类型：

  1. 包索引的本地/托管缓存。

  2. 包索引的本地/托管镜像。镜像是包索引的（完整或部分）副本，可以用来替代原始的索引。

  3. 具有回退到公共包索引功能的私人包索引（例如，用于缓解依赖混淆攻击），也称为代理。

.. tab:: 英文

  :Page Status: Incomplete
  :Last Reviewed: 2023-11-08

  Mirroring or caching of PyPI (and other
  :term:`package indexes <Package Index>`) can be used to speed up local
  package installation,
  allow offline work, handle corporate firewalls or just plain Internet flakiness.

  There are multiple classes of options in this area:

  1. local/hosted caching of package indexes.

  2. local/hosted mirroring of a package index. A mirror is a (whole or
     partial) copy of a package index, which can be used in place of the
     original index.

  3. private package index with fall-through to public package indexes (for
     example, to mitigate dependency confusion attacks), also known as a
     proxy.


使用 pip 缓存
----------------

**Caching with pip**

.. tab:: 中文

  pip 提供了多种加速安装的方式，通过使用本地缓存的 :term:`包 <Distribution Package>` 副本：

  1. :ref:`快速和本地安装 <pip:installing from local packages>`，通过下载项目所需的所有依赖项，然后让 pip 使用这些下载的文件，而不是去 PyPI。

  2. 上述方法的变体，通过使用 :ref:`python3 -m pip wheel <pip:pip wheel>` 预构建安装文件：

     .. code-block:: bash

        python3 -m pip wheel --wheel-dir=/tmp/wheelhouse SomeProject
        python3 -m pip install --no-index --find-links=/tmp/wheelhouse SomeProject

.. tab:: 英文

  pip provides a number of facilities for speeding up installation by using local
  cached copies of :term:`packages <Distribution Package>`:

  1. :ref:`Fast & local installs <pip:installing from local packages>`
     by downloading all the requirements for a project and then pointing pip at
     those downloaded files instead of going to PyPI.
  2. A variation on the above which pre-builds the installation files for
    the requirements using :ref:`python3 -m pip wheel <pip:pip wheel>`:

     .. code-block:: bash

        python3 -m pip wheel --wheel-dir=/tmp/wheelhouse SomeProject
        python3 -m pip install --no-index --find-links=/tmp/wheelhouse SomeProject


现有项目
-----------------

**Existing projects**

.. tab:: 中文

  .. list-table::
    :header-rows: 1

    * - 项目
      - 缓存
      - 镜像
      - 代理
      - 备注

    * - :ref:`devpi`
      - ✔
      - ✔
      -
      - 具有继承性的多个索引；同步、复制、故障转移；包上传

    * - :ref:`bandersnatch`
      - ✔
      - ✔
      -
      -

    * - :ref:`simpleindex`
      -
      -
      - ✔
      - 自定义插件启用缓存；重新路由到其他包索引

    * - :ref:`pypicloud`
      - ✔
      -
      - ✔
      - 未维护；身份验证，授权

    * - :ref:`pulppython`
      -
      - ✔
      - ✔
      - Pulp 插件；多个代理索引；包上传

    * - :ref:`proxpi`
      - ✔
      -
      - ✔
      - 多个代理索引

    * - :ref:`nginx_pypi_cache`
      - ✔
      -
      - ✔
      - 多个代理索引

    * - :ref:`flaskpypiproxy`
      - ✔
      -
      - ✔
      - 无人维护

    * - `Apache <https://httpd.apache.org/>`_
      - ✔
      -
      - ✔
      - 使用 `mod_rewrite <https://httpd.apache.org/docs/current/mod/mod_rewrite.html>`_ 和 `mod_cache_disk <https://httpd.apache.org/docs/current/mod/mod_cache_disk.html>`_，你可以通过 Apache 服务器缓存对包索引的请求    

.. tab:: 英文

  .. list-table::
    :header-rows: 1

    * - Project
      - Cache
      - Mirror
      - Proxy
      - Additional notes

    * - :ref:`devpi`
      - ✔
      - ✔
      -
      - multiple indexes with inheritance; syncing, replication, fail-over;
        package upload

    * - :ref:`bandersnatch`
      - ✔
      - ✔
      -
      -

    * - :ref:`simpleindex`
      -
      -
      - ✔
      - custom plugin enables caching; re-routing to other package indexes

    * - :ref:`pypicloud`
      - ✔
      -
      - ✔
      - unmaintained; authentication, authorisation

    * - :ref:`pulppython`
      -
      - ✔
      - ✔
      - plugin for Pulp; multiple proxied indexes; package upload

    * - :ref:`proxpi`
      - ✔
      -
      - ✔
      - multiple proxied indexes

    * - :ref:`nginx_pypi_cache`
      - ✔
      -
      - ✔
      - multiple proxied indexes

    * - :ref:`flaskpypiproxy`
      - ✔
      -
      - ✔
      - unmaintained

    * - `Apache <https://httpd.apache.org/>`_
      - ✔
      -
      - ✔
      - using
        `mod_rewrite
        <https://httpd.apache.org/docs/current/mod/mod_rewrite.html>`_
        and
        `mod_cache_disk
        <https://httpd.apache.org/docs/current/mod/mod_cache_disk.html>`_,
        you can cache requests to package indexes through an Apache server
