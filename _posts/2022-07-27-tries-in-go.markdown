---
layout: post
title: "Trie insert and search in Golang ( Go )"
date: 2022-07-26 09:00:00 +0530
categories: [Tech, Golang, Go]
permalink: /posts/tech/trie-in-go
---

```go
package main

import "fmt"

type TrieNode struct {
	isEnd    bool
	children map[string]*TrieNode
}

func NewTrie() *TrieNode {
	return &TrieNode{
		isEnd:    false,
		children: make(map[string]*TrieNode),
	}
}

func (trie *TrieNode) AddWord(word string) {
	var curr *TrieNode
	curr = trie
	for i, char := range word {
		key := string(char)
		if _, found := curr.children[key]; !found {
			var isEnd bool
			if i == len(word)-1 {
				isEnd = true
			}
			curr.children[key] = &TrieNode{
				isEnd:    isEnd,
				children: make(map[string]*TrieNode),
			}
			curr = curr.children[key]
		} else {
			curr = curr.children[key]
		}
	}
}

func (trie *TrieNode) SearchWord(word string) bool {
	var curr *TrieNode
	curr = trie
	for _, char := range word {
		key := string(char)
		if _, found := curr.children[key]; !found {
			return false
		} else {
			curr = curr.children[key]
		}
	}

	return curr.isEnd
}

func main() {
	root := NewTrie()

	words := []string{"banana", "bat", "cat", "car", "bake"}
	for _, word := range words {
		root.AddWord(word)
	}

	for _, word := range words {
		isFound := root.SearchWord(word)
		if isFound {
			fmt.Println(word, "found in trie")
		} else {
			fmt.Println(word, "not found in trie")
		}
	}
}

```
