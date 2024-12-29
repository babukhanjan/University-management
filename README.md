# University-management
university project(Dev c++)
#include <iostream>
#include <string>
#include <vector>
#include <iomanip>
#include <fstream>
#include <sstream>
using namespace std;
struct Student {
    string name;
    string rollnumber;
    vector<int>labPerformance;
    vector<int>labReports;
    int midterm=0;
    int cea=0;
    int finalTerm=0;
    double totalmarks=0;
    char grade;
};
struct Class {
    string className;
    vector<Student>students;};
void calculate_total_and_grade(Student &student, const vector<double> &weights) {
    double labPerformanceTotal=0,labReportsTotal=0;
    for (int marks:student.labPerformance) {
        labPerformanceTotal+=marks;
    }
    for(int marks:student.labReports) {
        labReportsTotal+=marks;
}
    student.totalmarks=(labPerformanceTotal/student.labPerformance.size())*weights[0] +
                         (labReportsTotal/student.labReports.size())*weights[1] +
                         student.midterm*weights[2] +
                         student.cea*weights[3] +
                         student.finalTerm *weights[4];

    if(student.totalmarks>=90&&student.totalmarks>=100)
        student.grade='A';
    else if(student.totalmarks>=80&&student.totalmarks<90)
        student.grade='B';
    else if(student.totalmarks>=70&&student.totalmarks<80)
        student.grade='C';
    else if(student.totalmarks>=60&&student.totalmarks<700)
        student.grade='D';
    else
        student.grade='F';}
int main() {
	ofstream mycsvfile("Fdata.csv");
    mycsvfile<<"Name,Roll Number,Total Marks,Grade\n";
    vector<Class>classes;
    vector<double>weights(5, 0);
 int size=2;
    int choice;
    string a[size]={"Teacher portal","Cafe Mangement"};
    for(int i=0;i<size;i++){
    	cout<<"\t\t---------------"<<i+1<<": "<<a[i]<<"-------------------"<<endl;}
	cout<<"--------------------";
	cout<<"\n|Enter your choice:|";
    cout<<"\n--------------------"<<endl;
 cin>>choice;
switch(choice){
    case 1:
    cout<<"Enter the number of classes: ";
    int numClasses;
    cin>>numClasses;
    cin.ignore();
    for(int i=0;i<numClasses; ++i) {
        Class newClass;
        cout<<"Enter the name of class"<<i+1<<":";
        getline(cin,newClass.className);
        classes.push_back(newClass);}
    cout<<"\nChoose a class:\n";
    for(size_t i=0;i<classes.size();++i) {
        cout<<"Press "<<i+1<<"for"<<classes[i].className << "\n";}
    int classChoice;
    cin>>classChoice;
    --classChoice; 
    cout<<"\nEnter the number of students for"<<classes[classChoice].className<<":";
    int numStudents;
    cin>>numStudents;
    cin.ignore();
   for(int i=0;i<numStudents;++i){
        Student newStudent;
        cout<<"Enter name of student  "<<i+1<<":  ";      
        getline(cin, newStudent.name);
        cout<<"Enter roll number of student  "<<i+1<<":  ";
        getline(cin, newStudent.rollnumber); 
        cout<<"Enter marks for 5 lab performances:\n";
        for(int j=0;j<5;++j) {
            int marks;
            cout<<"Lab  "<<j+1<<": ";
            cin>>marks;
            newStudent.labPerformance.push_back(marks);}
        cout<<"Enter marks for 5 lab reports:\n";
        for(int j=0; j<5; ++j){
            int marks;
            cout<<"Report: "<<j+1<<": ";
            cin>>marks;
            newStudent.labReports.push_back(marks);}
        cout<<"Enter marks for midterm: ";
        cin>>newStudent.midterm;
        cout<<"Enter marks for CEA: ";
        cin>>newStudent.cea;
        cout<<"Enter marks for final term: ";
        cin>>newStudent.finalTerm;
        classes[classChoice].students.push_back(newStudent);}
    cout<<"\n Enter weights for assessments(in perctage, tota 100%):\n";
    cout<<"Lab performance: ";
    cin>>weights[0];
    cout<<"Lab reports: ";
    cin>>weights[1];
    cout<<"Midterm: ";
    cin>>weights[2];
    cout<<"CEA: ";
    cin>>weights[3];
    cout<<"Final term: ";
    cin>>weights[4];
    for (double &weight : weights) {
        weight/=100.0;}
    for (Student &student : classes[classChoice].students) {
        calculate_total_and_grade(student,weights);}cout<<"\nResults:\n";
    cout<<left<<setw(15)<<"Name"<<setw(15)<<"Roll No"<<setw(15)<<"Total Marks"<<setw(10)<<"Grade "<<"\n";
    for(const Student &student:classes[classChoice].students){
        cout<<left<<setw(15)<<student.name<<setw(15)<<student.rollnumber<<setw(15)<<student.totalmarks<<setw(10)<<student.grade<<"\n";
        mycsvfile<<endl<<student.name<<","
		<<student.rollnumber<<","<<student.totalmarks<<","
		<<student.grade<<"\n";}
         mycsvfile.close();
    break;
    case 2:
    	cout<<"waqas";
    	const int menuSize =5;
    string menuItems[menuSize] = {"Fry Fish","Heart Tea"," Chicken Sandwich","Zinger Burger","Cheesy Pizza"};
    double prices[menuSize]={350,200,500,700,850};
    vector<int>quantities(menuSize,0);  
    int choice;
    int quantity;
    char continueOrder;
    double totalBill=0.0;
    cout<<"\nWelcome to the Cafe Management System!\n";
    cout<<"------------------------------------\n";
    cout << "Menu:\n";
    for(int i=0;i<menuSize;++i) {
        cout<<i+1<<"."<<menuItems[i]<<"-Rs"<<fixed<<setprecision(2)<<prices[i]<<"\n";}
do{cout<<"\nEnter the number of the item you want to order:";
        cin>>choice;
        if(choice<1||choice>menuSize){
cout<<"invalid chewice Please try again\n";
            continue;}
cout<<"Enter the quantity for"<<menuItems[choice-1]<<":";
        cin>>quantity;
if(quantity<1){
            cout<<"Invalid quantity.Please try again.\n";
            continue; }
        quantities[choice-1]+=quantity;
        totalBill+=prices[choice-1]*quantity;
        cout<<"\nDo you want to order more items[y/n]: ";
        cin>>continueOrder;
    } while(continueOrder=='y'||continueOrder=='Y');
    cout<<"\nYour Order Summary:\n";
    cout<<"-------------------\n";
    cout<<left<<setw(20)<<"Item"<<setw(10)<<"Quantity"<<"Price\n";
    for(int i=0;i<menuSize;++i) {
        if(quantities[i]>0) {
            cout<<left<<setw(20)<<menuItems[i]<<setw(10)<<quantities[i]<<"$"<<fixed<<setprecision(2)<<prices[i]*quantities[i]<<"\n"; }}
     cout<<"\nTotal Bill:$"<<fixed<<setprecision(2)<<totalBill<<"\n";
    cout <<"\nThank you for visiting our cafe!\n";
    ofstream WaqasFile("Waqas.txt"); 
    if (WaqasFile.is_open()) {  
        WaqasFile<< "Order Summary:\n";
        WaqasFile<< "-------------------\n";
        WaqasFile<<left<<setw(20)<<"Item"<<setw(10)<<"Quantity"<<"Price\n";
        for(int i=0;i<menuSize;++i) {
            if(quantities[i]>0) {
                WaqasFile<<left<<setw(20)<<menuItems[i]<<setw(10)<<quantities[i]
                         << "Rs"<<prices[i]*quantities[i]<<"\n";}}
        WaqasFile<<"\nTotal Bill:Rs"<<totalBill<<"\n";
        WaqasFile.close();
        cout << "\nOrder details saved to order.txt\n";
    } else {
        cout<< "Error opening file for writing!\n"; 
        return 0; 
    }
ifstream MyReadFile("Waqas.txt");
string line;
if (MyReadFile.is_open()) {
while (getline(MyReadFile, line)) {
cout << line << endl;
}
MyReadFile.close();
} else {
cout << "Unable to open the file for reading!" << endl;}
    


    	break;
    	
    
}

    return 0;
}

