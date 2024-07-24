# LeetCode Problem: [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/) 

## Problem Description
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

## Solutions

### Solution-1: Sorting by Frequency
#### Short Description
This solution uses a dictionary to count the frequency of each element in the array. It then sorts the dictionary in descending order based on the frequency of the elements. Finally, it returns the top `k` frequent elements by slicing the sorted list of keys.

#### Time Complexity
The time complexity of this solution is $O(n \log n)$, where `n` is the number of elements in the array. This is because sorting the dictionary entries by frequency takes $O(n \log n)$ time.

#### Space Complexity
The space complexity is $O(n)$, as the dictionary stores frequency counts for each unique element in the array.

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        most_frequent = {}

        for n in nums:
            if n in most_frequent:
                most_frequent[n] += 1
            else:
                most_frequent[n] = 1
        
        sorted_dic = dict(sorted(most_frequent.items(), key=lambda item: item[1], reverse=True))
        
        return list(sorted_dic.keys())[:k]
```

### Solution-2: Bucket Sort
#### Short Description
This solution uses a dictionary to count the frequency of each element and a list of lists called `freq`. The list `freq` is initialized with empty sublists, one for each possible frequency (up to the length of `nums`). Each element is added to the sublist corresponding to its frequency. The result is then built by collecting elements from the sublists, starting from the highest frequency.

#### Time Complexity
The time complexity of this solution is $O(n)$, where `n` is the number of elements in the array. This is because both the frequency counting and the bucket sort operations are linear in time complexity.

#### Space Complexity
The space complexity is $O(n)$, due to the space required for the dictionary and the bucket list.

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = {}
        freq = [[] for i in range(len(nums) + 1)]

        for n in nums:
            count[n] = 1 + count.get(n, 0)
        for n, c in count.items():
            freq[c].append(n)
        
        res = []
        for i in range(len(freq) - 1, 0, -1):
            for n in freq[i]:
                res.append(n)
                if len(res) == k:
                    return res
```

## Conclusion
- The first solution leverages sorting to find the top `k` frequent elements, making it straightforward but less efficient for large input sizes due to its $O(n \log n)$ time complexity.
- The second solution uses bucket sort, achieving linear time complexity by utilizing the frequency count and directly mapping frequencies to indices. This makes it more efficient for larger inputs.
- Depending on the constraints and requirements, the bucket sort solution is generally preferable for its linear time complexity.
