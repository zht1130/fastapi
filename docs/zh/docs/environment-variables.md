# 环境变量

/// tip

如果你已经了解了“环境变量”是什么以及如何使用它们，可以跳过这一部分。

///

环境变量（也称为 "**env var**"）是一种变量，它存在于 **Python 代码之外**，位于 **操作系统** 中，Python 代码（或其他程序）可以读取它。

环境变量可以用于处理应用程序的 **设置**，作为 Python **安装** 的一部分等。

## 创建和使用环境变量

你可以在 **终端** 中 **创建** 和使用环境变量，而不需要使用 Python：

//// tab | Linux、macOS、Windows Bash

<div class="termy">

```console
// 你可以创建一个名为 MY_NAME 的环境变量
$ export MY_NAME="Wade Wilson"

// 然后你可以用其他程序来使用它，比如
$ echo "Hello $MY_NAME"

Hello Wade Wilson
```

</div>

////

//// tab | Windows PowerShell

<div class="termy">

```console
// 创建一个名为 MY_NAME 的环境变量
$ $Env:MY_NAME = "Wade Wilson"

// 使用其他程序来调用它，比如
$ echo "Hello $Env:MY_NAME"

Hello Wade Wilson
```

</div>

////

## 在 Python 中读取环境变量

你也可以在 Python 之外创建环境变量，比如在终端中（或通过其他方法），然后在 **Python 中读取** 这些环境变量。

例如，你可以创建一个名为 `main.py` 的文件，内容如下：

```Python hl_lines="3"
import os

name = os.getenv("MY_NAME", "World")
print(f"Hello {name} from Python")
```

/// tip

`<a href="https://docs.python.org/3.8/library/os.html#os.getenv" class="external-link" target="_blank">os.getenv()</a>` 的第二个参数是默认返回值。

如果没有提供，默认返回 `None`，在这里我们提供 `"World"` 作为默认值。

///

然后你可以调用这个 Python 程序：

//// tab | Linux、macOS、Windows Bash

<div class="termy">

```console
// 这里我们还没有设置环境变量
$ python main.py

// 因为没有设置环境变量，所以使用了默认值

Hello World from Python

// 但是如果我们首先创建一个环境变量
$ export MY_NAME="Wade Wilson"

// 然后再次调用程序
$ python main.py

// 现在它可以读取环境变量

Hello Wade Wilson from Python
```

</div>

////

//// tab | Windows PowerShell

<div class="termy">

```console
// 这里我们还没有设置环境变量
$ python main.py

// 因为没有设置环境变量，所以使用了默认值

Hello World from Python

// 但是如果我们首先创建一个环境变量
$ $Env:MY_NAME = "Wade Wilson"

// 然后再次调用程序
$ python main.py

// 现在它可以读取环境变量

Hello Wade Wilson from Python
```

</div>

////

由于环境变量可以在代码之外设置，但可以被代码读取，并且不需要和其他文件一起存储（提交到 `git`），因此通常会用它们来做配置或 **设置**。

你还可以只为 **特定的程序调用** 创建环境变量，该变量仅对该程序有效，并且只在其运行期间有效。

要做到这一点，可以在同一行代码中，紧接着程序之前创建环境变量：

<div class="termy">

```console
// 为此程序调用在线创建一个环境变量 MY_NAME
$ MY_NAME="Wade Wilson" python main.py

// 现在它可以读取环境变量

Hello Wade Wilson from Python

// 此后环境变量不再存在
$ python main.py

Hello World from Python
```

</div>

/// tip

你可以在 <a href="https://12factor.net/config" class="external-link" target="_blank">The Twelve-Factor App: Config</a> 阅读更多相关内容。

///

## 类型与验证

这些环境变量只能处理 **文本字符串**，因为它们位于 Python 之外，并且必须与其他程序和整个系统兼容（甚至需要兼容不同的操作系统，如 Linux、Windows、macOS）。

这意味着，从环境变量中读取的 **任何值** 在 Python 中都将是 `str` 类型，任何类型转换或验证都必须在代码中进行。

你将会在 [高级用户指南 - 设置和环境变量](./advanced/settings.md){.internal-link target=_blank} 中学习更多关于如何使用环境变量来处理 **应用程序设置** 的内容。

## `PATH` 环境变量

有一个 **特殊的** 环境变量叫做 **`PATH`**，操作系统（Linux、macOS、Windows）用它来寻找要运行的程序。

`PATH` 环境变量的值是一个长字符串，在 Linux 和 macOS 中由冒号 `:` 分隔的目录构成，在 Windows 中由分号 `;` 分隔。

例如，`PATH` 环境变量可能看起来像这样：

//// tab | Linux、macOS

```plaintext
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

这意味着系统应该在以下目录中寻找程序：

* `/usr/local/bin`
* `/usr/bin`
* `/bin`
* `/usr/sbin`
* `/sbin`

////

//// tab | Windows

```plaintext
C:\Program Files\Python312\Scripts;C:\Program Files\Python312;C:\Windows\System32
```

这意味着系统应该在以下目录中寻找程序：

* `C:\Program Files\Python312\Scripts`
* `C:\Program Files\Python312`
* `C:\Windows\System32`

////

当你在终端中输入一个 **命令** 时，操作系统会 **在`PATH` 环境变量列出的目录中寻找**该程序。

例如，当你在终端中输入 `python` 时，操作系统会在该列表中的 **第一个目录** 中寻找名为 `python` 的程序。

如果找到，它就会 **使用它**。否则，它会继续在 **其他目录** 中寻找。

### 安装 Python 并更新 `PATH`

当你安装 Python 时，可能会问你是否要更新 `PATH` 环境变量。

//// tab | Linux、macOS

假设你安装了 Python，它最终被放在目录 `/opt/custompython/bin` 中。

如果你选择更新 `PATH` 环境变量，安装程序将会把 `/opt/custompython/bin` 添加到 `PATH` 环境变量中。

它可能看起来像这样：

```plaintext
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/custompython/bin
```

这样，当你在终端中输入 `python` 时，系统会在 `/opt/custompython/bin`（最后一个目录）中找到 Python 程序并使用它。

////

//// tab | Windows

假设你安装了 Python，它最终被放在目录 `C:\opt\custompython\bin` 中。

如果你选择更新 `PATH` 环境变量，安装程序将会把 `C:\opt\custompython\bin` 添加到 `PATH` 环境变量中。

```plaintext
C:\Program Files\Python312\Scripts;C:\Program Files\Python312;C:\Windows\System32;C:\opt\custompython\bin
```

这样，当你在终端中输入 `python` 时，系统会在 `C:\opt\custompython\bin`（最后一个目录）中找到 Python 程序并使用它。

////

这样，当你在终端中输入 `python` 时，系统会在 `/opt/custompython/bin`（最后一个目录）中找到 Python 程序并使用它。

因此，如果你输入：

<div class="termy">

```console
$ python
```

</div>

//// tab | Linux、macOS

系统将 **在 `/opt/custompython/bin`** 中找到 `python` 程序并运行它。

这大致等同于输入：

<div class="termy">

```console
$ /opt/custompython/bin/python
```

</div>

////

//// tab | Windows

系统将 **在 `C:\opt\custompython\bin\python`** 中找到 `python` 程序并运行它。

这大致等同于输入：

<div class="termy">

```console
$ C:\opt\custompython\bin\python
```

</div>

////

这些信息在学习 [虚拟环境](virtual-environments.md){.internal-link target=_blank} 时将会派上用场。

## 结论

通过这些内容，你应该对 **环境变量** 是什么以及如何在 Python 中使用它们有了基本的了解。

你还可以在 <a href="https://en.wikipedia.org/wiki/Environment_variable" class="external-link" target="_blank">环境变量维基百科</a> 上阅读更多相关内容。

在很多情况下，环境变量的用途和应用可能不会立刻显现出来。但它们会在开发的许多不同场景中不断出现，所以了解它们是有益的。

例如，你将在下一节 [虚拟环境](virtual-environments.md) 中需要这些信息。