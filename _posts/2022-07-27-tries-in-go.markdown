---
layout: post
title: "Implementing Trie data structure with inserting a word, searching a word and finding top K words matching a pattern in Go"
date: 2022-07-26 09:00:00 +0530
categories: [Tech, Golang, Go]
permalink: /posts/tech/trie-in-go
---

```go
package main

import (
	"fmt"
)

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
	fmt.Println("Inserting", word, "into the trie")
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

func (trie *TrieNode) GetTopKWordsWithPattern(pat string, k int) []string {
	curr := trie
	var word string
	for _, ch := range pat {
		key := string(ch)
		if v, ok := curr.children[key]; !ok {
			return []string{}
		} else {
			word += key
			curr = v
		}
	}

	var res []string
	if curr.isEnd {
		res = append(res, word)
	}
	curr.getWordsWithPatternHelper(&res, &word, k)
	return res
}

func (trie *TrieNode) getWordsWithPatternHelper(res *[]string, word *string, maxCount int) {
	if trie == nil {
		return
	}

	for k, v := range trie.children {
		*word += k
		if v.isEnd {
			if len(*res) == maxCount {
				return
			}

			*res = append(*res, *word)
		}
		trie.children[k].getWordsWithPatternHelper(res, word, maxCount)
		*word = (*word)[0 : len(*word) - 1]
	}
}

func main() {
	root := NewTrie()

	// inserting words, in list below, into the trie
	words := []string{"banana", "bat", "cat", "car", "bake", "batsman"}
	for _, word := range words {
		root.AddWord(word)
	}

	// checking for all words, in the list below, if it exists in trie or not
	wordsToCheckExistenceInTrieFor := []string{"banana", "bat", "batman", "car", "cake"}
	for _, word := range wordsToCheckExistenceInTrieFor {
		isFound := root.SearchWord(word)
		if isFound {
			fmt.Println("\nSearching '", word, "' in the trie. Status - Found")
		} else {
			fmt.Println("\nSearching '", word, "' in the trie. Status - Not Found")
		}
	}

	// top K matching words with a pattern string
	pattern := "ba"
	maxWordToFetch := 10
	matchingWords := root.GetTopKWordsWithPattern(pattern, maxWordToFetch)
	fmt.Println("\nTop", maxWordToFetch ,"words matching with '", pattern, "' are", matchingWords)
}
```
