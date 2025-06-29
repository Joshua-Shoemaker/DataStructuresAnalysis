# Binary Search Tree Template Code

### Table of Contents

- [Description](#Description)
- [Node Structure](#Node-Structure)
- [Course Structure](#Course)
- [BSTError](BSTError)
- [Constructor/Destructor](#Constructor)
  - [Destructor Helper](#Destructor-Helper)
- [Insert Node](#Insert-Node)
  - [Insert Helper](#Insert-Helper)
- [Delete Node](#Delete-Node)
  - [Delete Helper](#Delete-Helper)
- [In Order Print](#In-Order-Print)
  - [In Order Helper](#In-Order-Helper)
- [Search](#Search)

---

### Description
My Binary Search Tree (BST) implementation is a data structure where nodes are organized to satisfy the BST property: all elements in a node's left subtree are less than the node's value, and all elements in its right subtree are greater. This structure allows efficient operations like insertion, deletion, and searching. For example, inserting values like 13, 3, 4, and others in sequence creates a BST where an inorder traversal yields a sorted sequence of keys. The highest and lowest values can be quickly found by traversing to the rightmost and leftmost nodes, respectively, making this BST ideal for applications requiring ordered data management. Reference: ([W3Schools](https://www.w3schools.com/dsa/dsa_data_binarysearchtrees.php))

At the time of creating this tree I knew it was going to be used for an object that needed a print method and member variables. Therefore I didn't make the BST completely universal. To use this BST without modification the object stored in it will have to have a key member variable to compare for and a print method.

## Node Structure
I used templates for the creation of the node structure so it can store different data types.
**Member Variables:** left node pointer, right node pointer
```C++
template <typename T>
struct Node {
    T object;
    Node* left;
    Node* right;

    // Constructor w/ defaults
    Node(const T& aObject) 
        : object(aObject), left(nullptr), right(nullptr){}
};
```

## Course
The program i used the tree for has a course object member variables and a print function. This is not optimal because the tree itself calls the print method which is not standard for a templated data structure. Instead the data structure should return the object. The Structure for this class does illuminate the purpose behind the BST methods.
```C++
struct Course {
    string key;
    string Title;
    vector<string> Prereqs;
    Course(string id = "", string title="", vector<string> prereqs={})
        : key(id), Title(title), Prereqs(prereqs) { }
    void print(bool prereqs) {
        cout << key << ", " << Title << endl;
        if (prereqs) {
            if (!Prereqs.empty())
                for (size_t i = 0; i < Prereqs.size(); i++) {
                    if (i == 0) {
                        cout << "\tPrerequisites: ";
                    }
                    cout << Prereqs[i];
                    if (i == Prereqs.size() - 1) {
                        cout << endl;
                    }
                    else { cout << " "; }
                }
            else {
                cout << endl;
            }
        }        
    }
};
```

## BSTError
I created a custom exception to use in the binary search tree to help with testing and implementation.
The BSTError takes a message at time of creation and inputs it into a formatted error message.
**Parameters:** A String Message
```C++
class BSTError : public std::runtime_error {
public:
    BSTError(const std::string& message) 
        : std::runtime_error(std::string("Binary Search Tree Error: ").append(message)) {}
};
```

## Constructor
I used a simple constructor to create node pointer to the root node. root is defined as a node pointer member variable.
```C++
BinarySearchTree() : root(nullptr) {};
```
### Destructor
A simple destructor calling the destructor helper, "destroyTree()" method and passes the root as an argument.
```C++
 virtual ~BinarySearchTree() {
     destroyTree(root);
 };
```
Recursively calls destroy helper to traverse down the left and right subtree of the node and then deletes the data.
**Parameter:** a Node Pointer 
```C++
void destroyTree(Node<T>* node) {
    if (node) {
        destroyTree(node->left);
        destroyTree(node->right);
        delete node;
    }
};
```

## Insert Node
The Insert method checks if a root node is created and if not then it creates one.
If the root node is created it calls the insert helper method to find where to create the new node by passing the root node and the object as arguments.
 
**Parameter:** An object of undefined type.
```C++
void Insert(const T& object) {
    if (!root) {
        root = new Node<T>(object);
    }
    else {
        addNode(root, object);
    }
};
```

### Insert Helper
The Insert Helper, addNode(), method checks if a member variable of the current node's object, key, is equal to the new objects key. If so the helper method throws a Binary Search Tree Error defined in the bst namespace for a duplicate item. 
Then the method checks if the current nodes object key is greater than the new object's key. If the object key is less than the current node's and the current node's left pointer is a null pointer a new node is created at current's left. Otherwise a recursive call adds the object to the the tree.
If the object is greater than the current node's object the same checks are done but to the right.
**Parameters:** Node pointer to current node, Object of the type stored in the tree.
**Throws:** duplication BSTError
```C++
void addNode(Node<T>* node, const T& object) {
    //check for duplication
    if (node->object.key == object.key) {
        // Throw duplication error
        throw BSTError("Item already exists.");
    }
    // if node's object is larger than target object add to left
    else if (node->object.key > object.key) {
        // if no left node
        if (!node->left) {
            // this node becomes left
            node->left = new Node<T>(object);
        }
        // else recurse down the left node
        else {
            addNode(node->left, object);
        }
    }
    // else
    else {
        // if no right node
        if (!node->right) {
            // this node becomes right
            node->right = new Node<T>(object);
        }
        //else
        else {
            // recurse down the right node
            addNode(node->right, object);
        }
    }
};
```
In this segment I new the object stored in the tree will have a key member variable so i have the method accessing this member variable. A more standard way of making this BST would be to compare objects out right but this would require comparitive operators to be defined for the object. I now realize this would be better.

## Delete Node
The Remove method calls the remove helper function passing the string to search for and the root node as arguments and sets the return of this function to a node pointer. It then checks if the node pointer is null and if so the method throws an Error for Item not found.
**Paramters:** a string key to search for
**Throws:** BSTError: Item not found
```C++
void Remove(const std::string& key) {
   Node<T>* node = removeNode(root, key);
   if (!node) {
       throw BSTError("Item Does Not Exist!");
   }
};
```

### Delete Helper
The delete helper searches through the tree comparing the current node to the target key passed as an argument. if the node is not found a cursive call is used on the left node if the target value is less than the current. The right node is used if the searched value is greater than the current node. when the node is found it is deleted and properly replaced if applicable
**Parameters:** Node Pointer for current node, String Key to find desired node.
**
```C++
Node<T>* removeNode(Node<T>* node, const std::string key) {
    // if node = nullptr return node
    if (!node) {
        return node;
    }
    // (else if key is less than node's object recurse down the left subtree)
    else if (key < node->object.key) {
        // set node's left pointer to recursive call passing node's left 
        // pointer and key as arguments
        node->left = removeNode(node->left, key);
    }
    // (else if key is greater than node's object recurse down the right subtree)
    else if (key > node->object.key) {
        // set node's right pointer to recursive call passing node's right 
        // pointer and key as arguments
        node->right = removeNode(node->right, key);
    }
    // else node found
    else {
        // if node does not have left child or no child
        if (!node->left) {
            // hold node right in temp, delete node, return temp
            Node<T>* temp = node->right;
            delete node;
            return temp;
            }
        // otherwise node has left but does not have right child
        else if (!node->right) {
            // hold node left in temp, delete node, return temp
            Node<T>* temp = node->left;
            delete node;
            return temp;
            }
        // else node has both children
        else {
            // make current pointer hold nodes right child
            Node<T>* current = node->right;
            // while current has left move current left
            while (current->left) {
                current = current->left;
            }
            // set node's object to current's object
            node->object = current->object;
            // set node's right to recursive call passing node's right and current's object.key as arguments
            node->right = removeNode(node->right, current->object.key);
            }
        }
    // return node
    return node;
};
```
## In Order Print 
This method calls the helper function, passing the root and the boolean as arguments, if the root is not empty. otherwise the function throws a BSTError for an empty tree.
For this project I had the in order function take a boolean parameter because I wanted to have it print two different ways. 
The print function description can found [here](#Course).
**Parameters:** Boolean
**Throws:** BSTError: No Data
```C++
 void inorder(bool val = false) {
     if (root) {
         inOrder(root, val);
     }
     // else tree is empty throw BSTERROR: No Data
     else {
         throw BSTError("No Data!");
     }
 };
```
### In Order Helper
The in order helper travels down the BST from left to right by a recursive call to the left, calling the print function on the object passing the boolean val as an argument, and then traversing down the right recursively.
**Parameters:** Node pointer for current node, a boolean val for the print method.
```C++
void inOrder(Node<T>* node, bool val) {
    //if node is not equal to null ptr
    if (node) {
        //Recursively travel down left
        inOrder(node->left, val);
        // Call object.print()
        node->object.print(val);
        //Recursively travel down right
        inOrder(node->right, val);
    }
};
```

## Search
The Search method uses a pointer to track the current node then navigates through the tree comparing values until the target object is found. Once found that objects print function is called. If the bottom of the tree is reached and no object matches an error is thrown.
**Parameters:** Boolean for the print function argument, and a String Key to find desired object.
**Throws:** BSTError for no object found.
```C++
void Search(const std::string key, bool val = false) const {
    // set current node equal to root
    Node<T>* current = root;
    // keep looping downwards until bottom reached or matching object found
    while (current) {
        // if match found, print current object
        if (key == current->object.key) {
            current->object.print(val);
            return;
        }
        // if key is smaller than current node then traverse left
        else if (key < current->object.key) {
            current = current->left;
        }
        // else larger so traverse right
        else {
            current = current->right;
        }
    }
    throw BSTError("No results found!");
};
```
