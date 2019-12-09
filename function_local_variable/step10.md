# 第10步：複數文字的區域變數

前面的章節中我們設定變數名為1個字，讓a到z這26個變數固定存在。在這一章，要支援比1個字更長的識別符號，讓其可以編譯如下的程式碼：

```c
foo = 1;
bar = 2 + 3;
return foo + bar; // 回傳6
```


