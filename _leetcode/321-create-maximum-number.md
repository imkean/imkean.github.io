---
layout: leetcode
date: 2016-07-18
title: Create Maximum Number
tags: [Dynamic Programming, Greedy]
---

* Contents
{:toc #toc_of_keans_blog}

## Question

Given two arrays of length `m` and `n` with digits `0-9` representing two numbers. Create the maximum number of length `k <= m + n` from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the `k` digits. You should try to optimize your time and space complexity.

**Example 1:**

nums1 = ``[3, 4, 6, 5]``

nums2 = ``[9, 1, 2, 5, 8, 3]``

k = `5`

return ``[9, 8, 6, 5, 3]``

**Example 2:**

nums1 = ``[6, 7]``

nums2 = ``[6, 0, 4]``

k = `5`

return ``[6, 7, 6, 0, 4]``

**Example 3:**

nums1 = ``[3, 9]``

nums2 = ``[8, 9]``

k = `3`

return ``[9, 8, 9]``



***

## Solution

**Result:** Accepted **Time:**  44 ms

Here should be some explanations.

```c
#define  MAXN 65535
#define START 1
#define END 2
#define PTR 3

int a_index[MAXN], b_index[MAXN];
int a_idx[10][4], b_idx[10][4];
int a_max[MAXN], b_max[MAXN];
int ans[MAXN], tmp_ans[MAXN];

void init_index(int data[], int index[], int idx[10][4], int cnt)
{
    memset(idx, 0, sizeof(a_idx));
    for(int i = 0; i < cnt; i++)
        idx[data[i]][0]++;
    idx[9][END] = idx[9][START] = 0 ;
    for(int i = 8; i >= 0; i--)
        idx[i][END] = idx[i][START] = idx[i+1][START] + idx[i+1][0];
    for(int i = 0; i < cnt; i++)
        index[idx[data[i]][END]++] = i;
}

void init(int a[], int acnt, int b[], int bcnt)
{
    init_index(a,a_index,a_idx,acnt);
    init_index(b,b_index,b_idx,bcnt);
}

void get_len_max(int ans[],int data[], int index[], int idx[10][4], int cnt, int len)
{
    if(len == cnt)
    {
        for(int i = 0; i < cnt; i++)
            ans[i] = data[i];
        return;
    }
    int exist[10] = {0};
    for(int i = 0; i < 10; i++)
    {
        idx[i][PTR] = idx[i][START];
        exist[i] = idx[i][0];
    }
    for(int i = 0; len > 0; i++, len--)
        for(int j = 9; j >= 0; j--)
        {
            if( exist[j] > 0 && cnt - index[idx[j][PTR]] >= len)
            {
                ans[i] = j;
                exist[j] --;
                int mx = index[idx[j][PTR]];
                idx[j][PTR]++;
                for(int t = 0; t < 10; t++)
                    while(exist[t] > 0 && index[idx[t][PTR]] < mx)
                    {
                        idx[t][PTR] ++;
                        exist[t] --;
                    }
                break;
            }
        }
}

bool compare(int * a, int a_cnt, int *b, int b_cnt)
{
    int ptr = 0;
    while(ptr < a_cnt && ptr < b_cnt && a[ptr] == b[ptr]) ptr++;
    if(ptr == a_cnt || ptr == b_cnt)
        return ptr != a_cnt;
    return a[ptr] > b[ptr];
}

void merge_max(int ans[], int a_max[], int a_cnt, int b_max[], int b_cnt, int k)
{
    for(int i = 0, aptr = 0, bptr = 0; i < k; i++)
        if(compare(a_max+aptr,a_cnt-aptr,b_max+bptr,b_cnt-bptr))
            ans[i] = a_max[aptr++];
        else
            ans[i] = b_max[bptr++];
}

void update_ans(int* ans, int* tmp_ans, int cnt)
{
    int ptr = 0;
    while(ptr < cnt && ans[ptr] == tmp_ans[ptr] ) ptr++;
    if(ptr == cnt || ans[ptr] > tmp_ans[ptr])
        return ;
    while(ptr < cnt)
    {
        ans[ptr] = tmp_ans[ptr];
        ptr++;
    }
}

void deal(int *a, int a_cnt, int* b, int b_cnt,int k)
{
    init(a,a_cnt,b,b_cnt);
    int a_start = 0, a_end = a_cnt;
    if(k > b_cnt )
        a_start = k - b_cnt;
    if(k < a_cnt )
        a_end = k;
    memset(ans,0,sizeof(int)*k);
    for(int i = a_start; i <= a_end; i++)
    {
        get_len_max(a_max,a,a_index,a_idx,a_cnt,i);
        get_len_max(b_max,b,b_index,b_idx,b_cnt,k-i);
        merge_max(tmp_ans,a_max,i,b_max,k-i,k);
        update_ans(ans,tmp_ans,k);
    }
}

int* maxNumber(int* nums1, int nums1Size, int* nums2, int nums2Size, int k, int* returnSize) {
    deal(nums1,nums1Size,nums2,nums2Size,k);
    int * ret = malloc(sizeof(int)*(k));
    *returnSize = k;
    for(int i = 0; i < k; i++)
        ret[i] = ans[i];
    return ret;
}

/**
[6,7]
[6,0,4]
5

[3,4,8,9,3,0]
[6,1,9,1,1,2]
6
[3,3,3,2,3,7,3,8,6,0,5,0,7,8,9,2,9,6,6,9,9,7,9,7,6,1,7,2,7,5,5,1]
[5,6,4,9,6,9,2,2,7,5,4,3,0,0,1,7,1,8,1,5,2,5,7,0,4,3,8,7,3,8,5,3,8,3,4,0,2,3,8,2,7,1,2,3,8,7,6,7,1,1,3,9,0,5,2,8,2,8,7,5,0,8,0,7,2,8,5,6,5,9,5,1,5,2,6,2,4,9,9,7,6,5,7,9,2,8,8,3,5,9,5,1,8,8,4,6,6,3,8,4,6,6,1,3,4,1,6,7,0,8,0,3,3,1,8,2,2,4,5,7,3,7,7,4,3,7,3,0,7,3,0,9,7,6,0,3,0,3,1,5,1,4,5,2,7,6,2,4,2,9,5,5,9,8,4,2,3,6,1,9]
160
*/
```

**Complexity Analytics**

- Time Complexity: $$O(n^3)$$
- Space Complexity: $$O(n)$$
