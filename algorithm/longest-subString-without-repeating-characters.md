## 方法一：

### 暴力法

**思路**

逐个检查所有的子字符串，看它是否不含重复的字符

**算法**

假设我们有一个函数 `boolean allUnique(String subString)`，如果子字符串中的字符都是唯一的，它会返回true，否则会返回false。我们可以遍历给字符串`s`的所有可能的子字符串并调用函数`allUnique`。如果事实证明返回值为true，那么我们将会更新无重复字符串子串的最大长度的答案。

1. 为了枚举给定字符串的所有子字符串，我们需要枚举它们开始和结束的索引。假设开始和结束的索引分别为`i`和`j`。那么我们有0<= i < j <=n（这里的结束索引j是按惯例排除的）。因此使用i从0到n-1以及j从i+1到n这两个嵌套的循环，我们可以枚举出`s`的所有子字符串。
2. 要检查一个字符串是否有重复字符，我们可以使用集合。我们遍历字符串中的所有字符，并将它们逐个放入`set`中。在放置一个字符之前，我们检查该集合是否已经包含它。如果包含，我们会返回`false`。循环结束后，我们返回`true`。

```
public class Solution{
	public int lengthOfLongestSubstring(String s){
		int n = s.length();
		int ans = 0;
		for(int i = 0; i < n;i++){
			for(int j = i+1;j < = n;j++){
				if(allUnique(s,i,j)) ans = Math.max(ans,j - i);
			}
		}
		return ans;
	}

	public boolean allUnique(String s,ini start,int end){
		Set<Character> set = new HashSet<>();
		for(int i = start ; i < end; i++ ){
			Character ch = s.charAt(i);
			if(set.contains(ch)) return false; 
		}
	return true;
	}

}
```

**复杂度分析**

* 时间复杂度：O(n³)。

要验证索引范围在[i,j)内的字符是否都是唯一的，我们需要检查该范围中的所有字符。因此，它将花费O(j-i)内的时间。

对于给定的 i ，对于所有的 j ∈[i+1，n]所消耗的时间总和为  


...

* 空间复杂度：O(min(n,m)),我们需要O(k)的空间来检查子字符串中是否有重复字符，其中k表示`Set`的大小。而Set的大小取决于字符串n的大小以及字符串/字母m的小小。


## 方法二：

### 滑动窗口

**算法**

暴力法非常简单，但它太慢了。那么我们该如何优化它呢？

在暴力法中，我们会反复检查一个子字符串是否含有重复的字符，但这是没有必要的。如果从索引i到j-1之间的子字符串subString[i,j)已经被检查为没有重复字符。我们只需要检查s[j]对应的字符是否已经存在于子字符串中。

要检查一个字符是否已经存在子字符串中，我们可以检查整个子字符串，这将产生一个复杂度为O(n²)的算法。。。。。。。。。

通过使用HashSet作为滑动窗口，我们可以用O(1)的时间来完成对字符是否在当前的子字符串中检查。

滑动窗口是数组/字符串问题中常用的抽象概念。窗口通常是在数组/字符串中由开始和结束索引定义的一系列元素的集合，即[i,j)。而滑动窗口是可以将两个边界向某一方向“滑动”的窗口。例如，我们将[i,j)向右滑动1个元素，则将他变为[i+1,j+1)。

归于此问题，我们使用 HashSet 将字符串存储在当前窗口 [i,j)（最初j=i）中。然后我们向右侧滑动索引j，如果它不再hashSet中，我们会继续滑动j。知道s[j]已经存在于HashSet中。此时，我们找到的没有重复字符的最长子字符串会以索引i开头。

```

public class Solution{
	public int lengthOfLongestSubstring(String s){
		int n = s.length();
		Set<Character> set = new HashSet<>();
		int ans = 0,i = 0, j = 0;
		while(i < n && j < n){
			//try to extend the range[i,j]
			if(!set.contains(s.charAt(j))){
				set.add(s.charAt(j++));
				ans = Math.max(ans,j - i);
			}
			else{
				set.remove(s.charAt(i++));
			}
		}
		return ans;
	}
}

```

**复杂度分析**

* 时间复杂度：O(2n) = O(n)，在最糟糕的情况下，每个字符将被i和j访问两次。
* 空间复杂度：O(min(m,n))，之前的方法相同。胡奥定窗口法需要O(k)的空间，其中k表示`Set`的大小。而Set的大小取决于字符串n的大小以及字符集/字母m的大小。

## 方法三：优化的滑动窗口

上述的方法最多需要执行2n个步骤。事实上，它可以被进行进一步优化为仅需要n个步骤。我们可以定义到索引的映射，而不是使用集合类判断一个字符是否存在。当我们找到重复的字符时，我们可以立即跳过该窗口。

也就是说，如果s[j]在[i,j)范围内有与j1重复的字符串，我们不需要逐渐增加i。我们可以直接跳过[i,j1]范围内的所有元素，并将i变为j1 + 1。

```
(String s) -> {
int n = s.length(),ans = 0;
//current index of character
Map<Character,Integer> map = new HashMap<>();
//try to extend the range[i,j]
for(int i = 0,j = 0; j < n ;j++){
	if(map.containsKey(s.charAt(j))){
		i = Math.max(map.get(s.charAt(j)),i);	
	}
	ans = Math.max(ans,j - i + 1);
	map.put(s.charAt(j),j + 1);
}

return ans;
}
```

### Java(假设字符集为 ASCII 128）

以前的我们都没有对字符串 s 所使用的字符集进行假设。

当我们知道该字符集比较小的时候，我们可以用一个整数数组作为直接访问表来替换Map。

常用的表如下所示：

* `int[26]` 用于存储'a'-'z'或'A'-'Z'
* `int[128]`用于ASCII码
* `int [256]`用于扩展ASCII码

```
(String) -> {
	int n = s.length(),ans = 0;
	//current index of character
	int[] index = new int[128];
	
	for(int i = 0,j = 0; j < n ;j++){
		i = Math.max(index[s.charAt(j)] , i );
		ans = Math.max(ans, j - i + 1);
		index[s.charAt(j)] = j + 1;
	}
	return ans;
}
```

**复杂度分析**

* 时间复杂度：O(n)，索引j将会迭代n次。
* 空间复杂度（HashMap）：O（min(m,n)），与之前的方法相同
* 空间复杂度（table）：O(m),m是字符集的大小。