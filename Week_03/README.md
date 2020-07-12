学习笔记
本周主要学习了递归、分治和回溯

递归模板：
Python:
 def recursion(level, param1, param2, ...):
# recursion terminator
if level > MAX_LEVEL:
 	Process_result
	return
# process logic in current level
Process(level, data...)
# drill down
self.recursion(level+1, p1, ...)
# reverse the current level status if needed

分治模板：
def divide_conquer(problem, param1, param2, ...):
	if problem is None:
		print_result
		return
    # prepare data
    data = prepare_data(problem)
    subproblem = split_problem(problem, data) 
    
    # conquer subproblems
    subresult1 = self.divide_conquer(subproblems[0]. p1, ...)
    subresult2 = self.divide_conquer(subproblems[1], p2, ...)
	subresult3 = self.divide_conquer(subproblems[2], p3, ...)
    ...
    # process and generate the final result
    result = process_result(subresult1, subresult2, subresult3, ...)
    
    # revert the current level states
