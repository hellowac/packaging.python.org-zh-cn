.. _trusted-publishing:

=============================================================================
使用 GitHub Actions CI/CD 工作流发布软件包分发版本
=============================================================================

**Publishing package distribution releases using GitHub Actions CI/CD workflows**

.. tab:: 中文

   `GitHub Actions CI/CD`_ 允许你在 GitHub 平台上发生事件时运行一系列命令。一个常见的做法是通过 ``push`` 事件触发工作流。  
   本指南展示了如何在每次推送带标签的提交时发布 Python 分发包。  
   它将使用 `pypa/gh-action-pypi-publish GitHub Action`_ 来进行发布，还将使用 GitHub 的 `upload-artifact`_ 和 `download-artifact`_ 动作来临时存储和下载源代码包。

   .. 注意::

      本指南 *假设* 你已经有一个你知道如何构建分发包的项目，并且 *该项目托管在 GitHub 上*。本指南还避免了构建平台特定项目的细节。如果你的项目包含二进制组件，请查看 :ref:`cibuildwheel` 的 GitHub Action 示例。

.. tab:: 英文

   `GitHub Actions CI/CD`_ allows you to run a series of commands
   whenever an event occurs on the GitHub platform. One
   popular choice is having a workflow that's triggered by a
   ``push`` event.
   This guide shows you how to publish a Python distribution
   whenever a tagged commit is pushed.
   It will use the `pypa/gh-action-pypi-publish GitHub Action`_ for
   publishing. It also uses GitHub's `upload-artifact`_ and `download-artifact`_ actions
   for temporarily storing and downloading the source packages.

   .. attention::

      This guide *assumes* that you already have a project that you know how to
      build distributions for and *it lives on GitHub*.  This guide also avoids
      details of building platform specific projects. If you have binary
      components, check out :ref:`cibuildwheel`'s GitHub Action examples.

配置受信任的发布
==============================

**Configuring trusted publishing**

.. tab:: 中文

   本指南依赖于 PyPI 的 `trusted publishing`_ 实现来连接到 `GitHub Actions CI/CD`_。出于安全原因，推荐使用这种方法，因为生成的令牌是为每个项目单独创建的，并且会自动过期。否则，你将需要为 PyPI 和 TestPyPI 分别生成 `API 令牌 <API token>`_。如果要发布到第三方索引，如 :doc:`devpi <devpi:index>`，你可能需要提供用户名/密码组合。

   由于本指南将演示如何同时上传到 PyPI 和 TestPyPI，因此我们需要配置两个受信任的发布者。以下步骤将引导你创建新的 :term:`PyPI 项目 <Project>` 的“待定”发布者。  
   但如果你是某个现有项目的所有者，也可以将 `trusted publishing`_ 添加到任何已存在的项目。

   .. attention::

      如果你跟随了早期版本的本指南，你已经为直接访问 PyPI 和 TestPyPI 创建了秘密 `PYPI_API_TOKEN` 和 `TEST_PYPI_API_TOKEN`。这些现在已经过时，如果你打算用新的设置替代旧的设置，请从你的 GitHub 仓库中删除它们，并在你的 PyPI 和 TestPyPI 账户设置中撤销它们。

   开始吧！ 🚀

   1. 访问 https://pypi.org/manage/account/publishing/。
   2. 填入你希望发布新 :term:`PyPI 项目 <Project>` 的名称（即你 `setup.cfg` 或 `pyproject.toml` 文件中的 `name` 值）、GitHub 仓库所有者的名称（组织或用户）、仓库名称，以及 `release workflow` 文件在 `.github/` 文件夹下的名称，见 :ref:`workflow-definition`。最后，添加我们将在你的仓库下设置的 GitHub 环境名称（`pypi`）。注册受信任的发布者。
   3. 然后，访问 https://test.pypi.org/manage/account/publishing/，重复第二步，但这次输入 `testpypi` 作为 GitHub 环境的名称。
   4. 你的“待定”发布者现在已经准备好首次使用，并且会在第一次使用时自动创建你的项目。

      .. note::

         如果你没有 TestPyPI 账户，你需要创建一个。它与普通的 PyPI 账户不同。

      .. attention::

         出于安全原因，你必须对每次运行要求 `手动批准 <https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-protection-rules>`_，尤其是对 `pypi` 环境的每次运行。

.. tab:: 英文

   This guide relies on PyPI's `trusted publishing`_ implementation to connect
   to `GitHub Actions CI/CD`_. This is recommended for security reasons, since
   the generated tokens are created for each of your projects
   individually and expire automatically. Otherwise, you'll need to generate an
   `API token`_ for both PyPI and TestPyPI. In case of publishing to third-party
   indexes like :doc:`devpi <devpi:index>`, you may need to provide a
   username/password combination.

   Since this guide will demonstrate uploading to both
   PyPI and TestPyPI, we'll need two trusted publishers configured.
   The following steps will lead you through creating the "pending" publishers
   for your new :term:`PyPI project <Project>`.
   However it is also possible to add `trusted publishing`_ to any
   pre-existing project, if you are its owner.

   .. attention::

      If you followed earlier versions of this guide, you
      have created the secrets ``PYPI_API_TOKEN`` and ``TEST_PYPI_API_TOKEN``
      for direct PyPI and TestPyPI access. These are obsolete now and
      you should remove them from your GitHub repository and revoke
      them in your PyPI and TestPyPI account settings in case you are replacing your old setup with the new one.


   Let's begin! 🚀

   1. Go to https://pypi.org/manage/account/publishing/.
   2. Fill in the name you wish to publish your new
      :term:`PyPI project <Project>` under
      (the ``name`` value in your ``setup.cfg`` or ``pyproject.toml``),
      the GitHub repository owner's name (org or user),
      and repository name, and the name of the release workflow file under
      the ``.github/`` folder, see :ref:`workflow-definition`.
      Finally, add the name of the GitHub Environment
      (``pypi``) we're going set up under your repository.
      Register the trusted publisher.
   3. Now, go to https://test.pypi.org/manage/account/publishing/ and repeat
      the second step, but this time, enter ``testpypi`` as the name of the
      GitHub Environment.
   4. Your "pending" publishers are now ready for their first use and will
      create your projects automatically once you use them
      for the first time.

      .. note::

         If you don't have a TestPyPI account, you'll need to
         create it. It's not the same as a regular PyPI account.


      .. attention::

         For security reasons, you must require `manual approval <https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment#deployment-protection-rules>`_
         on each run for the ``pypi`` environment.


.. _workflow-definition:

创建工作流定义
==============================

**Creating a workflow definition**

.. tab:: 中文

   GitHub CI/CD 工作流在存储库的 ``.github/workflows/`` 目录下的 YAML 文件中声明。

   我们来创建一个 ``.github/workflows/publish-to-test-pypi.yml`` 文件。

   首先给它起一个有意义的名字，并定义触发 GitHub 运行此工作流的事件：

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :end-before: jobs:

.. tab:: 英文

   GitHub CI/CD workflows are declared in YAML files stored in the
   ``.github/workflows/`` directory of your repository.

   Let's create a ``.github/workflows/publish-to-test-pypi.yml``
   file.

   Start it with a meaningful name and define the event that
   should make GitHub run this workflow:

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :end-before: jobs:

检查项目并构建分发
===================================================

**Checking out the project and building distributions**

.. tab:: 中文

   我们需要定义两个作业分别发布到 PyPI 和 TestPyPI，以及一个额外的作业来构建发行包。

   首先，我们定义用于构建项目的发行包并将其存储以备后用的作业：

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-at: jobs:
      :end-before: Install pypa/build

   这将把你的存储库下载到 CI 运行器中，然后安装并激活最新的可用 Python 3 版本。

   现在我们可以从源代码构建发行包并将其存储。
   在这个例子中，我们将使用 ``build`` 包。
   所以将其添加到步骤列表中：

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-at: Install pypa/build
      :end-before: publish-to-pypi

.. tab:: 英文

   We will have to define two jobs to publish to PyPI
   and TestPyPI respectively, and an additional job to
   build the distribution packages.

   First, we'll define the job for building the dist packages of
   your project and storing them for later use:

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-at: jobs:
      :end-before: Install pypa/build

   This will download your repository into the CI runner and then
   install and activate the newest available Python 3 release.

   And now we can build the dists from source and store them.
   In this example, we'll use the ``build`` package.
   So add this to the steps list:

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-at: Install pypa/build
      :end-before: publish-to-pypi

定义工作流作业环境
===================================

**Defining a workflow job environment**

.. tab:: 中文

   现在，让我们为发布到 PyPI 的作业添加初始设置。
   这是一个执行我们稍后定义的命令的过程。
   在本指南中，我们将使用 GitHub Actions 提供的最新稳定版本的 Ubuntu LTS。 
   这还定义了一个 GitHub 环境，作业将在其上下文中运行，并提供一个在 GitHub UI 中显示的 URL，便于查看。此外，它还允许获取一个 OpenID Connect 令牌，这是 ``pypi-publish`` 动作实现无密钥信任发布到 PyPI 所必需的。

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-after: path: dist/
      :end-before: steps:

   这也确保了只有当当前提交被标记时，才会触发 PyPI 发布工作流。

.. tab:: 英文

   Now, let's add initial setup for our job that will publish to PyPI.
   It's a process that will execute commands that we'll define later.
   In this guide, we'll use the latest stable Ubuntu LTS version
   provided by GitHub Actions. This also defines a GitHub Environment
   for the job to run in its context and a URL to be displayed in GitHub's
   UI nicely. Additionally, it allows acquiring an OpenID Connect token
   that the ``pypi-publish`` actions needs to implement secretless
   trusted publishing to PyPI.

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-after: path: dist/
      :end-before: steps:

   This will also ensure that the PyPI publishing workflow is only triggered
   if the current commit is tagged.

将分发发布到 PyPI
===================================

**Publishing the distribution to PyPI**

.. tab:: 中文

   最后，在末尾添加以下步骤：

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-after: id-token: write
      :end-before: github-release:

   此步骤使用了 `pypa/gh-action-pypi-publish`_ GitHub Action：在 `download-artifact`_ 动作下载了存储的分发包之后，它会无条件地将 ``dist/`` 文件夹的内容上传到 PyPI。

.. tab:: 英文

   Finally, add the following steps at the end:

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-after: id-token: write
      :end-before:  github-release:

   This step uses the `pypa/gh-action-pypi-publish`_ GitHub
   Action: after the stored distribution package has been
   downloaded by the `download-artifact`_ action, it uploads
   the contents of the ``dist/`` folder into PyPI unconditionally.

签署分发包
=================================

**Signing the distribution packages**

.. tab:: 中文

   以下任务使用 `Sigstore`_ 对分发包进行签名， ``Sigstore`` 是 `用于签署 CPython <https://www.python.org/download/sigstore/>`_ 的相同工件签名系统 。

   首先，它使用 `sigstore/gh-action-sigstore-python GitHub Action`_ 来签署分发包。在下一步中，使用 ``gh`` CLI 从当前标签创建一个空的 GitHub 发布。请注意，这一步可以进一步自定义。可以参考 `gh release 文档 <https://cli.github.com/manual/gh_release>`_。

   .. tip::

      你可能需要管理你的 ``GITHUB_TOKEN`` 权限，以
      启用创建 GitHub 发布。有关说明，请参阅 `GitHub 文档 <https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#configuring-the-default-github_token-permissions>`_。
      特别是，令牌需要 ``contents: write`` 权限。

   最后，已签名的分发包将上传到 GitHub 发布中。

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-at: github-release:
      :end-before: publish-to-testpypi

   .. note::

      这是 GPG 签名的替代方案，支持 GPG 签名的功能 `已从 PyPI 中 移除 <https://blog.pypi.org/posts/2023-05-23-removing-pgp/>`_。 然而，这个任务对于上传到 PyPI 不是强制要求的，可以省略。

.. tab:: 英文

   The following job signs the distribution packages with `Sigstore`_,
   the same artifact signing system `used to sign CPython <https://www.python.org/download/sigstore/>`_.

   Firstly, it uses the `sigstore/gh-action-sigstore-python GitHub Action`_
   to sign the distribution packages. In the next step, an empty GitHub Release
   from the current tag is created using the ``gh`` CLI. Note this step can be further
   customised. See the `gh release documentation <https://cli.github.com/manual/gh_release>`_
   as a reference.

   .. tip::

      You may need to manage your ``GITHUB_TOKEN`` permissions to
      enable creating the GitHub Release. See the `GitHub
      documentation <https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#configuring-the-default-github_token-permissions>`_
      for instructions. Specifically, the token needs the
      ``contents: write`` permission.

   Finally, the signed distributions are uploaded to the GitHub Release.

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-at: github-release:
      :end-before:  publish-to-testpypi


   .. note::

      This is a replacement for GPG signatures, for which support has been
      `removed from PyPI <https://blog.pypi.org/posts/2023-05-23-removing-pgp/>`_.
      However, this job is not mandatory for uploading to PyPI and can be omitted.


用于发布到 TestPyPI 的单独工作流
============================================

**Separate workflow for publishing to TestPyPI**

.. tab:: 中文

   现在，重复这些步骤并在 ``jobs`` 部分为发布到 TestPyPI 包索引创建另一个任务：

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-at: publish-to-testpypi

   .. tip::

      在 ``testpypi`` GitHub 环境中要求手动批准通常是不必要的，因为它旨在在每次提交到主分支时运行，通常用于指示健康的发布管道。

.. tab:: 英文

   Now, repeat these steps and create another job for
   publishing to the TestPyPI package index under the ``jobs``
   section:

   .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
      :language: yaml
      :start-at: publish-to-testpypi

   .. tip::

      Requiring manual approvals in the ``testpypi`` GitHub Environment is typically unnecessary as it's designed to run on each commit to the main branch and is often used to indicate a healthy release publishing pipeline.


整个 CI/CD 工作流
========================

**The whole CI/CD workflow**

.. tab:: 中文

   本段展示了遵循上述指南后的整个工作流程。

   .. collapse:: Click here to display the entire GitHub Actions CI/CD workflow definition

      .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
         :language: yaml

.. tab:: 英文

   This paragraph showcases the whole workflow after following the above guide.

   .. collapse:: Click here to display the entire GitHub Actions CI/CD workflow definition

      .. literalinclude:: github-actions-ci-cd-sample/publish-to-test-pypi.yml
         :language: yaml

就这些了，伙计们！
==================

**That's all, folks!**

.. tab:: 中文

   现在，每当你将带有标签的提交推送到 GitHub 上的 Git 仓库远程时，这个工作流将会将其发布到 PyPI。它还会将任何推送发布到 TestPyPI，这对提供测试版本给你的 alpha 用户以及确保你的发布管道保持健康非常有用！

   .. attention::

      如果你的仓库有频繁的提交活动，并且每次推送都按照描述上传到 TestPyPI，项目可能会超出 `PyPI 项目大小限制 <https://pypi.org/help/#project-size-limit>`_。虽然该限制可以提高，但更好的解决方案可能是在 CI 中使用兼容 PyPI 的服务器，如 :ref:`pypiserver`，用于测试目的。

   .. note::

      推荐保持集成的 GitHub Actions 在最新版本，并定期更新它们。

.. tab:: 英文

   Now, whenever you push a tagged commit to your Git repository remote
   on GitHub, this workflow will publish it to PyPI.
   And it'll publish any push to TestPyPI which is useful for
   providing test builds to your alpha users as well as making
   sure that your release pipeline remains healthy!

   .. attention::

   If your repository has frequent commit activity and every push is uploaded
   to TestPyPI as described, the project might exceed the
   `PyPI project size limit <https://pypi.org/help/#project-size-limit>`_.
   The limit could be increased, but a better solution may constitute to
   use a PyPI-compatible server like :ref:`pypiserver` in the CI for testing purposes.

   .. note::

      It is recommended to keep the integrated GitHub Actions at their latest
      versions, updating them frequently.


.. _API token: https://pypi.org/help/#apitoken
.. _GitHub Actions CI/CD: https://github.com/features/actions
.. _join the waitlist: https://github.com/features/actions/signup
.. _pypa/gh-action-pypi-publish:
   https://github.com/pypa/gh-action-pypi-publish
.. _`pypa/gh-action-pypi-publish GitHub Action`:
   https://github.com/marketplace/actions/pypi-publish
.. _`download-artifact`:
   https://github.com/actions/download-artifact
.. _`upload-artifact`:
   https://github.com/actions/upload-artifact
.. _Sigstore: https://www.sigstore.dev/
.. _`sigstore/gh-action-sigstore-python GitHub Action`:
   https://github.com/marketplace/actions/gh-action-sigstore-python
.. _Secrets:
   https://docs.github.com/en/actions/reference/encrypted-secrets
.. _trusted publishing: https://docs.pypi.org/trusted-publishers/
