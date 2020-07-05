学习笔记


//  前 K 个高频元素
vector<int> topKFrequent(vector<int>& nums, int k) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> p_queue;
        vector<int> res;
        int temp;
        unordered_map<int, int> unmap;
        for (int i = 0; i < nums.size(); ++i)
            unmap[nums[i]]++;
    
        for (auto kv : unmap) {
            p_queue.push(make_pair(kv.second, kv.first));     // sort value
            // cout << kv.second << " " << kv.first << endl;
            while (p_queue.size() > k) p_queue.pop();
        }
        int kk = p_queue.size();
        cout << kk << endl;
        while (!p_queue.empty()) {
            temp = p_queue.top().second;
            cout << p_queue.top().second << " " << p_queue.top().first << endl;
            p_queue.pop();
            res[kk--] = temp;
        }
        return res;
    }
    
    
 // 两数之和
 vector<int> twoSum(vector<int>& nums, int target) {
        int temp;
        vector<int> a;
        for (int i = 0; i < nums.size(); ++i) {
            temp = target - nums[i];
            for (int j = 0; j < nums.size(); ++j) {
                if (i != j && nums[j] == temp)
                {
                    a.push_back(i);
                    a.push_back(j);
                    return a;
                }
            }
        }
        return a;
    }
