# Array

数组长度也是类型的一部分



数组在函数间进行传递会进行完成复制操作，这样不利于优化内存和性能，比较好的一种做法是使用指针，将数组的地址传入函数，此时只需要分配8字节的内存给指针就可以了
