---
title: "最短路径问题"
date: "2022-08-18"
tags: ["算法"]
summary: "求解最短路径的算法通常都依赖于一种性质：两点之间的最短路径也包含了路径上其他顶点间的最短路径。"
---
求解最短路径的算法通常都依赖于一种性质：**两点之间的最短路径也包含了路径上其他顶点间的最短路径**。

- 单源最短路径（图中某一顶点到其他各顶点的最短路径）：Dijkstra算法
- 每对顶点间的最短路径：Floyd算法

# Dijkstra算法求单源最短路经问题

基于贪心策略

设置一个集合`S`记录已求得的最短路径的顶点，初始时其中只有源点`v0`，集合`S`每并入一个新顶点`vi`，都要修改源点到集合`V-S`中顶点当前的最短路径长度值。

# Floyd算法求各顶点之间最短路径问题

初始时，方阵内记录边的权值，作为它们之间的最短路径长度。以后，逐步尝试在原路径中加入顶点k作为中间顶点，修正方阵中的旧路径。

```c
/* Print the Matrix after each step */
#include <stdio.h>
#include <limits.h>                         // INT_MAX(infinity)
int main(void)
{
	int Vertex, Edge;
	scanf("%d %d", &Vertex, &Edge);			// 输入顶点数、边数
	int Floyd[Vertex][Vertex], Path[Vertex][Vertex];
	for (int row = 0; row < Vertex; row++) {
		for (int col = 0; col < Vertex; col++) {
			Floyd[row][col] = INT_MAX;		// 初始化为INF_MAX
			Path[row][col] = -1;			// 初始化为-1
		}
	}
	for (int row = 0; row < Edge; row++) {
		int x, y, weight;
		scanf("%d %d %d", &x, &y, &weight);	// 获取每条有向边及其权重
		Floyd[x][y] = weight;
		Floyd[row][row] = 0;
	}
	puts("Please confirm Floyd(-1)");		// 确认输入
	for (int row = 0; row < Vertex; row++)
		for (int col = 0; col < Vertex; col++) {
			if (col != Vertex - 1)
				printf("%10d ", Floyd[row][col]);
			else
				printf("%10d\n", Floyd[row][col]);
			}
	for (int k = 0; k < Vertex; k++) {		// Floyd算法核心
		for (int row = 0; row < Vertex; row++)
			for (int col = 0; col < Vertex; col++)
				if (Floyd[row][k]+Floyd[k][col] < Floyd[row][col]) {
					Floyd[row][col] = Floyd[row][k] + Floyd[k][col];
					Path[row][col] = k;
				}
		printf("Floyd(%d)\n", k);			// 输出距离数组
		for (int row = 0; row < Vertex; row++)
			for (int col = 0; col < Vertex; col++) {
				if (col != Vertex - 1)
					printf("%10d ", Floyd[row][col]);
				else
					printf("%10d\n", Floyd[row][col]);
				}
		printf("Path(%d)\n", k);			// 输出路径数组
		for (int row = 0; row < Vertex; row++)
			for (int col = 0; col < Vertex; col++) {
				if (col != Vertex - 1)
					printf("%3d ", Path[row][col]);
				else
					printf("%3d\n", Path[row][col]);
				}
	}
	return 0;
}
```
