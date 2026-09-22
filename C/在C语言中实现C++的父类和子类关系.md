C++ 中有继承的概念，继承中有父类和子类。子类会继承父类的所有属性。这种性质在C 中怎么体现呢？

思考一下： C++ 之前继承画的子类对象内存分布图是 父类属性在上面，然后紧挨着子类定义的属性。

C 中也可以实现这种父类和子类概念。

## C 语言中实现 C++ 的父类和子类

1. 抽出公共部分，定义一个公共的结构体
2. 凡是子类，其结构体定义中第一个成员放这个公共结构体变量

只需要这两步，就形成了父子关系。 公共结构体就是父类，如下面的 `MemoryContextData`，其余的就是子类，如下面的 `AllocSetContext`。

这种方式在 Postgresql 中大量存在。

```c
typedef struct MemoryContextData
{
	pg_node_attr(abstract)		/* there are no nodes of this type */

	NodeTag		type;			/* identifies exact kind of context */
	/* these two fields are placed here to minimize alignment wastage: */
	bool		isReset;		/* T = no space alloced since last reset */
	bool		allowInCritSection; /* allow palloc in critical section */
	Size		mem_allocated;	/* track memory allocated for this context */
	const MemoryContextMethods *methods;	/* virtual function table */
	MemoryContext parent;		/* NULL if no parent (toplevel context) */
	// 当前节点的第一个子节点
	MemoryContext firstchild;	/* head of linked list of children */
	// 下面是当前节点的兄弟节点
	MemoryContext prevchild;	/* previous child of same parent */
	MemoryContext nextchild;	/* next child of same parent */
	const char *name;			/* context name (just for debugging) */
	const char *ident;			/* context ID if any (just for debugging) */
	MemoryContextCallback *reset_cbs;	/* list of reset/delete callbacks */
} MemoryContextData;
```

```c
/*
 * AllocSetContext is our standard implementation of MemoryContext.
 *
 * Note: header.isReset means there is nothing for AllocSetReset to do.
 * This is different from the aset being physically empty (empty blocks list)
 * because we will still have a keeper block.  It's also different from the set
 * being logically empty, because we don't attempt to detect pfree'ing the
 * last active chunk.
 */
typedef struct AllocSetContext
{
	MemoryContextData header;	/* Standard memory-context fields */
	/* Info about storage allocated in this context: */
	// 当前 context 拥有的全部 Blocks ,用链表的形式串联起来了
	AllocBlock	blocks;			/* head of list of blocks in this set */
	// 空闲的 memory chunks 链表。相同大小的 chunk 在同一个链表上。
	MemoryChunk *freelist[ALLOCSET_NUM_FREELISTS];	/* free chunk lists */
	/* Allocation parameters for this context: */
	Size		initBlockSize;	/* initial block size */
	Size		maxBlockSize;	/* maximum block size */
	Size		nextBlockSize;	/* next block size to allocate */
	Size		allocChunkLimit;	/* effective chunk size limit */
	AllocBlock	keeper;			/* keep this block over resets */
	/* freelist this context could be put in, or -1 if not a candidate: */
	int			freeListIndex;	/* index in context_freelists[], or -1 */
} AllocSetContext;

typedef AllocSetContext *AllocSet;
```

这里 `MemoryContextData` 就是 父类，`AllocSetContext` 是子类，`AllocSetContext` 是内存上下文 `MemoryContextData` 的一种实现。

`AllocSetContext *` 可以安全的转化成 `MemoryContextData *`。 当父类指针`MemoryContextData *` 实际上指向的是一个   `AllocSetContext ` 对象时候，

父类指针也可以强制类型转化成 `AllocSetContext *` 。

## Postgresql 内存上下文父子类实现

### （这是 PostgreSQL 非常经典的设计）

可以把 `MemoryContextData` 看成一个**基类**：

```
                MemoryContextData
                      ▲
      ┌───────────────┼───────────────┐
      │               │               │
      │               │               │
 AllocSetContext  GenerationContext  SlabContext
```

每个"子类"都把 `MemoryContextData` 放在结构体的**第一个成员**：

```
typedef struct AllocSetContext
{
    MemoryContextData header;   // 必须在第一个
    ...
} AllocSetContext;
```

因此：

- `AllocSetContext *` 可以安全地当作 `MemoryContext`（即 `MemoryContextData *`）传递给通用接口；
- 当调用 `AllocSetAlloc()` 时，又可以把这个 `MemoryContext` 强制转换回 `AllocSetContext *`，因为两者指向的是**同一块内存的起始地址**。

这种设计在 Linux 内核、PostgreSQL、Git 等大型 C 项目中都非常常见，本质上就是利用**第一个成员偏移量为 0**来模拟 C++ 的继承和多态。