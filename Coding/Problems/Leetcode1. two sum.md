1. ~~vector.length()~~  vector. **size()** 
2. 原来我的版本是
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) { // j should start from i+1
                if (nums[i] + nums[j] == target) {
                    return {i, j}; // Return the indices as a vector
                }
            }
        }
        return {}; // Return an empty vector if no solution is found
    }
};

```
  但是报错![[Pasted image 20240724020258.png]]
  gpt修改为：
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) { // j should start from i+1
                if (nums[i] + nums[j] == target) {
                    return {i, j}; // Return the indices as a vector
                }
            }
        }
	    return {}; // Return an empty vector if no solution is found
    }
};

```
后通过；

【解析】这是因为在这个函数中，返回类型是"vector<int>"
所以采用"return {}"来符合返回类型条件