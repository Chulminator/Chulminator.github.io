---
title: NeetCode - Combination Target Sum
author: chulmin
date: 2024-10-17 00:00:00 -0500
categories: [Daily Smashing Through Random Problems, Neetcode]
tags: [coding test, c++, neetcode, Backtracking, Combination Target Sum]
math: true
---

## Idea
### Before solving the problem
In tackling the Combination Target Sum problem, I realized that the target sum can be constructed from smaller sums. This insight led me to consider a dynamic programming approach to store combinations that are less than the target. Initially, I opted for a set data structure to avoid including the same combination in different orders, ensuring that each unique combination is counted only once.

### While solving the problem
During the implementation, I encountered a challenge when trying to convert from set<vector<int>> to vector<vector<int>>. This conversion was necessary to return the final result in the required format.

### After solving the problem
Upon completing the solution, I recognized that this problem is fundamentally about backtracking.


## Code

``` c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& nums, int target) {
        vector<set<vector<int>>> vec(target+1);
        for( int num : nums){
            vec[num].insert({num});
        }

        for ( int sum = 2; sum <= target; ++sum){
            for (int jj = 1; jj <= sum/2; ++jj ){
                int complement = sum - jj;
                if( !vec[jj].empty() && !vec[complement].empty() ){
                    for( const auto& partSum1 :vec[jj]){
                        for( const auto& partSum2 :vec[complement]){
                            vector<int> tmp = partSum1;
                            tmp.insert(tmp.end(), partSum2.begin(), partSum2.end());
                            sort( tmp.begin(), tmp.end() );
                            vec[sum].insert( tmp );
                        }
                    }
                    

                }
            }

        }
        std::vector<std::vector<int>> result(vec[target].begin(), vec[target].end());
        return result;
    }
};

```


## Reference
- [Combination Target Sum](https://neetcode.io/problems/combination-target-sum)