# contactbook-project
This is my first project i used c++ to complete this. and it helped me to gain good knowledge of classes and objects and string manipulation.
<br>
authr Karthik
<br>

#include <iostream>
#include <vector>
#include <string>
using namespace std;
//defining a class named contact
class Contact {
    private:
    string name;//name
    string phone;//phone no.
    string email;//email
    public:
    Contact(string n, string p, string e) : name(n),phone(p), email(e){}//defining a constructer

    void displaycontact() const{//const is used to access the data as it is. and is helpful. 
        cout << "\nName: " << name << "\n" << "Phone: " << phone << "\n" << "Email " << email <<"\n"<< endl;//this line displays contact when asked
    }
    string getname() const{//this function returns name
        return name;
    }
};
bool isValidPhone(const string& phone) {//this folder checks weather the phone no is valid or not
    if (phone.length() != 10) return false; // Check length
    for (char ch : phone) {//ensures every letter is number
        if (!isdigit(ch)) return false; // Ensure only digits
    }
    return true;
}
bool isValidName(const string& name) { //this folder checks weather the name is valid or not and takes only alphabet
    for (char ch : name) {  
        if (!isalpha(ch) && ch != ' ') return false; // Allow spaces
    }
    return true;
}
bool isValidEmail(const string& email) {
    // This function checks whether the email is valid or not.
    size_t atPos = email.find('@');
    if (atPos == string::npos) return false; // Must contain '@'

    string localPart = email.substr(0, atPos); // Part before '@'
    if (localPart.empty()) return false; // Local part must not be empty

    // Add additional checks for valid characters if needed (e.g., letters, digits, '.', '_', etc.)

    // Ensure there's a domain part
    string domainPart = email.substr(atPos + 1);
    if (domainPart.empty() || domainPart.find('.') == string::npos) return false; // Must contain '.'

    return true;
}


void addcontact(vector <Contact>& contacts){//this function add the contact to contact book
    string name, phone, email;
    do{
        cout << "Enter Name: ";
        cin.ignore(); //this refreshs the input variable everytime so the input can be given smoothly
        getline(cin,name);
        if(!isValidName(name)){
            cout << "Invalid name! Name should not contain numbers.\n";
        }
    }while(!isValidName(name)); // this block tells if the name is valid or not

    do{
        cout << "Enter Phone: ";
        cin >> phone;
        if(!isValidPhone(phone)){
            cout << "Invalid Phone NUMBER! Phone Number should be 10 digits.\n ";
        }
    }while(!isValidPhone(phone));//this block tells if the phone is valid or not

    do{
        cout<< "Enter Email: ";
        cin >> email;
        if(!isValidEmail(email)){
            cout << "Invalid email! Local part of email should not contain numbers.\n";
        }
    }while(!isValidEmail);//this block checks if the email is valid or not

    contacts.emplace_back(name,phone,email); /*It constructs a new object in-place at the end of the vector by passing the arguments
    directly to the constructor of the object type stored in the vector.*/
    cout << "Contact Added Successfully!" << endl;
}

void displayallcontact(const vector<Contact>& contacts){//this block displays the contact saved earlier
    if(contacts.empty()){
        cout << "No contacts available." << endl;
        return;
    }
    for(const auto& contact : contacts){ 
        contact.displaycontact();
    }
}

void searchcontact(const vector<Contact>& contacts){ //this block searches the contact 
    string searchname;
    cout << "Enter the name to search: ";
    cin.ignore();
    getline(cin, searchname);

    bool found = false;    
    for(const auto& contact : contacts){
        if(contact.getname() == searchname){
            cout << "Contact found: ";
            contact.displaycontact();
            found = true;
            break;
        }
    }
    if(!found){
        cout << "Constant not found." << endl;
    }
}

int main(){
    vector <Contact> contacts;
    int choice;

    do{    //using do while will run the code for the first time even the code has error
        cout << "---Contact Book ---";
        cout << "1. Add Contacts\n";
        cout << "2. Display All Contacts\n";
        cout << "3. Search Contact by Name\n";
        cout << "4. EXIT\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch(choice){ //switch cases used to call the required functions 
            case 1:
            addcontact(contacts);
            break;
            case 2:
            displayallcontact(contacts);
            break;
            case 3:
            searchcontact(contacts);
            break;
            case 4:
            cout << "Exiting Contact Book. Goodbyr!" << endl;
            break;
            default:
            cout << "Invalid choice. try again!" << endl;
        }
    }while (choice != 4);
    return 0;
}
