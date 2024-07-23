---
layout : post
title : "shell脚本学习笔记"
author : "法厄异"
header-style: text
catalog : true
tags:
     -shell
---
# 一般使用步骤

Shell 脚本是使用 Shell 编程语言编写的脚本文件，可以执行一系列的命令。在 Linux 和 Unix 系统中，最常用的 Shell 是 Bash（Bourne Again SHell）。下面是一个简单的示例，说明如何编写一个 Bash 脚本：

1. **创建脚本文件**：
   使用文本编辑器创建一个新的文件，例如使用 `nano` 或 `vim`。

   ```bash
   nano hello.sh
   ```

2. **添加她他行（Shebang）**：
   在脚本的第一行，你需要指定解释器的位置，通常是 `/bin/bash`。

   ```bash
   #!/bin/bash
   ```

3. **编写脚本**：
   接下来，你可以添加你想要执行的任何命令。例如，以下脚本将打印 "Hello, World!" 到控制台。

   ```bash
   #!/bin/bash
   
   echo "Hello, World!"
   ```

4. **保存并退出编辑器**：
   如果你使用的是 `nano`，按下 `Ctrl+X`，然后按 `Y` 保存更改，最后按 `Enter` 键确认。

5. **给脚本添加执行权限**：
   使用 `chmod` 命令来改变文件的权限，使其可执行。

   ```bash
   chmod +x hello.sh
   ```

6. **运行脚本**：
   现在，你可以通过以下命令运行脚本：

   ```bash
   ./hello.sh
   ```

这是一个非常基础的例子。Shell 脚本可以变得非常复杂，包括条件语句、循环、函数、输入输出重定向等高级特性。这里有一个稍微复杂一点的例子，它会读取用户的名字并打招呼：

```bash
#!/bin/bash

# 提示用户输入名字
echo -n "Please enter your name: "
read name

# 打招呼
echo "Hello, $name!"
```

这个脚本使用了 `read` 命令来从用户那里获取输入，并使用变量 `$name` 来存储输入的名字。

# 基本语法

Shell 脚本的基本语法涵盖了变量、注释、流程控制结构（如条件语句和循环）、函数定义以及一些内置命令的使用。下面是这些概念的简要介绍和示例：

## 变量
在 Shell 中，变量用于存储数据。它们不需要显式声明类型，而且通常以美元符号 `$` 引用。

#### 示例：
```bash
name="Alice"
echo "Hello, $name!"
```

### 注释
单行注释以 `#` 开头。

#### 示例：
```bash
# 这是一条注释
echo "Hello, world!"
```

## 流程控制

#### 条件语句 (if-else)
```bash
value=5
if [ $value -gt 10 ]; then
    echo "Value is greater than 10."
elif [ $value -eq 10 ]; then
    echo "Value is equal to 10."
else
    echo "Value is less than 10."
fi
```

这段代码是一个典型的 Bash shell 脚本中的条件判断语句，用来根据变量 `value` 的数值进行不同的输出。让我们逐行分析：

1. `value=5` 
   - 这一行设置了一个名为 `value` 的变量，并将其赋值为数字 `5`。

2. `if [ $value -gt 10 ]; then`
   - 这是 `if` 语句的开始。`[` 和 `]` 是测试命令的一部分，用来评估逻辑表达式的真假。`$value -gt 10` 表达式检查 `value` 是否大于 `10`。如果这个条件为真（即 `value` 大于 `10`），那么接下来的代码块会被执行。

3. `    echo "Value is greater than 10."`
   - 如果上面的条件为真，这行代码将被执行，输出字符串 "Value is greater than 10."。

4. `elif [ $value -eq 10 ]; then`
   - `elif` 是 "else if" 的缩写，表示另一个条件语句。如果第一个条件 (`$value -gt 10`) 不成立，则会检查 `value` 是否等于 `10`。如果这个条件为真，接下来的代码块会被执行。

5. `    echo "Value is equal to 10."`
   - 如果第二个条件 (`$value -eq 10`) 为真，这行代码将被执行，输出字符串 "Value is equal to 10."。

6. `else`
   - 如果前面的所有条件都不满足，`else` 子句将被触发，执行其下的代码块。

7. `    echo "Value is less than 10."`
   - 当 `else` 子句被触发时，这行代码将被执行，输出字符串 "Value is less than 10."。

8. `fi`
   - `fi` 是 `if` 的反向书写，表示条件语句的结束。

在这个特定的例子中，因为 `value` 被设定为 `5`，所以最终输出将是 "Value is less than 10."，因为 `5` 小于 `10`。

##### -gt -eq是什么意思？

在 Shell 脚本中，`-gt` 和 `-eq` 是用于比较整数的测试运算符，它们是作为 `[` 或 `test` 命令的一部分使用的。这里是它们的具体含义：

- `-gt`：这个运算符代表 "greater than"，用于检查左边的整数是否大于右边的整数。例如，`[ $a -gt $b ]` 会检查变量 `$a` 的值是否大于变量 `$b` 的值。

- `-eq`：这个运算符代表 "equal"，用于检查两边的整数是否相等。例如，`[ $a -eq $b ]` 会检查变量 `$a` 的值是否等于变量 `$b` 的值。

这些运算符通常与条件语句（如 `if` 语句）一起使用，以便根据比较的结果执行不同的代码块。例如：

```bash
a=5
b=10

if [ $a -gt $b ]; then
    echo "a is greater than b"
elif [ $a -eq $b ]; then
    echo "a is equal to b"
else
    echo "a is less than or equal to b"
fi
```

在这个例子中，由于 `$a` 的值小于 `$b` 的值，最后一段 `else` 代码块会被执行，输出 "a is less than or equal to b"。

注意，在使用这些运算符时，两边的空格是必需的，例如 `[ $a -gt $b ]` 而不是 `[ $a-gt$b ]`。这是因为 Shell 解释器需要能够区分出运算符和变量名。

在 Shell 脚本中，检测一个整数是否小于另一个整数，你可以使用 `-lt` 运算符。`-lt` 是 "less than" 的缩写，用于测试左侧的整数是否小于右侧的整数。

例如：

```bash
a=3
b=5

if [ $a -lt $b ]; then
    echo "$a is less than $b"
else
    echo "$a is not less than $b"
fi
```

在这个例子中，因为 `$a` 的值是 `3`，而 `$b` 的值是 `5`，`$a` 确实小于 `$b`，所以输出将会是 `"3 is less than 5"`。

同样地，如果你想要检查一个整数是否小于等于另一个整数，你可以使用 `-le` 运算符（"less than or equal" 的缩写）。例如：

```bash
a=3
b=3

if [ $a -le $b ]; then
    echo "$a is less than or equal to $b"
else
    echo "$a is greater than $b"
fi
```

在这个情况下，因为 `$a` 和 `$b` 都等于 `3`，所以输出将会是 `"3 is less than or equal to 3"`。

#### 循环 (for 和 while)

```bash
# for 循环
for i in {1..5}; do
    echo "Number: $i"
done

# while 循环
j=1
while [ $j -le 5 ]; do
    echo "Counting: $j"
    j=$((j+1))
done
```

## 函数定义
```bash
greet() {
    local name="$1"
    echo "Hello, $name!"
}

greet "Bob"
```

## 输入/输出
使用 `read` 命令从标准输入读取数据，使用 `echo` 或 `printf` 将数据写入标准输出。

#### 示例：
```bash
echo -n "What is your name? "
read name
echo "Hello, $name!"
```

## 内置命令和外部命令
内置命令直接由 shell 解释器处理，而外部命令则由系统中的其他程序执行。

#### 内置命令示例：
```bash
cd /path/to/directory
```

#### 外部命令示例：
```bash
ls -l
```

## 参数传递
脚本可以从命令行接收参数。

#### 示例：
```bash
#!/bin/bash
echo "First argument: $1"
echo "Second argument: $2"
```

运行时：
```bash
./script.sh arg1 arg2
```

## 退出状态
脚本或命令执行后返回一个退出状态码。成功通常为 0，失败为非零值。

#### 示例：
```bash
if command_that_might_fail; then
    echo "Command succeeded."
else
    echo "Command failed with exit code $?"
fi
```

这些只是 Shell 脚本编程的基础，但它们足以让你开始编写简单的脚本来自动化任务或进行更复杂的系统管理操作。
