# Bank-management-system
oop java project usiing main concepts in it.


Person class(abstract class):
public abstract class Person {
    protected String name;
    protected String id;
    protected String contactInfo;

    public Person(String name, String id, String contactInfo) {
        this.name = name;
        this.id = id;
        this.contactInfo = contactInfo;
    }

    public String getName() {
        return name;
    }

    public String getId() {
        return id;
    }

    public abstract void displayDetails();
}
Customer class:

import java.util.ArrayList;
import java.util.List;

public class Customer extends Person implements AccountManagement, LoanManagement {
    private List<Account> accounts;
    private List<Loan> loans;
    private String pinCode;

    public Customer(String name, String id, String contactInfo, String pinCode) {
        super(name, id, contactInfo);
        this.accounts = new ArrayList<>();
        this.loans = new ArrayList<>();
        this.pinCode = pinCode;
    }

    public boolean verifyPin(String pin) {
        return this.pinCode.equals(pin);
    }

    @Override
    public void openAccount(Account account) {
        accounts.add(account);
        System.out.println("Account opened: " + account);
    }

    @Override
    public void closeAccount(String accountNumber) {
        accounts.removeIf(acc -> acc.getAccountNumber().equals(accountNumber));
        System.out.println("Account closed: " + accountNumber);
    }

    @Override
    public void applyForLoan(Loan loan) {
        loans.add(loan);
        System.out.println("Loan application submitted: " + loan);
    }

    @Override
    public void approveLoan(String loanId) {

    }

    @Override
    public void listAccounts() {
        if (accounts.isEmpty()) {
            System.out.println("No accounts found.");
            return;
        }
        for (Account account : accounts) {
            System.out.println(account);
        }
    }

    @Override
    public void viewAllLoans() {
        for (Loan loan : loans) {
            System.out.println(loan);
        }
    }

    @Override
    public void viewLoans() {

    }

    @Override
    public void displayDetails() {
        System.out.println("Customer Details: Name = " + name + ", ID = " + id + ", Contact = " + contactInfo);
    }
}
Employee class:
public class Employee extends Person implements CustomerManagement {
    private double salary;
    private double bonus;
    private String pinCode;

    public Employee(String name, String id, String contactInfo, double salary, String pinCode) {
        super(name, id, contactInfo);
        this.salary = salary;
        this.bonus = 0;
        this.pinCode = pinCode;
    }

    public boolean verifyPin(String pin) {
        return this.pinCode.equals(pin);
    }

    @Override
    public void removeCustomer(String customerId) {
        System.out.println("Customer removed: ID = " + customerId);
    }

    @Override
    public void editCustomer(String customerId, String newName, String newContactInfo) {
        System.out.println("Customer edited: ID = " + customerId + ", New Name = " + newName + ", New Contact Info = " + newContactInfo);
    }

    @Override
    public void searchCustomer(String customerId) {
        System.out.println("Searching for customer: ID = " + customerId);
    }

    @Override
    public void displayDetails() {
        System.out.println("Employee Details: Name = " + name + ", ID = " + id + ", Contact = " + contactInfo + ", Salary = " + salary);
    }
Accounts class:
public class Account {
    private String accountNumber;
    private double balance;
    private double interestRate;

    public Account(String accountNumber, double balance, double interestRate) {
        this.accountNumber = accountNumber;
        this.balance = balance;
        this.interestRate = interestRate;
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    @Override
    public String toString() {
        return "Account[AccountNumber=" + accountNumber + ", Balance=" + balance + "]";
    }
}
Bank class:
import java.util.ArrayList;
import java.util.List;

public class Bank {
    private List<Customer> customers;
    private List<Employee> employees;
    private List<Loan> loanRequests;
    private List<Customer> pendingCustomers;

    public Bank() {
        customers = new ArrayList<>();
        employees = new ArrayList<>();
        loanRequests = new ArrayList<>();
        pendingCustomers = new ArrayList<>();
    }

    public List<Customer> getCustomers() {
        return customers;
    }

    public List<Employee> getEmployees() {
        return employees;
    }

    public List<Customer> getPendingCustomers() {
        return pendingCustomers;
    }

    public void addPendingCustomer(Customer customer) {
        pendingCustomers.add(customer);
    }

    public void addCustomer(Customer customer) {
        customers.add(customer);
    }

    public void addEmployee(Employee employee) {
        employees.add(employee);
    }

    public void removeEmployee(String employeeId) {
        employees.removeIf(emp -> emp.getId().equals(employeeId));
    }

    public Employee findEmployeeById(String employeeId) {
        for (Employee emp : employees) {
            if (emp.getId().equals(employeeId)) {
                return emp;
            }
        }
        return null;
    }

    public void approveLoan(String loanId) {
        for (Loan loan : loanRequests) {
            if (loanId.equals(loan.getLoanID())) {
                loan.approveLoan();
                return;
            }
        }
    }

    public void displayAllLoans() {
        for (Loan loan : loanRequests) {
            System.out.println(loan);
        }
    }

    public Customer findCustomerById(String customerId) {
    return customers.stream().filter(c -> c.getId().equals(customerId)).findFirst().orElse(null);}
}


Admin class:
public class Admin extends Person implements LoanManagement {
    private Bank bank;
    private String pinCode;

    public Admin(String name, String id, String contactInfo, String pinCode, Bank bank) {
        super(name, id, contactInfo);
        this.pinCode = pinCode;
        this.bank = bank;
    }

    public boolean verifyPin(String pin) {
        return this.pinCode.equals(pin);
    }

    public void hireEmployee(String name, String contactInfo, double salary, String pinCode) {
        String employeeId = "E" + String.format("%03d", bank.getEmployees().size() + 1);
        Employee newEmployee = new Employee(name, employeeId, contactInfo, salary, pinCode);
        bank.addEmployee(newEmployee);
        System.out.println("Employee hired: " + name);
    }

    public void fireEmployee(String employeeId) {
        bank.removeEmployee(employeeId);
        System.out.println("Employee fired: ID = " + employeeId);
    }

    public void manageSalary(String employeeId, double newSalary) {
        Employee employee = bank.findEmployeeById(employeeId);
        if (employee != null) {
            System.out.println("Updated salary for employee: " + employee.getName());
        } else {
            System.out.println("Employee not found.");
        }
    }

    public void addBonus(String employeeId, double bonus) {
        Employee employee = bank.findEmployeeById(employeeId);
        if (employee != null) {
            System.out.println("Bonus added to employee: " + employee.getName());
        } else {
            System.out.println("Employee not found.");
        }
    }

    @Override
    public void applyForLoan(Loan loan) {
        
    }

    @Override
    public void approveLoan(String loanId) {
        bank.approveLoan(loanId);
        System.out.println("Loan approved: ID = " + loanId);
    }

    @Override
    public void viewAllLoans() {
        bank.displayAllLoans();
    }

    @Override
    public void viewLoans() {

    }

    @Override
    public void displayDetails() {
        System.out.println("Admin Details: Name = " + name + ", ID = " + id);
    }
}
Loan class:
public class Loan {
    private String loanID;
    private double amount;
    private double interestRate;
    private int tenure;
    private boolean approved;

    public Loan(String loanID, double amount, double interestRate, int tenure) {
        this.loanID = loanID;
        this.amount = amount;
        this.interestRate = interestRate;
        this.tenure = tenure;
        this.approved = false;
    }

    public String getLoanID() {
        return loanID;
    }

    public void approveLoan() {
        approved = true;
    }

    @Override
    public String toString() {
        return "Loan[LoanID=" + loanID + ", Amount=" + amount + ", InterestRate=" + interestRate + ", Tenure=" + tenure + ", Approved=" + approved + "]";
    }
}
Account management class(interface):

public interface AccountManagement {
    void openAccount(Account account);
    void closeAccount(String accountNumber);
    void listAccounts();
}
Customer management class(interface):
public interface CustomerManagement {
    void removeCustomer(String customerId);
    void editCustomer(String customerId, String newName, String newContactInfo);
    void searchCustomer(String customerId);
}
Loan management clas(interface):
public interface LoanManagement {
    void applyForLoan(Loan loan);
    void approveLoan(String loanId);
    void viewAllLoans();

    void viewLoans();
}



BankManagementSystem class(main class):
import java.util.List;
import java.util.Scanner;
public class BankManagementSystem {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Bank bank = new Bank();
        Admin admin = new Admin("Admin", "A001", "admin@bank.com", "admin123", bank);
        System.out.println("\n\t\t\t\t\t\t\t\t**************************************");
        System.out.println("\t\t\t\t\t\t\t\tWelcome to the Bank Management System");
        System.out.println("\t\t\t\t\t\t\t\t**************************************\n\n");
        while (true) {
            System.out.println("************************");
            System.out.println("1.Sign Up as Customer  ");
            System.out.println("2.Login as Customer    ");
            System.out.println("3.Login as Employee    ");
            System.out.println("4.Login as Admin       ");
            System.out.println("5.Exit                 ");
            System.out.println("************************");
            System.out.println("==================");
            System.out.print("Enter your choice: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); 

            switch (choice) {
                case 1:
                    signUpCustomer(scanner, bank);
                    break;
                case 2:
                    customerLogin(scanner, bank);
                    break;
                case 3:
                    employeeLogin(scanner, bank);
                    break;
                case 4:
                    adminLogin(scanner, admin);
                    break;
                case 5:
                    System.out.println("****************************");
                    System.out.println("Exiting the system. Goodbye!");
                    System.out.println("****************************");
                    scanner.close();
                    return;
                default:
                    System.out.println("================================");
                    System.out.println("\nInvalid choice. Please try again.");
            }
        }
    }

    private static void signUpCustomer(Scanner scanner, Bank bank) {
        System.out.println("\nCustomer Sign-Up:");
        System.out.print("Enter Your Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Contact Info: ");
        String contactInfo = scanner.nextLine();
        System.out.print("Set Your PIN Code: ");
        String pinCode = scanner.nextLine();

        String customerId = "C" + String.format("%03d", bank.getCustomers().size() + 1);
        Customer newCustomer = new Customer(name, customerId, contactInfo, pinCode);

        System.out.println("Your Customer ID is: " + customerId + ". Please wait for employee approval to activate your account.");
        bank.addPendingCustomer(newCustomer);
    }

    private static void customerLogin(Scanner scanner, Bank bank) {
        System.out.print("Enter Customer ID: ");
        String customerId = scanner.nextLine();
        System.out.print("Enter PIN Code: ");
        String customerPin = scanner.nextLine();

        Customer customer = bank.findCustomerById(customerId);
        if (customer != null && customer.verifyPin(customerPin)) {
            System.out.println("Login successful! Welcome, " + customer.getName());
            CustomerMenu.display(scanner, customer);
        } else {
            System.out.println("Invalid Customer ID or PIN.");
        }
    }

    private static void employeeLogin(Scanner scanner, Bank bank) {
        System.out.print("Enter Employee ID: ");
        String employeeId = scanner.nextLine();
        System.out.print("Enter PIN Code: ");
        String employeePin = scanner.nextLine();

        Employee employee = bank.findEmployeeById(employeeId);
        if (employee != null && employee.verifyPin(employeePin)) {
            System.out.println("Login successful! Welcome, " + employee.getName());
            EmployeeMenu.display(scanner, employee, bank);
        } else {
            System.out.println("Invalid Employee ID or PIN.");
        }
    }

    private static void adminLogin(Scanner scanner, Admin admin) {
        System.out.print("Enter Admin PIN: ");
        String adminPin = scanner.nextLine();

        if (admin.verifyPin(adminPin)) {
            System.out.println("Login successful! Welcome, Admin.");
            AdminMenu.display(scanner, admin);
        } else {
            System.out.println("Invalid Admin PIN.");
        }
    }
}

class CustomerMenu {
    public static void display(Scanner scanner, Customer customer) {
        while (true) {
            System.out.println("\nCustomer Menu:");
            System.out.println("1. Open Account");
            System.out.println("2. Close Account");
            System.out.println("3. View Accounts");
            System.out.println("4. Apply for Loan");
            System.out.println("5. Logout");
            System.out.print("Enter your choice: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // consume newline

            switch (choice) {
                case 1:
                    openAccount(scanner, customer);
                    break;
                case 2:
                    closeAccount(scanner, customer);
                    break;
                case 3:
                    viewAccounts(customer);
                    break;
                case 4:
                    applyForLoan(scanner, customer);
                    break;
                case 5:
                    System.out.println("Logging out...");
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }

    private static void openAccount(Scanner scanner, Customer customer) {
        System.out.print("Enter Account Number: ");
        String accNum = scanner.nextLine();
        System.out.print("Enter Initial Deposit: ");
        double deposit = scanner.nextDouble();
        System.out.print("Enter Interest Rate: ");
        double rate = scanner.nextDouble();

        customer.openAccount(new Account(accNum, deposit, rate));
        System.out.println("Account opened successfully!");
    }

    private static void closeAccount(Scanner scanner, Customer customer) {
        System.out.print("Enter Account Number to Close: ");
        String accNum = scanner.nextLine();
        customer.closeAccount(accNum);
        System.out.println("Account closed successfully!");
    }

    private static void viewAccounts(Customer customer) {
        System.out.println("Viewing all accounts:");
        customer.listAccounts(); // Assuming listAccounts() is implemented in the Customer class
    }

    private static void applyForLoan(Scanner scanner, Customer customer) {
        System.out.print("Enter Loan Amount: ");
        double loanAmount = scanner.nextDouble();
        System.out.print("Enter Interest Rate: ");
        double loanRate = scanner.nextDouble();
        System.out.print("Enter Tenure (months): ");
        int tenure = scanner.nextInt();

        String loanId = "L" + System.currentTimeMillis(); // Unique Loan ID
        Loan loan = new Loan(loanId, loanAmount, loanRate, tenure);
        customer.applyForLoan(loan);
        System.out.println("Loan application submitted. Loan ID: " + loanId);
    }
}

class EmployeeMenu {
    public static void display(Scanner scanner, Employee employee, Bank bank) {
        while (true) {
            System.out.println("\nEmployee Menu:");
            System.out.println("1. Approve Customer Sign-Ups");
            System.out.println("2. Remove Customer");
            System.out.println("3. Edit Customer");
            System.out.println("4. Search Customer");
            System.out.println("5. Logout");
            System.out.print("Enter your choice: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // consume newline

            switch (choice) {
                case 1:
                    approveCustomerSignUps(scanner, bank);
                    break;
                case 2:
                    removeCustomer(scanner, employee);
                    break;
                case 3:
                    editCustomer(scanner, employee);
                    break;
                case 4:
                    searchCustomer(scanner, employee);
                    break;
                case 5:
                    System.out.println("Logging out...");
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }

    private static void approveCustomerSignUps(Scanner scanner, Bank bank) {
        System.out.println("Pending Customer Approvals:");
        List<Customer> pendingCustomers = bank.getPendingCustomers();

        if (pendingCustomers.isEmpty()) {
            System.out.println("No pending customer sign-ups.");
            return;
        }

        for (Customer pending : pendingCustomers) {
            System.out.println("Customer ID: " + pending.getId() + ", Name: " + pending.getName());
            System.out.print("Approve this customer? (yes/no): ");
            String approval = scanner.nextLine();
            if (approval.equalsIgnoreCase("yes")) {
                bank.addCustomer(pending);
                System.out.println("Customer approved: " + pending.getName());
            } else {
                System.out.println("Customer not approved: " + pending.getName());
            }
        }
        pendingCustomers.clear();
    }

    private static void removeCustomer(Scanner scanner, Employee employee) {
        System.out.print("Enter Customer ID to Remove: ");
        String customerId = scanner.nextLine();
        employee.removeCustomer(customerId);
        System.out.println("Customer removed successfully.");
    }

    private static void editCustomer(Scanner scanner, Employee employee) {
        System.out.print("Enter Customer ID to Edit: ");
        String customerId = scanner.nextLine();
        System.out.print("Enter New Name: ");
        String newName = scanner.nextLine();
        System.out.print("Enter New Contact Info: ");
        String newContactInfo = scanner.nextLine();

        employee.editCustomer(customerId, newName, newContactInfo);
        System.out.println("Customer details updated successfully.");
    }

    private static void searchCustomer(Scanner scanner, Employee employee) {
        System.out.print("Enter Customer ID to Search: ");
        String customerId = scanner.nextLine();
        employee.searchCustomer(customerId);
    }
}
class AdminMenu {
    public static void display(Scanner scanner, Admin admin) {
        while (true) {
            System.out.println("\nAdmin Menu:");
            System.out.println("1. Hire Employee");
            System.out.println("2. Fire Employee");
            System.out.println("3. Manage Salary");
            System.out.println("4. Add Bonus");
            System.out.println("5. Approve Loan");
            System.out.println("6. View Loans");
            System.out.println("7. Logout");
            System.out.print("Enter your choice: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // consume newline

            switch (choice) {
                case 1:
                    hireEmployee(scanner, admin);
                    break;
                case 2:
                    fireEmployee(scanner, admin);
                    break;
                case 3:
                    manageSalary(scanner, admin);
                    break;
                case 4:
                    addBonus(scanner, admin);
                    break;
                case 5:
                    approveLoan(scanner, admin);
                    break;
                case 6:
                    viewLoans(admin);
                    break;
                case 7:
                    System.out.println("Logging out...");
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }

    private static void hireEmployee(Scanner scanner, Admin admin) {
        System.out.print("Enter Employee Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Contact Info: ");
        String contactInfo = scanner.nextLine();
        System.out.print("Set PIN Code: ");
        String pinCode = scanner.nextLine();
        System.out.print("Enter Salary: ");
        double salary = scanner.nextDouble();

        admin.hireEmployee(name, contactInfo, salary, pinCode);
        System.out.println("Employee hired successfully!");
    }

    private static void fireEmployee(Scanner scanner, Admin admin) {
        System.out.print("Enter Employee ID to Fire: ");
        String employeeId = scanner.nextLine();
        admin.fireEmployee(employeeId);
        System.out.println("Employee fired successfully!");
    }

    private static void manageSalary(Scanner scanner, Admin admin) {
        System.out.print("Enter Employee ID to Manage Salary: ");
        String employeeId = scanner.nextLine();
        System.out.print("Enter New Salary: ");
        double newSalary = scanner.nextDouble();

        admin.manageSalary(employeeId, newSalary);
        System.out.println("Salary updated successfully!");
    }

    private static void addBonus(Scanner scanner, Admin admin) {
        System.out.print("Enter Employee ID to Add Bonus: ");
        String employeeId = scanner.nextLine();
        System.out.print("Enter Bonus Amount: ");
        double bonus = scanner.nextDouble();

        admin.addBonus(employeeId, bonus);
        System.out.println("Bonus added successfully!");
    }

    private static void approveLoan(Scanner scanner, Admin admin) {
        System.out.print("Enter Loan ID to Approve: ");
        String loanId = scanner.nextLine();
        admin.approveLoan(loanId);
        System.out.println("Loan approved successfully!");
    }

    private static void viewLoans(Admin admin) {
        System.out.println("Viewing all loans:");
        admin.viewLoans();
    }
}

