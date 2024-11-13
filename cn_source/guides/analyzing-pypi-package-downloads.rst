.. _analyzing-pypi-package-downloads:

================================
分析 PyPI 软件包下载
================================

**Analyzing PyPI package downloads**

.. tab:: 中文

  本节介绍如何使用公共的 PyPI 下载统计数据集，来了解更多关于托管在 PyPI 上的包（或多个包）的下载情况。例如，您可以使用该数据集来发现用于下载某个包的 Python 版本的分布情况。

.. tab:: 英文

  This section covers how to use the public PyPI download statistics dataset
  to learn more about downloads of a package (or packages) hosted on PyPI. For
  example, you can use it to discover the distribution of Python versions used to
  download a package.


背景
==========

**Background**

.. tab:: 中文

  PyPI 不显示下载统计信息，原因有很多：[#]_

  - **与内容分发网络（CDN）配合使用效率低：**  
    下载统计信息是不断变化的。如果将其包含在项目页面中，而这些页面是高度缓存的，就需要更频繁地使缓存失效，从而降低缓存的整体有效性。

  - **高度不准确：**  
    有许多因素会导致下载计数不准确，其中包括：

    - ``pip`` 的下载缓存（会降低下载计数）
    - 内部或非官方镜像（可能会增加或减少下载计数）
    - 未托管在 PyPI 上的包（用于对比）
    - 非官方脚本或下载计数膨胀的尝试（增加下载计数）
    - 已知的历史数据质量问题（降低下载计数）

  - **不太有用：**  
    一个项目被下载得很频繁并不意味着它很好；同样，一个项目没有被下载很多次也并不意味着它不好！

  简而言之，由于下载统计信息的价值较低，并且使其正常工作所需的权衡成本较高，因此这并不是有限资源的有效使用方式。

.. tab:: 英文

  PyPI does not display download statistics for a number of reasons: [#]_

  - **Inefficient to make work with a Content Distribution Network (CDN):**
    Download statistics change constantly. Including them in project pages, which
    are heavily cached, would require invalidating the cache more often, and
    reduce the overall effectiveness of the cache.

  - **Highly inaccurate:** A number of things prevent the download counts from
    being accurate, some of which include:

    - ``pip``'s download cache (lowers download counts)
    - Internal or unofficial mirrors (can both raise or lower download counts)
    - Packages not hosted on PyPI (for comparisons sake)
    - Unofficial scripts or attempts at download count inflation (raises download
      counts)
    - Known historical data quality issues (lowers download counts)

  - **Not particularly useful:** Just because a project has been downloaded a lot
    doesn't mean it's good; Similarly just because a project hasn't been
    downloaded a lot doesn't mean it's bad!

  In short, because its value is low for various reasons, and the tradeoffs
  required to make it work are high, it has been not an effective use of
  limited resources.

公共数据集
==============

**Public dataset**

.. tab:: 中文

  作为替代方案， `Linehaul 项目 <https://github.com/pypa/linehaul-cloud-function/>`__ 将 PyPI 的下载日志流式传输到 `Google BigQuery`_ [#]_，并将其存储为公开的数据集。

.. tab:: 英文

  As an alternative, the `Linehaul project <https://github.com/pypa/linehaul-cloud-function/>`__
  streams download logs from PyPI to `Google BigQuery`_ [#]_, where they are
  stored as a public dataset.

设置
--------------

**Getting set up**

.. tab:: 中文

  为了使用 `Google BigQuery`_ 查询 `公开的 PyPI 下载统计数据集 <public PyPI download statistics dataset_>`_，你需要一个 Google 账号，并在 Google Cloud Platform 项目中启用 BigQuery API。你可以在每月使用最多 1TB 的查询， `使用 BigQuery 免费套餐而无需信用卡 <https://cloud.google.com/blog/products/data-analytics/query-without-a-credit-card-introducing-bigquery-sandbox>`__。

  - 访问 `BigQuery Web UI`_ 。
  - 创建一个新项目。
  - 启用 `BigQuery API <https://console.developers.google.com/apis/library/bigquery-json.googleapis.com>`__。

  有关如何开始使用 BigQuery 的更详细说明，请参阅 `BigQuery 快速入门指南 <https://cloud.google.com/bigquery/docs/quickstarts/quickstart-web-ui>`__。

.. tab:: 英文

  In order to use `Google BigQuery`_ to query the `public PyPI download statistics dataset`_, you'll need a Google account and to enable the BigQuery API on a Google Cloud Platform project. You can run up to 1TB of queries per month `using the BigQuery free tier without a credit card <https://cloud.google.com/blog/products/data-analytics/query-without-a-credit-card-introducing-bigquery-sandbox>`__

  - Navigate to the `BigQuery web UI`_.
  - Create a new project.
  - Enable the `BigQuery API <https://console.developers.google.com/apis/library/bigquery-json.googleapis.com>`__.

  For more detailed instructions on how to get started with BigQuery, check out the `BigQuery quickstart guide <https://cloud.google.com/bigquery/docs/quickstarts/quickstart-web-ui>`__.


数据模式
-----------

**Data schema**

.. tab:: 中文

  Linehaul 在 ``bigquery-public-data.pypi.file_downloads`` 表中为每个下载写入一条记录。该表包含有关下载了哪个文件以及如何下载的信息。以下是一些来自 `表格模式 <https://console.cloud.google.com/bigquery?pli=1&p=bigquery-public-data&d=pypi&t=file_downloads&page=table>`__ 的有用列：

  +------------------------+-----------------+-----------------------------+
  | 列名                   | 描述            | 示例                        |
  +========================+=================+=============================+
  | timestamp              | 日期和时间      | ``2020-03-09 00:33:03 UTC`` |
  +------------------------+-----------------+-----------------------------+
  | file.project           | 项目名称        | ``pipenv``, ``nose``        |
  +------------------------+-----------------+-----------------------------+
  | file.version           | 包版本          | ``0.1.6``, ``1.4.2``        |
  +------------------------+-----------------+-----------------------------+
  | details.installer.name | 安装器          | pip, :ref:`bandersnatch`    |
  +------------------------+-----------------+-----------------------------+
  | details.python         | Python 版本     | ``2.7.12``, ``3.6.4``       |
  +------------------------+-----------------+-----------------------------+

.. tab:: 英文

  Linehaul writes an entry in a ``bigquery-public-data.pypi.file_downloads`` table for each
  download. The table contains information about what file was downloaded and how
  it was downloaded. Some useful columns from the `table schema
  <https://console.cloud.google.com/bigquery?pli=1&p=bigquery-public-data&d=pypi&t=file_downloads&page=table>`__
  include:

  +------------------------+-----------------+-----------------------------+
  | Column                 | Description     | Examples                    |
  +========================+=================+=============================+
  | timestamp              | Date and time   | ``2020-03-09 00:33:03 UTC`` |
  +------------------------+-----------------+-----------------------------+
  | file.project           | Project name    | ``pipenv``, ``nose``        |
  +------------------------+-----------------+-----------------------------+
  | file.version           | Package version | ``0.1.6``, ``1.4.2``        |
  +------------------------+-----------------+-----------------------------+
  | details.installer.name | Installer       | pip, :ref:`bandersnatch`    |
  +------------------------+-----------------+-----------------------------+
  | details.python         | Python version  | ``2.7.12``, ``3.6.4``       |
  +------------------------+-----------------+-----------------------------+


有用的查询
--------------

**Useful queries**

.. tab:: 中文

  在 `BigQuery web UI`_ 中运行查询，点击“Compose query”按钮。

  请注意，这些行存储在一个分区表中，这有助于限制查询的成本。这些示例查询通过过滤 ``timestamp`` 列来分析最近历史的下载数据。

.. tab:: 英文

  Run queries in the `BigQuery web UI`_ by clicking the "Compose query" button.

  Note that the rows are stored in a partitioned table, which helps
  limit the cost of queries. These example queries analyze downloads from
  recent history by filtering on the ``timestamp`` column.

统计包下载量
~~~~~~~~~~~~~~~~~~~~~~~~~~

**Counting package downloads**

.. tab:: 中文

  以下查询计算项目 "pytest" 的下载总数。

  ::

      #standardSQL
      SELECT COUNT(*) AS num_downloads
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE file.project = 'pytest'
        -- 仅查询过去30天的历史数据
        AND DATE(timestamp)
          BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
          AND CURRENT_DATE()

  +---------------+
  | num_downloads |
  +===============+
  | 26190085      |
  +---------------+

  要仅统计 pip 的下载数，请在 ``details.installer.name`` 列上过滤。

  ::

      #standardSQL
      SELECT COUNT(*) AS num_downloads
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE file.project = 'pytest'
        AND details.installer.name = 'pip'
        -- 仅查询过去30天的历史数据
        AND DATE(timestamp)
          BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
          AND CURRENT_DATE()

  +---------------+
  | num_downloads |
  +===============+
  | 24334215      |
  +---------------+

.. tab:: 英文

  The following query counts the total number of downloads for the project "pytest".

  ::

      #standardSQL
      SELECT COUNT(*) AS num_downloads
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE file.project = 'pytest'
        -- Only query the last 30 days of history
        AND DATE(timestamp)
          BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
          AND CURRENT_DATE()

  +---------------+
  | num_downloads |
  +===============+
  | 26190085      |
  +---------------+

  To count downloads from pip only, filter on the ``details.installer.name``
  column.

  ::

      #standardSQL
      SELECT COUNT(*) AS num_downloads
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE file.project = 'pytest'
        AND details.installer.name = 'pip'
        -- Only query the last 30 days of history
        AND DATE(timestamp)
          BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
          AND CURRENT_DATE()

  +---------------+
  | num_downloads |
  +===============+
  | 24334215      |
  +---------------+

随时间变化的软件包下载
~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Package downloads over time**

.. tab:: 中文

  要按月下载量分组，请使用“TIMESTAMP_TRUNC”函数。通过此列进行筛选还可以降低相应的成本。

  ::

      #standardSQL
      SELECT
        COUNT(*) AS num_downloads,
        DATE_TRUNC(DATE(timestamp), MONTH) AS `month`
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE
        file.project = 'pytest'
        -- Only query the last 6 months of history
        AND DATE(timestamp)
          BETWEEN DATE_TRUNC(DATE_SUB(CURRENT_DATE(), INTERVAL 6 MONTH), MONTH)
          AND CURRENT_DATE()
      GROUP BY `month`
      ORDER BY `month` DESC

  +---------------+------------+
  | num_downloads | month      |
  +===============+============+
  | 1956741       | 2018-01-01 |
  +---------------+------------+
  | 2344692       | 2017-12-01 |
  +---------------+------------+
  | 1730398       | 2017-11-01 |
  +---------------+------------+
  | 2047310       | 2017-10-01 |
  +---------------+------------+
  | 1744443       | 2017-09-01 |
  +---------------+------------+
  | 1916952       | 2017-08-01 |
  +---------------+------------+



.. tab:: 英文

  To group by monthly downloads, use the ``TIMESTAMP_TRUNC`` function. Also
  filtering by this column reduces corresponding costs.

  ::

      #standardSQL
      SELECT
        COUNT(*) AS num_downloads,
        DATE_TRUNC(DATE(timestamp), MONTH) AS `month`
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE
        file.project = 'pytest'
        -- Only query the last 6 months of history
        AND DATE(timestamp)
          BETWEEN DATE_TRUNC(DATE_SUB(CURRENT_DATE(), INTERVAL 6 MONTH), MONTH)
          AND CURRENT_DATE()
      GROUP BY `month`
      ORDER BY `month` DESC

  +---------------+------------+
  | num_downloads | month      |
  +===============+============+
  | 1956741       | 2018-01-01 |
  +---------------+------------+
  | 2344692       | 2017-12-01 |
  +---------------+------------+
  | 1730398       | 2017-11-01 |
  +---------------+------------+
  | 2047310       | 2017-10-01 |
  +---------------+------------+
  | 1744443       | 2017-09-01 |
  +---------------+------------+
  | 1916952       | 2017-08-01 |
  +---------------+------------+

随时间变化的 Python 版本
~~~~~~~~~~~~~~~~~~~~~~~~~

**Python versions over time**

.. tab:: 中文

  从“details.python”列中提取 Python 版本。警告：此查询处理超过 500 GB 的数据。

  ::

      #standardSQL
      SELECT
        REGEXP_EXTRACT(details.python, r"[0-9]+\.[0-9]+") AS python_version,
        COUNT(*) AS num_downloads,
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE
        -- Only query the last 6 months of history
        DATE(timestamp)
          BETWEEN DATE_TRUNC(DATE_SUB(CURRENT_DATE(), INTERVAL 6 MONTH), MONTH)
          AND CURRENT_DATE()
      GROUP BY `python_version`
      ORDER BY `num_downloads` DESC

  +--------+---------------+
  | python | num_downloads |
  +========+===============+
  | 3.7    | 18051328726   |
  +--------+---------------+
  | 3.6    | 9635067203    |
  +--------+---------------+
  | 3.8    | 7781904681    |
  +--------+---------------+
  | 2.7    | 6381252241    |
  +--------+---------------+
  | null   | 2026630299    |
  +--------+---------------+
  | 3.5    | 1894153540    |
  +--------+---------------+

.. tab:: 英文

  Extract the Python version from the ``details.python`` column. Warning: This
  query processes over 500 GB of data.

  ::

      #standardSQL
      SELECT
        REGEXP_EXTRACT(details.python, r"[0-9]+\.[0-9]+") AS python_version,
        COUNT(*) AS num_downloads,
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE
        -- Only query the last 6 months of history
        DATE(timestamp)
          BETWEEN DATE_TRUNC(DATE_SUB(CURRENT_DATE(), INTERVAL 6 MONTH), MONTH)
          AND CURRENT_DATE()
      GROUP BY `python_version`
      ORDER BY `num_downloads` DESC

  +--------+---------------+
  | python | num_downloads |
  +========+===============+
  | 3.7    | 18051328726   |
  +--------+---------------+
  | 3.6    | 9635067203    |
  +--------+---------------+
  | 3.8    | 7781904681    |
  +--------+---------------+
  | 2.7    | 6381252241    |
  +--------+---------------+
  | null   | 2026630299    |
  +--------+---------------+
  | 3.5    | 1894153540    |
  +--------+---------------+


获取工件的绝对链接
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Getting absolute links to artifacts**

.. tab:: 中文

  有时，根据文件的哈希值获取 PyPI 上的下载链接是很有帮助的，例如，如果某个特定的项目或版本已从 PyPI 中删除。元数据表包含了 ``path`` 列，其中包含哈希值和工件文件名。

  .. note::
    这里生成的 URL 并不能保证稳定，但当前与 PyPI 托管工件的 URL 一致。

  ::

      SELECT
        CONCAT('https://files.pythonhosted.org/packages', path) as url
      FROM
        `bigquery-public-data.pypi.distribution_metadata`
      WHERE
        filename LIKE 'sampleproject%'

  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | url                                                                                                                                                               |
  +===================================================================================================================================================================+
  | https://files.pythonhosted.org/packages/eb/45/79be82bdeafcecb9dca474cad4003e32ef8e4a0dec6abbd4145ccb02abe1/sampleproject-1.2.0.tar.gz                             |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | https://files.pythonhosted.org/packages/56/0a/178e8bbb585ec5b13af42dae48b1d7425d6575b3ff9b02e5ec475e38e1d6/sampleproject_nomura-1.2.0-py2.py3-none-any.whl        |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | https://files.pythonhosted.org/packages/63/88/3200eeaf22571f18d2c41e288862502e33365ccbdc12b892db23f51f8e70/sampleproject_nomura-1.2.0.tar.gz                      |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | https://files.pythonhosted.org/packages/21/e9/2743311822e71c0756394b6c5ab15cb64ca66c78c6c6a5cd872c9ed33154/sampleproject_doubleyoung18-1.3.0-py2.py3-none-any.whl |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | https://files.pythonhosted.org/packages/6f/5b/2f3fe94e1c02816fe23c7ceee5292fb186912929e1972eee7fb729fa27af/sampleproject-1.3.1.tar.gz                             |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. tab:: 英文

  It's sometimes helpful to be able to get the absolute links to download
  artifacts from PyPI based on their hashes, e.g. if a particular project or
  release has been deleted from PyPI. The metadata table includes the ``path``
  column, which includes the hash and artifact filename.

  .. note::
    The URL generated here is not guaranteed to be stable, but currently aligns with the URL where PyPI artifacts are hosted.

  ::

      SELECT
        CONCAT('https://files.pythonhosted.org/packages', path) as url
      FROM
        `bigquery-public-data.pypi.distribution_metadata`
      WHERE
        filename LIKE 'sampleproject%'


  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | url                                                                                                                                                               |
  +===================================================================================================================================================================+
  | https://files.pythonhosted.org/packages/eb/45/79be82bdeafcecb9dca474cad4003e32ef8e4a0dec6abbd4145ccb02abe1/sampleproject-1.2.0.tar.gz                             |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | https://files.pythonhosted.org/packages/56/0a/178e8bbb585ec5b13af42dae48b1d7425d6575b3ff9b02e5ec475e38e1d6/sampleproject_nomura-1.2.0-py2.py3-none-any.whl        |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | https://files.pythonhosted.org/packages/63/88/3200eeaf22571f18d2c41e288862502e33365ccbdc12b892db23f51f8e70/sampleproject_nomura-1.2.0.tar.gz                      |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | https://files.pythonhosted.org/packages/21/e9/2743311822e71c0756394b6c5ab15cb64ca66c78c6c6a5cd872c9ed33154/sampleproject_doubleyoung18-1.3.0-py2.py3-none-any.whl |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | https://files.pythonhosted.org/packages/6f/5b/2f3fe94e1c02816fe23c7ceee5292fb186912929e1972eee7fb729fa27af/sampleproject-1.3.1.tar.gz                             |
  +-------------------------------------------------------------------------------------------------------------------------------------------------------------------+


注意事项
=========

**Caveats**

.. tab:: 中文

  除了上述背景中列出的警告之外，Linehaul 还存在一个 bug，导致它在 2018 年 7 月 26 日之前显著低报了下载统计数据。这个日期之前的下载数据在比例上是准确的（例如，Python 2 与 Python 3 的下载比例），但总下载量低于实际值，差距大约是一个数量级。

.. tab:: 英文

  In addition to the caveats listed in the background above, Linehaul suffered
  from a bug which caused it to significantly under-report download statistics
  prior to July 26, 2018. Downloads before this date are proportionally accurate
  (e.g. the percentage of Python 2 vs. Python 3 downloads) but total numbers are
  lower than actual by an order of magnitude.


其他工具
================

**Additional tools**

.. tab:: 中文

  除了使用 BigQuery 控制台之外，还有一些其他工具在分析下载统计数据时可能会非常有用。

.. tab:: 英文

  Besides using the BigQuery console, there are some additional tools which may
  be useful when analyzing download statistics.

``google-cloud-bigquery``
-------------------------

.. tab:: 中文

  您还可以通过 BigQuery API 和 `google-cloud-bigquery`_ 项目（BigQuery 的官方 Python 客户端库）以编程方式访问公开的 PyPI 下载统计数据集。

  .. code-block:: python

      from google.cloud import bigquery

      # 注意：根据代码运行的位置，可能需要额外的身份验证。详情请见：
      # https://cloud.google.com/bigquery/docs/authentication/
      client = bigquery.Client()

      query_job = client.query("""
      SELECT COUNT(*) AS num_downloads
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE file.project = 'pytest'
        -- 仅查询过去 30 天的历史数据
        AND DATE(timestamp)
          BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
          AND CURRENT_DATE()""")

      results = query_job.result()  # 等待任务完成。
      for row in results:
          print("{} 次下载".format(row.num_downloads))

.. tab:: 英文

  You can also access the public PyPI download statistics dataset
  programmatically via the BigQuery API and the `google-cloud-bigquery`_ project,
  the official Python client library for BigQuery.

  .. code-block:: python

      from google.cloud import bigquery

      # Note: depending on where this code is being run, you may require
      # additional authentication. See:
      # https://cloud.google.com/bigquery/docs/authentication/
      client = bigquery.Client()

      query_job = client.query("""
      SELECT COUNT(*) AS num_downloads
      FROM `bigquery-public-data.pypi.file_downloads`
      WHERE file.project = 'pytest'
        -- Only query the last 30 days of history
        AND DATE(timestamp)
          BETWEEN DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
          AND CURRENT_DATE()""")

      results = query_job.result()  # Waits for job to complete.
      for row in results:
          print("{} downloads".format(row.num_downloads))


``pypinfo``
-----------

.. tab:: 中文

  `pypinfo`_ 是一个命令行工具，可以访问该数据集并生成几个有用的查询。例如，您可以使用命令 ``pypinfo package_name`` 查询某个包的总下载次数。

  使用 pip 安装 `pypinfo`_。

  .. code-block:: bash

      python3 -m pip install pypinfo

  用法：

  .. code-block:: console

      $ pypinfo requests
      Served from cache: False
      Data processed: 6.87 GiB
      Data billed: 6.87 GiB
      Estimated cost: $0.04

      | download_count |
      | -------------- |
      |      9,316,415 |

.. tab:: 英文

  `pypinfo`_ is a command-line tool which provides access to the dataset and
  can generate several useful queries. For example, you can query the total
  number of download for a package with the command ``pypinfo package_name``.

  Install `pypinfo`_ using pip.

  .. code-block:: bash

      python3 -m pip install pypinfo

  Usage:

  .. code-block:: console

      $ pypinfo requests
      Served from cache: False
      Data processed: 6.87 GiB
      Data billed: 6.87 GiB
      Estimated cost: $0.04

      | download_count |
      | -------------- |
      |      9,316,415 |


``pandas-gbq``
--------------

.. tab:: 中文

  `pandas-gbq`_ 项目允许通过 `Pandas`_ 访问查询结果。

.. tab:: 英文

  The `pandas-gbq`_ project allows for accessing query results via `Pandas`_.


参考资料
==========

**References**

.. [#] `PyPI 下载计数弃用电子邮件 <https://mail.python.org/pipermail/distutils-sig/2013-May/020855.html>`__
.. [#] `PyPI Download Counts deprecation email <https://mail.python.org/pipermail/distutils-sig/2013-May/020855.html>`__
.. [#] `PyPI BigQuery 数据集公告电子邮件 <https://mail.python.org/pipermail/distutils-sig/2016-May/028986.html>`__
.. [#] `PyPI BigQuery dataset announcement email <https://mail.python.org/pipermail/distutils-sig/2016-May/028986.html>`__

.. _public PyPI download statistics dataset: https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=pypi&page=dataset
.. _Google BigQuery: https://cloud.google.com/bigquery
.. _BigQuery web UI: https://console.cloud.google.com/bigquery
.. _pypinfo: https://github.com/ofek/pypinfo
.. _google-cloud-bigquery: https://cloud.google.com/bigquery/docs/reference/libraries
.. _pandas-gbq: https://pandas-gbq.readthedocs.io/en/latest/
.. _Pandas: https://pandas.pydata.org/
