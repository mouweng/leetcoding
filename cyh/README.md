# cyh

### 2021-12-29
[1995. 统计特殊四元组](https://leetcode-cn.com/problems/count-special-quadruplets/)
```c++
class Solution {
public:
    int countQuadruplets(vector<int>& nums) {
        unordered_map<int,int> cnt;
        int len=nums.size();
        int ans=0;
        for(int b=len-3;b>=1;b--){
            for(int d=b+2;d<len;d++){
                cnt[nums[d]-nums[b+1]]++;
            }
            for(int a=0;a<b;a++){
                auto iter=cnt.find(nums[a]+nums[b]);
                if(iter!=cnt.end()){
                    ans+=iter->second;
                }
            }
        }
        return ans;
    }
};
```