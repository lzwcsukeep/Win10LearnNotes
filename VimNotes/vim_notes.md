# vim 小知识
vim 中有motion,operator。 一个operator 后面加motion 表示对这个移动
范围内的字符进行operator 操作。这种情况下，移动开始和结束的字符包不包括在
operator 内是有区别的。
这种区别分为 includsive 和 excludsive 。
includesive ：移动的最后一个字符也包括在内。
excludesive : 移动的最后一个字符不包括在内。

光标开始位置处的字符始终包含在内。
