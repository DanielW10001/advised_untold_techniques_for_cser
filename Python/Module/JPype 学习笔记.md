# JPype 学习笔记
```python
import jpype # 导入JPype包

jpype.startJVM(jpype.getDefaultJVMPath()) # 启动JVM
jpype.java.lang.System.out.println("Hello World") # 调用Java方法
jpype.shutdownJVM # 关闭JVM
```
- 缺点
	- 不能运行Default包中的类
	- 不能重启JVM
