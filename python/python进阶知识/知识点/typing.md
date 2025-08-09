```python
from typing import Union

def process_value(value: Union[int, str]) -> None:
```

- **类型导入**：从 `typing` 模块导入 `Union`，它的作用是允许参数接受多种类型。
- **参数类型**：`value: Union[int, str]` 表明 `value` 可以是整数或者字符串。
- **返回值**：`-> None` 说明函数没有返回值。

### 类型检查与处理逻辑

```python
    if isinstance(value, int):
        print(f"接收到的是整数：{value}")
    elif isinstance(value, str):
        print(f"接收到的是字符串：{value}")
    else:
        raise TypeError("只支持整数或字符串类型")
```

- **运行时检查**：运用 `isinstance` 进行类型判断，这种方式在 Python 运行时是有效的。

- 分支处理

  ：

  - 当传入的是整数时，会打印一条包含该整数的消息。
  - 当传入的是字符串时，会打印一条包含该字符串的消息。
  - 如果传入的既不是整数也不是字符串，就会抛出 `TypeError` 异常。

### 调用示例

```python
process_value(42)       # 输出：接收到的是整数：42
process_value("hello")  # 输出：接收到的是字符串：hello
```

- **有效调用**：这两个示例分别传入了整数和字符串，都能正常执行并输出相应结果。
- **类型安全**：虽然 Python 不会在运行时强制进行类型检查，但类型提示可以帮助 IDE 或静态分析工具发现类型方面的错误。

### 代码特性

1. 静态类型检查

   像 MyPy 这类工具能够验证调用是否符合类型要求。例如：

   

   ```python
   process_value(3.14)  # 静态类型检查会报错，但运行时才会抛出异常
   ```

2. **运行时验证**：通过 `isinstance` 确保代码在运行时的行为是正确的。

3. **防御性编程**：`else` 分支能捕捉到非法类型，避免出现未定义的行为。