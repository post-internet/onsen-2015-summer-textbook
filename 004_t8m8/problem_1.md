# 問題1 (Is it a Right Triangle?) 解説

## 問題

[Is it a Right Triangle?](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0003&lang=jp)  
三角形の3辺が与えられるので、直角三角形かどうかを判定する問題です。

## 解答例

3辺をそれぞれa,b,cとします。  
一番長い辺がaだとすると、ピタゴラスの定理により、直角三角形の場合には`a*a = b*b + c*c`が成り立つので、  この性質を利用して直角三角形の判定を行います。


```java

int n = in.nextInt();

// for文でn回ループしてももちろんok
while (n-- > 0) {
	int a = in.nextInt();
	int b = in.nextInt();
	int c = in.nextInt();

	// aが一番大きくなるようにswapをする
	if (a < b) {
		int t = a; a = b; b = t;
	}
	if (a < c) {
		int t = a; a = c; c = t;
	}

	System.out.println(a*a == b*b + c*c ? "YES" : "NO");
}
```
以下のように書いてもok

```java

int n = in.nextInt();

while (n-- > 0) {
	int a = in.nextInt();
	int b = in.nextInt();
	int c = in.nextInt();

	if (a*a == b*b + c*c || b*b == c*c + a*a || c*c == a*a + b*b) {
		System.out.println("YES");
	} else {
		System.out.println("NO");
	}
}

```

