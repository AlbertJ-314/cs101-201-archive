OJ 的 pylint 是静态检查，有时会导致 “没道理的” CE。解决方法有两种：
1）第一行加 `# pylint: skip-file`。
2）如果函数内使用 immutable 类型全局变量（如 `int`），需要在程序最开始声明一下。如果是全局变量是 `list` 类型，则不受影响。



```python
import sys
sys.setrecursionlimit(1<<30)
```



```python
import sys
data = sys.stdin.read().split()
sys.stdout.write("\n".join(map(str, results)) + "\n")
```
