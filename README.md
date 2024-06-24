using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using static ASM2_Main.program;

namespace ASM2_Main
{
    internal class program
    {
        internal interface IMenu
        {
            void showMenu();
        }
        public interface Information
        {
            void ShowInformation();
        }
        public abstract class Person : Information
        {
           
            public int id { get; private set; }
            public string name { get; private set; }
            public string address { get; private set; }
            public int age { get; private set; }
            public int ID
            {
                get { return id; }
                set { id = value; }
            }

            public string Name
            {
                get { return name; }
                set { name = value; }
            }

            public string Address
            {
                get { return address; }
                set { address = value; }
            }

            public int Age
            {
                get { return age; }
                set { age = value; }
            }
  


            public virtual void ShowInformation()
            {
                Console.WriteLine($"Person ID: {ID}, Name: {Name}, Address: {Address}, Age: {Age}");
            }
        }

        public class Customer : Person
        {

            public int customerid { get; private set; } 
            public string namecustomer { get; private set; } 
            public string addresscustomer { get; private set; } 
            public int agecustomer { get; private set; } 

            public int CustomerID
            {
                get { return customerid; }
                set { customerid = value; }
            }

            public string NameCustomer
            {
                get { return namecustomer; }
                set { namecustomer = value; }
            }

            public string AddressCustomer
            {
                get { return addresscustomer; }
                set { addresscustomer = value; }
            }

            public int AgeCustomer
            {
                get { return agecustomer; }
                set { agecustomer = value; }
            }

            public Customer(int customerid, string namecustomer, string addresscustomer, int agecustomer)
            {
                CustomerID = customerid;
                NameCustomer = namecustomer;
                AddressCustomer = addresscustomer;
                AgeCustomer = agecustomer;
            }

            public Customer() { }

            public override void ShowInformation()
            {
                Console.WriteLine($"Customer ID: {CustomerID}, Namecustomer: {NameCustomer}, AddressCustomer: {AddressCustomer}, AgeCustomer: {AgeCustomer}");
            }
        }
        public class CustomerManagement : Customer, IMenu
        {
            private static CustomerManagement instance;
            private List<Customer> customerlist;

            public CustomerManagement()
            {
                this.customerlist = new List<Customer>();
            }
            //pattern
            public static CustomerManagement Instance
            {
                get
                {
                    
                    if (instance == null)
                    {
                        instance = new CustomerManagement();
                    }
                    return instance;
                }
            }
            //---
            public void showMenu()
            {
                Program program = new Program(); 
                int choice =-1;

                do
                {
                    bool validChoice = false;
                    Console.WriteLine("===================================");
                    Console.WriteLine("-----Welcome to CUSTOMER MANAGER-----");
                    Console.WriteLine("===================================");
                    Console.WriteLine("Choose an option:");
                    Console.WriteLine("1. Add Customer");
                    Console.WriteLine("2. Update Customer");
                    Console.WriteLine("3. Delete Customer");
                    Console.WriteLine("4. Show All Customer");
                    Console.WriteLine("5. Search Customer");
                    Console.WriteLine("6. Back to Menu Main");
                    Console.WriteLine("-----------------------");
                    Console.Write("Please enter your choice: ");
                    while (!validChoice)
                    {
                        try
                        {
                            choice = int.Parse(Console.ReadLine());
                            if (choice > 0 && choice <= 6)
                            {
                                validChoice = true;
                            }
                            else
                            {
                                Console.WriteLine("Invalid choice. Please enter a number between 1 and 6.");
                                Console.Write("Please enter your choice: ");
                            }
                        }
                        catch (FormatException)
                        {
                            Console.WriteLine("Invalid input. Please enter a valid number.");
                            Console.Write("Please enter your choice: ");
                        }
                    }





                    switch (choice)
                    {
                        case 1:
                            AddCustomer(customerlist);
                            break;
                        case 2:
                            UpdateCustomer(customerlist);
                            break;
                        case 3:
                            DeleteCustomer(customerlist);
                            break;
                        case 4:
                            ShowInformation();
                            break;
                            
                        case 5:
                            SearchCustomer(customerlist);
                            break;
                        case 6:
                            program.ShowMenu();
                            break;
                        default:
                            Console.WriteLine("Incorrect choice, please try again!!");
                            break;
                    }
                } while (choice != 6);
            }
            public void AddCustomer(List<Customer> customerlist)
            {
                Console.WriteLine("-----------------------");
                Console.WriteLine("Enter how many customer you need to add: ");
                int n = 0;
                bool isValidInput = false;

                while (!isValidInput)
                {
                    try
                    {
                        n = int.Parse(Console.ReadLine());
                        if (n > 0)
                        {
                            isValidInput = true;
                        }
                        else
                        {
                          
                            Console.WriteLine("Invalid input. Please enter a number greater than 0.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid input. Please enter a valid number.");
                        Console.ResetColor();
                    }
                }
                for (int i = 0; i < n; i++)
                {
                    CustomerManagement customermanagement = new CustomerManagement();
                    customermanagement.InputInforCustomer();
                    customerlist.Add(customermanagement);
                }
            }

            private static List<int> idAlreadyExists = new List<int>();
            public void InputInforCustomer()
            {
                // Customer ID
                Console.WriteLine("Please enter the Customer ID: ");
                bool isCustomerIdValid = false;
                while (!isCustomerIdValid)
                {
                    try
                    {
                        int input = int.Parse(Console.ReadLine());
                        if (input > 0)
                        {
                            if (idAlreadyExists.Contains(input))
                            {
                                Console.ForegroundColor = ConsoleColor.Red;
                                Console.WriteLine("This Customer ID already exists. Please enter a different ID.");
                                Console.ResetColor();
                            }
                            else
                            {
                                CustomerID = input;
                                idAlreadyExists.Add(input);
                                isCustomerIdValid = true;
                            }
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the Customer ID.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Please enter a valid number for the Customer ID.");
                        Console.ResetColor();
                    }
                }
                // Customer Name
                while (string.IsNullOrWhiteSpace(NameCustomer))
                {
                    Console.WriteLine("Please enter the customer name: ");
                    NameCustomer = Console.ReadLine();
                    if (string.IsNullOrWhiteSpace(NameCustomer))
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Customer name cannot be empty. Please enter a valid name.");
                        Console.ResetColor();
                    }
                }
                // Address Name
                while (string.IsNullOrWhiteSpace(AddressCustomer))
                {
                    Console.WriteLine("Please enter the address name: ");
                    AddressCustomer = Console.ReadLine();
                    if (string.IsNullOrWhiteSpace(AddressCustomer))
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Address name cannot be empty. Please enter a valid name.");
                        Console.ResetColor();
                    }
                }
                // age customer
                Console.WriteLine("Please enter the customer age: ");
                while (true)
                {
                    try
                    {
                        int age = int.Parse(Console.ReadLine());
                        if (age > 0)
                        {
                            AgeCustomer = age;
                            break;
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the age.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid age format. Please enter a valid integer age.");
                        Console.ResetColor();
                    }
                }
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine($"Customer with ID {CustomerID} added successfully!");
                Console.ResetColor();
                Console.WriteLine("---------------------------- ");
            }

            public void UpdateCustomer(List<Customer> customerlist)
            {
                bool isValidID = false;
                int customerid = -1;
                Customer customerToUpdate = null;
                string userInput;

                while (!isValidID)
                {
                    Console.WriteLine("--- Update Customer ---");
                    Console.Write("Enter the ID of the customer to update (Enter 'exit' to cancel): ");
                    userInput = Console.ReadLine();
                    if (userInput.ToLower() == "exit")
                    {
                        Console.WriteLine("Update canceled.");
                        return;
                    }
                    try
                    {
                        customerid = int.Parse(userInput);
                        if (customerid > 0)
                        {
                            for (int i = 0; i < customerlist.Count; i++)
                            {
                                if (customerlist[i].CustomerID == customerid)
                                {
                                    customerToUpdate = customerlist[i];
                                    isValidID = true;
                                    break;
                                }
                            }
                            if (!isValidID)
                            {
                                Console.ForegroundColor = ConsoleColor.Red;
                                Console.WriteLine("Customer not found. Please try again or enter 'exit' to cancel.");
                                Console.ResetColor();
                            }
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the Customer ID.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid ID. Please enter a valid integer ID or enter 'exit' to cancel.");
                        Console.ResetColor();
                    }
                }

                Console.WriteLine("Enter the new customer name (or leave blank to keep the existing name): ");
                string newCustomerName = Console.ReadLine();
                if (!string.IsNullOrWhiteSpace(newCustomerName))
                {
                    customerToUpdate.NameCustomer = newCustomerName;
                }

                Console.WriteLine("Enter the new address name (or leave blank to keep the existing address name): ");
                string newAddressCustomer = Console.ReadLine();
                if (!string.IsNullOrWhiteSpace(newAddressCustomer))
                {
                    customerToUpdate.AddressCustomer = newAddressCustomer;
                }

                Console.WriteLine("Enter the new age: ");
                while (true)
                {
                    try
                    {
                        int age = int.Parse(Console.ReadLine());
                        if (age > 0)
                        {
                            customerToUpdate.AgeCustomer = age;
                            break;
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the age.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid age format. Please enter a valid integer age.");
                        Console.ResetColor();
                    }
                }
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine($"Customer with ID {customerid} updated successfully!");
                Console.ResetColor();
                Console.WriteLine("---------------------------- ");
            }

            public void DeleteCustomer(List<Customer> customerlist)
            {
                bool isValidID = false;
                int customerid = -1;
                Customer customerToDelete = null;
                string userInput;

                while (!isValidID)
                {
                    Console.WriteLine("--- Delete Customer ---");
                    Console.Write("Enter the ID of the customer to delete (Enter 'exit' to cancel): ");
                    userInput = Console.ReadLine();

                    if (userInput.ToLower() == "exit")
                    {
                        Console.WriteLine("Delete canceled.");
                        return;
                    }
                    try
                    {
                        customerid = int.Parse(userInput);
                        if (customerid > 0)
                        {
                            for (int i = 0; i < customerlist.Count; i++)
                            {
                                if (customerlist[i].CustomerID == customerid)
                                {
                                    customerToDelete = customerlist[i];
                                    isValidID = true;
                                    break;
                                }
                            }

                            if (!isValidID)
                            {
                                Console.ForegroundColor = ConsoleColor.Red;
                                Console.WriteLine("Customer not found. Please try again.");
                                Console.ResetColor();
                            }
                            else
                            {
                                customerlist.Remove(customerToDelete); // Remove the comic from the list
                                Console.ForegroundColor = ConsoleColor.Green;
                                Console.WriteLine($"Customer with ID {customerid} deleted successfully!");
                                Console.ResetColor();
                                Console.WriteLine("---------------------------- ");
                            }
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the Customer ID.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid ID. Please enter a valid integer ID or enter 'exit' to cancel.");
                        Console.ResetColor();
                    }
                }
            }
 

            public override void ShowInformation()
            {
                Console.WriteLine("---------------Customer information---------------");
                if (customerlist.Count == 0)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("No customer available in the list.");
                    Console.ResetColor();
                }
                else
                {
                    for (int i = 0; i < customerlist.Count; i++)
                    {
                        var customer = customerlist[i];
                        Console.WriteLine($":CustomerID {customer.CustomerID}; NameCustomer: {customer.NameCustomer}; AddressCustomer: {customer.AddressCustomer}; AgeCustomer: {customer.AgeCustomer}");
                        Console.ForegroundColor = ConsoleColor.Green;
                        Console.WriteLine("Displayed successfully!");
                        Console.ResetColor();
                        Console.WriteLine("---------------------------- ");
                    }
                }
            }

            public void SearchCustomer(List<Customer> customerlist)
            {
                bool continueSearching = true;

                while (continueSearching)
                {
                    int customerid = -1;
                    string userInput;

                    Console.WriteLine("--- Search Customer ---");
                    Console.Write("Enter the ID of the customer to search (Enter 'exit' to cancel): ");
                    userInput = Console.ReadLine();

                    if (userInput.ToLower() == "exit")
                    {
                        Console.WriteLine("Search canceled.");
                        return;
                    }

                    try
                    {
                        customerid = int.Parse(userInput);

                        if (customerid <= 0)
                        {
                            throw new FormatException();
                        }

                        bool found = false;
                        foreach (var customer in customerlist)
                        {
                            if (customer.CustomerID == customerid)
                            {
                                Console.WriteLine($"CustomerID: {customer.CustomerID}; NameCustomer: {customer.NameCustomer}; AddressCustomer: {customer.AddressCustomer}; AgeCustomer: {customer.AgeCustomer}");
                                found = true;
                                Console.ForegroundColor = ConsoleColor.Green;
                                Console.WriteLine("Customer found successfully.");
                                Console.ResetColor();
                                break;
                            }
                        }

                        if (!found)
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Customer not found. Please try again.");
                            Console.ResetColor();
                        }
                        else
                        {
                            continueSearching = false;
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid ID. Please enter a valid integer ID or enter 'exit' to cancel.");
                        Console.ResetColor();
                    }
                }
            }
           

        }
        public class Employee : Person
        {


            public int employeeid { get; private set; }
            public string nameemployee { get; private set; }
            public string addressemployye { get; private set; }
            public int ageemployee { get; private set; }




            public int EmployeeID
            {
                get { return employeeid; }
                set { employeeid = value; }
            }

            public string NameEmployee
            {
                get { return nameemployee; }
                set { nameemployee = value; }
            }

            public string AddressEmployye
            {
                get { return addressemployye; }
                set { addressemployye = value; }
            }

            public int AgeEmployee
            {
                get { return ageemployee; }
                set { ageemployee = value; }
            }

            public Employee(int employid, string nameemployee, string addressemployee, int ageemployee)
            {
                EmployeeID = employid;
                NameEmployee = nameemployee;
                AddressEmployye = addressemployee;
                AgeEmployee = ageemployee;
            }

            public Employee() { }

            public override void ShowInformation()
            {
                Console.WriteLine($"EmployeeID: {EmployeeID}, NameEmployee: {NameEmployee}, AddressEmployye: {AddressEmployye}, AgeEmployee: {AgeEmployee}");
            }
        }

        public class EmployyeManagement : Employee, IMenu
        {
            //pattern
            private static EmployyeManagement instance;


            private List<Employee> employyelist;
            public EmployyeManagement()
            {
                this.employyelist = new List<Employee>();
            }

            //pattern
            public static EmployyeManagement Instance
            {
                get
                {
                    
                    if (instance == null)
                    {
                        instance = new EmployyeManagement();
                    }
                    return instance;
                }
            }
           
            public void showMenu()
            {
                Program program = new Program();
                int choice = -1;
                
                do
                {
                    bool validChoice = false;
                    Console.WriteLine("==================================");
                    Console.WriteLine("-----Welcome to EMPLOYEE MANAGER-----");
                    Console.WriteLine("==================================");
                    Console.WriteLine("Choose an option:");
                    Console.WriteLine("1. Add Employee");
                    Console.WriteLine("2. Update Employee");
                    Console.WriteLine("3. Delete Employee");
                    Console.WriteLine("4. Show All Employee");
                    Console.WriteLine("5. Search Employee");
                    Console.WriteLine("6. Back to Menu Main");
                    Console.WriteLine("-----------------------");
                    Console.Write("Please enter your choice: ");
                  

                    while (!validChoice)
                    {
                        try
                        {
                            choice = int.Parse(Console.ReadLine());
                            if (choice > 0 && choice <= 6)
                            {
                                validChoice = true;
                            }
                            else
                            {
                                Console.WriteLine("Invalid choice. Please enter a number between 1 and 6.");
                                Console.Write("Please enter your choice: ");
                            }
                        }
                        catch (FormatException)
                        {
                            Console.WriteLine("Invalid input. Please enter a valid number.");
                            Console.Write("Please enter your choice: ");
                        }
                    }


                    switch (choice)
                    {
                        case 1:
                            AddEmployye(employyelist);
                            break;
                        case 2:
                            UpdateEmployye(employyelist);
                            break;
                        case 3:
                            DeleteEmployye(employyelist);
                            break;
                        case 4:
                            ShowInformation();
                            break;
                        case 5:
                            SearchEmployye(employyelist);
                            break;
                        case 6:
                            program.ShowMenu();
                            break;
                        default:
                            Console.WriteLine("Incorrect choice, please try again!!");
                            break;
                    }
                } while (choice != 6);
            }

            public void AddEmployye(List<Employee> employyelist)
            {
                Console.WriteLine("-----------------------");
                Console.WriteLine("Enter how many employye you need to add: ");
                int n = 0;
                bool isValidInput = false;

                while (!isValidInput)
                {
                    try
                    {
                        n = int.Parse(Console.ReadLine());
                        if (n > 0)
                        {
                            isValidInput = true;
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Invalid input. Please enter a number greater than 0.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid input. Please enter a valid number.");
                        Console.ResetColor();
                    }
                }
                for (int i = 0; i < n; i++)
                {
                    EmployyeManagement employyemanagement = new EmployyeManagement();
                    employyemanagement.InputInforemployye();
                    employyelist.Add(employyemanagement);
                }
            }

            private static List<int> idAlreadyExists = new List<int>();
            public void InputInforemployye()
            {
                // Employye ID
                Console.WriteLine("Please enter the Employye ID: ");
                bool isEmployyeIdValid = false;
                while (!isEmployyeIdValid)
                {
                    try
                    {
                        int input = int.Parse(Console.ReadLine());
                        if (input > 0)
                        {
                            if (idAlreadyExists.Contains(input))
                            {
                                Console.ForegroundColor = ConsoleColor.Red;
                                Console.WriteLine("This Employye ID already exists. Please enter a different ID.");
                                Console.ResetColor();
                            }
                            else // Nếu trùng
                            {
                                EmployeeID = input;  // nhập employye ID
                                idAlreadyExists.Add(input);
                                isEmployyeIdValid = true;
                            }
                        }
                        else // Nếu nhỏ hơn 0
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the Employee ID.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException) // Nếu nhập chữ
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Please enter a valid number for the Employee ID.");
                        Console.ResetColor();
                    }
                }
                // Employye Name
                while (string.IsNullOrWhiteSpace(NameEmployee))  // kiểm tra xem có phải null không, nếu có thì tiếp tục vòng lặp 
                {
                    Console.WriteLine("Please enter the employye name: ");
                    NameEmployee = Console.ReadLine();  // nhập tên nhan vien
                    if (string.IsNullOrWhiteSpace(NameEmployee))  // kiểm tra xem nhập vào có bị trống không, nếu có thì hiện thông báo lỗi
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Employye name cannot be empty. Please enter a valid name.");
                        Console.ResetColor();
                    }
                }
                // Address Name
                while (string.IsNullOrWhiteSpace(AddressEmployye))
                {
                    Console.WriteLine("Please enter the address name: ");
                    AddressEmployye = Console.ReadLine();
                    if (string.IsNullOrWhiteSpace(AddressEmployye))
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Address name cannot be empty. Please enter a valid name.");
                        Console.ResetColor();
                    }
                }
                // age
                Console.WriteLine("Please enter the employye age: ");
                while (true)
                {
                    try
                    {
                        int age = int.Parse(Console.ReadLine());  // chuyển đổi chuỗi thành số thực
                        if (age > 0)  // kiểm tra lớn hơn 0
                        {
                            AgeEmployee = age;
                            break;
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the age.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid age format. Please enter a valid integer age.");
                        Console.ResetColor();
                    }
                }
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine($"Employye with ID {EmployeeID} added successfully!");
                Console.ResetColor();
                Console.WriteLine("---------------------------- ");
            }

            public void UpdateEmployye(List<Employee> employyelist)
            {
                bool isValidID = false;
                int employyeid = -1;
                Employee employyeToUpdate = null;
                string userInput;

                while (!isValidID)
                {
                    Console.WriteLine("--- Update Employye ---");
                    Console.Write("Enter the ID of the employye to update (Enter 'exit' to cancel): ");
                    userInput = Console.ReadLine();
                    if (userInput.ToLower() == "exit")
                    {
                        Console.WriteLine("Update canceled.");
                        return;
                    }
                    try
                    {
                        employyeid = int.Parse(userInput);
                        if (employyeid > 0)
                        {
                            for (int i = 0; i < employyelist.Count; i++)
                            {
                                if (employyelist[i].EmployeeID == employyeid)
                                {
                                    employyeToUpdate = employyelist[i];
                                    isValidID = true;
                                    break;
                                }
                            }
                            if (!isValidID)
                            {
                                Console.ForegroundColor = ConsoleColor.Red;
                                Console.WriteLine("Employye not found. Please try again or enter 'exit' to cancel.");
                                Console.ResetColor();
                            }
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the Employee ID.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid ID. Please enter a valid integer ID or enter 'exit' to cancel.");
                        Console.ResetColor();
                    }
                }

                Console.WriteLine("Enter the new employye name (or leave blank to keep the existing name): ");
                string newEmployyeName = Console.ReadLine();
                if (!string.IsNullOrWhiteSpace(newEmployyeName))
                {
                    employyeToUpdate.NameEmployee = newEmployyeName;
                }

                Console.WriteLine("Enter the new address name (or leave blank to keep the existing address name): ");
                string newAddressName = Console.ReadLine();
                if (!string.IsNullOrWhiteSpace(newAddressName))
                {
                    employyeToUpdate.AddressEmployye = newAddressName;
                }

                Console.WriteLine("Enter the new age: ");
                while (true)
                {
                    try
                    {
                        int age = int.Parse(Console.ReadLine());
                        if (age > 0)
                        {
                            employyeToUpdate.AgeEmployee = age;
                            break;
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the age.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid age format. Please enter a valid integer age.");
                        Console.ResetColor();
                    }
                }
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine($"Employye with ID {employyeid} updated successfully!");
                Console.ResetColor();
                Console.WriteLine("---------------------------- ");
            }

            public void DeleteEmployye(List<Employee> employyelist)
            {
                // Implement the delete employye action here
                bool isValidID = false;
                int employyeid = -1;
                Employee employyeToDelete = null;
                string userInput;

                while (!isValidID)
                {
                    Console.WriteLine("--- Delete employyee ---");
                    Console.Write("Enter the ID of the employyee to delete (Enter 'exit' to cancel): ");
                    userInput = Console.ReadLine();

                    if (userInput.ToLower() == "exit")
                    {
                        Console.WriteLine("Delete canceled.");
                        return;
                    }
                    try
                    {
                        employyeid = int.Parse(userInput);
                        if (employyeid > 0)
                        {
                            for (int i = 0; i < employyelist.Count; i++)
                            {
                                if (employyelist[i].EmployeeID == employyeid)
                                {
                                    employyeToDelete = employyelist[i];
                                    isValidID = true;
                                    break;
                                }
                            }

                            if (!isValidID)
                            {
                                Console.ForegroundColor = ConsoleColor.Red;
                                Console.WriteLine("Employye not found. Please try again.");
                                Console.ResetColor();
                            }
                            else
                            {
                                employyelist.Remove(employyeToDelete); // Remove the employee from the list
                                Console.ForegroundColor = ConsoleColor.Green;
                                Console.WriteLine($"Employye with ID {employyeid} deleted successfully!");
                                Console.ResetColor();
                                Console.WriteLine("---------------------------- ");
                            }
                        }
                        else
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Please enter a valid number greater than 0 for the Employye ID.");
                            Console.ResetColor();
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid ID. Please enter a valid integer ID or enter 'exit' to cancel.");
                        Console.ResetColor();
                    }
                }
            }
            public override void ShowInformation()
            {
                Console.WriteLine("---------------Employye information---------------");
                if (employyelist.Count == 0)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("No employye available in the list.");
                    Console.ResetColor();
                }
                else
                {
                    for (int i = 0; i < employyelist.Count; i++)
                    {
                        var employee = employyelist[i];
                        Console.WriteLine($"EmployeeID: {employee.EmployeeID}; NameEmployee: {employee.NameEmployee}; AddressEmployye: {employee.AddressEmployye}; AgeEmployee: {employee.AgeEmployee}");
                        Console.ForegroundColor = ConsoleColor.Green;
                        Console.WriteLine("Displayed successfully!");
                        Console.ResetColor();
                        Console.WriteLine("---------------------------- ");
                    }
                }
            }


            public void SearchEmployye(List<Employee> employyelist)
            {
                bool continueSearching = true;

                while (continueSearching)
                {
                    int employeeid = -1;
                    string userInput;

                    Console.WriteLine("--- Search employye ---");
                    Console.Write("Enter the ID of the employee to search (Enter 'exit' to cancel): ");
                    userInput = Console.ReadLine();

                    if (userInput.ToLower() == "exit")
                    {
                        Console.WriteLine("Search canceled.");
                        return;
                    }

                    try
                    {
                        employeeid = int.Parse(userInput);

                        if (employeeid <= 0)
                        {
                            throw new FormatException();
                        }

                        bool found = false;
                        foreach (var employee in employyelist)
                        {
                            if (employee.EmployeeID == employeeid)
                            {
                                Console.WriteLine($"EmployeeID: {employee.EmployeeID}; NameEmployee: {employee.NameEmployee}; AddressEmployye: {employee.AddressEmployye}; AgeEmployee: {employee.AgeEmployee}");
                                found = true;
                                Console.ForegroundColor = ConsoleColor.Green;
                                Console.WriteLine("Employye found successfully.");
                                Console.ResetColor();
                                break;
                            }
                        }

                        if (!found)
                        {
                            Console.ForegroundColor = ConsoleColor.Red;
                            Console.WriteLine("Employye not found. Please try again.");
                            Console.ResetColor();
                        }
                        else
                        {
                            continueSearching = false;
                        }
                    }
                    catch (FormatException)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Invalid ID. Please enter a valid integer ID or enter 'exit' to cancel.");
                        Console.ResetColor();
                    }
                }
            }
        }
        class Program
        {
            public bool isLoggedIn = false;
            private IMenu menu;
            public void Login()
            {
                Console.Clear();
                Console.WriteLine("===================================");
                Console.WriteLine("         Welcome to Login          ");
                Console.WriteLine("===================================");
                Console.WriteLine();

                while (!isLoggedIn)
                {
                    Console.Write("Enter your username: ");
                    string username = Console.ReadLine();

                    Console.Write("Enter your password: ");
                    string password = Console.ReadLine();

                    if (username == "Nghia" && password == "123")
                    {
                        isLoggedIn = true;
                        Console.ForegroundColor = ConsoleColor.Green;
                        Console.WriteLine($"Login successfully!");
                        Console.ResetColor();
                        ShowMenu();
                    }
                    else
                    {
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        Console.WriteLine("Invalid username or password. Please try again.");
                        Console.ResetColor();
                    }
                }
            }

            public void ShowMenu()
            {
                Console.WriteLine("===================================");
                Console.WriteLine("Welcome Menu Person!");
                Console.WriteLine("1. Menu Employye");
                Console.WriteLine("2. Menu Customer");
                Console.WriteLine("3. Exit");
                Console.WriteLine("-----------------------------------");
                Console.Write("Please enter your choice: ");
                int choice;
                while (!int.TryParse(Console.ReadLine(), out choice) || choice <= 0 || choice > 3)
                {
                    Console.WriteLine("Incorrect choice, please re-enter!!");
                    Console.Write("Please enter your choice: ");
                }
                switch (choice)
                {
                    case 1:
                        menu = new EmployyeManagement();
                        menu.showMenu();
                        break;
                    case 2:
                        menu = new CustomerManagement();
                        menu.showMenu();
                        break;
                    case 3:
                        Console.ForegroundColor = ConsoleColor.Yellow;
                        Console.WriteLine("Exiting...");
                        break;
                    default:
                        Console.WriteLine("Incorrect choice, please try again!!");
                        break;
                }
            }
            static void Main(string[] args)
            {
                Program program = new Program();
                program.Login();

                EmployyeManagement employyeManagement =  EmployyeManagement.Instance;
                employyeManagement.showMenu();
                employyeManagement.ShowInformation();

                CustomerManagement customerManagement = CustomerManagement.Instance;
                customerManagement.showMenu();
                customerManagement.ShowInformation();
                
              

            }
        }
    }
}
