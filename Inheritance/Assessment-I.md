**Problem Statement:**
Given UML Diagram for Employee Management System (with Employee, IndividualContributor, Manager and CEO). Create classes for the same with the proper inheritance relationship. Do not modify the main method.

![image](https://user-images.githubusercontent.com/45598340/232580732-51b43f79-9af0-42d6-bb32-5bd206ee732d.png)

**Expected Output**

```
1 Phillip Price 10000000
2 Terry Colby 5000000 0
3 Tyrell Wellick 4000000 0
4 Elliot Alderson 100000 2
5 Angela Moss 100000 3
```

```
#include <bits/stdc++.h>
using namespace std;

class Employee{
	
	int id;
	long salary;
	string name;
	
	public:
	Employee(int id, string name, long salary ){
		this->id =id;
		this->name=name;
		this->salary=salary;
	}
	
	int getId(){  return id;}
	long getSalary(){ return salary;}
	string getName(){ return name;}
};

class CEO:public Employee{
	
	int ceoReportees[10];
	
	public:
	CEO(int id, string name, long salary, int ceoReportees[]):Employee(id, name,salary){
		this->ceoReportees[0]=ceoReportees[0];
		this->ceoReportees[1]=ceoReportees[1];
	}
	//int[] getReprotees(){ return ceoReportees;}
	
};

class Manager:public Employee{
	
	int manager;
	int managerReprotees[10];
	public:
	Manager(int id, string name, long salary, int manager,int managerReproties[]):Employee(id, name,salary){
		this->manager=manager;
		
		this->managerReprotees[0]=managerReprotees[0];
	}
	
	int getManager() { return manager;}
	//int[] getReprotees(){ return managerReprotees;}
	
};

class IndividualContributor: public Employee{
	
	int manager;
	
	public:IndividualContributor(int id, string name, long salary, int manager):Employee(id, name,salary){
		this->manager=manager;
	}
	int getManager() { return manager;}
};
int main() {
	// your code goes here
	int ceoReportees[] = {1, 2};
	int terryReportees[] = {4};
	int tyrellReportees[] = {5};
	CEO phillip(1, "Phillip Price", 10000000, ceoReportees);
	Manager terry(2, "Terry Colby", 5000000, 0, terryReportees);
	Manager tyrell(3, "Tyrell Wellick", 4000000, 0, tyrellReportees);
	IndividualContributor elliot(4, "Elliot Alderson", 100000, 2);
	IndividualContributor angela(5, "Angela Moss", 100000, 3);
	
	cout << phillip.getId() << " " << phillip.getName() << " " << phillip.getSalary() << endl;
	cout << terry.getId() << " " << terry.getName() << " " << terry.getSalary() << " " << terry.getManager() << endl;
	cout << tyrell.getId() << " " << tyrell.getName() << " " << tyrell.getSalary() << " " << tyrell.getManager() << endl;
	cout << elliot.getId() << " " << elliot.getName() << " " << elliot.getSalary() << " " << elliot.getManager() << endl;
	cout << angela.getId() << " " << angela.getName() << " " << angela.getSalary() << " " << angela.getManager() << endl;
	return 0;
}
```
