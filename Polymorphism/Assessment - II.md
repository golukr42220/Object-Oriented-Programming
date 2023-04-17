Problem Statement:
Given IndividualContributor, Manager and CEO classes (Code prefilled in the editor)

![image](https://user-images.githubusercontent.com/45598340/232586609-ad1199ca-da36-4080-a7d9-194e1d5b3817.png)

Create three functions named max each with the following definition:

- Takes two IndividualContributor objects as parameters and returns the object with the maximum salary.
- Takes two Manager objects as parameters and returns the object with the maximum salary.
- Takes two CEO objects as parameters and returns the object with the maximum salary.
Do not modify the main method.

**Expected Output**
```
20000000 20000000
5000000 5000000
100000 100000
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
	
	IndividualContributor max( IndividualContributor emp2 ){
		if(this->getSalary() > emp2.getSalary())
			 return *this;
		else
			return emp2;
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
	
	Manager max(  Manager emp2 ){
		if(this->getSalary() > emp2.getSalary())
			 return *this;
		else
			return emp2;
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
	
	CEO max( CEO emp2 ){
		if(this->getSalary() > emp2.getSalary())
			 return *this;
		else
			return emp2;
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
	
	cout << max(phillip, lester).getSalary() << " " << max(lester, phillip).getSalary() << endl;
	cout << max(terry, tyrell).getSalary() << " " << max(tyrell, terry).getSalary() << endl;
	cout << max(elliot, angela).getSalary() << " " << max(angela, elliot).getSalary() << endl;
	return 0;
}

```



