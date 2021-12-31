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

### 2021-12-30

#### [846. 一手顺子](https://leetcode-cn.com/problems/hand-of-straights/)

```cpp
class Solution {
public:
    bool isNStraightHand(vector<int>& hand, int groupSize) {
        if(hand.size()%groupSize!=0){
            return false;
        }
        sort(hand.begin(),hand.end());
        unordered_map<int,int> cnt;
        vector<int> nums;
        for(int i=0;i<hand.size();i++){
            if(i==0||hand[i]!=hand[i-1]){
                nums.push_back(hand[i]);
            }
            cnt[hand[i]]++;
        }
        for(int num:nums){
            int require_num=cnt[num];
            if(require_num==0){
                continue;
            }
            for(int i=1;i<groupSize;i++){
                if(cnt.count(num+i)==0||cnt[num+i]<require_num){
                    return false;
                }
                cnt[num+i]-=require_num;
            }
        }
        return true;
    }
};
```

