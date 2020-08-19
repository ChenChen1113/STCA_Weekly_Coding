### 标准的kmp模板：  
```c++
    vector<int> getNext(string& needle){
        int size = needle.size();
        vector<int> next;
        next.push_back(-1);

        int j = 0, k = -1;
        while(j < size){
            if(k == -1 || needle[k] == needle[j]){ j ++; k ++; next.push_back(k); }//next的前两个元素为-1，0
            else{ k = next[k]; }
        }
        return next;
    }
```

参考：https://blog.csdn.net/qq_43152052/article/details/100059716
next数组详解：https://blog.csdn.net/BaiDingLT/article/details/69808221