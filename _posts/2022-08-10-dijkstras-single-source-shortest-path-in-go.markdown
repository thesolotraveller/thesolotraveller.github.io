---
layout: post
title: "Implementing Dijkstra's algorithm to find shortest path from a single source in Go"
date: 2022-08-10 01:00:00 +0530
categories: [Tech, Golang, Go]
permalink: /posts/tech/dijkstras-single-source-shortest-path-in-go
---

```go
package main

import (
	"container/heap"
	"fmt"
	"math"
)

type gNode int

type graph map[gNode]map[gNode]int

type gNodeHeapEle struct {
	node gNode
	dist int
}

type gNodeHeap []gNodeHeapEle

func (h gNodeHeap) Len() int {
	return len(h)
}

func (h gNodeHeap) Less(i, j int) bool {
	return h[i].dist < h[j].dist
}

func (h gNodeHeap) Swap(i, j int) {
	h[i], h[j] = h[j], h[i]
}

func (h *gNodeHeap) Push(v interface{}) {
	*h = append(*h, v.(gNodeHeapEle))
}

func (h *gNodeHeap) Pop() interface{} {
	v := (*h)[len(*h)-1]
	*h = (*h)[0 : len(*h)-1]
	return v
}

func (g graph) AddNode(v int) {
	node := gNode(v)
	if _, ok := g[node]; !ok {
		g[node] = make(map[gNode]int)
	} else {
		return
	}
}

func (g graph) AddEdge(src, dest, weight int) {
	srcNode := gNode(src)
	if _, ok := g[srcNode]; !ok {
		g.AddNode(src)
	}

	destNode := gNode(dest)
	if _, ok := g[destNode]; !ok {
		g.AddNode(dest)
	}

	g[srcNode][destNode] = weight
}

func NewGraph() graph {
	return graph{}
}

func (g graph) ShortestDist(s int) {
	src := gNode(s)

	visited := make(map[gNode]bool)
	distances := make(map[gNode]int)
	h := gNodeHeap{}

	for n, _ := range g {
		var weight int = math.MaxInt32
		if n == gNode(src) {
			weight = 0
		}
		heap.Push(&h, gNodeHeapEle{n, weight})
		distances[n] = weight
	}

	for len(h) > 0 && len(visited) < len(g) {
		n := heap.Pop(&h).(gNodeHeapEle)

		nNode := n.node
		nDist := n.dist

		visited[nNode] = true

		for k, v := range g[nNode] {
			if _, ok := visited[k]; !ok {
				if v+nDist < distances[k] {
					distances[k] = v + nDist
					heap.Push(&h, gNodeHeapEle{k, v + nDist})
				}
			}
		}
	}

	for k, v := range distances {
		if k == src {
			continue
		}

		if v == math.MaxInt32 {
			fmt.Println(int(k), "not reachable from", src)
			continue
		}

		fmt.Println("Shortest distance from", src, "to", int(k), "is", v)
	}
}

func main() {
	g := NewGraph()

	g.AddNode(1)
	g.AddNode(2)
	g.AddNode(3)
	g.AddNode(4)
	g.AddNode(5)
	g.AddNode(6)

	g.AddEdge(1, 2, 2)
	g.AddEdge(1, 3, 4)
	g.AddEdge(2, 3, 1)
	g.AddEdge(2, 4, 7)
	g.AddEdge(3, 5, 3)
	g.AddEdge(5, 4, 2)
	g.AddEdge(5, 6, 5)
	g.AddEdge(4, 6, 1)

	g.ShortestDist(1)
}

```
