
#  代码随想录算法训练营

## Day 2 ｜977. 有序数组的平方

### 977. 有序数组的平方
*给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。*

**思路：**
1. _非递减顺序_ 说明是有序数组，_平方_ 说明 会打破原本的顺序。
2. 可以利用归并排序的思路，先将数组分成两个 有序的子数组，然后再排序，组成一个新数组。

Code:
```
func sortedSquares(nums []int) []int {
	inArray, deArray := splitArray(nums)
	iLen := len(inArray)
	dLen := len(deArray)
	result := make([]int, 0, iLen+dLen)
	i := 0
	j := dLen - 1
	for i < iLen && j >= 0 {
		if inArray[i]*inArray[i] < deArray[j]*deArray[j] {
			result = append(result, inArray[i]*inArray[i])
			i++
		} else {
			result = append(result, deArray[j]*deArray[j])
			j--
		}
	}
	
	for i < iLen {
		result = append(result, inArray[i]*inArray[i])
		i++
	}
	
	for j >= 0 {
		result = append(result, deArray[j]*deArray[j])
		j--
	}
	return result
}

func splitArray(nums []int) ([]int, []int) {
	if len(nums) == 0 {
		return nil, nil
	}

  // 全正
	if nums[0] >= 0 {
		return nums, nil
	}

  // 全负
	if nums[len(nums)-1] < 0 {
		return nil, nums
	}

  // 有正有负
	for i := 0; i < len(nums)-1; i++ {
		if nums[i] < 0 && nums[i+1] >= 0 {
			return nums[i+1:],nums[:i+1]
		}
	}
	return nil, nil
}
```

