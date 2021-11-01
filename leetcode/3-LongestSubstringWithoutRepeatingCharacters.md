# [\#3 Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

給一個字串 s，求最長的子字串長度，且該子字串中的字元不重複。

## 思考
因為是子字串而非子序列，所以子字串為連續的，則一旦有重複的字元出現就會是分割點。

**用個 sliding window 找最大的連續子字串:**
- 如果接下來的字元不在 window 的範圍內，則代表可以延長目前的子字串(window)，更新目前 window 的長度的最大值，並將該字元位置記錄下來
- 如果已在 window 內，則代表到了分割點，將該字元在 window 內出現的位置之下一個位置指定為新的 window 之開頭，並更 新該字元的出現位置

記錄 window 出現字元的部分使用 unordered_map，要注意所記錄的字元位置是否在目前 window 範圍內。

> 僅遍歷整個字串一次，因此時間複雜度為 O(n)，為記錄出現過的字元位置，空間複雜度為 O(n)。

## 程式碼

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<int,int> hash;
        int st=0, max_len = 0;
        for (int i=0; i<s.length(); i++) {
            auto seen = hash.find(s[i]);
            if (seen == hash.end()) {
                hash.insert(pair<int,int>(s[i],i));
                max_len = max(max_len, i-st+1);
            } else if (seen->second < st) {
                seen->second = i;
                max_len = max(max_len, i-st+1);
            } else {
                st = seen->second + 1;
                seen->second = i;
            }
        }
        return max_len;
    }
};
```