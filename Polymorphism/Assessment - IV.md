Problem Statement:
Update this code to use the following logic.

![image](https://user-images.githubusercontent.com/45598340/232587313-2304cf1c-f51f-4e80-bc47-9cf4693e3ba2.png)

New Bonus Structure:

- IC: 10% of salary
- Manager: if salary > 30,00,000 => (1,50,000 + 20% of salary) else 25% of salary
- CEO: 500000 + 50% of salary

**Expected Output**

```
10500000.00 5500000.00
1150000.00 950000.00
10000.00 10000.00
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
		return (salary*10)/100.0;
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
	double computeBonus() {
		return (this->getSalary()*10)/100.0;
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
	double computeBonus() {
		if(this->getSalary() > 3000000) 
			return 150000 + (this->getSalary() *20)/100.0;
		else
			return (this->getSalary() *25)/100.0;
			
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
	double computeBonus() {
		return 500000+ (this->getSalary()*50)/100.0;
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
	
	cout << fixed << setprecision(2);
	
	cout << lester.computeBonus() << " " << phillip.computeBonus() << endl;
	cout << terry.computeBonus() << " " << tyrell.computeBonus() << endl;
	cout << elliot.computeBonus() << " " << angela.computeBonus() << endl;
	return 0;
}

```

