
#  代码随想录算法训练营

## Day 2 ｜977. 有序数组的平方，209.长度最小的子数组，59. 螺旋矩阵 II

### 977. 有序数组的平方
*给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。*

**思路：**
1. **_非递减顺序_** 说明是有序数组，_平方_ 说明 会打破原本的顺序。
2. 可以利用归并排序的思路，先将数组分成两个 有序的子数组，然后再排序，组成一个新数组。

Code:
``` go
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
### 209. 长度最小的子数组
_给定一个含有 n 个正整数的数组和一个正整数 target 。
找出该数组中满足其总和大于等于 target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。_

#### **思路：**
1. **_正整数的数组_** -> 由i，j组成的子数组，j增大，和增大；i增大，和减小
2. 采用双指针法，来滑动子数组区间，判断区间和是否大于等于target，并记录最小区间和
3. 采用前缀和数组来快速计算每个连续区间的和。

``` go
func minSubArrayLen(target int, nums []int) int {
// 构建前缀和数组
sumArray:= []int{}
    sum := 0
    sumArray = append(sumArray,0)
    for _,num := range nums{
        sum += num
        sumArray =append(sumArray, sum)
    }

// 滑动子数组区间
    i,j := 0,0
    minLen := math.MaxInt
    for j< len(sumArray){
        if sumArray[j]-sumArray[i] >= target {
            if j-i < minLen{
                minLen = j-i
            }
            i++
        }else{
            j++
        }
    }

    if minLen == math.MaxInt{
        return 0
    }
    return minLen
}
```

### 59. 螺旋矩阵 II
_给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。_
![WechatIMG12642](https://github.com/snail-miao/Algorithms/assets/43983739/70902ea0-a651-4070-86a5-13e164333e2d)

**思路：**
1. 通过例子找规律，顺着旋转方向每一行每一列的公式写出来。

``` go
func generateMatrix(n int) [][]int {
    row_start := 0
    row_end:= n-1
    col_start:= 0
    col_end := n-1
    result := make([][]int,n)
    for i:=0;i<n;i++{
        result[i] = make([]int,n)
    }

    k:= 1
    for row_start<=row_end || col_start <=col_end{
        for j:=col_start;j<= col_end;j++{
            result[row_start][j] = k
            k ++
        }
        row_start ++

        for i:= row_start;i<=row_end;i++{
            result[i][col_end]= k
            k++
        }

        col_end--

        for j := col_end;j >= col_start;j--{
            result[row_end][j]= k
            k++
        }
        row_end --

        for i := row_end;i>=row_start;i--{
            result[i][col_start] = k
            k++
        }
        col_start ++
    }

     return result
}
```
