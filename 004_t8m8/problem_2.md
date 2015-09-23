# 問題2 (Drawing Lots) 解説

## 問題

[Drawing Lots](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=0011&lang=jp)  
あみだくじの縦線の数と横線が与えられるので、最終的な並びを答える問題です。

## 解答例

始めの状態から順にswapしていくと最終的な状態が得られます。  
横線は上にあるものから順に適用していかないと正しい解が得られませんが、今回の問題は親切に上から順に与えてくれるので順番通りに交換していきましょう。  
縦線の本数が入力で与えられるので、状態は配列として持つと良いと思います。


```java
int w = in.nextInt();
int n = in.nextInt();

// 配列に初期状態を持っておく
int[] x = new int[w];
for (int i=0; i<w; i++) {
	x[i] = i + 1;
}

while (n-- > 0) {
	String[] str = in.next().split(",");
	// 配列は0-baseなのに対して、入力は1-baseで与えられるので注意
	int a = Integer.parseInt(str[0]) - 1;
	int b = Integer.parseInt(str[1]) - 1;
	
	// swap
	int t = x[a];
	x[a] = x[b];
	x[b] = t;
}

for (int ans : x) {
	System.out.println(ans);
}
```
