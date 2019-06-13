# E24046307 Proect report
## 圖的設定
1. 圖為有向圖
2. 每個邊的權重值皆為1
3. 起始點為最小輸入點.（ex: 當圖的點為b,c,d 則以b為起始點）

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
counter 用於紀錄目前BFS演算法走到哪  

```
//initialize e_arr_bonus;
    for(int i=0;i< v_num;i++){
     vector <int> e_arr_bonus_temp; 
     for(int j=0;j< v_num;j++)
       e_arr_bonus_temp.push_back(0);
     e_arr_bonus.push_back(e_arr_bonus_temp);
    }
```
把e_arr_bonus 的元素初始化成0  
```
//find shortest path
    node_now = start;
    path_buffer.push_back(start);
    previous_node.push_back(-1);

    while(!fin_flag){
      for(int i=0;i< v_num;i++)
        if(e_arr[node_now][i]){
          path_buffer.push_back(i);
          previous_node.push_back(counter);
        }
      counter++;
      node_now = path_buffer[counter];
      for(int i;i<path_buffer.size();i++)
        if(path_buffer[i] == end)
          fin_flag = 1;
    }    
    node_now = end;
    for(int i=0;i<path_buffer.size();i++)
      if(path_buffer[i] == end){
        node_pre_index = i;
        break;
      }
    while(node_now != start){
      node_pre_index = previous_node[node_pre_index];
      node_pre = path_buffer[node_pre_index];
      e_arr_bonus[node_pre][node_now]++;
      node_now = node_pre;
    }
    
    return e_arr_bonus;
}
```
這段程式碼利用BFS找輸入兩點間的最短路徑並將經過的路徑紀錄到e_arr_bonus並回傳之  
```
// create NetworkManager first
NetworkManager *nm = new NetworkManager();
```
宣告一個新的NetworkManager
```
int main(int argc, char** argv){
    // build basic topo
    nm->interpret(argv[1]);
    Vertex *v = nm->get_all_nodes();
```
把輸入的圖特徵檔餵給NetworkManager,藉以得到圖的各種資訊
```
//variable declaration
    ofstream solution;
    vector <string>  v_arr;
    vector <vector<int> > e_arr;
    vector <vector<int> > e_arr_bonus;
    vector <int> degree_arr;
    vector <int> depth_arr;
    vector <int> depth_buffer;
    vector <int> path_buffer;
    int edge_num = 0;
    int node_now = 0;
    int fin_flag = 0;
    int add_flag = 0;
    int next_node = 0;
    int next_node_depth = 0;
    int count_in = 0;
    int count_out = 0;
    int start = 0;
    int end = 0;
```
solution 用於打開輸出檔案
v_arr 用於紀錄各點
e_arr 用於紀錄各邊
e_arr_bonus 用於紀錄shortest_path的回傳值
degree_arr 用於紀錄各點degree
depth_arr 用於紀錄各點與起點的距離,距離較大則優先訪問（DFS演算法）
path_buffe 用於紀錄所經路徑
edge_num 用於紀錄邊的總數
node_now 用於紀錄現在的點
fin_flag 用於紀錄while loop是否完成目標工作
add_flag 用於紀錄是否需要進行shortest_path的運算
next_node 用於紀錄下一個點是誰
next_node_depth 用於紀錄下一個點的深度
count_in 用於紀錄目前的點的indegree
count_out 用於紀錄目前的點的outdegree
start 用於紀錄shortest_path起始點
end 用於紀錄shortest_path終點
```
//open file
    solution.open("solution.txt");
```
打開輸出檔solution.txt
```
//bulid edge array
    for(int i=0;i< v_arr.size();i++){
      vector <int> e_arr_temp; 
      for(int j=0;j< v_arr.size();j++)
        if(i==j) e_arr_temp.push_back(0);
        else {
          e_arr_temp.push_back(!nm->connected_d(v_arr[i],v_arr[j]));
          if(!nm->connected_d(v_arr[i],v_arr[j]))
            edge_num++;
        }
      e_arr.push_back(e_arr_temp);
    }
```
利用nm->connected_d判斷兩點間是否有邊,並建立邊的關係圖
```
//build degree array
    for(int i=0;i< v_arr.size();i++){
      count_out = 0;
      count_in = 0;
      for(int j=0;j< v_arr.size();j++){
        if(e_arr[i][j]) count_out++;
        if(e_arr[j][i]) count_in++;
      }
      degree_arr.push_back(count_out-count_in);
    }

    //print degree array
    cout<<"------ degree array ------"<<endl;
    for(int i=0;i< degree_arr.size();i++)
      cout<<"degree_arr["<< i <<"]= "<< degree_arr[i]<<endl;
```
利用上面的關係圖取得各點之間的degree關係（如果>0,則outdegree > indegree;如果<0,則outdegree < indegree）
