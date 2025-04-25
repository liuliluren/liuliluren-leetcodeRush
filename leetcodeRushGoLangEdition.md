> a file that records the process of Leetcode Rush with Go Lang.



记录模板

````
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

### （1）40——字母异位词分组

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

