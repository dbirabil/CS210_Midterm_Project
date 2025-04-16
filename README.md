# CS210_Midterm_Project
#Dorothy Birabil
#include <iostream>
#include <fstream>
#include <sstream>
#include <vector>
#include <string>

using namespace std;

struct School {
    School* next;
    string name;
    string address;
    string city;
    string state;
    string county;
    School(string name, string address, string city, string state, string county)
        : next(nullptr), name(name), address(address), city(city), state(state), county(county) {
    }
};

class SchoolBST {
    School* root;

    School* insert(School* node, School* newSchool) {
        if (node == nullptr) {
            return newSchool;
        }
        if (newSchool->name < node->name) {
            node->left = insert(node->left, newSchool);
        }
        else {
            node->right = insert(node->right, newSchool);
        }
        return node;
    }

    School* findMin(School* node) {
        while (node->left != nullptr) {
            node = node->left;
        }
        return node;
    }

    School* deleteByName(School* node, string name) {
        if (node == nullptr) {
            return node;
        }
        if (name < node->name) {
            node->left = deleteByName(node->left, name);
        }
        else if (name > node->name) {
            node->right = deleteByName(node->right, name);
        }
        else {
            if (node->left == nullptr) {
                School* temp = node->right;
                delete node;
                return temp;
            }
            else if (node->right == nullptr) {
                School* temp = node->left;
                delete node;
                return temp;
            }
            School* temp = findMin(node->right);
            node->name = temp->name;
            node->address = temp->address;
            node->city = temp->city;
            node->state = temp->state;
            node->county = temp->county;
            node->right = deleteByName(node->right, temp->name);
        }
        return node;
    }

    School* findByName(School* node, string name) {
        if (node == nullptr || node->name == name) {
            return node;
        }
        if (name < node->name) {
            return findByName(node->left, name);
        }
        return findByName(node->right, name);
    }

    void displayInOrder(School* node) {
        if (node != nullptr) {
            displayInOrder(node->left);
            cout << node->name << " -> ";
            displayInOrder(node->right);
        }
    }

    void displayPreOrder(School* node) {
        if (node != nullptr) {
            cout << node->name << " -> ";
            displayPreOrder(node->left);
            displayPreOrder(node->right);
        }
    }

    void displayPostOrder(School* node) {
        if (node != nullptr) {
            displayPostOrder(node->left);
            displayPostOrder(node->right);
            cout << node->name << " -> ";
        }
    }

public:
    SchoolBST() : root(nullptr) {}

    void insert(School* newSchool) {
        root = insert(root, newSchool);
    }

    void deleteByName(string name) {
        root = deleteByName(root, name);
    }

    School* findByName(string name) {
        return findByName(root, name);
    }

    void displayInOrder() {
        displayInOrder(root);
        cout << "nullptr" << endl;
    }

    void displayPreOrder() {
        displayPreOrder(root);
        cout << "nullptr" << endl;
    }

    void displayPostOrder() {
        displayPostOrder(root);
        cout << "nullptr" << endl;
    }
};

struct LinkedListNode {
    string name;
    LinkedListNode* next;

    LinkedListNode(string name) : name(name), next(nullptr) {}
};

class LinkedList {
    LinkedListNode* head;

public:
    LinkedList() : head(nullptr) {}

    void insert(string name) {
        LinkedListNode* newNode = new LinkedListNode(name);
        newNode->next = head;
        head = newNode;
    }

    void deleteByName(string name) {
        LinkedListNode* current = head;
        LinkedListNode* prev = nullptr;

        while (current != nullptr && current->name != name) {
            prev = current;
            current = current->next;
        }

        if (current == nullptr) return; // Not found

        if (prev == nullptr) {
            head = current->next;
        }
        else {
            prev->next = current->next;
        }

        delete current;
    }

    LinkedListNode* searchByName(string name) {
        LinkedListNode* current = head;
        while (current != nullptr) {
            if (current->name == name) {
                return current;
            }
            current = current->next;
        }
        return nullptr;
    }
};

class HashTable {
    unordered_map<string, string> table;

public:
    void insert(string name) {
        table[name] = name;
    }

    void deleteByName(string name) {
        table.erase(name);
    }

    bool searchByName(string name) {
        return table.find(name) != table.end();
    }
};

int main() {
    Timer timer;
    LinkedList linkedList;
    SchoolBST bst;
    HashTable hashTable;

    vector<string> names = { "Alice", "Bob", "Charlie", "David", "Eve" };

    cout << "Insertion:" << endl;

   
    timer.startTimer();
    for (const auto& name : names) {
        linkedList.insert(name);
    }
    timer.stopTimer();
    cout << "Linked List: " << timer.getDuration() << " ms" << endl;

 
    timer.startTimer();
    for (const auto& name : names) {
        bst.insert(new School(name, "", "", "", ""));
    }
    timer.stopTimer();
    cout << "Binary Search Tree: " << timer.getDuration() << " ms" << endl;

  
    timer.startTimer();
    for (const auto& name : names) {
        hashTable.insert(name);
    }
    timer.stopTimer();
    cout << "Hash Table: " << timer.getDuration() << " ms" << endl;

    cout << "\nSearch by Name:" << endl;
    string searchName = "Charlie";

    timer.startTimer();
    linkedList.searchByName(searchName);
    timer.stopTimer();
    cout << "Linked List: " << timer.getDuration() << " ms" << endl;

   
    timer.startTimer();
    bst.findByName(searchName);
    timer.stopTimer();
    cout << "Binary Search Tree: " << timer.getDuration() << " ms" << endl;

   
    timer.startTimer();
    hashTable.searchByName(searchName);
    timer.stopTimer();
    cout << "Hash Table: " << timer.getDuration() << " ms" << endl;

  
    cout << "\nDeletion by Name:" << endl;

    timer.startTimer();
    linkedList.deleteByName(searchName);
    timer.stopTimer();
    cout << "Linked List: " << timer.getDuration() << " ms" << endl;


    timer.startTimer();
    bst.deleteByName(searchName);
    timer.stopTimer();
    cout << "Binary Search Tree: " << timer.getDuration() << " ms" << endl;


    timer.startTimer();
    hashTable.deleteByName(searchName);
    timer.stopTimer();
    cout << "Hash Table: " << timer.getDuration() << " ms" << endl;

    return 0;
}

class SchoolHashTable {
private:
    static const int TABLE_SIZE = 100;
    vector<list<School>> table;

    int hashFunction(const string& name) {
        return polynomialHash(name, TABLE_SIZE);
    }

public:
    SchoolHashTable() {
        table.resize(TABLE_SIZE);
    }

    void insert(School school) {
        int key = hashFunction(school.name);
        for (auto& s : table[key]) {
            if (s.name == school.name) {
                cout << "Duplicate found. Updating existing school.\n";
                s = school;
                return;
            }
        }
        table[key].push_back(school);
    }

    void deleteByName(const string& name) {
        int key = hashFunction(name);
        auto& bucket = table[key];
        for (auto it = bucket.begin(); it != bucket.end(); ++it) {
            if (it->name == name) {
                bucket.erase(it);
                cout << "Deleted " << name << " from hash table.\n";
                return;
            }
        }
        cout << "School not found.\n";
    }

    School* findByName(const string& name) {
        int key = hashFunction(name);
        for (auto& s : table[key]) {
            if (s.name == name) {
                return &s;
            }
        }
        return nullptr;
    }

    void display() {
        for (int i = 0; i < TABLE_SIZE; ++i) {
            if (!table[i].empty()) {
                cout << "[" << i << "]: ";
                for (const auto& s : table[i]) {
                    cout << s.name << " -> ";
                }
                cout << "NULL\n";
            }
        }
    }
};


class CSVReader {
public:
    static vector<vector<string>> readCSV(const string& filename) {
        ifstream file(filename);
        vector<vector<string>> data;
        string line, word;

        if (!file.is_open()) {
            cerr << "Error: Could not open file " << filename << endl;
            return data;
        }

        while (getline(file, line)) {
            stringstream ss(line);
            vector<string> row;
            while (getline(ss, word, ',')) {
                row.push_back(word);
            }
            data.push_back(row);
        }
        file.close();
        return data;
    }
};

int main() {
    SchoolBST bst;
    vector<vector<string>> data = CSVReader::readCSV("schools.csv");

    for (const auto& row : data) {
        if (row.size() == 5) {
            bst.insert(new School(row[0], row[1], row[2], row[3], row[4]));
        }
    }

    cout << "In-Order Traversal:" << endl;
    bst.displayInOrder();
    cout << "Pre-Order Traversal:" << endl;
    bst.displayPreOrder();
    cout << "Post-Order Traversal:" << endl;
    bst.displayPostOrder();

    string searchName = "PLEASANT VALLEY MIDDLE SCHOOL";
    School* found = bst.findByName(searchName);
    if (found) {
        cout << "Found: " << found->name << " at " << found->address << endl;
    }
    else {
        cout << "School not found." << endl;
    }

    bst.deleteByName(searchName);
    cout << "After Deletion In-Order Traversal:" << endl;
    bst.displayInOrder();

    return 0;
}
void loadCSV(const string& filename, SchoolHashTable& ht) {
    ifstream file(filename);
    if (!file.is_open()) {
        cerr << "Error opening file.\n";
        return;
    }

    string line;
    getline(file, line); // skip header
    while (getline(file, line)) {
        stringstream ss(line);
        string name, address, city, state, county;

        getline(ss, name, ',');
        getline(ss, address, ',');
        getline(ss, city, ',');
        getline(ss, state, ',');
        getline(ss, county, ',');

        ht.insert(School(name, address, city, state, county));
    }

    file.close();
}

