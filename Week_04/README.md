学习笔记

# 递归模板

树一般用递归

void recursion(level, param1, param2, ...) {

​	# recursion terminator                                   终止条件

​	# process logic in current level                      当前层的逻辑处理

​	# drill down                                                   下探到下一层

​	# reverse the current level status if needed   清理当前层

}

1.不要人肉递归

2.找最近重复子问题

3.数学归纳法     从简单到复杂

# 

# DFS

如果在图的搜索中，DFS和BFS都别忘记加visited，树的DFS和BFS可以不加，因为树的节点是自上而下的

## 

递归DFS
visited = set()
def dfs(node, visited):
    if node in visited:      # terminator
        # already visited
        return
    
    visited.add(node)     # attention
    
    # process current node here
    
    for next_node in node.children():
        if not next_node in visited:
            dfs(next_node, visited)
            
            
BFS
def bfs(node, visited):
	queue = [];
    queue.append([start])
    
    visited = set()
    
    while queue:
        node = queue.pop()
        visited.add(node)
        
        process(node)
        nodes = generater_related_notes(node)
        queue.push(nodes)
        
        
