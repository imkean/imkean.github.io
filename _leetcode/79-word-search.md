---
layout: leetcode
date: 2016-06-28
title: Word Search
tags: [Array, Backtracking]
---

* Contents
{:toc #toc_of_keans_blog}

## Question

> Question Description
>
>     examples
>     

***

## Solution

**Result:** Accepted **Time:** 8 ms

Here should be some explanations.

```c

int dx[4]={0,0,1,-1};
int dy[4]={1,-1,0,0};
int ptr = 0;
bool dfs(int x,int y,char **board,const int row,const int col, const char * word)
{
    if(!word[ptr]) return true;
    if(x < 0 || y < 0 || x >= row || y >= col || board[x][y]!=word[ptr])
        return false;
    ptr++;
    board[x][y] |= 128;
    bool ret = false;
    for(int i = 0; i < 4; i++)
        if(ret = dfs(x+dx[i],y+dy[i],board,row,col,word))
            break;
    board[x][y] ^= 128;
    ptr--;
    return ret;
}

bool exist(char** board, int row, int col, char* word) {
    for(int i = 0; i < row; i++)
        for(int j = 0; j < col; j++)
            if(board[i][j] == word[0] && dfs(i,j,board,row,col,word))
                    return true;
    return false;
}
```

**Complexity Analytics**

- Time Complexity: $$O(n^2*k)$$
- Space Complexity: $$O(1)$$
