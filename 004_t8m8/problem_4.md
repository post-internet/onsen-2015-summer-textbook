# 問題4 (Osaki) 解説

## 問題

[Osaki](http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=2013)  
列車の発車時刻と到着時刻がn個与えられて、最低限必要な列車の本数を求める問題です。



## 解答例

秒単位で何本の列車が同時に走っているかがわかれば、その中の最大値が解になります。  
これは、秒数をindexとする配列に累積和を保持することで求めることができます。  

累積和は、発車時刻を秒単位で表したものをstart、到着時刻を秒単位で表したものをendとすると、配列のstart番目から(end-1)番目までに1を足していくことをn回行うことで求めることができます。  
(到着と同時に出発できるという問題の設定上、end-1としないとケースによっては解が1多くなってしまいます)  

ここまでをまとめると、"各start,endに対して、配列の区間[start,end)に1を足すことで秒単位の必要列車数を求め、その最大値を出力する。"です。

ですが、実はこれをナイーブに実装すると多くの言語では実行時間制限に間に合いません(ためしてないので間に合っちゃうかも)。ナイーブに実装した場合、与えられるstartとendの数をn、startからendまでの長さをmとすると1つのデータセットに対しO(nm)かかります。

これを解決するためにO(n+m)での実装を行います。  

ナイーブな実装での、startとendが与えられる度にループでその区間に値を加えるという考え方に対して、O(n+m)実装では、配列のstartとendの位置にメモをしておいて、n個のメモが終わったところで一気にループで累積和を求めようという考え方をします。

書いてみると意外とあっけないので、詳しくは下の実装を見てみてください。

このアルゴリズムには名前がつけられていて、[「いもす法」](http://imoz.jp/algorithms/imos_method.html)と呼ばれています。多次元に拡張することで信号処理や、画像処理に応用できるそうです。


```java
while (true) {
	int n = in.nextInt();
	if (n == 0) break;
	
	// 秒単位で1列の配列を持つ
	int[] cnt = new int[24*60*60+1];

	for (int i=0; i<n; i++) {
		String[] s = in.next().split(":");
		String[] t = in.next().split(":");
		
		// startとendを秒単位に変更
		int start = Integer.parseInt(s[0])*3600 + Integer.parseInt(s[1])*60 + Integer.parseInt(s[2]);
		int end = Integer.parseInt(t[0])*3600 + Integer.parseInt(t[1])*60 + Integer.parseInt(t[2]);
		
		// 配列にstartの位置とendの位置をメモ
		cnt[start]++;
		cnt[end]--;
	}
	
	// 一気に累積和を求めつつ解を求めていく
	int ans = cnt[0];
	for (int i=1; i<=24*60*60; i++) {
		cnt[i] += cnt[i-1];
		ans = Math.max(ans,cnt[i]);
	}
	System.out.println(ans);
}
```
