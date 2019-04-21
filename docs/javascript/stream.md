# Stream



## 等待读写完毕

读取完或者写完stream要调用stream.end()方法, 然后监听finish事件(writestream)或者end事件(readstream)

stream.end()后买上就返回时不可取的, 因为文件可以还没io完毕.
