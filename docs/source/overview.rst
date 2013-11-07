.. _overview:

====
引言
====

概览
====

Python最实用的功能之一是它的交互式解释器.
它可以非常快让你试验你的想法, 不用像大多数编程语言那样多出创建测试文件的开销.
然而, Python发行版的解释器很难扩展, 并且交互性也不太好.

IPython的目标是要建立一个具有互动性和可探索的全面的计算环境.
为了达成这一目标, IPython有三个主要的组件:

* 增强的交互式Python shell.
* 一个去耦合的 :ref:`进程间通信模型 <ipythonzmq>`, 它允许多个客户端连接到一个计算内核,
  最引人瞩目的案例是基于Web的 :ref:`notebook <htmlnotebook>`
* 一个交互式并行计算架构.

IPython是开源的 (基于修订过的BSD许可证发布).

增强的交互式Python shell
========================

IPython的交互式shell (:command:`ipython`), 实现了以下目标:

1. 提供一个比Python默认shell好的交互式shell. IPython在交互式模式中添加了tab键自动完成,
   包含对象自省, 系统shell访问, 检索跨会话的命令历史, 和IPython自己的特殊指令系统.
   它试图为开发代码和勘察问题(如进行数据分析)提供一个高效的环境.
  
2. 在你自己的程序中充当一个可嵌入的, 随时可用的解释器.
   在一个程序内部可以简单地调用一个IPython shell, 并且可以访问当前的命名空间.
   这样的功能在调试时是非常有用的.
  
3. Offer a flexible framework which can be used as the base
   environment for working with other systems, with Python as the underlying
   bridge language. Specifically scientific environments like Mathematica,
   IDL and Matlab inspired its design, but similar ideas can be
   useful in many fields.
  
4. Allow interactive testing of threaded graphical toolkits. IPython
   has support for interactive, non-blocking control of GTK, Qt, WX, GLUT, and
   OS X applications via special threading flags. The normal Python
   shell can only do this for Tkinter applications.

交互式shell的主要功能
-------------------------

* Dynamic object introspection. One can access docstrings, function
  definition prototypes, source code, source files and other details
  of any object accessible to the interpreter with a single
  keystroke (:samp:`?`, and using :samp:`??` provides additional detail).
  
* Searching through modules and namespaces with :samp:`*` wildcards, both
  when using the :samp:`?` system and via the :samp:`%psearch` command.

* Completion in the local namespace, by typing :kbd:`TAB` at the prompt.
  This works for keywords, modules, methods, variables and files in the
  current directory. This is supported via the readline library, and
  full access to configuring readline's behavior is provided. 
  Custom completers can be implemented easily for different purposes
  (system commands, magic arguments etc.)

* Numbered input/output prompts with command history (persistent
  across sessions and tied to each profile), full searching in this
  history and caching of all input and output.

* User-extensible 'magic' commands. A set of commands prefixed with
  :samp:`%` is available for controlling IPython itself and provides
  directory control, namespace information and many aliases to
  common system shell commands.

* Alias facility for defining your own system aliases.

* Complete system shell access. Lines starting with :samp:`!` are passed
  directly to the system shell, and using :samp:`!!` or :samp:`var = !cmd` 
  captures shell output into python variables for further use.

* The ability to expand python variables when calling the system shell. In a
  shell command, any python variable prefixed with :samp:`$` is expanded. A
  double :samp:`$$` allows passing a literal :samp:`$` to the shell (for access
  to shell and environment variables like :envvar:`PATH`).

* Filesystem navigation, via a magic :samp:`%cd` command, along with a
  persistent bookmark system (using :samp:`%bookmark`) for fast access to
  frequently visited directories.

* A lightweight persistence framework via the :samp:`%store` command, which
  allows you to save arbitrary Python variables. These get restored
  when you run the :samp:`%store -r` command.

* Automatic indentation (optional) of code as you type (through the
  readline library).

* Macro system for quickly re-executing multiple lines of previous
  input with a single name via the :samp:`%macro` command. Macros can be
  stored persistently via :samp:`%store` and edited via :samp:`%edit`.

* Session logging (you can then later use these logs as code in your
  programs). Logs can optionally timestamp all input, and also store
  session output (marked as comments, so the log remains valid
  Python source code).

* Session restoring: logs can be replayed to restore a previous
  session to the state where you left it.

* Verbose and colored exception traceback printouts. Easier to parse
  visually, and in verbose mode they produce a lot of useful
  debugging information (basically a terminal version of the cgitb
  module).

* Auto-parentheses via the :samp:`%autocall` command: callable objects can be
  executed without parentheses: :samp:`sin 3` is automatically converted to
  :samp:`sin(3)`

* Auto-quoting: using :samp:`,`, or :samp:`;` as the first character forces
  auto-quoting of the rest of the line: :samp:`,my_function a b` becomes
  automatically :samp:`my_function("a","b")`, while :samp:`;my_function a b`
  becomes :samp:`my_function("a b")`.

* Extensible input syntax. You can define filters that pre-process
  user input to simplify input in special situations. This allows
  for example pasting multi-line code fragments which start with
  :samp:`>>>` or :samp:`...` such as those from other python sessions or the
  standard Python documentation.

* Flexible :ref:`configuration system <config_overview>`. It uses a
  configuration file which allows permanent setting of all command-line
  options, module loading, code and file execution. The system allows
  recursive file inclusion, so you can have a base file with defaults and
  layers which load other customizations for particular projects.

* Embeddable. You can call IPython as a python shell inside your own
  python programs. This can be used both for debugging code or for
  providing interactive abilities to your programs with knowledge
  about the local namespaces (very useful in debugging and data
  analysis situations).

* Easy debugger access. You can set IPython to call up an enhanced version of
  the Python debugger (pdb) every time there is an uncaught exception. This
  drops you inside the code which triggered the exception with all the data
  live and it is possible to navigate the stack to rapidly isolate the source
  of a bug. The :samp:`%run` magic command (with the :samp:`-d` option) can run
  any script under pdb's control, automatically setting initial breakpoints for
  you.  This version of pdb has IPython-specific improvements, including
  tab-completion and traceback coloring support. For even easier debugger
  access, try :samp:`%debug` after seeing an exception.

* Profiler support. You can run single statements (similar to
  :samp:`profile.run()`) or complete programs under the profiler's control.
  While this is possible with standard cProfile or profile modules,
  IPython wraps this functionality with magic commands (see :samp:`%prun`
  and :samp:`%run -p`) convenient for rapid interactive work.

* Simple timing information. You can use the :samp:`%timeit` command to get
  the execution time of a Python statement or expression. This machinery is
  intelligent enough to do more repetitions for commands that finish very
  quickly in order to get a better estimate of their running time. 

.. sourcecode:: ipython

    In [1]: %timeit 1+1
    10000000 loops, best of 3: 25.5 ns per loop

    In [2]: %timeit [math.sin(x) for x in range(5000)]
    1000 loops, best of 3: 719 µs per loop

.. 

  To get the timing information for more than one expression, use the
  :samp:`%%timeit` cell magic command.
  

* Doctest support. The special :samp:`%doctest_mode` command toggles a mode
  to use doctest-compatible prompts, so you can use IPython sessions as
  doctest code. By default, IPython also allows you to paste existing
  doctests, and strips out the leading :samp:`>>>` and :samp:`...` prompts in
  them.

.. _ipythonzmq:

解耦合的双进程模型
==============================

IPython has abstracted and extended the notion of a traditional
*Read-Evaluate-Print Loop* (REPL) environment by decoupling the *evaluation*
into its own process. We call this process a **kernel**: it receives execution
instructions from clients and communicates the results back to them.

This decoupling allows us to have several clients connected to the same
kernel, and even allows clients and kernels to live on different machines.
With the exclusion of the traditional single process terminal-based IPython
(what you start if you run ``ipython`` without any subcommands), all
other IPython machinery uses this two-process model. This includes ``ipython
console``,  ``ipython qtconsole``, and ``ipython notebook``.

As an example, this means that when you start ``ipython qtconsole``, you're
really starting two processes, a kernel and a Qt-based client can send
commands to and receive results from that kernel. If there is already a kernel
running that you want to connect to, you can pass the  ``--existing`` flag
which will skip initiating a new kernel and connect to the most recent kernel,
instead. To connect to a specific kernel once you have several kernels
running, use the ``%connect_info`` magic to get the unique connection file,
which will be something like ``--existing kernel-19732.json`` but with
different numbers which correspond to the Process ID of the kernel.

You can read more about using :ref:`ipython qtconsole <qtconsole>`, and
:ref:`ipython notebook <htmlnotebook>`. There is also a :ref:`message spec
<messaging>` which documents the protocol for communication between kernels
and clients.

.. seealso::
    
    `Frontend/Kernel Model`_ example notebook


交互式并行计算
==============================

Increasingly, parallel computer hardware, such as multicore CPUs, clusters and
supercomputers, is becoming ubiquitous. Over the last several years, we have
developed an architecture within IPython that allows such hardware to be used
quickly and easily from Python. Moreover, this architecture is designed to
support interactive and collaborative parallel computing.

The main features of this system are:

* Quickly parallelize Python code from an interactive Python/IPython session.

* A flexible and dynamic process model that be deployed on anything from 
  multicore workstations to supercomputers.

* An architecture that supports many different styles of parallelism, from
  message passing to task farming.  And all of these styles can be handled
  interactively.

* Both blocking and fully asynchronous interfaces.

* High level APIs that enable many things to be parallelized in a few lines
  of code.

* Write parallel code that will run unchanged on everything from multicore
  workstations to supercomputers.

* Full integration with Message Passing libraries (MPI).

* Capabilities based security model with full encryption of network connections.

* Share live parallel jobs with other users securely.  We call this
  collaborative parallel computing.

* Dynamically load balanced task farming system.

* Robust error handling.  Python exceptions raised in parallel execution are
  gathered and presented to the top-level code.

For more information, see our :ref:`overview <parallel_index>` of using IPython
for parallel computing.

可移植性和Python依赖
-----------------------------------

* 1.0 版本的IPython支持Python 2.6, 2.7, 3.2 和 3.3.

* 0.12 版本开始全面支持Python 3.

* 0.11 版本仅支持Python 2.6 和 2.7.

* 0.9 和 0.10 版本支持Python 2.4以上的版本 (不包括Python 3).

IPython可以工作在以下操作系统:

	* Linux
	* 许多类Unix操作系统(AIX, Solaris, BSD, 等等)
	* Mac OS X
	* Windows (CygWin, XP, Vista, 等等)

请参阅 :ref:`此处 <install_index>` 的指示了解如何安装IPython.

.. include:: links.txt
