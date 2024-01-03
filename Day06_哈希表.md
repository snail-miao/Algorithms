# 代码随想录算法训练营
## Day 06 ｜242.有效的字母异位词,349. 两个数组的交集,202. 快乐数, 1. 两数之和   
### 242.有效的字母异位词
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。
#### 思路：
1. 用map记录其中一个字符串

``` go
func isAnagram(s string, t string) bool {
	anagramMap := make(map[byte]int, len(s))
	for i := 0; i < len(s); i++ {
		anagramMap[s[i]]++
	}
	for i := 0; i < len(t); i++ {
		if _, ok := anagramMap[t[i]]; !ok {
			return false
		} else {
			anagramMap[t[i]]--
			if anagramMap[t[i]] == 0 {
				delete(anagramMap, t[i])
			}
		}
	}
	
	if len(anagramMap) > 0 {
		return false
	}
	
	return true
}

```

### 349. 两个数组的交集

给定两个数组 nums1 和 nums2 ，返回 它们的交集 。输出结果中的每个元素一定是 唯一 的。我们可以 不考虑输出结果的顺序 。

#### 思路：
1. 同样用map
``` go
func intersection(nums1 []int, nums2 []int) []int {
	nums1Map := make(map[int]int, len(nums1))
	for _, num := range nums1 {
		nums1Map[num]++
	}

	var res []int
	for _, num := range nums2 {
		if _, ok := nums1Map[num]; ok {
			res = append(res, num)
			delete(nums1Map, num)
		}
	}
	return res
}

```

### 202. 快乐数

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」 定义为：

对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
如果这个过程 结果为 1，那么这个数就是快乐数。
如果 n 是 快乐数 就返回 true ；不是，则返回 false 。

#### 思路：
1. 难点在于 循环的终止条件是什么？
2. 快乐数意味着 无限循环，无限循环意味着每一次的结果都是不一样的，那么循环终止条件是，是否有重复的结果，如果有，那么就不是，如果没有就一直找到1 为止。

```go
func isHappy(n int) bool {
	m := make(map[int]int)
	for n!=1{
		if _, ok := m[n]; ok {
			return false
		}
		m[n]++
		n = getSum(n)
	}
	return true
}

func getSum(n int) int {
	sum := 0
	for n > 0 {
		sum += (n % 10) * (n % 10)
		n = n / 10
	}
	return sum
}
```

###  1. 两数之和

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

#### 思路：
1. 用map 保存数组的值
``` go
func twoSum(nums []int, target int) []int {
numsMap := make(map[int]int,len(nums))
for i,num := range nums{
	numsMap[num] = i
}

for i,num := range nums{
	if v,ok := numsMap[target-num];ok && i!= v{
	return []int{i,v}
	}
}
return nil
}
```
