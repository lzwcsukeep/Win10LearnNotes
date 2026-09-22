# 结构体写入文件之后又从文件读取恢复成结构体

C 语言里可以把一个结构体变量写入文件，之后又从这个文件中读取这个结构体，恢复成内存里面的结构体变量。这真的很神奇。

请直接看代码演示：

```c
#include <stdio.h>

typedef struct {

	int id ;
	int age ;
	char name[10] ;
} Student;



// 演示 把一个结构体变量保存进 文件，然后又把保存进文件的 结构体变量读取出来
int main()
{

	Student zhangsan = {.id = 1, .age = 32, .name = "zhangsan2"} ;

	printf("ID: %d, Age: %d, Name: %s\n", zhangsan.id, zhangsan.age, zhangsan.name) ;


	FILE *fp = fopen("student.dat", "wb") ;
	if (fp == NULL) {
		perror("Failed to open file");
		return 1;
	}

	// 把结构体变量写入文件
	fwrite(&zhangsan, sizeof(Student), 1, fp) ;

	fclose(fp) ;

	// 从文件中读取结构体变量
	Student zhang_copy ;
	fp = fopen("student.dat", "rb") ;
	if (fp == NULL) {
		perror("Failed to open file");
		return 1;
	}
	fread(&zhang_copy, sizeof(Student), 1, fp) ;
	fclose(fp) ;

	printf("ID: %d, Age: %d, Name: %s\n", zhang_copy.id, zhang_copy.age, zhang_copy.name) ;
	
	return 0 ;
}
```

这真的很神奇啊，感觉这个就是那些索引文件，索引结构体的基本原理和基本技术。