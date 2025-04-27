> a file that records the process of Leetcode Rush with Go Lang.



记录模板

````go
## ？	2025-？-？

### （1）？——？

**个人题解**：

```go

```

> 核心思路

**官方题解**：

```go

```

> 核心思路
````



## 1	2025-04-25

### （1）49——字母异位词分组

**个人题解**：

```go
func groupAnagrams(strs []string) [][]string {
  mp := make(map[string][]string)
  for _, str := range strs{
    s := []byte(str)
    sort.Slice(s, func(i, j int)bool {
      return s[i] < s[j]
    })
    sortedS := string(s)
    mp[sortedS] = append(mp[sortedS], str)
  }
  res := make([][]string, 0, len(mp))
  for _, v := range mp{
    res = append(res, v)
  }
  return res
}
```

> 核心思路
>
> 没有解题难度，主要在于对于go语言语法的学习

**官方题解**：

```go
func groupAnagrams(strs []string) [][]string {
    mp := map[[26]int][]string{}
    for _, str := range strs {
        cnt := [26]int{}
        for _, b := range str {
            cnt[b-'a']++
        }
        mp[cnt] = append(mp[cnt], str)
    }
    ans := make([][]string, 0, len(mp))
    for _, v := range mp {
        ans = append(ans, v)
    }
    return ans
}
```

> 核心思路
>
> 一个更好的解法，mp的key不再用排序后的字符串作为key
>
> 而是用统计字符数量后的拼接字符作为key，可以减少排序的计算需求。

## 2	2025-04-27 

### （1）128——最长连续序列

**个人题解**：

```go
func longestConsecutive(nums []int) int {
	mySet := map[int]bool{}
	for _, val := range nums {
		mySet[val] = true
	}
	maxLen := 0
	for i := range mySet {
		if !mySet[i-1] {
			currLen := 1
			currNum := i
			for mySet[currNum+1] {
				currLen++
				currNum++
			}
			if currLen > maxLen {
				maxLen = currLen
			}
		}
	}
	return maxLen
}
```

> 核心思路
>
> 核心思路是找到一个最长序列的头。即从一个没有前边相邻的数开始从小到大遍历查找。
