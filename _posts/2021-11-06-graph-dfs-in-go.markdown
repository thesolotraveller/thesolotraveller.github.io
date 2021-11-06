---
layout: post
title:  "Implementing Graph Traversals (DFS and BFS) in Go"
date:   2021-11-06 14:50:36 +0530
categories: jekyll update
permalink: /graph-dfs-bfs-go
---
```go
package main

import "fmt"

type GraphNode struct {
	val int
}

type Graph struct {
	 nodes map[GraphNode]map[GraphNode]bool
	 isDirected bool
}

func newGraph(isDirected bool) *Graph {
	return &Graph{
		nodes: make(map[GraphNode]map[GraphNode]bool),
		isDirected: isDirected,
	}
}

func (graph *Graph) addNode(val int) {
	newNode := GraphNode{val: val}
	_, keyFound := graph.nodes[newNode]
	if keyFound {
		return
	} else {
		graph.nodes[newNode] = make(map[GraphNode]bool)
	}
}

func (graph *Graph) addEdge(src int, dest int) {
	srcNode := GraphNode{src}
	destNode := GraphNode{dest}

	_, srcExists := graph.nodes[srcNode]
	if !srcExists {
		graph.addNode(src)
	}

	_, destExists := graph.nodes[destNode]
	if !destExists {
		graph.addNode(dest)
	}

	graph.nodes[srcNode][destNode] = true

	if !graph.isDirected {
		graph.nodes[destNode][srcNode] = true
	}
}

func (graph *Graph) dfs(start int) {
	fmt.Println("\nPrinting depth first traversal of the graph")
	visited := make(map[GraphNode]bool)
	graph.dfsHelper(start, visited)

	for node, _ := range graph.nodes {
		if !visited[node] {
			graph.dfsHelper(node.val, visited)
		}
	}
}

func (graph *Graph) dfsHelper(start int, visited map[GraphNode]bool) {
	startNode := GraphNode{start}
	visited[startNode] = true
	fmt.Println(start)

	for key, _ := range graph.nodes[startNode] {
		if !visited[key] {
			visited[key] = true
			graph.dfsHelper(key.val, visited)
		}
	}
}

func (graph *Graph) bfs(start int) {
	fmt.Println("\nPrinting breadth first traversal of the graph")

	visited := make(map[GraphNode]bool)
	graph.bfsHelper(start, visited)

	for node, _ := range graph.nodes {
		if !visited[node] {
			graph.bfsHelper(node.val, visited)
		}
	}
}

func (graph *Graph) bfsHelper(start int, visited map[GraphNode]bool) {
	queue := []GraphNode{{start}}
	visited[GraphNode{start}] = true

	for len(queue) > 0 {
		var firstEle GraphNode
		firstEle, queue = queue[0], queue[1:]
		fmt.Println(firstEle.val)

		for key, _ := range graph.nodes[firstEle] {
			if !visited[key] {
				visited[key] = true
				queue = append(queue, key)
			}
		}
	}
}

func main() {
	graph := newGraph(true)

	graph.addNode(1)
	graph.addNode(2)
	graph.addNode(3)
	graph.addNode(4)

	graph.addEdge(1, 2)
	graph.addEdge(1, 3)
	graph.addEdge(1, 4)
	graph.addEdge(2, 3)
	graph.addEdge(2, 4)
	graph.addEdge(3, 4)

	graph.dfs(1)
	graph.bfs(2)
}
```