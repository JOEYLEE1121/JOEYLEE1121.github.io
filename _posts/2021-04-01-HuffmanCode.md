---
layout: single
title: "Huffman Code"
categories: "school-project"
tag: [python]
toc: true
toc_sticky: true
---

Lossless message encryption

score: 100 / 100
## About

Huffman code is a lossless data compression technique used to reduce the size of data without losing any information. It was invented by David A. Huffman in 1952. The basic idea behind Huffman coding is to assign a variable-length code to each symbol in the input data, where the length of the code for each symbol is based on its frequency of occurrence in the input data. The more frequently a symbol occurs, the shorter its corresponding code will be.

To create a Huffman code, the input data is first analyzed to determine the frequency of each symbol. This frequency information is then used to construct a binary tree, called a Huffman tree, where each leaf node represents a symbol and each non-leaf node represents the sum of the frequencies of its child nodes. The Huffman tree is then traversed to assign binary codes to each symbol, where the left child represents a 0 and the right child represents a 1.

The resulting Huffman code is a prefix code, meaning that no code word is a prefix of any other code word. This property allows for efficient decoding of the compressed data, as each code word can be uniquely identified without the need for a delimiter. Huffman coding is widely used in applications such as file compression, image compression, and network transmission.

## Code

```python
import sys

sum = 0
totalfreq = 0
listofnode = []
hmencode = ""

# A Huffman Tree Node
class node:
    def __init__(self, freq, symbol, left=None, right=None):
        # frequency of symbol
        self.freq = freq
 
        # symbol name (character)
        if (len(symbol) == 1 and ord(symbol) == 32):
            self.symbol = "space"
        else:
            self.symbol = symbol
 
        # node left of current node
        self.left = left
 
        # node right of current node
        self.right = right
 
        # tree direction (0/1)
        self.huff = ''

        # Huffman code of node
        self.newVal = 0
 
# utility function to print huffman
# codes for all symbols in the newly
# created Huffman tree
 
def printNodes(node, val=''):
    # huffman code for current node
    newVal = val + str(node.huff)
 
    # if node is not an edge node
    # then traverse inside it
    if(node.left):
        printNodes(node.left, newVal)
    if(node.right):
        printNodes(node.right, newVal)
 
        # if node is edge node then
        # display its huffman code
    if(not node.left and not node.right):
        print(f"{node.symbol}: {newVal}")
        node.newVal = newVal
        global sum, totalfreq
        sum += node.freq * len(newVal)
        totalfreq += node.freq

def find(node, x):
    global hmencode
    if(node.symbol == "space" and x == ' '):
        hmencode += node.newVal
    if(len(node.symbol) == 1 and node.symbol == x):
        hmencode += node.newVal
    else:
        if(node.left):
            find(node.left, x)
        if(node.right):
            find(node.right, x)


line = "hello how do you do"

# characters for huffman tree
print(line)

# characters for huffman tree
chars = []

for i in line:
    if i not in chars:
        chars.append(i)

chars = sorted(chars)
print(chars)

#buffer frequency list
bfreq = []

for i in line:
    bfreq.append(0)

for i in line:
    index = chars.index(i)
    bfreq[index] += 1

#frequency list
freq = []
for i in bfreq:
    if (i != 0):
        freq.append(i)

print(freq)
 
# list containing unused nodes
nodes = []
 
# converting characters and frequencies
# into huffman tree nodes
for x in range(len(chars)):
    nodes.append(node(freq[x], chars[x]))
 
while len(nodes) > 1:
    
    # pick 2 smallest nodes
    min_value = min(freq)
    min_index = freq.index(min_value)
    left = nodes[min_index]
    left.huff = 0
    freq.pop(min_index)
    nodes.remove(left)

    print(freq)
    
    secmin_value = min(freq)
    secmin_index = freq.index(secmin_value)
    right = nodes[secmin_index]
    right.huff = 1
    freq.pop(secmin_index)
    nodes.remove(right)

    print(freq)
 
    # combine the 2 smallest nodes to create
    # new node as their parent
    newNode = node(left.freq+right.freq, left.symbol+right.symbol, left, right)
    if (min_index > secmin_index):
        temp = secmin_index
        secmin_index = min_index
        min_index = temp
        left.huff = 1
        right.huff = 0
    freq.insert(min_index, min_value + secmin_value)
    nodes.insert(min_index, newNode)

    print(freq)
 
# Huffman Tree is ready!
printNodes(nodes[0])
print(f"Ave = {sum/totalfreq:0.3f} bits per symbol")

for i in line:
    find(nodes[0],i)

encodemsg = open("encodemsg.txt", 'w')
encodemsg.write(hmencode)
encodemsg.close()

sys.stdout = open("code.txt", 'w')
printNodes(nodes[0])
print(f"Ave = {sum/totalfreq:0.3f} bits per symbol")

infile = open(sys.argv[1])
line = infile.readline()
infile.close()
print(line)
```

## Result

### Input Sample

![sampleInput]({{site.url}}/images/2021-04-01-HuffmanCode/sampleInput.png)

### Output

![Output]({{site.url}}/images/2021-04-01-HuffmanCode/Output.png)