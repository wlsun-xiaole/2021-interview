# [\#1 Two Sum](https://leetcode.com/problems/two-sum/)
給一個整數陣列 nums 和一整數之目標數 target，回傳一個包含兩個索引值的陣列，使兩索引值所對應之數字和等於 target。
可以假設每組輸入只會有一個解，且索引值的順序不限。

## 思考
1. 該陣列是否排序過?
  - 是，則直覺上會有 O(nlogn) 解法 (binary search)
  - 否，只剩暴力解 O(n^2)?
  
``` 因為一個數只會有一個對應的解，在遍歷一次的過程中把看過的數字和其索引值存在 hash table 中，當遇到符合看過之數字解的數字後，即可從 hash table 中用 O(1) 的時間取得該數之索引值。
```

> 整體而言只需遍歷一次，因此實驗複雜度為O(n)；另外需有一 hash table 儲存遍歷過的數，空間複雜度為 O(n)。


## 程式碼

此處使用 unordered_map 來做 hash table，需要注意的是 unordered_map 的使用上要搭配 pair ，以及 find() 會回傳的是 iterator。

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        vector<int> ans;
        for (int i=0; i<nums.size(); i++) {
            auto seen = hash.find(target-nums[i]);
            if (seen == hash.end()) {
                hash.insert(pair<int, int>(nums[i],i));
            } else {
                ans.push_back(seen->second);
                ans.push_back(i);
                break;
            }
        }
        return ans;
    }
};
```
  