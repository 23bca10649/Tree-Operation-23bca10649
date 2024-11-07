#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
    
    Node(int value) : data(value), left(nullptr), right(nullptr) {}
};

class BinarySearchTree {
public:
    Node* insert(Node* root, int value) {
        if (!root) return new Node(value);
        
        if (value < root->data) 
            root->left = insert(root->left, value);
        else 
            root->right = insert(root->right, value);
        
        return root;
    }

    Node* minValueNode(Node* node) {
        Node* current = node;
        while (current && current->left)
            current = current->left;
        return current;
    }
    
    Node* deleteNode(Node* root, int value) {
        if (!root) return root;
        
        if (value < root->data)
            root->left = deleteNode(root->left, value);
        else if (value > root->data)
            root->right = deleteNode(root->right, value);
        else {
            if (!root->left) {
                Node* temp = root->right;
                delete root;
                return temp;
            }
            else if (!root->right) {
                Node* temp = root->left;
                delete root;
                return temp;
            }
            
            Node* temp = minValueNode(root->right);
            root->data = temp->data;
            root->right = deleteNode(root->right, temp->data);
        }
        
        return root;
    }

    void inorderTraversal(Node* root) {
        if (root) {
            inorderTraversal(root->left);
            cout << root->data << " ";
            inorderTraversal(root->right);
        }
    }
};

int main() {
    BinarySearchTree bst;
    Node* root = nullptr;

    // Insert nodes
    root = bst.insert(root, 50);
    bst.insert(root, 30);
    bst.insert(root, 70);
    bst.insert(root, 20);
    bst.insert(root, 40);
    bst.insert(root, 60);
    bst.insert(root, 80);

    cout << "Inorder traversal: ";
    bst.inorderTraversal(root);
    cout << endl;

    // Delete a node
    root = bst.deleteNode(root, 50);
    cout << "Inorder traversal after deletion: ";
    bst.inorderTraversal(root);
    cout << endl;

    return 0;
}
