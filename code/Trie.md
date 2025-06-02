## Trie

```python
class TrieNode:
    def __init__(self):
        self.children: dict[str, TrieNode] = {} # can be replaced by list when vocabulary is given
        self.is_terminal: bool = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
```



#### Insert

```python
    def insert(self, word: str) -> None:
        curr = self.root
        for char in word:
            if char not in curr.children:
                curr.children[char] = TrieNode()
            curr = curr.children[char]
        curr.is_terminal = True
```

Time complexity: $O\left(n\right)$, where $n$ denotes the length of word.

Space complexity: $O\left(n\right)$.



#### Search word / prefix

```python
    def search(self, word: str) -> bool:
        curr = self.root
        for char in word:
            if char not in curr.children:
                return False
            curr = curr.children[char]
        return curr.is_terminal

    def starts_with(self, prefix: str) -> bool:
        """
        Return True if there is any word in the trie that starts with the given prefix.
        """
        curr = self.root
        for char in prefix:
            if char not in curr.children:
                return False
            curr = curr.children[char]
        return True
```

Time complexity: $O\left(n\right)$, where $n$ denotes the length of word / prefix.

Space complexity: $O\left(1\right)$.



#### Delete

```python
    def delete(self, word: str) -> None:
        def _delete(node: TrieNode, index: int) -> bool:
            if index == len(word):
                if not node.is_terminal:
                    return False
                node.is_terminal = False
                return not node.children
            char = word[index]
            if char in node.children and _delete(node.children[char], index + 1):
                del node.children[char]
                return not node.children and not node.is_terminal
            return False

        _delete(self.root, 0)
```

Time complexity: $O\left(n\right)$, where $n$ denotes the length of word.

Space complexity: $O\left(n\right)$.
