# 开发文档

## 代码整体思路
代码分为四个部分
```dtd
--> CodingSet类的实现
--> HuffmanFolderCompressor
--> HuffmanFolderDecompressor
--> Main(MainGUI)
```

```dtd
--> CodingSet类用来实现Huffman编码的构造
--> HuffmanFolderCompressor用来实现文件夹的压缩
        其中通过递归找到文件，并将文件路径，将文件名
        文件的哈夫曼编码集等一并存储在映射表中，并利用
        java的序列化将其写入到压缩后的文件中进行保存
--> HuffmanFolderDecompressor用来实现文件的解压缩
        其中通过java的序列化（注意顺序）来读出文件
        名称，路径，以及编码表，然后就是对文件进行
        配对解压缩
--> Main(GUIMain)用来实现用户操作
```

## 对核心问题的解决

>文件的压缩与解压缩 

（1）最主要的就是注意Buffered的输入输出流的操作，如果单个字节的读取太慢
缓冲区我选择的比较打，MB单位的，这样会快一些

（2）第二就是说注意不要一次将文件全部读入，例如，我在解压缩的时候我用字符串
来表示我的压缩后的文件流，如果一次性读入的话，由于字符串在内存中以字符数组
的形式存储，需要分配下标，而下标时整形的，所以当下标超过21亿，也就是文件
接近G单位时，会报错OutOfMemory，Heap溢出，这个是更改堆大小改变不了的，
因为它超过数组的最大可能长度

（3）第三，文件名的还原，我在压缩递归的时候，用一个HashMap来存储文件的路径
