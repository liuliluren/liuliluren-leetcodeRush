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

## 3	2025-04-28

### （1）283——移动零

**个人题解**：

```go
func moveZeroes(nums []int) {
	zeroIndex := 0
	swapIndex := 0
	for swapIndex < len(nums) {
		for zeroIndex < len(nums) && nums[zeroIndex] != 0 {
			zeroIndex++
		}
		if zeroIndex+1 > swapIndex {
			swapIndex = zeroIndex + 1
		}
		for swapIndex < len(nums) && nums[swapIndex] == 0 {
			swapIndex++
		}
		if swapIndex < len(nums) {
			nums[zeroIndex], nums[swapIndex] = nums[swapIndex], nums[zeroIndex]
		}
	}
}
```

> 其实还能再优化一下：
>
> ```go
> func moveZeroes(nums []int) {
> 	zeroIndex := 0
> 	for swapIndex := 0; swapIndex < len(nums); swapIndex++ {
> 		if nums[swapIndex] != 0 {
> 			nums[swapIndex], nums[zeroIndex] = nums[zeroIndex], nums[swapIndex]
> 			zeroIndex++
> 		}
> 	}
> }
> ```
>
> tips:
>
> 这里原本希望再优化一下这里的代码;
>
> ```go
> nums[swapIndex], nums[zeroIndex] = nums[zeroIndex], nums[swapIndex]
> zeroIndex++
> ```
>
> 修改成：
>
> ```go
> nums[swapIndex], nums[zeroIndex++] = nums[zeroIndex], nums[swapIndex]
> ```
>
> 但是这实际上是不允许的，需要单独的在外边对内部的值进行操作。
>
> 但这并不意味着go中不支持类似于`nums[zeroIndex++] = i`的这种效果，只是不能在这种场景下使用而已。
>
> ![image-20250429200340034](https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20250429200340034.png)

### （2）11——盛最多水的容器

```go
func maxArea(height []int) int {
	left := 0
	right := len(height) - 1
	maxCapacity := calcMin(height[left], height[right]) * right
	for right > left {
		if height[left] < height[right] {
			left++
		} else {
			right--
		}
		maxCapacity = calcMax(maxCapacity, calcMin(height[left], height[right])*(right-left))
	}
	return maxCapacity
}
func calcMax(a int, b int) int {
	if a > b {
		return a
	}
	return b
}
func calcMin(a int, b int) int {
	if a < b {
		return a
	}
	return b
}

```

**回顾**：

> 经典题目，用go写后无论是时间还是内存都小了很多

<img src="https://gitee.com/hanhandehanpi/cloudimage/raw/master/image-20250429194552026.png" alt="image-20250429194552026" style="zoom:33%;" />

## 4	2025-04-30

### （1）15——三数之和

**个人题解**：

```go
func threeSum(nums []int) [][]int {
	length := len(nums)
	res := [][]int{}
	sort.Slice(nums, func(i, j int) bool {
		return nums[i] < nums[j]
	})
	for i, val := range nums {
		if i > 0 && val == nums[i-1] {
			continue
		}
		target := -val
		left := i + 1
		right := length - 1
		for left < right {
			if left > i+1 && nums[left] == nums[left-1] {
				left++
				continue
			}
			currSum := nums[left] + nums[right]
			if currSum == target {
				res = append(res, []int{val, nums[left], nums[right]})
				left++
				right--
				continue
			} else if currSum > target {
				right--
			} else {
				left++
			}
		}
	}
	return res
}
```

> 核心思路
>
> 已经掌握过，再用go完成一遍

**官方题解**：

> 官方题解不够清晰

### （2）42——接雨水

**个人题解**：

```go
func trap(height []int) int {
	res := 0
	length := len(height)
	left := 0
	for left < length-1 {
		leftHeight := height[left]
		maxRightHeight := 0
		maxRightIndex := left + 1
		for right := left + 1; right < length; right++ {
			if height[right] > maxRightHeight {
				maxRightHeight = height[right]
				maxRightIndex = right
			}
		}
		res += calcCapacity(left, maxRightIndex, height)
		// fmt.Println("left:", left, "maxRightIndex", maxRightIndex)
		left = maxRightIndex
	}
	// fmt.Println("res:", res)
	return res
}

func calcCapacity(left int, right int, height []int) int {
	return (right-left-1)*min(height[left], height[right]) - sum(height[left+1:right])
}

func sum(nums []int) int {
	res := 0
	for _, num := range nums {
		res += num
	}
	return res
}

```

> 忘记了原本的最优解法。
>
> 这里采用的解法是从每个最左侧的位置出发，找到下一个该用来作为计算的右侧。
>
> 然后递归的处理。
>
> 但是发现时间开销有点大。

### （3）438——找到字符串中的所有字母异位词

**个人题解**：

```go
func findAnagrams(s string, p string) []int {
	pLetterCount := [26]int{}
	currLetterCount := [26]int{}
	sLength := len(s)
	pLength := len(p)
	if sLength < pLength {
		return []int{}
	}
	res := []int{}
	for _, char := range p {
		pLetterCount[char-'a']++
	}
	for i := 0; i <= pLength-1; i++ {
		currLetterCount[s[i]-'a']++
	}
	i := pLength
	for i <= sLength {
		if currLetterCount == pLetterCount {
			res = append(res, i-pLength)
		}
		// remove oldest
		currLetterCount[s[i-pLength]-'a']--
		// add newest
		if i == sLength {
			break
		}
		currLetterCount[s[i]-'a']++
		i++
	}
	fmt.Println(res)
	return res
}
```

> 没有难度

## 5	2025-05-01

### （1）560——和为K的子数组

**个人题解**：

```go
func subarraySum(nums []int, k int) int {
	mp := map[int]int{}
	pre := 0
	mp[0] = 1
	res := 0
	for _, val := range nums {
		pre += val
		target := pre - k
		res += mp[target]
		mp[pre] += 1
	}
	fmt.Println(res)
	return res
}

```

> 核心思路
>
> 关键在于保存一个前缀和的map记录，以及一个记录到当前检查位置的所有的和pre
>
> map记录从0~i为止的所有的和出现的次数。
>
> 然后对于当前位置j来说，如果要找目标值k，也就是找当前位置所有的和pre - map
>
> 中存在的差值。

### （2）239——滑动窗口最大值

**个人题解**：

```go
func maxSlidingWindow(nums []int, k int) []int {
	bucket := [20002]int{}
	offset := 10001
	currMax := -10001
	res := []int{}
	if k > len(nums) {
		k = len(nums)
	}
	for i := 0; i < k; i++ {
		currMax = max(currMax, nums[i])
		bucket[nums[i]+offset]++
	}
	res = append(res, currMax)
	for i := k; i < len(nums); i++ {
		if nums[i] >= currMax {
			currMax = nums[i]
			bucket[nums[i-k]+offset]--
			bucket[nums[i]+offset]++
		} else if nums[i-k] < currMax ||
			(nums[i-k] == currMax && bucket[currMax+offset] > 1) {
			bucket[nums[i-k]+offset]--
			bucket[nums[i]+offset]++
		} else {
			bucket[nums[i-k]+offset]--
			bucket[nums[i]+offset]++
			// 找新的最大值
			for bucket[currMax+offset] == 0 {
				currMax--
			}
		}
		res = append(res, currMax)
	}
	fmt.Println(res)
	return res
}
```

> 核心思路：
>
> 使用bucket来检测最大值。
>
> 根据每次移除的数与新增加的数与当前记录的最大值的比较，去决定计算方式

**官方题解**：

```go

```

