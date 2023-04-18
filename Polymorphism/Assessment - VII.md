**Problem Statement:**

![image](https://user-images.githubusercontent.com/45598340/232898875-b0fdb47d-6265-4a62-89eb-c85b5985bc96.png)

Since Employee class only acts as a base class and it does not make sense to create an Employee object directly, we can make it an abstract class.

- Update this code to use the following logic.
- Add the addresses of all the objects to the employees array like this:
```
employees[0] = &lester
```
- Convert the Employee class into an abstract class
- 
**Expected Output**
 ```
10500000.00
5500000.00
1150000.00
950000.00
10000.00
10000.00
```

```
#include <bits/stdc++.h>
using namespace std;

class Employee {
	public:
	int id;
	string name;
	long salary;


	Employee(int id, string name, long salary) {
		this->id = id;
		this->name = name;
		this->salary = salary;
	}
	
	virtual int getId() =0;

    virtual string getName() =0;
	
	virtual long getSalary() =0;
	
    virtual double computeBonus() =0;
	
};

class IndividualContributor: public Employee {
	int manager;
	
public:
	IndividualContributor(int id, string name, long salary, int manager): Employee(id, name, salary) {
		this->manager = manager;
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
	
	int getManager() {
		return manager;
	}
	
	double computeBonus() {
		return (getSalary()*10)/100.0;
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
	
	int getId() {
		return id;
	}
	string getName() {
		return name;
	}
	long getSalary() {
		return salary;
	}
	
	int getManager() {
		return manager;
	}
	
	int* getReportees() {
		return reportees;
	}
	
	double computeBonus() {
		if (getSalary() > 3000000) {
			return 150000 + (getSalary()*20)/100.0;
		} else {
			return (getSalary()*25)/100.0;
		}
	}
};

class CEO: public Employee {
	int *reportees;
	
public:
	CEO(int id, string name, long salary, int *reportees): Employee(id, name, salary) {
		this->reportees = reportees;
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
	
	int* getReportees() {
		return reportees;
	}
	
	double computeBonus() {
		return 500000 + (getSalary()*50)/100.0;
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
	
	Employee *employees[6];
	//Assign addresses of the above employees (ceos, managers and ics) to the employees array
	
	employees[0]=&lester;
	employees[1]=&phillip;
	employees[2]=&terry;
	employees[3]=&tyrell;
	employees[4]=&elliot;
	employees[5]=&angela;
	
	cout << fixed << setprecision(2);
	
	for (int i = 0; i < 6; i++) {
		cout << employees[i]->computeBonus() << endl;
	}
	return 0;
}
```
