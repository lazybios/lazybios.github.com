---
layout: post
title: 字节顺序与大小端问题
categories:
- Linux 网络编程
tags:
- linux
- socket
---

网络字节顺序(Net Byte Order)与主机字节顺序(Host Byte Order)是两个相对应的概念，准确的说网络字节顺序是主机字节顺序的子集，主机字节顺序根据不同的cpu架构一般分为大端和小端两种模式，即二者存放操作数的方式不同，前者将低有效位存放在高地址，高有效位存放在低地址，后者相反高有效位存放在高地址，低有效位存放在地址，
> 大端 双字节数0x1234   
        | data |<-- address   
        | 0x12 |<-- 0x00002000  
        | 0x34 |<-- 0x00002001   
 
.
> 小端 双字节数0x1234   
        | data |<-- address  
        | 0x34 |<-- 0x00002000  
        | 0x12 |<-- 0x00002001   
        
因为网络字节顺序用于在各种不同网络间进行数据传输，所以其模式是固定的，采用大端模式，但主机由于不同实现方式，故是不断变化的，所以主机在收到网络字节顺序要根据自己的实际情况进行字节顺序的转换

当然之所以叫做字节顺序，即是字节之间的顺序，只有大于1字节的数据才会有顺序的差别，1字节内包括1字节的顺序在任何机器上都是一样的，不需要任何形式的转换

既然要转换就要知道自己机器的实际处理器类型模式，一般可以通过宏定义，或利用共用体的存储特性去识别
######宏定义
```c
#include <stdio.h>
#include <endian.h>

int main(void){
	printf("Big-endian:%d \nLittle-endian:%d \nmine:%d\n",__BIG_ENDIAN,
									__LITTLE_ENDIAN,__BYTE_ORDER);
	return 0;
}
```

######共用体特性
```c
#include <stdio.h>
#include <stdlib.h>

union word{
	int a;
	char b;
}c;

int checkCPU(void)
{
	c.a = 1;
	return (c.b==1);
}

int main(void)
{
	int i;
	i = checkCPU();
	if(i==0) printf("BIG ENDIAN\n");
	if(i==1) printf("LITTLE ENDIAN\n");
	return 0;
}
```
