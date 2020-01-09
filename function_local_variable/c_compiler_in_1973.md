# 1973年的C編譯器

到現在為止，我們以逐步漸進的方式開發了C編譯器。這樣的開發流程在某種意義上，可以說是走過了C語言的歷史。

看看現在的C語言，充滿了意義不清和過於複雜之處，這些部份如果跳脫歷史來看根本無法理解。現今C語言的難解之處，如果看看早期C的程式碼，了解早期C語言的樣子和後來語言與編譯器的發展，會覺得很多地方茅塞頓開。

C在1972年，為了Unix而開始開發。1972、1973年當時的，也就是C語言歷史中極早期的程式原始碼有留在磁帶裡，該檔案有被讀出並公開放在網路上。我們來看看[當時的C編譯器程式碼](https://github.com/qrush/unix/tree/master/src/c)吧。底下的程式碼，是`printf`格式中，接收訊息並編譯為錯誤訊息顯示的函式：

```c
error(s, p1, p2) {
  extern printf, line, fout, flush, putchar, nerror;
  int f;

  nerror++;
  flush();
  f = fout;
  fout = 1;
  printf("%d: ", line);
  printf(s, p1, p2);
  putchar('\n');
  fout = f;
}
```


