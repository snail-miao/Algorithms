# 代码随想录算法训练营
##Day 07 ｜454.四数相加,383 赎金信. 两个数组的交集,15. 三数之和, 18. 四数之和

### 454. 四数相加
给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：

0 <= i, j, k, l < n
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

#### 思路：
1. 在 四个数组中各取一个数，使其和为0，可以用 map 表来 存储器中某两个数组的所有和值的可能及个数。
2. 暴力解法时，这道题的时间复杂读是 O(n^4), 用map 来将复杂读降低到 O(n^2).
``` go
func fourSumCount(nums1 []int, nums2 []int, nums3 []int, nums4 []int) int {
	nums1Map := make(map[int]int, len(nums1))
	for _, num1 := range nums1 {
		for _, num2 := range nums2 {
			nums1Map[num1+num2]++
		}

	}

	res := 0
	for _, num3 := range nums3 {
		for _, num4 := range nums4 {
			if v, ok := nums1Map[0-num3-num4]; ok {
				res += v
			}
		}
	}

	return res
}
```

### 383. 赎金信
给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。

如果可以，返回 true ；否则返回 false 。

magazine 中的每个字符只能在 ransomNote 中使用一次。

#### 思路：
1. 此题很简单，将 magazine 存入map 中，然后遍历 ransomNote，是否每个元素都存在

``` go
func canConstruct(ransomNote string, magazine string) bool {
    magazineMap := make(map[byte]int)
    for i:=0;i<len(magazine);i++{
        magazineMap[magazine[i]]++
    }

    for i:=0;i<len(ransomNote);i++{
        num, ok := magazineMap[ransomNote[i]]
       if  !ok||num <= 0 {
            return false
        }
        magazineMap[ransomNote[i]] -- 
    }
    return true
}
```


### 15. 三数之和
给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请

你返回所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

#### 思路：
1. 同一个数组中找出四个元素使其和为 target，要求返回的是数组值，不是 index，这里可以考虑用排序+双指针。如果要求返回 index，那么不可以用双指针
2. 要求结果不可以重复，那么需要考虑去重，a 和 b， b 和 c 去重

```
func threeSum(nums []int) [][]int {
	sort.Ints(nums)
    i := 0
    var res [][]int
    for i < len(nums)-2 {
        left := i+1
        right := len(nums)-1
        for left < right && i < left {
            if nums[i]+nums[left]+nums[right] == 0{
                res = append(res,[]int{nums[i],nums[left],nums[right]})
                for left < right && nums[left] == nums[left+1]{
                    left ++
                }
                left ++
                continue
            }

            if nums[i]+nums[left]+nums[right] > 0{
                right --
                continue
            }

            if nums[i]+nums[left]+nums[right] < 0{
                left ++
                continue
            }
        }
        for i < len(nums)-2 && nums[i] == nums[i+1] {
                    i++
        }
        i++
    }
    return res
}
```

### 18. 四数之和
给你一个由 n 个整数组成的数组 nums ，和一个目标值 target 。请你找出并返回满足下述全部条件且不重复的四元组 [nums[a], nums[b], nums[c], nums[d]] （若两个四元组元素一一对应，则认为两个四元组重复）：

0 <= a, b, c, d < n
a、b、c 和 d 互不相同
nums[a] + nums[b] + nums[c] + nums[d] == target
你可以按 任意顺序 返回答案 。

#### 思路：
1. 同一个数组中找出四个元素使其和为 target，要求返回的是数组值，不是 index，这里可以考虑用排序+双指针。如果要求返回 index，那么不可以用双指针
2. 要求结果不可以重复，那么需要考虑去重，a 和 b， b 和 c，c 和 d 去重

```go
func fourSum(nums []int, target int) [][]int {
  // 排序数组
    sort.Ints(nums)
    var res [][]int
  // 先确定 a 的值 
    for i:=0;i<len(nums)-3;i++{
        newTarget := target - nums[i]
  // 再确定 b 的值
        for j := i+1;j<len(nums)-2;j++{
            left := j+1
            right := len(nums)-1
            // 确定c 和 d 的值，并进行 c 和 d 去重
            for left <right && j<left {
                if nums[left]+nums[right]+nums[j] == newTarget{
                    res = append(res,[]int{nums[i],nums[j],nums[left],nums[right]})
                    // 去重 c
                    for left <right && nums[left] == nums[left+1]{
                        left ++
                    }
                    left ++
                }else if nums[left]+nums[right]+nums[j] > newTarget{
                    right --
                }else{
                    left ++
                }
            }
            // 去重 b
            for j <len(nums)-2 && nums[j] == nums[j+1]{
                j++
            }
        }
        去重 a
        for i < len(nums)-2 && nums[i] == nums[i+1]{
            i++
        }
    }
return res
}
```
