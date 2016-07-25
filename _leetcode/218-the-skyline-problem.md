---
layout: leetcode
date: 2016-07-25
title: The Sky Line Problem
tags: [Segment Tree, Heap, Binary Indexed Tree, Divide and Conquer]
---

* Contents
{:toc #toc_of_keans_blog}

## Question

A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are **given the locations and height of all the buildings** as shown on a cityscape photo (Figure A), write a program to **output the skyline** formed by these buildings collectively (Figure B).
![Skyline Figure A](/images/leetcode/218-skyline-1.jpg)
![Skyline Figure B](/images/leetcode/218-skyline-2.jpg)
The geometric information of each building is represented by a triplet of integers ``[Li, Ri, Hi]``, where Li and Ri are the x coordinates of the left and right edge of the i<sup>th</sup> building, respectively, and `Hi` is its height. It is guaranteed that `0 ≤ Li`, `Ri ≤ INT_MAX`, `0 < Hi ≤ INT_MAX`, and `Ri - Li > 0`. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height `0`.

For instance, the dimensions of all buildings in Figure A are recorded as: ``[ [2 9 10]``, ``[3 7 15]``, ``[5 12 12]``, ``[15 20 10]``, ``[19 24 8] ]`` .

The output is a list of "key points" (red dots in Figure B) in the format of ``[ [x1,y1], [x2, y2], [x3, y3], ... ]`` that uniquely defines a skyline. A key point is the left endpoint of a horizontal line segment. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.

For instance, the skyline in Figure B should be represented as:``[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ]``.

**Notes:**

-  The number of buildings in any input list is guaranteed to be in the range ``[0, 10000]``.
-  The input list is already sorted in ascending order by the left `x` position `Li`.
- The output list must be sorted by the `x` position.
- There must be no consecutive horizontal lines of equal height in the output skyline. For instance, ``[...[2 3], [4 5], [7 5], [11 5], [12 7]...]`` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: ``[...[2 3], [4 5], [12 7], ...]``

***

## Solution

**Result:** Accepted **Time:**  60 ms

Here should be some explanations.

```cpp
#define MAXN (10240+5)
#define lson(rt)  (rt << 1)
#define rson(rt)  ((rt << 1) | 1)
#define ls  lson(rt)
#define rs  rson(rt)
#define middle   (left + (right - left)/2)
#define DefaultMinValue  0x7ffffff
#define DefaultMaxValue  0x0000000
#define NotAddValue  0xffffffff    

struct  SegTreeNode
{
    int maxValue, minValue, setValue;
    SegTreeNode():maxValue(DefaultMaxValue),minValue(DefaultMinValue),setValue(NotAddValue){}
};


int data[MAXN]={0}, nums[MAXN*3];

struct SegTree{
    SegTreeNode * segTree;
    SegTree(int n){
        segTree = new SegTreeNode[n<<4];
    }
    ~SegTree(){ delete [] segTree;}

    void buildTree(int rt,int left, int right)/// [left,right]
    {
        int mid = middle;
        if(left == right)
        {
            segTree[rt].minValue =  segTree[rt].maxValue = data[left];
            return;
        }
        buildTree(lson(rt),left,mid);
        buildTree(rson(rt),mid+1,right);
        segTree[rt].maxValue = max(segTree[rson(rt)].maxValue,segTree[lson(rt)].maxValue);
        segTree[rt].minValue = min(segTree[rson(rt)].minValue,segTree[lson(rt)].minValue);
    }
    SegTreeNode query(int rt, int left ,int right, const int ql,const int qr)//// min([ql,qr])
    {
        SegTreeNode tmp;
        if(segTree[rt].setValue != NotAddValue)
            return segTree[rt];
        if(ql <= left && right <= qr)
            return segTree[rt];
        int mid = middle;
        SegTreeNode ans;
        if(ql <= mid)
            tmp = query(lson(rt), left, mid,ql, qr);
        if(mid < qr)
            tmp = query(rson(rt), mid+1, right, ql, qr);
        ans.minValue = min(ans.minValue,tmp.minValue);
        ans.maxValue = max(ans.maxValue,tmp.maxValue);
        return ans;
    }
    void setCurNode(int rt,int left,int right,int value)
    {
        segTree[rt].setValue = value;
        segTree[rt].minValue = value;
        segTree[rt].maxValue = value;
    }
    void pushDown(int rt,int left,int right,int value)
    {
        if(left != right && value != NotAddValue)
        {
           setCurNode(ls,left,middle,value);
           setCurNode(rs,middle + 1, right,value);
           segTree[rt].setValue = NotAddValue;
        }
    }
    void setMaintain(int rt,int left,int right)
    {
        if( right > left ) /// left != right
        {
            segTree[rt].minValue = min(segTree[lson(rt)].minValue,segTree[rson(rt)].minValue);
            segTree[rt].maxValue = max(segTree[lson(rt)].maxValue,segTree[rson(rt)].maxValue);
        }
    }
    void update(int rt,int left, int right,const int cl,const int cr,const int addv)/// set value
    {
        pushDown(rt,left,right,segTree[rt].setValue);
        if(addv < segTree[rt].minValue) return ;
        int mid = middle;
        if(cl <= left && right <= cr && addv > segTree[rt].maxValue)
            return setCurNode(rt,left,right,addv);
        else
        {
            if(cl <= mid)
                update(ls,left,mid,cl,cr,addv);
            if(cr > mid)
                update(rs,mid+1,right,cl,cr,addv);
        }
        setMaintain(rt,left,right);
    }
};

class Solution {
public:
    vector<pair<int, int>> getSkyline(vector<vector<int>>& buildings) {
    if(buildings.size() < 1)
        return vector<pair<int, int>>();
    int cnt = 0;
    for (const auto & vec:buildings)
    {
        nums[cnt++] = vec[0];
        nums[cnt++] = vec[1];
        nums[cnt++] = vec[1] - 1;
    }
    sort(nums,nums+cnt);
    int ptr = 0;
    for(int i =  1; i < cnt; i++)
        if(nums[ptr] != nums[i])
            nums[++ptr] = nums[i];
    cnt = ++ptr;
    unordered_map<int,int> mp;
    for(int i = 0; i < cnt; i++)
        mp[nums[i]] = i+1;
    SegTree segtree(cnt);
    segtree.buildTree(1,1,cnt);// set zero
    for (const auto & vec:buildings)
    {
        int left = vec[0], right = vec[1] - 1, height = vec[2];
        segtree.update(1,1,cnt,mp[left],mp[right],height);
    }
    vector<pair<int,int>> ret;
    for(int i = 1,last = -1; i <= cnt; i++)
    {
        int tmp = segtree.query(1,1,cnt,i,i).maxValue;
        if(tmp != last)
            ret.push_back(pair<int,int>(nums[i-1],tmp));
        last = tmp;
    }
    return ret;
}
};

```

**Complexity Analytics**

- Time Complexity: $$O(nlog(n))$$
- Space Complexity: $$O(n)$$
