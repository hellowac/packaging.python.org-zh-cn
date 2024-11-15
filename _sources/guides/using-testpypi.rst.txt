.. _using-test-pypi:

==============
使用 TestPyPI
==============

**Using TestPyPI**

.. tab:: 中文

    ``TestPyPI`` 是一个独立的 :term:`Python 包索引 (PyPI) <Python Package Index (PyPI)>` 实例，它允许你在不必担心影响真实索引的情况下尝试分发工具和流程。TestPyPI 托管在 `test.pypi.org <https://test.pypi.org>`_。

.. tab:: 英文

    ``TestPyPI`` is a separate instance of the :term:`Python Package Index (PyPI)`
    that allows you to try out the distribution tools and process without worrying
    about affecting the real index. TestPyPI is hosted at
    `test.pypi.org <https://test.pypi.org>`_

注册您的帐户
------------------------

**Registering your account**

.. tab:: 中文

    由于 TestPyPI 拥有与真实 PyPI 分开的数据库，你需要为 TestPyPI 创建一个单独的用户帐户。请访问 https://test.pypi.org/account/register/ 注册你的帐户。

    .. note:: TestPyPI 的数据库可能会定期进行清理，因此用户帐户被删除并不罕见。

.. tab:: 英文

    Because TestPyPI has a separate database from the live PyPI, you'll need a
    separate user account specifically for TestPyPI. Go to
    https://test.pypi.org/account/register/ to register your account.

    .. note:: The database for TestPyPI may be periodically pruned, so it is not
        unusual for user accounts to be deleted.


使用 TestPyPI 和 Twine
-------------------------

**Using TestPyPI with Twine**

.. tab:: 中文

    你可以通过指定 ``--repository`` 标志，使用 :ref:`twine` 将你的分发包上传到 TestPyPI：

    .. code-block:: bash

        twine upload --repository testpypi dist/*

    你可以通过访问 URL ``https://test.pypi.org/project/<sampleproject>`` 来查看你的包是否已经成功上传，其中 ``sampleproject`` 是你上传的项目名称。可能需要一两分钟，才能在网站上看到你的项目。

.. tab:: 英文

    You can upload your distributions to TestPyPI using :ref:`twine` by specifying
    the ``--repository`` flag:

    .. code-block:: bash

        twine upload --repository testpypi dist/*

    You can see if your package has successfully uploaded by navigating to the URL
    ``https://test.pypi.org/project/<sampleproject>`` where ``sampleproject`` is
    the name of your project that you uploaded. It may take a minute or two for
    your project to appear on the site.

使用 TestPyPI 和 pip
-----------------------

**Using TestPyPI with pip**

.. tab:: 中文

    你可以通过指定 ``--index-url`` 标志，告诉 :ref:`pip` 从 TestPyPI 下载包，而不是从 PyPI 下载：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url https://test.pypi.org/simple/ your-package

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url https://test.pypi.org/simple/ your-package

    如果你希望 pip 也能够从 PyPI 下载包，你可以指定 ``--extra-index-url`` 指向 PyPI。这在你测试的包有依赖时很有用：

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ your-package

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ your-package

.. tab:: 英文

    You can tell :ref:`pip` to download packages from TestPyPI instead of PyPI by
    specifying the ``--index-url`` flag:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url https://test.pypi.org/simple/ your-package

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url https://test.pypi.org/simple/ your-package

    If you want to allow pip to also download packages from PyPI, you can
    specify ``--extra-index-url`` to point to PyPI. This is useful when the package
    you're testing has dependencies:

    .. tab:: Unix/macOS

        .. code-block:: bash

            python3 -m pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ your-package

    .. tab:: Windows

        .. code-block:: bat

            py -m pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ your-package

在 :file:`.pypirc` 中设置 TestPyPI
--------------------------------------

**Setting up TestPyPI in** :file:`.pypirc`

.. tab:: 中文

    如果你想避免每次都被提示输入用户名和密码，可以在你的 :file:`$HOME/.pypirc` 文件中配置 TestPyPI：

    .. code:: ini

        [testpypi]
        username = __token__
        password = <your TestPyPI API Token>

    有关更多详情，请参阅 :ref:`.pypirc` 的规范 <pypirc>。

.. tab:: 英文

    If you want to avoid being prompted for your username and password every time,
    you can configure TestPyPI in your :file:`$HOME/.pypirc`:

    .. code:: ini

        [testpypi]
        username = __token__
        password = <your TestPyPI API Token>

    For more details, see the :ref:`specification <pypirc>` for :file:`.pypirc`.
