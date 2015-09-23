# 問題3 (The Great Summer Contest) 解説

## 問題

[The Great Summer Contest](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=1077)  
競プロの問題を分類すると"Math, Greedy, Geometry, DP, Graph, Other"の6種類になりました。(ちなみにDPというのは日本語で動的計画法と言うアルゴリズムで、幅広い問題に適用することができます。)


1. MathとDPの中から3問
2. GreedyとGraphの中から3問 
3. GeometryとOtherの中から3問
4. MathとDPの中から1問、GreedyとGraphの中から1問、GeometryとOtherの中から1問

の4種類のコンテストを開くことができます。1度使った問題は使用できません。

一番多く開催できるように組み合わせたときのコンテスト数を求めてください。

## 解答例

この問題で気がつくべきことは、"1〜3番のコンテストを1回ずつ開催することと、4番のコンテストを3回開催することは等価である"ということです。  
このことから、4番目のコンテストを0回開催する場合、1回開催する場合、2回開催する場合の3パターンのみを考慮して、その中の最大値を求めれば良いことになります。


```java
while (true) {

	int outer = 0;

	int[] num = new int[6];
	for (int i=0; i<6; i++) {
		num[i] = in.nextInt();
		outer += num[i];
	}
	
	// すべて0のとき(=和が0のとき)入力の終わりなのでbreakする
	if (outer == 0) break;
	
	// i番目と(i+3)番目の組でコンテストが開催できるのでまとめる
	int[] p = new int[3];
	for (int i=0; i<3; i++) {
		p[i] = num[i] + num[i+3];
	}
	
	// 
	int ans = 0;
	
	// iは4番のコンテストを開催する回数
	for (int i=0; i<3; i++) {
		int tmp = i;
		for (int j=0; j<3; j++) {
		
			// p[j] - i < 0 のとき(4番のコンテストを開催することが不可能な場合)は
			// tmp = -1として解の更新が起こらないようにする
			if (p[j] - i < 0) {
				tmp = -1;
				break;
			} 
			
			// j番の開催可能なコンテスト数を求める
			// i回4番のコンテストを開いているので小数点以下切り捨てで(p[j]-i)/3回開催することができる
			else {
				tmp += (p[j] - i)/3;
			}
		}
		
		// ansよりtmpが大きければ更新を行う
		// if (ans < tmp) ans = tmp; と同じ
		ans = Math.max(ans, tmp);
	}

	System.out.println(ans);
}
```
