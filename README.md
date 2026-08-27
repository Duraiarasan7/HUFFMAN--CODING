# Huffman-Coding
## Aim
To implement Huffman coding to compress the data using Python.

## Software Required
1. Anaconda - Python 3.7

## Algorithm:
### Step1:
<br>
Get the input string.


### Step2:
<br>
Create tree nodes.

### Step3:
<br>
Main function to implement huffman coding.

### Step4:
<br>
calculate frequency of occurence.

### Step5:
<br>
print the characters and its huffmancode.


## Program:

``` 
import heapq
from collections import defaultdict
import matplotlib.pyplot as plt

# ---------- Tree Node ----------
class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None

    def __lt__(self, other):
        return self.freq < other.freq


# ---------- Frequency calculation ----------
def calculate_frequency(data):
    freq = defaultdict(int)
    for char in data:
        freq[char] += 1
    return freq


# ---------- Build Huffman Tree ----------
def build_huffman_tree(freq):
    heap = [Node(char, f) for char, f in freq.items()]
    heapq.heapify(heap)

    if len(heap) == 1:
        only_node = heap[0]
        parent = Node(None, only_node.freq)
        parent.left = only_node
        heapq.heappush(heap, parent)

    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        merged = Node(None, left.freq + right.freq)
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)

    return heap[0]


def generate_codes(node, current_code="", codes=None):
    if codes is None:
        codes = {}
    if node is None:
        return codes
    if node.char is not None:
        codes[node.char] = current_code if current_code else "0"
        return codes
    generate_codes(node.left, current_code + "0", codes)
    generate_codes(node.right, current_code + "1", codes)
    return codes


def huffman_coding(data):
    freq = calculate_frequency(data)
    root = build_huffman_tree(freq)
    codes = generate_codes(root)
    return codes, root, freq


# ---------- Render as plain text-style image ----------
def make_table_image(data, sort_by="code_length"):
    codes, root, freq = huffman_coding(data)

    if sort_by == "code_length":
        items = sorted(codes.items(), key=lambda x: (len(x[1]), x[1]))
    else:
        items = sorted(codes.items(), key=lambda x: -freq[x[0]])

    lines = []
    lines.append(f"{'Character':<12}| {'Huffman Code'}")
    lines.append("-" * 28)
    for char, code in items:
        display_char = "space" if char == " " else char
        lines.append(f"{display_char:<12}| {code}")

    text_block = "\n".join(lines)

    fig_height = 0.35 * len(lines)
    fig, ax = plt.subplots(figsize=(6, fig_height), facecolor="white")
    ax.axis("off")
    ax.set_xlim(0, 1)
    ax.set_ylim(0, 1)

    ax.text(0.0, 1.0, text_block,
            fontsize=16, fontfamily="monospace",
            ha="left", va="top", transform=ax.transAxes,
            color="black")

    plt.tight_layout()
    plt.show()


# ---- Run it ----
data = input("Enter the string: ")
make_table_image(data)


```
## Output:

### Print the characters and its huffmancode



![alt text](<Screenshot 2026-08-19 190401.png>)




## Result
Thus the huffman coding was implemented to compress the data using python programming.
