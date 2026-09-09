1. 嵌套字符串
2. 八皇后
```go
func combinationSum(candidates []int, target int) [][]int {
    res := [][]int{}
    path := []int{}

    var dfs func(candidates []int, index, sum int)
    dfs = func(candidates []int, index, sum int){
        if index == len(candidates) || sum > target {
            if sum == target{
                res = append(res, append([]int{}, path...))
            }
            return
        }
        dfs(candidates, index+1, sum)
        path = append(path, candidates[index])
        dfs(candidates, index, sum+candidates[index])
        path = path[:len(path)-1]
    }
    
    dfs(candidates, 0,0)
    return res
}
```