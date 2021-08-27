# Leetcode - 1104. Path In Zigzag Labelled Binary Tree
[Leetcode problem](https://leetcode.com/problems/path-in-zigzag-labelled-binary-tree/) - [My Leetcode soln](https://leetcode.com/problems/path-in-zigzag-labelled-binary-tree/discuss/1428745/Java-O(n)-simple-recursive-solution-with-explanation)

**Idea / Approach**

So I would start to approach the problem from the information we have.

1. We know the id of the node whose path is to be found.
1. We can find the height of the level in which it is there as h
1. The number of nodes until the previous height is **2^(h-1)-1**.
1. So the position of the current node in the level alone would be **label - (num nodes until prev level)**
1. So if the node is in xth position in its level, the parent would be in **(x-1)/2** th position from the end in its level . Or we can say **(x-1)/2 parents will be there before the desired parent in that level**.
1. The end in its level will have id of? Thats right from point 3 we get the max id in that level and we have to reduce the number of parents that come before the current node as **(2^(h-1)-1-(x-1)/2)**.
Hope you find it useful!! :)
**Recursive**
```java
class Solution {
    public List<Integer> pathInZigZagTree(int label) {
        int h = 1+(int)(Math.log(label)/Math.log(2)+1e-10);
        int[] pow = new int[h+1]; // (not too important) storing the required powers so that it doesn't have to be recalculated for each level
        pow[0]=1;
        for(int i=1;i<=h;i++)
            pow[i]=pow[i-1]*2;
        ArrayList<Integer> res =new ArrayList<>();
        rec(h, label, res, pow);
        Collections.reverse(res);
        return res;
    }
    void rec(int h, int num, ArrayList<Integer> path, int[] pow){
        if(h==0) return;
        path.add(num);
        int count = num-(pow[h-1]-1);
        int next = pow[h-1]-1 - (count-1)/2;
        rec(h-1, next, path, pow);
    }
}
```
I later saw that it can be converted to an iterative solution which just keeps reducing height and runs the quickest !!
**Iterative**

```java
class Solution {
    public List<Integer> pathInZigZagTree(int label) {
        int h = 1+(int)(Math.log(label)/Math.log(2)+1e-10);
        int[] pow = new int[h+1]; 
        pow[0]=1;
        for(int i=1;i<=h;i++)
            pow[i]=pow[i-1]*2;
        ArrayList<Integer> res =new ArrayList<>(h);
        int count, next;
        while(--h>=0){ // since we need only the previous  level height and not the current height we can reduce and use the same for finding the number of elements in the previous level and the parent label
            res.add(label);
            count = label-(pow[h]-1);
            label = pow[h]-1 - (count-1)/2;
        }
        Collections.reverse(res);
        return res;
    }
}
```