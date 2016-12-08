# Python相关
## pyserial
在创建serial实例时，两个timeout参数会影响到串口读写的行为
> \_\_init\_\_(port=None, baudrate=9600, bytesize=EIGHTBITS, parity=PARITY_NONE, stopbits=STOPBITS_ONE, `timeout`=None, xonxoff=False, rtscts=False, `write_timeout`=None, dsrdtr=False, inter_byte_timeout=None)

timeout英文解释如下
> 
- timeout = None: wait forever / until requested number of bytes are received  即阻塞模式
- timeout = 0: non-blocking mode, return immediately in any case, returning zero or more, up to the requested number of bytes  实际使用，如果设为0，通常无法读到一个完整的485数据帧，例如一个完整的modbus消息，会读到零碎的数据字节，这种使用方法，要求程序要做相应的组包处理
- timeout = x: set timeout to x seconds (float allowed) returns immediately when the requested number of bytes are available, otherwise wait until the timeout expires and return all bytes that were received until then.
