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

class SchoolList {
    School* head;

public:
    SchoolList() : head(nullptr) {}

    void insertFirst(string name, string address, string city, string state, string county) {
        School* newNode = new School(name, address, city, state, county);
        newNode->next = head;
        head = newNode;
    }

    void insertLast(string name, string address, string city, string state, string county) {
        School* newNode = new School(name, address, city, state, county);
        if (!head) {
            head = newNode;
            return;
        }
        School* temp = head;
        while (temp->next) {
            temp = temp->next;
        }
        temp->next = newNode;
    }

    void deleteByName(string name) {
        if (!head) return;

        School* temp = head;
        School* prev = nullptr;

        while (temp && temp->name != name) {
            prev = temp;
            temp = temp->next;
        }

        if (!temp) return; // Name not found

        if (!prev) {
            head = temp->next;
        }
        else {
            prev->next = temp->next;
        }

        delete temp;
    }

    School* findByName(string name) {
        School* temp = head;
        while (temp) {
            if (temp->name == name) {
                return temp;
            }
            temp = temp->next;
        }
        return nullptr;
    }

    void display() {
        School* temp = head;
        while (temp) {
            cout << temp->name << " -> ";
            temp = temp->next;
        }
        cout << "nullptr" << endl;
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
    SchoolList list;
    list.insertFirst("KELLAR PRIMARY SCHOOL", "6413 MT HAWLEY RD", "PEORIA", "IL", "PEORIA");
    list.insertFirst("PLEASANT VALLEY MIDDLE SCHOOL", "3314 W RICHWOODS BVD", "PEORIA", "IL", "PEORIA");
    list.insertLast("FRANKLIN PRIMARY SCHOOL", "807 W COLUMBIA TER", "PEORIA", "IL", "PEORIA");
    list.display();

    School* found = list.findByName("PLEASANT VALLEY MIDDLE SCHOOL");
    if (found) {
        cout << "Found: " << found->name << " at " << found->address << endl;
    }

    list.deleteByName("PLEASANT VALLEY MIDDLE SCHOOL");
    list.display();
    return 0;
}


