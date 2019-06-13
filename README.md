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

...
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
    int add_flag = 0;
...

## How to run

### Linux

```
# clone first (with submodule)
$ git clone --recursive https://github.com/<your-name>/Graph-Theory-Project-Template.git
# (Option) build libfakemn.a
$ cd fake-mininet && make lib && cd .. && cp fake-mininet/libfakemn.a .
# build
$ make 
# run 
$ ./main.out
```

### Windows

If your PC is `window 10`, then you need to install the related dependencies. For example, if you are using `VSCode` as your IDE, then you can install the plugin of `C/C++` in your vscode. And then install the following programs/packages:
* mingw
* git

Step by step to create `c_cpp_properties.json` and `tasks.json`:
* `Ctrl+Shift+P` -> `C/C++: Edit Configuration` -> then you have the first JSON file.
* `Ctrl+Shift+P` -> `Tasks: Configure Task` -> then you have the second JSON file.

Here is the setting of `c_cpp_properties.json` and `tasks.json`:
```json
# c_cpp_properties.json (If your mingw installed in another folder, then you have to change the value in `compilterPath`)
{
    "configurations": [
        {
            "name": "MinGW",
            "intelliSenseMode": "gcc-x64",
            "includePath": [
                "$(workspaceFolder)"
            ],
            "compilerPath": "C:/mingw/bin/gcc.exe",
            "cStandard": "c11",
            "cppStandard": "c++11"
        }
    ],
    "version": 4
}

# tasks.json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "g++",
            "args": [
                "-g",
                "main.cc",
                "-o",
                "main.exe",
                "-Ifake-mininet/lib",
                "-g3",
                "-L.",
                "-lfakemn_win",
                "-std=c++11"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```

After all the settings have been done, we can press `Ctrl+Shift+B` to run the task, if your settings is correct, then there will have an executable file which named `main.exe`.

Or you can just [DOWNLOAD a pre-built VM images](http://gofile.me/39GpL/XU5tznyO6) provided by TA.

> [Guide of environment setting](https://hackmd.io/-5WZQC-1QqOeV3KUX65tEw?view) on Windows.

## TODO - Your descriptions about solutions/algorithms/results
