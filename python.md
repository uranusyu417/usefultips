# Python相关
## pyserial
在创建serial实例时，两个timeout参数会影响到串口读写的行为
> 
\_\_init\_\_(port=None, baudrate=9600, bytesize=EIGHTBITS, parity=PARITY_NONE, stopbits=STOPBITS_ONE, `timeout`=None, xonxoff=False, rtscts=False, `write_timeout`=None, dsrdtr=False, inter_byte_timeout=None)

timeout英文解释如下
> 
- timeout = None: wait forever / until requested number of bytes are received  即阻塞模式
- timeout = 0: non-blocking mode, return immediately in any case, returning zero or more, up to the requested number of bytes  实际使用，如果设为0，通常无法读到一个完整的485数据帧，例如一个完整的modbus消息，会读到零碎的数据字节，这种使用方法，要求程序要做相应的组包处理
- timeout = x: set timeout to x seconds (float allowed) returns immediately when the requested number of bytes are available, otherwise wait until the timeout expires and return all bytes that were received until then.》
>

## pure Python code to C
使用Cython可将每个py module编成对应的C文件，链接成独立的动态链接库文件

从隐藏代码的目的出发，可以用Cython转成so文件，对于package，将对应\_\_init\_\_.py文件在`py_modules中`声明，随后通过`python setup.py build`就可以生成包含so与\_\_init\_\_.py的package包。
   
## 文档化
使用Sphinx生成文档，快速使用参考范例[Documenting Your Project Using Sphinx](https://pythonhosted.org/an_example_pypi_project/sphinx.html)
