# Make && New



make 只能声明：map，slice，chan 类型

定义 map 不需要指定长度，通过 make 指定长度也没有意义，map本来就是动态的，即使指定长度没有数据就认为len为0



make用于内建类型（map、slice 和channel）的内存分配。

make返回对象, 且值不为nil; make返回初始化后的（非零）值



new用于各种类型的内存分配,返回指针, 且值为 nil; 

new(T)分配了零值填充的T类型的内存空间，并且返回其地址

