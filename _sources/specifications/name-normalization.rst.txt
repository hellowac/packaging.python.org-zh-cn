=======================
名称和规范化
=======================

**Names and normalization**

.. tab:: 中文

  本规范定义了软件包和附加组件的名称必须遵循的格式。它还描述了如何对它们进行规范化，这应该在查找和比较之前完成。

.. tab:: 英文

  This specification defines the format that names for packages and extras are
  required to follow. It also describes how to normalize them, which should be
  done before lookups and comparisons.


.. _name-format:

名称格式
===========

**Name format**

.. tab:: 中文

  有效名称仅由 ASCII 字母和数字、句点、下划线和连字符组成。它必须以字母或数字开头和结尾。这意味着有效的项目名称仅限于与以下正则表达式匹配的名称（使用 :py:data:`re.IGNORECASE` 运行）::

    ^([A-Z0-9]|[A-Z0-9][A-Z0-9._-]*[A-Z0-9])$

.. tab:: 英文

  A valid name consists only of ASCII letters and numbers, period,
  underscore and hyphen. It must start and end with a letter or number.
  This means that valid project names are limited to those which match the
  following regex (run with :py:data:`re.IGNORECASE`)::

    ^([A-Z0-9]|[A-Z0-9][A-Z0-9._-]*[A-Z0-9])$


.. _name-normalization:

名称规范化
==================

**Name normalization**

.. tab:: 中文

  名称应该小写，并将所有连续的字符 ``.``、 ``-`` 或 ``_`` 替换为一个单独的 ``-`` 字符。可以通过 Python 的 `re` 模块来实现：

  .. code-block:: python

      import re

      def normalize(name):
          return re.sub(r"[-_.]+", "-", name).lower()

  这意味着以下名称是等效的：

  * ``friendly-bard`` （标准化形式）
  * ``Friendly-Bard``
  * ``FRIENDLY-BARD``
  * ``friendly.bard``
  * ``friendly_bard``
  * ``friendly--bard``
  * ``FrIeNdLy-._.-bArD`` （虽然这种写法很糟糕，但它是有效的）

.. tab:: 英文

  The name should be lowercased with all runs of the characters ``.``, ``-``, or
  ``_`` replaced with a single ``-`` character. This can be implemented in Python
  with the re module:

  .. code-block:: python

      import re

      def normalize(name):
          return re.sub(r"[-_.]+", "-", name).lower()

  This means that the following names are all equivalent:

  * ``friendly-bard`` (normalized form)
  * ``Friendly-Bard``
  * ``FRIENDLY-BARD``
  * ``friendly.bard``
  * ``friendly_bard``
  * ``friendly--bard``
  * ``FrIeNdLy-._.-bArD`` (a *terrible* way to write a name, but it is valid)

历史
=======

**History**

.. tab:: 中文

  - 2015年9月: 名称标准化规范通过了 :pep:`503 <503#normalized-names>` 。
  - 2015年11月: 有效名称规范通过了 :pep:`508 <508#names>` 。

.. tab:: 英文

  - September 2015: The specification of name normalized was approved through :pep:`503 <503#normalized-names>`.
  - November 2015: The specification of valid names was approved through :pep:`508 <508#names>`.
