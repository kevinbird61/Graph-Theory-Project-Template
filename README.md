# E24046307 Proect report
## 演算法思路
1. 讀取圖片後,轉成二維的點點關係圖
2. 利用該點點關係圖,取得各點的indegree與outdegree
3. 若各點的indegree = outdegree則此問題為一筆劃問題
4. 若indegree ！= outdegree 則在degree不相等的兩點間增加edge 使各點的indegree = outdegree
5. 完成一筆劃問題

## 程式碼解釋
```
#include <algorithm>
#include <iostream>
#include <unistd.h>
#include <map>
#include "network_manager.h"
#include "gplot.h"
#include "path.h"
#include "fstream"

using namespace std;
```
此段程式碼宣告所需的標頭檔

```
vector<vector<int> > shortest_path(int start, int end, int v_num, vector<vector<int> >& e_arr){
    //variable declaration
    vector <vector<int> > e_arr_bonus;
    vector <int> path_buffer;
    vector <int> previous_node;
    int node_now;
    int node_pre_index;
    int node_pre;
    int fin_flag = 0;
    int counter = 0;
```

此function的目的在於找兩點間的最短路徑 並回傳哪些路徑需要多走一次  
e_arr_bonus 用於紀錄兩點間的所經路徑,之後用於加入edge matrix中  
path_buffer 用於紀錄兩點間的所經節點  
previous_node 用於紀錄前一個節點在path_buffer中的位置  
node_now 用於紀錄現在的節點位置  
node_pre_index 用於紀錄前一個節點在path_buffer中的位置  
node_pre 用於紀錄前一個節點是誰  
fin_flag 用於紀錄while loop是否完成目標工作  
counter 用於紀錄目前BFS演算法走到  
