.. _`Binary Extensions`:

===========================
打包二进制扩展
===========================

**Packaging binary extensions**

.. tab:: 中文

  :页面状态: 不完整(Incomplete)
  :最后更新: 2013-12-08

  CPython 参考解释器的一个特点是，除了允许执行 Python 代码外，它还提供了一个丰富的 C API，供其他软件使用。这个 C API 的最常见用途之一是创建可导入的 C 扩展，允许实现一些在纯 Python 代码中不容易实现的功能。    

.. tab:: 英文
  
  :Page Status: Incomplete
  :Last Reviewed: 2013-12-08

  One of the features of the CPython reference interpreter is that, in
  addition to allowing the execution of Python code, it also exposes a rich
  C API for use by other software. One of the most common uses of this C API
  is to create importable C extensions that allow things which aren't
  always easy to achieve in pure Python code.


二进制扩展概述
================================

**An overview of binary extensions**

用例
---------

**Use cases**

.. tab:: 中文

  二进制扩展的典型用例可以分为以下三类：

  * **加速模块**：这些模块是完全自包含的，专门创建用于比等效的纯 Python 代码在 CPython 中运行得更快。理想情况下，加速模块总是应该有一个纯 Python 版本，作为备选方案，以便在给定系统上没有加速版本时使用。CPython 标准库广泛使用加速模块。  
    *示例*：当导入 `datetime` 时，如果 C 实现（`_datetimemodule.c`）不可用，Python 会回退到 `datetime.py <https://github.com/python/cpython/blob/main/Lib/datetime.py>`_ 模块。

  * **包装模块**：这些模块是为了将现有的 C 接口暴露给 Python 代码而创建的。它们可以直接暴露底层的 C 接口，或者暴露一个更“Pythonic”的 API，利用 Python 的语言特性使 API 更易于使用。CPython 标准库广泛使用包装模块。  
    *示例*：`functools.py <https://github.com/python/cpython/blob/main/Lib/functools.py>`_ 是一个 Python 模块包装器，用于包装 `_functoolsmodule.c <https://github.com/python/cpython/blob/main/Modules/_functoolsmodule.c>`_。

  * **低级系统访问**：这些模块是为了访问 CPython 运行时、操作系统或底层硬件的低级特性而创建的。通过平台特定的代码，扩展模块可以实现纯 Python 代码无法实现的功能。许多 CPython 标准库模块是用 C 编写的，以便访问解释器内部的内容，这些内容在语言层面上无法暴露。  
    *示例*：`sys` 模块来自 `sysmodule.c <https://github.com/python/cpython/blob/main/Python/sysmodule.c>`_。

  一个特别显著的 C 扩展特点是，当它们不需要回调到解释器运行时时，它们可以释放 CPython 的全局解释器锁，进行长时间运行的操作（无论这些操作是 CPU 绑定还是 IO 绑定）。

  并非所有扩展模块都能完美地归入上述类别。例如，NumPy 包含的扩展模块涉及所有三种用例——它们为了加速将内部循环移动到 C，包装了用 C、FORTRAN 和其他语言编写的外部库，并利用 CPython 和底层操作系统的低级系统接口支持向量化操作的并发执行，同时紧密控制创建对象的内存布局。

.. tab:: 英文

  The typical use cases for binary extensions break down into just three
  conventional categories:

  * **accelerator modules**: these modules are completely self-contained, and
    are created solely to run faster than the equivalent pure Python code
    runs in CPython. Ideally, accelerator modules will always have a pure
    Python equivalent to use as a fallback if the accelerated version isn't
    available on a given system. The CPython standard library makes extensive
    use of accelerator modules.
    *Example*: When importing ``datetime``, Python falls back to the
    `datetime.py <https://github.com/python/cpython/blob/main/Lib/datetime.py>`_
    module if the C implementation (
    `_datetimemodule.c <https://github.com/python/cpython/blob/main/Modules/_datetimemodule.c>`_)
    is not available.
  * **wrapper modules**: these modules are created to expose existing C interfaces
    to Python code. They may either expose the underlying C interface directly,
    or else expose a more "Pythonic" API that makes use of Python language
    features to make the API easier to use. The CPython standard library makes
    extensive use of wrapper modules.
    *Example*: `functools.py <https://github.com/python/cpython/blob/main/Lib/functools.py>`_
    is a Python module wrapper for
    `_functoolsmodule.c <https://github.com/python/cpython/blob/main/Modules/_functoolsmodule.c>`_.
  * **low-level system access**: these modules are created to access lower level
    features of the CPython runtime, the operating system, or the underlying
    hardware. Through platform specific code, extension modules may achieve
    things that aren't possible in pure Python code. A number of CPython
    standard library modules are written in C in order to access interpreter
    internals that aren't exposed at the language level.
    *Example*: ``sys``, which comes from
    `sysmodule.c <https://github.com/python/cpython/blob/main/Python/sysmodule.c>`_.

    One particularly notable feature of C extensions is that, when they don't
    need to call back into the interpreter runtime, they can release CPython's
    global interpreter lock around long-running operations (regardless of
    whether those operations are CPU or IO bound).

  Not all extension modules will fit neatly into the above categories. The
  extension modules included with NumPy, for example, span all three use cases
  - they move inner loops to C for speed reasons, wrap external libraries
  written in C, FORTRAN and other languages, and use low level system
  interfaces for both CPython and the underlying operation system to support
  concurrent execution of vectorised operations and to tightly control the
  exact memory layout of created objects.


缺点
-------------

**Disadvantages**

.. tab:: 中文

  使用二进制扩展的主要缺点是，它使得软件的后续分发变得更加困难。使用 Python 的一个优势是它在多个平台上都能运行，而用来编写扩展模块的语言（通常是 C 或 C++，但实际上任何可以绑定到 CPython C API 的语言都可以）通常要求为不同的平台创建自定义的二进制文件。

  这意味着二进制扩展：

  * 需要最终用户能够从源代码构建它们，或者需要有人为常见平台发布预编译的二进制文件。

  * 可能与 CPython 参考解释器的不同构建不兼容。

  * 通常无法与其他解释器（如 PyPy、IronPython 或 Jython）正常工作。

  * 如果是手工编码的，会使维护变得更加困难，因为维护人员不仅需要熟悉 Python，还需要熟悉用于创建二进制扩展的语言，以及 CPython C API 的细节。

  * 如果提供了纯 Python 备用实现，维护起来会更加困难，因为需要在两个地方实施更改，并且需要在测试套件中引入额外的复杂性，以确保两个版本始终被执行。

  依赖二进制扩展的另一个缺点是，替代的导入机制（例如直接从 zip 文件导入模块的能力）通常无法用于扩展模块（因为大多数平台的动态加载机制只能从磁盘加载库）。

.. tab:: 英文

  The main disadvantage of using binary extensions is the fact that it makes
  subsequent distribution of the software more difficult. One of the
  advantages of using Python is that it is largely cross platform, and the
  languages used to write extension modules (typically C or C++, but really
  any language that can bind to the CPython C API) typically require that
  custom binaries be created for different platforms.

  This means that binary extensions:

  * require that end users be able to either build them from source, or else
    that someone publish pre-built binaries for common platforms

  * may not be compatible with different builds of the CPython reference
    interpreter

  * often will not work correctly with alternative interpreters such as PyPy,
    IronPython or Jython

  * if handcoded, make maintenance more difficult by requiring that
    maintainers be familiar not only with Python, but also with the language
    used to create the binary extension, as well as with the details of the
    CPython C API.

  * if a pure Python fallback implementation is provided, make maintenance
    more difficult by requiring that changes be implemented in two places,
    and introducing additional complexity in the test suite to ensure both
    versions are always executed.

  Another disadvantage of relying on binary extensions is that alternative
  import mechanisms (such as the ability to import modules directly from
  zipfiles) often won't work for extension modules (as the dynamic loading
  mechanisms on most platforms can only load libraries from disk).


手工编码加速器模块的替代方案
---------------------------------------------

**Alternatives to handcoded accelerator modules**

.. tab:: 中文

  当扩展模块仅用于加速代码运行时（在分析确定某些代码的速度提升值得额外的维护工作后），还应考虑以下几种替代方案：

  * 寻找现有的优化替代方案。CPython 标准库包含了许多优化过的数据结构和算法（特别是在内建函数和 `collections`、 `itertools` 模块中）。Python 包索引也提供了额外的替代方案。有时，选择合适的标准库或第三方模块可以避免创建自己的加速器模块。

  * 对于长时间运行的应用程序，JIT 编译的 `PyPy 解释器 <https://www.pypy.org/>`__ 可能是标准 CPython 运行时的合适替代方案。采用 PyPy 的主要障碍通常是依赖其他二进制扩展模块——虽然 PyPy 会模拟 CPython C API，但依赖此 API 的模块会给 PyPy JIT 带来问题，模拟层通常会暴露出扩展模块中 CPython 目前容忍的潜在缺陷（通常是引用计数错误——一个对象拥有一个有效引用而不是两个通常不会破坏任何东西，但没有引用而是一个有效引用则是一个重大问题）。

  * `Cython <https://cython.org/>`__ 是一个成熟的静态编译器，可以将大部分 Python 代码编译为 C 扩展模块。初步编译提供了一些速度提升（通过绕过 CPython 解释器层），而 Cython 的可选静态类型特性可以提供额外的速度提升机会。使用 Cython 仍然存在使用二进制扩展的 `缺点`_，但相比于 C 或 C++ 等语言，它具有较低的入门门槛，适合 Python 程序员。

  * `Numba <http://numba.pydata.org/>`__ 是一个较新的工具，由科学 Python 社区的成员创建，旨在利用 LLVM 允许在运行时选择性地将 Python 应用程序的部分代码编译为本地机器代码。它要求系统上有 LLVM 支持，但可以显著提升速度，尤其对于能够进行向量化的操作。

.. tab:: 英文

  When extension modules are just being used to make code run faster (after
  profiling has identified the code where the speed increase is worth
  additional maintenance effort), a number of other alternatives should
  also be considered:

  * look for existing optimised alternatives. The CPython standard library
    includes a number of optimised data structures and algorithms (especially
    in the builtins and the ``collections`` and ``itertools`` modules). The
    Python Package Index also offers additional alternatives. Sometimes, the
    appropriate choice of standard library or third party module can avoid the
    need to create your own accelerator module.

  * for long running applications, the JIT compiled `PyPy interpreter
    <https://www.pypy.org/>`__ may offer a suitable alternative to the standard
    CPython runtime. The main barrier to adopting PyPy is typically reliance
    on other binary extension modules - while PyPy does emulate the CPython
    C API, modules that rely on that cause problems for the PyPy JIT, and the
    emulation layer can often expose latent defects in extension modules that
    CPython currently tolerates (frequently around reference counting errors -
    an object having one live reference instead of two often won't break
    anything, but no references instead of one is a major problem).

  * `Cython <https://cython.org/>`__ is a mature static compiler that can
    compile most Python code to C extension modules. The initial compilation
    provides some speed increases (by bypassing the CPython interpreter layer),
    and Cython's optional static typing features can offer additional
    opportunities for speed increases. Using Cython still carries the
    `disadvantages`_ associated with using binary extensions,
    but has the benefit of having a reduced barrier to entry for Python
    programmers (relative to other languages like C or C++).

  * `Numba <http://numba.pydata.org/>`__ is a newer tool, created by members
    of the scientific Python community, that aims to leverage LLVM to allow
    selective compilation of pieces of a Python application to native
    machine code at runtime. It requires that LLVM be available on the
    system where the code is running, but can provide significant speed
    increases, especially for operations that are amenable to vectorisation.


手工编码包装器模块的替代方案
-----------------------------------------

**Alternatives to handcoded wrapper modules**

.. tab:: 中文

  C ABI（应用二进制接口）是一个共享功能的通用标准，允许多个应用程序之间互操作。CPython C API（应用程序编程接口）的一大优势是它允许 Python 用户利用这些功能。然而，手动包装模块是相当繁琐的，因此应该考虑一些其他替代方法。

  下面描述的方法虽然并没有简化分发过程，但它们 *可以* 显著减少维护包装模块的工作量。

  * 除了用于创建加速器模块，`Cython <https://cython.org/>`__ 也广泛用于为 C 或 C++ API 创建包装模块。它涉及手动包装接口，这为设计和优化包装代码提供了很大的灵活性，但可能不适合快速包装非常大的 API。查看 `第三方工具列表 <https://github.com/cython/cython/wiki/AutoPxd>`_ 了解如何使用 Cython 自动包装。它还支持一些面向性能的 Python 实现，这些实现提供了类似 CPython 的 C API，比如 PyPy 和 Pyston。

  * :doc:`pybind11 <pybind11:index>` 是一个纯 C++11 库，提供了一个干净的 C++ 接口来访问 CPython（和 PyPy）C API。它不需要预处理步骤；它完全用模板化的 C++ 编写。它还包括一些辅助工具，用于 Setuptools 或 CMake 构建。它基于 `Boost.Python <https://www.boost.org/doc/libs/1_76_0/libs/python/doc/html/index.html>`__，但不需要 Boost 库或 BJam。

  * :doc:`cffi <cffi:index>` 是一个由一些 PyPy 开发者创建的项目，旨在让已经熟悉 Python 和 C 的开发者能够轻松地将他们的 C 模块暴露给 Python 应用程序。它也使得即使你自己不懂 C，基于头文件包装 C 模块变得相对简单。

    ``cffi`` 的一个关键优势是它与 PyPy JIT 兼容，允许 CFFI 包装模块完全参与 PyPy 的追踪 JIT 优化。

  * `SWIG <http://www.swig.org/>`__ 是一个包装接口生成器，允许多种编程语言（包括 Python）与 C 和 C++ 代码进行接口交互。

  * 标准库的 ``ctypes`` 模块，虽然在没有头文件信息时可以方便地访问 C 层接口，但它的缺点是仅在 C ABI 层操作，因此不能自动检查库导出的接口和 Python 代码中声明的接口之间的一致性。与此不同，以上替代方案都能够在 C *API* 层操作，使用 C 头文件确保库导出的接口与 Python 包装模块所期望的接口一致。虽然 ``cffi`` *也可以* 直接在 C ABI 层操作，但当这样使用时，它和 ``ctypes`` 一样会面临接口一致性问题。

.. tab:: 英文

  The C ABI (Application Binary Interface) is a common standard for sharing
  functionality between multiple applications. One of the strengths of the
  CPython C API (Application Programming Interface) is allowing Python users
  to tap into that functionality. However, wrapping modules by hand is quite
  tedious, so a number of other alternative approaches should be considered.

  The approaches described below don't simplify the distribution case at all,
  but they *can* significantly reduce the maintenance burden of keeping
  wrapper modules up to date.

  * In addition to being useful for the creation of accelerator modules,
    `Cython <https://cython.org/>`__ is also widely used for creating wrapper
    modules for C or C++ APIs. It involves wrapping the interfaces by
    hand, which gives a wide range of freedom in designing and optimising
    the wrapper code, but may not be a good choice for wrapping very
    large APIs quickly. See the
    `list of third-party tools <https://github.com/cython/cython/wiki/AutoPxd>`_
    for automatic wrapping with Cython. It also supports performance-oriented
    Python implementations that provide a CPython-like C-API, such as PyPy
    and Pyston.

  * :doc:`pybind11 <pybind11:index>` is a pure C++11 library
    that provides a clean C++ interface to the CPython (and PyPy) C API. It
    does not require a pre-processing step; it is written entirely in
    templated C++. Helpers are included for Setuptools or CMake builds. It
    was based on `Boost.Python <https://www.boost.org/doc/libs/1_76_0/libs/python/doc/html/index.html>`__,
    but doesn't require the Boost libraries or BJam.

  * :doc:`cffi <cffi:index>` is a project created by some of the PyPy
    developers to make it straightforward for developers that already know
    both Python and C to expose their C modules to Python applications. It
    also makes it relatively straightforward to wrap a C module based on its
    header files, even if you don't know C yourself.

    One of the key advantages of ``cffi`` is that it is compatible with the
    PyPy JIT, allowing CFFI wrapper modules to participate fully in PyPy's
    tracing JIT optimisations.

  * `SWIG <http://www.swig.org/>`__ is a wrapper interface generator that
    allows a variety of programming languages, including Python, to interface
    with C and C++ code.

  * The standard library's ``ctypes`` module, while useful for getting access
    to C level interfaces when header information isn't available, suffers
    from the fact that it operates solely at the C ABI level, and thus has
    no automatic consistency checking between the interface actually being
    exported by the library and the one declared in the Python code. By
    contrast, the above alternatives are all able to operate at the C *API*
    level, using C header files to ensure consistency between the interface
    exported by the library being wrapped and the one expected by the Python
    wrapper module. While ``cffi`` *can* operate directly at the C ABI level,
    it suffers from the same interface inconsistency problems as ``ctypes``
    when it is used that way.


低级系统访问的替代方案
----------------------------------------

**Alternatives for low level system access**

.. tab:: 中文

  对于需要低级系统访问的应用程序（无论原因如何），二进制扩展模块通常是最合适的选择。这尤其适用于需要低级访问 CPython 运行时本身的情况，因为一些操作（如释放全局解释器锁）在解释器执行代码时是无效的，即使使用像 ``ctypes`` 或 ``cffi`` 这样的模块来访问相关的 C API 接口也无法绕过这些限制。

  对于那些扩展模块操作的是底层操作系统或硬件（而不是 CPython 运行时）的情况，有时更好的做法是直接编写一个普通的 C 库（或使用其他系统编程语言如 C++ 或 Rust 编写的库，这些语言能够导出兼容 C 的 ABI），然后使用上述描述的包装技术之一将接口暴露为可导入的 Python 模块。

.. tab:: 英文

  For applications that need low level system access (regardless of the
  reason), a binary extension module often *is* the best way to go about it.
  This is particularly true for low level access to the CPython runtime
  itself, since some operations (like releasing the Global Interpreter Lock)
  are simply invalid when the interpreter is running code, even if a module
  like ``ctypes`` or ``cffi`` is used to obtain access to the relevant C
  API interfaces.

  For cases where the extension module is manipulating the underlying
  operating system or hardware (rather than the CPython runtime), it may
  sometimes be better to just write an ordinary C library (or a library in
  another systems programming language like C++ or Rust that can export a C
  compatible ABI), and then use one of the wrapping techniques described
  above to make the interface available as an importable Python module.


实现二进制扩展
==============================

**Implementing binary extensions**

.. tab:: 中文

  CPython :doc:`扩展和嵌入 <python:extending/index>` 指南包括了如何编写
  :doc:`自定义 C 扩展模块 <python:extending/extending>` 的介绍。

  FIXME: 需要进一步说明，所有这些正是为什么你可能 *不* 想手工编写扩展模块的原因之一 :)

.. tab:: 英文

  The CPython :doc:`Extending and Embedding <python:extending/index>`
  guide includes an introduction to writing a
  :doc:`custom extension module in C <python:extending/extending>`.

  FIXME: Elaborate that all this is one of the reasons why you probably
  *don't* want to handcode your extension modules :)


扩展模块生命周期
--------------------------

**Extension module lifecycle**

.. tab:: 中文

  FIXME: 此部分需要充实。

.. tab:: 英文

  FIXME: This section needs to be fleshed out.


共享静态状态和子解释器的含义
-------------------------------------------------------

**Implications of shared static state and subinterpreters**

.. tab:: 中文

  FIXME: 此部分需要充实。

.. tab:: 英文

  FIXME: This section needs to be fleshed out.


GIL 的含义
-----------------------

**Implications of the GIL**

.. tab:: 中文

  FIXME: 此部分需要充实。

.. tab:: 英文

  FIXME: This section needs to be fleshed out.


内存分配 API
----------------------

**Memory allocation APIs**

.. tab:: 中文

  FIXME: 此部分需要充实。

.. tab:: 英文

  FIXME: This section needs to be fleshed out.


.. _cpython-stable-abi:

ABI 兼容性
-----------------

**ABI Compatibility**

.. tab:: 中文

  CPython C API 不保证在次要版本之间（如 3.2、3.3、3.4 等）保持 ABI 的稳定性。这意味着，通常情况下，如果你针对某个版本的 Python 构建了一个扩展模块，那么该模块只会在相同的次要版本的 Python 中正常工作，而不能在其他次要版本中使用。

  Python 3.2 引入了有限 API（Limited API），这是 Python C API 的一个明确定义的子集。有限 API 所需的符号组成了“稳定的 ABI”，该 ABI 保证在所有 Python 3.x 版本中都是兼容的。使用针对稳定 ABI 构建的扩展的 wheel 文件，使用 ``abi3`` ABI 标签，以表示它们与所有 Python 3.x 版本兼容。

  CPython 的 :doc:`C API 稳定性<python:c-api/stable>` 页面提供了关于 API / ABI 稳定性保证、如何使用有限 API 以及“有限 API”具体内容的详细信息。

.. tab:: 英文

  The CPython C API does not guarantee ABI stability between minor releases
  (3.2, 3.3, 3.4, etc.). This means that, typically, if you build an
  extension module against one version of Python, it is only guaranteed to
  work with the same minor version of Python and not with any other minor
  versions.

  Python 3.2 introduced the Limited API, with is a well-defined subset of
  Python's C API. The symbols needed for the Limited API form the
  "Stable ABI" which is guaranteed to be compatible across all Python 3.x
  versions. Wheels containing extensions built against the stable ABI use
  the ``abi3`` ABI tag, to reflect that they're compatible with all Python
  3.x versions.

  CPython's :doc:`C API stability<python:c-api/stable>` page provides
  detailed information about the API / ABI stability guarantees, how to use
  the Limited API and the exact contents of the "Limited API".


构建二进制扩展
==========================

**Building binary extensions**

.. tab:: 中文

  FIXME: 涵盖可用于构建扩展的构建后端。

.. tab:: 英文

  FIXME: Cover the build-backends available for building extensions.

为多个平台构建扩展
------------------------------------------

**Building extensions for multiple platforms**

.. tab:: 中文

  如果你计划分发你的扩展模块，你应该为你打算支持的所有平台提供 :term:`wheels <Wheel>` 文件。这些通常是在持续集成（CI）系统上构建的。也有一些工具可以帮助你从 CI 构建高度可重分发的二进制文件，这些工具包括 :ref:`cibuildwheel` 和 :ref:`multibuild`。

  对于大多数扩展模块，你需要为你打算支持的所有平台构建 wheels 文件。这意味着你需要构建的 wheels 数量是以下各项的乘积：

    count(Python 次版本) * count(操作系统) * count(架构)

  使用 CPython 的 :ref:`稳定 ABI <cpython-stable-abi>` 可以显著减少你需要提供的 wheels 数量，因为在一个平台上构建的单个 wheel 可以与所有 Python 次版本兼容，从而消除矩阵中的一个维度。这也避免了为每个新的 Python 次版本生成新的 wheels 文件的需求。

.. tab:: 英文

  If you plan to distribute your extension, you should provide
  :term:`wheels <Wheel>` for all the platforms you intend to support. These
  are usually built on continuous integration (CI) systems. There are tools
  to help you build highly redistributable binaries from CI; these include
  :ref:`cibuildwheel` and :ref:`multibuild`.

  For most extensions, you will need to build wheels for all the platforms
  you intend to support. This means that the number of wheels you need to
  build is the product of::

    count(Python minor versions) * count(OS) * count(architectures)

  Using CPython's :ref:`Stable ABI <cpython-stable-abi>` can help significantly
  reduce the number of wheels you need to provide, since a single wheel on a
  platform can be used with all Python minor versions; eliminating one dimension
  of the matrix. It also removes the need to generate new wheels for each new
  minor version of Python.

适用于 Windows 的二进制扩展
-----------------------------

**Binary extensions for Windows**

.. tab:: 中文

  在构建二进制扩展之前，必须确保你有合适的编译器可用。在 Windows 上，官方的 CPython 解释器是使用 Visual C 构建的，因此构建兼容的二进制扩展时也应该使用 Visual C。为了设置构建环境，安装 `Visual Studio Community Edition <https://visualstudio.microsoft.com/downloads/>`__ 即可，任何较新的版本都可以。

  有一个需要注意的地方：如果你使用的是 Visual Studio 2019 或更高版本，你的扩展将除了依赖于所有早期版本（从 2015 年开始）都依赖的 ``VCRUNTIME140.dll`` 外，还需要依赖一个“额外的”文件， ``VCRUNTIME140_1.dll``。这将增加一个额外的要求，即在不包含这个额外文件的 CPython 版本中使用你的扩展时会遇到问题。为了避免这种情况，你可以添加编译时参数 ``/d2FH4-``。一些较新的 Python 版本可能会包含这个文件。

  不推荐为 3.5 之前的 Python 版本构建扩展，因为 Microsoft 已经不再提供旧版本的 Visual Studio。如果你确实需要为旧版本构建扩展，你可以设置 ``DISTUTILS_USE_SDK=1`` 和 ``MSSdk=1``，强制找到当前激活的 MSVC 版本，并且在设计扩展时应小心避免在不同的库之间进行 malloc/free 操作，避免依赖已更改的数据结构等。用于生成扩展模块的工具通常会为你避免这些问题。

.. tab:: 英文

  Before it is possible to build a binary extension, it is necessary to ensure
  that you have a suitable compiler available. On Windows, Visual C is used to
  build the official CPython interpreter, and should be used to build compatible
  binary extensions.  To set up a build environment for binary extensions, install
  `Visual Studio Community Edition <https://visualstudio.microsoft.com/downloads/>`__
  - any recent version is fine.

  One caveat: if you use Visual Studio 2019 or later, your extension will depend
  on an "extra" file, ``VCRUNTIME140_1.dll``, in addition to the
  ``VCRUNTIME140.dll`` that all previous versions back to 2015 depend on. This
  will add an extra requirement to using your extension on versions of CPython
  that do not include this extra file. To avoid this, you can add the
  compile-time argument ``/d2FH4-``. Recent versions of Python may include this
  file.

  Building for Python prior to 3.5 is discouraged, because older versions of
  Visual Studio are no longer available from Microsoft. If you do need to build
  for older versions, you can set ``DISTUTILS_USE_SDK=1`` and ``MSSdk=1`` to
  force a the currently activated version of MSVC to be found, and you should
  exercise care when designing your extension not to malloc/free memory across
  different libraries, avoid relying on changed data structures, and so on. Tools
  for generating extension modules usually avoid these things for you.



适用于 Linux 的二进制扩展
---------------------------

**Binary extensions for Linux**

.. tab:: 中文

  Linux 二进制文件必须使用足够旧的 glibc 版本，以确保与旧版发行版的兼容性。 `manylinux <https://github.com/pypa/manylinux>`_ Docker 镜像提供了一个构建环境，包含足够旧的 glibc 版本，能够支持大多数当前 Linux 发行版在常见架构上的兼容性。

.. tab:: 英文

  Linux binaries must use a sufficiently old glibc to be compatible with older
  distributions. The `manylinux <https://github.com/pypa/manylinux>`_ Docker
  images provide a build environment with a glibc old enough to support most
  current Linux distributions on common architectures.

适用于 macOS 的二进制扩展
---------------------------

**Binary extensions for macOS**

.. tab:: 中文

  在 macOS 上的二进制兼容性由目标最小部署系统决定，例如 *10.9*，通常在 macOS 上构建二进制文件时，通过 ``MACOSX_DEPLOYMENT_TARGET`` 环境变量来指定。当使用 setuptools / distutils 构建时，部署目标通过标志 ``--plat-name`` 指定，例如 ``macosx-10.9-x86_64``。有关 macOS Python 发行版的常见部署目标，请参阅 `MacPython Spinning Wheels wiki <https://github.com/MacPython/wiki/wiki/Spinning-wheels>`_ 。

.. tab:: 英文

  Binary compatibility on macOS is determined by the target minimum deployment
  system, e.g. *10.9*, which is often specified with the
  ``MACOSX_DEPLOYMENT_TARGET`` environmental variable when building binaries on
  macOS. When building with setuptools / distutils, the deployment target is
  specified with the flag ``--plat-name``, e.g. ``macosx-10.9-x86_64``. For
  common deployment targets for macOS Python distributions, see the `MacPython
  Spinning Wheels wiki
  <https://github.com/MacPython/wiki/wiki/Spinning-wheels>`_.

发布二进制扩展
============================

**Publishing binary extensions**

.. tab:: 中文

  通过 PyPI 发布二进制扩展使用与发布纯 Python 包相同的上传机制。你可以使用构建后端为你的扩展构建一个 wheel 文件，并通过 :doc:`twine <twine:index>` 将其上传到 PyPI。

.. tab:: 英文

  Publishing binary extensions through PyPI uses the same upload mechanisms as
  publishing pure Python packages. You build a wheel file for your extension
  using the build-backend and upload it to PyPI using
  :doc:`twine <twine:index>`.

避免仅发布二进制版本
--------------------------

**Avoid binary-only releases**

.. tab:: 中文

  强烈建议你发布二进制扩展文件以及用于构建它们的源代码。这使得用户在需要时可以从源代码构建扩展。特别是，对于某些 Linux 发行版，它们在自己的构建系统中从源代码构建扩展，以便将其纳入发行版的软件包仓库，这是必需的。

.. tab:: 英文

  It is strongly recommended that you publish your binary extensions as
  well as the source code that was used to build them. This allows users to
  build the extension from source if they need to. Notably, this is required
  for certain Linux distributions that build from source within their
  own build systems for the distro package repositories.

弱链接
------------

**Weak linking**

.. tab:: 中文

  FIXME: 此部分需要充实。

.. tab:: 英文

  FIXME: This section needs to be fleshed out.

其他资源
====================

**Additional resources**

.. tab:: 中文

  跨平台开发和分发扩展模块是一个复杂的话题，因此本指南主要集中在提供指向各种自动化处理底层技术挑战的工具的建议。本节中的附加资源旨在帮助开发者深入了解这些系统在运行时所依赖的底层二进制接口。

.. tab:: 英文

  Cross-platform development and distribution of extension modules is a complex topic,
  so this guide focuses primarily on providing pointers to various tools that automate
  dealing with the underlying technical challenges. The additional resources in this
  section are instead intended for developers looking to understand more about the
  underlying binary interfaces that those systems rely on at runtime.

使用 scikit-build 生成跨平台 wheel
-------------------------------------------------

**Cross-platform wheel generation with scikit-build**

.. tab:: 中文

  `scikit-build <https://scikit-build.readthedocs.io/en/latest/>`_ 包帮助抽象化跨平台构建操作，并在创建二进制扩展包时提供额外的功能。关于 Python 二进制扩展模块的 C 运行时、编译器和构建系统生成器的更多文档，也可以在以下链接中找到： `C 运行时, 编译, 以及构建系统生成器 <https://scikit-build.readthedocs.io/en/latest/generators.html>`_。

.. tab:: 英文

  The `scikit-build <https://scikit-build.readthedocs.io/en/latest/>`_ package
  helps abstract cross-platform build operations and provides additional capabilities
  when creating binary extension packages. Additional documentation is also available on
  the `C runtime, compiler, and build system generator
  <https://scikit-build.readthedocs.io/en/latest/generators.html>`_ for Python
  binary extension modules.

C/C++ 扩展模块简介
---------------------------------------

**Introduction to C/C++ extension modules**

.. tab:: 中文

  有关在 Debian 系统上 CPython 如何使用扩展模块的更深入解释，请参阅以下文章：

  * `什么是 (c)python 扩展模块？ <https://thomasnyberg.com/what_are_extension_modules.html>`_
  * `释放 GIL <https://thomasnyberg.com/releasing_the_gil.html>`_
  * `使用 C++ 编写 CPython 扩展模块 <https://thomasnyberg.com/cpp_extension_modules.html>`_

.. tab:: 英文

  For a more in depth explanation of how extension modules are used by CPython on
  a Debian system, see the following articles:

  * `What are (c)python extension modules? <https://thomasnyberg.com/what_are_extension_modules.html>`_
  * `Releasing the gil <https://thomasnyberg.com/releasing_the_gil.html>`_
  * `Writing cpython extension modules using C++ <https://thomasnyberg.com/cpp_extension_modules.html>`_

二进制 wheel 的其他注意事项
-------------------------------------------

**Additional considerations for binary wheels**

.. tab:: 中文

  `pypackaging-native <https://pypackaging-native.github.io/>`_ 网站提供了有关使用本地代码打包 Python 包的更多内容。它旨在概述此类项目中最重要的打包问题，并提供深入的解释和参考资料。

  涵盖的主题示例包括非 Python 编译依赖项（“本地依赖项”）、本地代码的 ABI（应用程序二进制接口）重要性、对 SIMD 代码的依赖以及交叉编译等。

.. tab:: 英文

  The `pypackaging-native <https://pypackaging-native.github.io/>`_ website has
  additional coverage of packaging Python packages with native code. It aims to
  provide an overview of the most important packaging issues for such projects,
  with in-depth explanations and references.

  Examples of topics covered are non-Python compiled dependencies ("native
  dependencies"), the importance of the ABI (Application Binary Interface) of
  native code, dependency on SIMD code and cross compilation.
