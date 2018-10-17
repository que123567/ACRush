Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have *exactly* one solution, and you may not use the *same* element twice.

Example:

    Given nums = [2, 7, 11, 15], target = 9,

    Because nums[0] + nums[1] = 2 + 7 = 9,
    return [0, 1].

### Approach 1: 
- 最简单的思路是嵌套两个for循环，此时时间复杂度是O(N²),发现可以用map来优化。
### Approach 2：
- 遍历一次,填充map的同时查询map里是否有符合题设的值。

### 关键点
若只需查询可使用unordered_map替代map.
> STL中，map 对应的数据结构是红黑树。红黑树是一种近似于平衡的二叉查找树，里面的数据是有序的。在红黑树上做 查找操作的时间复杂度为 O(logN)。而 unordered_map 对应  哈希表，哈希表的特点就是查找效率高，时间复杂度为常数级别 O(1)， 而额外空间复杂度则要高出许多。所以对于需要高效率查询的情况，使用 unordered_map 容器。而如果对内存大小比较敏感或者数据存储要求有序的话，则可以用 map 容器。

### Runtime:4ms Beat99.98%

```js
class Solution {
public:
   vector<int> twoSum(vector<int> &nums, int target) {
    std::unordered_map<int, int> map;
    std::vector<int> res;
    for (int i = 0; i < nums.size(); ++i) {
        const int gap = target - nums[i];
        //find it
        if (map.find(gap) != map.end()) {
            //keep order
            if (i < map[gap]) {
                res.push_back(i);
                res.push_back(map[gap]);
            } else {
                res.push_back(map[gap]);
                res.push_back(i);
            }
        }
        map.insert(pair<int, int>{nums[i], i});
    }
    return res;
}
};
```