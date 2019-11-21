# 修改分析器

遞迴下降分析法中，只要理解文法，就可以機械性地對應到函式呼叫。因此，考慮到分析器要做的修改，必須思考加上變數名（識別符號）的新文法會長什麼樣。

識別符號我們以`ident`代表，和`num`一樣是終端符號。變數在所有能用數值的地方都可以用，所以我們在原本是`num`的地方改成`num | ident`，文法中使用數值的地方就也可以使用變數了。

同時，文法還得補上代入式。變數得要可以代入才算是變數，我們想要文法能接受像`a=1`這樣的式子。在此配合C的語法，來讓文法可以接受像`a=b=1`這樣的寫法吧。

還有，我們希望可以用分號來區分不同敘述（statement），最終文法會變成以下這樣：

```text
program    = stmt*
stmt       = expr ";"
expr       = assign
assign     = equality ("=" assign)?
equality   = relational ("==" relational | "!=" relational)*
relational = add ("<" add | "<=" add | ">" add | ">=" add)*
add        = mul ("+" mul | "-" mul)*
mul        = unary ("*" unary | "/" unary)*
unary      = ("+" | "-")? primary
primary    = num | ident | "(" expr ")"
```

首先請先確認看看像`42;`或`a=b=2; a+b`這樣的程式是否合乎文法。然後，請修改我們到目前做好的分析器，讓其能分析上述的文法吧。在現階段，像`a+1=5`這樣的式子也可以普通地被分析出來，這是正常運作的結果。要排除像那樣不合法的式子，我們等下一階段再來做。改造分析器，並沒有特別難解之處，只要照至今為止的作法讓文法的成員原原本本對應到函式呼叫應該就可以完成。

因為要加上以分號來區分複數式子，我們得把分析的結果存成複數的結點才行。在這個階段，請準備如下的全域陣列，在其中照分析出來的順序把結點存入。在最後一個結點填上`NULL`，就可以知道尾端在哪裡。新增的程式碼的部份如下所示：

```c
Node *code[100];

Node *assign() {
  Node *node = equality();
  if (consume("="))
    node = new_node(ND_ASSIGN, node, assign());
  return node;
}

Node *expr() {
  return assign();
}

Node *stmt() {
  Node *node = expr();
  expect(";");
  return node;
}

void program() {
  int i = 0;
  while (!at_eof())
    code[i++] = stmt();
  code[i] = NULL;
}
```


