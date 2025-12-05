---
title: 如何在 Linux Shell 脚本中进行函数编程
date: 2025-12-02T09:28:12+08:00
author: LiangMingJian
---

# 概述

Linux Shell 既是一种命令语言，又是一种程序设计语言。

因此，Linux Shell 当然也就支持函数编程，用户可以在 Shell 脚本运用函数封装各种功能，提高脚本的可读性和效率。

# Shell 函数

**Shell 函数的定义**

在 Shell 脚本中，可以按以下格式定义一个函数：

```shell
#!/bin/bash

Fun() {
    echo "Hello World"  
}

Fun
```

与其他编程语言类似，Shell 函数同样使用 `Funtion(){ ... }` 这样的格式来定义函数，不同之处在于，在调用时，Shell 函数直接使用函数名即可，不需要携带括号 `()`。

**Shell 函数的返回**

Shell 函数可以返回值，但返回值只能是一个介于 0 到 255 之间的整数。同时，这个返回值可以在后续语句中通过 `$?` 获取。

> `$?` 会显示最后命令的退出状态（返回值），因此，如果有多个函数顺序执行，这个只会返回最后一个函数的执行结果。

```shell
#!/bin/bash

Fun1() {
    return 0
}

Fun2() {
    return 1
}

Fun1
Fun2
echo $?
# 输出 1
```

**Shell 函数的参数**

与其他编程语言一样，Shell 函数同样支持参数传递。不同的是，Shell 函数只能接收位置参数，同时，这种位置参数不能自定义参数名，只能在函数体内通过 `$1, $2, $3 ... ${10}` 这种形式调用。

> 另外，还可以通过 `$#` 输出参数个数，或 `$*` 以字符串输出所有参数。

```shell
#!/bin/bash

Fun1() {
    echo "参数 $1"
    echo "参数 $2"
    echo "参数 $3"
    echo "参数数量 $#"
    echo "参数字符 $*"
}

Fun1 1 2 3 4 5
```

![](_images/drawingbed/img/Pasted%20image%2020251202160214.png)

# 拓展阅读：给脚本传递的参数，怎么传递到函数？

 假如有 Shell 脚本 `main.sh`，里面的结构如下所示：

```shell
#!/bin/bash

main() {
    echo "Hello World"
}

main
```

现在想要修改这个脚本，让在执行脚本时，支持传入参数，同时这个参数可以传递到函数 main 中使用，比如执行命令 `sh main.sh -i test.txt`。

要实现上述的功能，我们要用到特殊字符 `$*`。

在上文中，`$*` 可以以字符串形式输出所有参数，因此，当我们在脚本中使用这个字符，而不是在函数体内使用这个字符时，就相当于将向脚本传递的所有参数以字符串的形式传递给函数体。因此我们可以将 `main.sh` 修改为：

```shell
#!/bin/bash

main() {
    echo "Hello World"
    echo "$1"
    echo "$2"
}

main $*
```

执行 `sh main.sh -i test.txt` 有：

![](_images/drawingbed/img/Pasted%20image%2020251202162724.png)

不难看出，上述的结果符合我们的需求。

# 拓展阅读：`$*` 与 `$@`

在 Shell 中，`$*` 与 `$@` 是一个相似参数，这两个参数都可以将传入参数以字符串的形式输出，但不同之处在于，这两个参数当与**双引号**一起使用时，将会有不同的变化。

**单独使用时，两者提供一样的功能。**

```shell
#!/bin/bash

main() {
    echo "$1"
    echo "$2"
}

main $*
# 分别输出 -i 和 test.txt
main $@
# 分别输出 -i 和 test.txt
```

**当 `$*` 与双引号一起使用时，会将参数合并为一个字符。**

```shell
#!/bin/bash

main() {
    echo "$1"
    echo "$2"
}

main "$*"
# 会输出 -i test.txt 和一个空白
```

![](_images/drawingbed/img/Pasted%20image%2020251202163301.png)

**当 `$@` 与双引号一起使用时，参数还是分隔的。**

```shell
#!/bin/bash

main() {
    echo "$1"
    echo "$2"
}

main "$@"
# 分别输出 -i 和 test.txt
```

![](_images/drawingbed/img/Pasted%20image%2020251202163354.png)

以上！
