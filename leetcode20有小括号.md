# leetcode-20|有效括号

### problem:

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

### thinking:

1. 栈的数据结构不熟悉

2. 准备一个空栈，从头到尾遍历字符串，每当遇到任何左括号时将该括号压入栈，每当遇到右括号时将当前栈顶元素弹出，观察能否与当前右括号匹配，如果不匹配则返回False，遍历结束后如果栈为空则括号有效，否则无效，返回False。

3. 利用python构建栈，即把list作为栈使用

### functions:
1. list.find(x) 即在list里面寻找x，并返回所在的下标值，如果不存在则返回-1；

2. continue即跳出当前循环；

3. list.pop() 函数用于移除列表中的一个元素（默认最后一个元素），并且返回该元素的值。

```python
   #!/usr/bin/python3
   #coding=utf-8
    
   list1 = ['Google', 'Runoob', 'Taobao']
   list_pop=list1.pop(1)
   print "删除的项为 :", list_pop
   print "列表现在为 : ", list1
OUT:   
删除的项为 : Runoob
列表现在为 :  ['Google', 'Taobao']
```

### solutions:

```python
ls = []
parentheses = "()[]{}"
for i in range(len(s)):
   # 如果不是括号则继续
   if parentheses.find(s[i]) == -1:
      continue
   # 左括号入栈
   if s[i] == '(' or s[i] == '[' or s[i] == '{':
        ls.append(s[i])
        continue
   if len(ls) == 0:
        return False
   else:
        p = ls.pop()
   # 出栈比较
   if (p == '(' and s[i] == ')') or (p == '[' and s[i] == ']') or (p == '{' and s[i] == '}'):
        continue
   else:
        return False
   if len(ls) > 0:
   		return False
return True
```

