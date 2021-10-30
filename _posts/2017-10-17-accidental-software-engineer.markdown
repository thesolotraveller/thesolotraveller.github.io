---
layout: post
title:  "Implementing Binary Search Tree (BST) in Go"
date:   2021-10-17 14:50:36 +0530
categories: jekyll update
permalink: /binary-search-tree-go
---
```go
package main

import "fmt"

type TreeNode struct {
	val int
	left *TreeNode
	right *TreeNode
}

func (root *TreeNode) printInorder() {
	if root == nil {
		return
	}

	root.left.printInorder()
	fmt.Printf("%d\n", root.val)
	root.right.printInorder()
}

func (root *TreeNode) insertIntoBST(val int) {
	// @todo - check if the tree is a BST or not

	if val < root.val {
		if root.left == nil {
			root.left = &TreeNode{val: val}
			return
		} else {
			root.left.insertIntoBST(val)
		}
	} else if val > root.val {
		if root.right == nil {
			root.right = &TreeNode{val: val}
			return
		} else {
			root.right.insertIntoBST(val)
		}
	}
	return
}

func main() {
	var tree *TreeNode = &TreeNode{5, nil, nil}
	tree.insertIntoBST(1)
	tree.insertIntoBST(9)

	tree.printInorder()
}
```