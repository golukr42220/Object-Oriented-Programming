Problem Statement:
Given IndividualContributor, Manager and CEO class. Code prefilled in the editor.

![image](https://user-images.githubusercontent.com/45598340/232586989-fd0ed968-1c97-4d53-9408-09dc40cdfef3.png)

Overload the greater than operator (>) each with the following definition:

- Takes two IndividualContributor objects as parameters and returns 1 if first object's salary is greater otherwise returns 0.
- Takes two Manager objects as parameters and returns 1 if first object's salary is greater otherwise returns 0.
- Takes two CEO objects as parameters and returns 1 if first object's salary is greater otherwise returns 0.
Do not modify the main method.

**Expected Output**
```
0 1
1 0
0 0
```

```
#include <bits/stdc++.h>
using namespace std;

class Employee {
	int id;
	string name;
	long salary;

public:
	Employee(int id, string name, long salary) {
		this->id = id;
		this->name = name;
		this->salary = salary;
	}
	
	int getId() {
		return id;
	}
	string getName() {
		return name;
	}
	long getSalary() {
		return salary;
	}
	
	double computeBonus() {
		return (salary*100)/10.0;
	}
};

class IndividualContributor: public Employee {
	int manager;
	
public:
	IndividualContributor(int id, string name, long salary, int manager): Employee(id, name, salary) {
		this->manager = manager;
	}
	
	int getManager() {
		return manager;
	}
	
	int operator > (IndividualContributor emp){
		return this->getSalary() > emp.getSalary()?1:0;
	}
};

class Manager: public Employee {
	int manager;
	int *reportees;
	
public:
	Manager(int id, string name, long salary, int manager, int *reportees): Employee(id, name, salary) {
		this->manager = manager;
		this->reportees = reportees;
	}
	
	int getManager() {
		return manager;
	}
	
	int* getReportees() {
		return reportees;
	}
	
	int operator > (Manager emp){
		return this->getSalary() > emp.getSalary()?1:0;
	}
};

class CEO: public Employee {
	int *reportees;
	
public:
	CEO(int id, string name, long salary, int *reportees): Employee(id, name, salary) {
		this->reportees = reportees;
	}
	
	int* getReportees() {
		return reportees;
	}
	
	int operator > (CEO emp){
		return this->getSalary() > emp.getSalary()?1:0;
	}
};

int main() {
	// your code goes here
	int ceoReportees[] = {1, 2};
	int terryReportees[] = {4};
	int tyrellReportees[] = {5};
	CEO lester(0, "Lester Moore", 20000000, ceoReportees);
	CEO phillip(1, "Phillip Price", 10000000, ceoReportees);
	Manager terry(2, "Terry Colby", 5000000, 0, terryReportees);
	Manager tyrell(3, "Tyrell Wellick", 4000000, 0, tyrellReportees);
	IndividualContributor elliot(4, "Elliot Alderson", 100000, 2);
	IndividualContributor angela(5, "Angela Moss", 100000, 3);
	
	cout << (phillip > lester) << " " << (lester > phillip) << endl;
	cout << (terry > tyrell) << " " << (tyrell > terry) << endl;
	cout << (elliot > angela) << " " << (angela > elliot) << endl;
	return 0;
}
```
