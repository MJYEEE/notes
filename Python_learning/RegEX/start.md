# 正则表达式的快速入门

正则表示式在指定规则下匹配任何字符串通过正则表达式，我们实现指定文本的分割、文本的快速替换等等。

---

### 基本正则表达式语法

- `.` : 匹配除换行符以外的任意一个字符。
- `^` : 匹配字符串的开始。
- `$` : 匹配字符串的结尾。
- `*` : 匹配前面的字符零次或多次。
- `+` : 匹配前面的字符一次或多次。
- `?` : 匹配前面的字符零次或一次。
- `\d` : 匹配一个数字字符，等价于 [0-9]。
- `\D` : 匹配一个非数字字符，等价于 [^0-9]。
- `\w` : 匹配一个字母、数字或下划线字符，等价于 [a-zA-Z0-9_]。
- `\W` : 匹配一个非字母、非数字、非下划线字符，等价于 [^a-zA-Z0-9_]。
- `\s` : 匹配一个空白字符（包括空格、制表符等），等价于 [\t\n\r\f\v]。
- `\S` : 匹配一个非空白字符，等价于 [^ \t\n\r\f\v]。
- `[ ]` : 匹配括号中的任意一个字符，例如 [aeiou] 匹配任意一个元音字母。
- `{m,n}` : 匹配前面的字符至少 m 次，至多 n 次。
- `( )` : 用于获取匹配的部分


### python中的re模块
第一次直接看上面应该都会显得一脸懵逼，接下来我们通过python中的re模块来学习上面语法

---

#### re.search()
`re.search(pattern, string)`在字符串中查找正则表达式模式的第一次匹配。如果找到，返回 Match 对象，否则返回 None。

``` python
import re

text = "Hello, my number is 12345"
match = re.search(r'\d+', text)  # 查找连续的数字
if match:
    print(match.group())  # 输出: 12345
```

#### re.match()
`re.match(pattern, string)` 从字符串的起始位置匹配正则表达式。如果匹配成功，返回 Match 对象，否则返回 None。

```python
import re

text = "123abc"
match = re.match(r'\d+', text)  # 从字符串开头查找连续的数字,如果开头不是就没了
if match:
    print(match.group())  # 输出: 123
```

#### re.findall()
`re.findall(pattern, string)` 返回字符串中所有与正则表达式匹配的部分，结果是一个列表。

```python
import re

text = "Here are some numbers: 123, 456, and 789."
matches = re.findall(r'\d+', text)  # 查找所有的数字
print(matches)  # 输出: ['123', '456', '789']
```


#### re.sub()
`re.sub(pattern, repl, string)` 使用 repl 替换字符串中所有与正则表达式匹配的部分。

```python
import re

text = "I love cats and dogs."
new_text = re.sub(r'cats|dogs', 'animals', text)  # 替换"cats"和"dogs"为"animals"
print(new_text)  # 输出: I love animals and animals.
```

#### re.split()
`re.split(pattern, string)` 根据匹配的正则表达式分割字符串，返回一个列表。

```python
import re

text = "apple, orange; banana:grape"
fruits = re.split(r'[,\s;:]+', text)  # 根据逗号、空格、分号或冒号分割
print(fruits)  # 输出: ['apple', 'orange', 'banana', 'grape']
```

### 提取邮箱实现
```python
import re

text = "Please contact us at support@example.com or sales@example.co.uk"
emails = re.findall(r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}', text)
print(emails)  # 输出: ['support@example.com', 'sales@example.co.uk']
```

正则表达式具体解释：
- `[a-zA-Z0-9._%+-]+`：
    - `[a-zA-Z0-9._%+-]`：匹配电子邮件地址的本地部分（即 @ 符号前面的部分），包括字母、数字、点、下划线、百分号、加号、减号等字符。
    - `+`：表示前面的字符集必须出现一次或多次。
    - `@`：匹配 @ 符号，电子邮件地址的分隔符。

- `[a-zA-Z0-9.-]+`：
    - `[a-zA-Z0-9.-]`：匹配域名部分，域名可以包含字母、数字、点和短横线。
    - `+`：表示前面的字符集必须出现一次或多次。

- `\.[a-zA-Z]{2,}`：
    - `\.`：匹配点 . 符号，表示域名与顶级域名之间的分隔符。
    - `[a-zA-Z]{2,}`：匹配顶级域名部分，要求至少包含两个字母，例如 .com 或 .co.uk。