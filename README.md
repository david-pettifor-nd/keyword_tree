# Keyword Tree

A data structure for searching for keywords.

- Supports Python 2.7, Python 3.5

## Download

- PyPI: https://pypi.org/projects/keywordtree/
- GitHub: https://github.com/david-pettifor-nd/keyword_tree

## Installation

`pip install keywordtree`
 or download the package and `python setup.py install`

## Usage

### An empty Tree
```python
from keywordtree import Tree
    
# empty tree with all defaults
tree = Tree()
```

### A tree with a list of words
```python
# tree with a list of keywords passed in
tree = Tree(keywords=['bobby', 'blanket', 'bubbles', 'book', 'blood', 'bar', 'cat', 'category', 'car', 'cost'])
```

### A tree with a 1MB memory limit

```python
# tree with a memory consumption limit set to 1 MB (pass in number of bytes; default = no limit)
tree = Tree(memory_limit=(1024 * 1024), 
            keywords=['bobby', 'blanket', 'bubbles', 'book', 'blood', 'bar', 'cat', 'category', 'car', 'cost'])
```

### A tree built on a list of dictionary entries, allowing for case-sensitive or RegularExpressions to be enforced

```python
# A list of keywords, the first preserves capitalization, the second doesn't use regular expressions, the third both, and the fourth neither
keyword_list = [
    {'keyword': 'Notre Dame', 'case': True},
    {'keyword': 'university', 'regex': False},
    {'keyword': 'Fighting', 'case': True, 'regex': False},
    'irish'
]
tree = Tree(keywords=keyword_list)
```
 
 ### Adding a list of words to an existing tree
 ```python
# You can also add a list to a pre-existing tree
tree = KeywordTree()
keyword_list = [
    {'keyword': 'Notre Dame', 'case': True},
    {'keyword': 'university', 'regex': False},
    {'keyword': 'Fighting', 'case': True, 'regex': False},
    'irish'
]
tree.add_list(keyword_list)
```

### Removing a Keyword
```python
# tree with a list of keywords passed in
tree = Tree(keywords=['bobby', 'blanket', 'bubbles', 'book', 'blood', 'bar', 'cat', 'category', 'car', 'cost'])
# then remove the word 'bubbles'
tree.remove_keyword('bubbles')
```

### Exporting a tree to a local file (so you don't lose a large tree upon power outage)
```python
# tree with a list of keywords passed in
tree = Tree(keywords=['bobby', 'blanket', 'bubbles', 'book', 'blood', 'bar', 'cat', 'category', 'car', 'cost'])
# then export to a file 'mytree.json'
file_out = open('mytree.json', 'w')
file_out.write(tree.dump())
```

### Loading a tree from a previously dumped file
```python
# empty tree
tree = Tree()
# then import from a file 'mytree.json'
file_in = open('mytree.json', 'r')
tree.load(file.read())
```

## Examples

### Simple list
```python
# Import the keyword tree
from keywordtree import Tree

# Create a simple list of keywords
word_list = ['bobby', 'blanket', 'bubbles', 'book', 'blood', 'bar', 'cat', 'category', 'car', 'cost']

# Create the tree
tree = Tree(word_list)

# print the tree
tree.print_tree()

# Search the following text for the keywords listed
found_words = tree.prune('My cat and I are reading a book, covered with my favorite blanket at the bar.')

# print the words we found - this is a list
print(found_words)
```

Output:
```text
Tree Size: 10808 bytes
root
        -  'b'
                -  'bubbles'
                -  'bo'
                        -  'book'
                        -  'bobby'
                -  'bl'
                        -  'blood'
                        -  'blanket'
                -  'bar'
        -  'c'
                -  'cost'
                -  'ca'
                        -  'car'
                        -  'cat'
                                -  'category'
                                -  'cat'
['book', 'blanket', 'bar', 'cat']
```

### Words with capitalization restrictions
Note the keyword `Nashville` - by default capitalization is turned off.  So the construction will cast it to lower case (see output)
```python
# Import the keyword tree
from keywordtree import Tree

# Create a simple list of keywords
word_list = [{'keyword': 'Notre Dame', 'case': True}, 'Nashville', {'keyword': 'North Dakota', 'case': True}]

# Create the tree
tree = Tree(word_list)

# print the tree
tree.print_tree()
```

Output:
```text
Tree Size: 3408 bytes
root
        -  'nashville'
        -  'No'
                -  'North Dakota'
                -  'Notre Dame'
```

### Differences Regular Expressions can make
Note that for the word `cat` we turn regular expressions (`regex`) OFF (`False`).  This means that it will find the word `cat` within the larger word `catastrophe`.  Yet, because `regex` is `True` by default, the word `his` does not show up in the first word `This`.
```python
# Import the keyword tree
from keywordtree import Tree

# Create a simple list of keywords
word_list = ['his', {'keyword': 'cat', 'regex': False}]

# Create the tree
tree = Tree(word_list)

# Find the above keywords in the following text:
print tree.prune('This sentence could be a catastrophe.')
```