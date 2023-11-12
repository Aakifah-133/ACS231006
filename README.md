#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Student {
private:
    string name;
    int id;

public:
    Student(string name, int id) : name(name), id(id) {}

    string getName() const {
        return name;
    }

    int getID() const {
        return id;
    }
};

class Course {
private:
    string courseName;
    vector<Student*> students;

public:
    
    Course(string name) : courseName(name) {}

    
    Course(const Course& other) : courseName(other.courseName) {
        for (const auto& student : other.students) {
            students.push_back(new Student(student->getName(), student->getID()));
        }
    }

    ~Course() {
        for (auto& student : students) {
            delete student;
        }
    }

    
    void addStudent(Student* student) {
        students.push_back(student);
    }

    void dropStudent(int id) {
        auto it = remove_if(students.begin(), students.end(),
            [id](Student* student) { return student->getID() == id; });

        if (it != students.end()) {
            students.erase(it, students.end());
        }
    }

    
    void printStudents() const {
        for (const auto& student : students) {
            cout << "Name: " << student->getName() << ", ID: " << student->getID() << endl;
        }
    }
};

int main() {
    Course oop("oop");

    oop.addStudent(new Student("Ali", 1));
    oop.addStudent(new Student("amna", 2));
    oop.addStudent(new Student("aakifah", 3));

    
    Course physics = oop;

    physics.dropStudent(2);

    
    cout << "oop Course Students:" << endl;
    oop.printStudents();

    cout << "\nPhysics Course Students:" << endl;
    physics.printStudents();

    return 0;
}
