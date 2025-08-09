import java.util.*;
import javax.swing.*;
import java.io.*;

class UtilityBill {
    String utilityType;
    double amountDue;

    public void displayBill() {
        System.out.println("Utility: " + utilityType + ", Amount Due: RM" + amountDue);
    }
}

class Customer extends UtilityBill {
    String name;
    String accountNo;

    public Customer(String name, String accountNo, String utilityType, double amountDue) {
        this.name = name;
        this.accountNo = accountNo;
        this.utilityType = utilityType;
        this.amountDue = amountDue;
    }

    @Override
    public void displayBill() {
        System.out.println("Name: " + name + ", Account No: " + accountNo + ", Utility: " + utilityType
                + ", Due: RM" + amountDue);
    }

    @Override
    public String toString() {
        return "Name: " + name + ", Account No: " + accountNo + ", Type: " + utilityType + ", Due: RM"
                + String.format("%.2f", amountDue);
    }
}

public class PayUtilitiesApp {
    static ArrayList<Customer> customerList = new ArrayList<>();
    static final String FILE_NAME = "customers.txt";

    public static void main(String[] args) {
        loadFromFile();

        // User Role Selection
        String[] userTypes = { "Admin", "Customer", "Exit" };
        int userType = JOptionPane.showOptionDialog(null, "Login As:", "User Role",
                JOptionPane.DEFAULT_OPTION, JOptionPane.INFORMATION_MESSAGE, null, userTypes, userTypes[0]);

        if (userType == 2 || userType == JOptionPane.CLOSED_OPTION) {
            JOptionPane.showMessageDialog(null, "App Closed.");
            return;
        }

        boolean isAdmin = userType == 0;

        if (isAdmin) {
            String pass = JOptionPane.showInputDialog("Enter Admin Password:");
            if (!"admin123".equals(pass)) {
                JOptionPane.showMessageDialog(null, "Wrong password. Access denied.");
                return;
            }
        }

        JOptionPane.showMessageDialog(null, "Welcome to Pay Utilities Online App!");

        boolean running = true;
        while (running) {
            String[] utilityOptions = { "Water", "Electric", "Logout" };
            int utilityChoice = JOptionPane.showOptionDialog(null, "Choose Utility Type:", "Utility Selection",
                    JOptionPane.DEFAULT_OPTION, JOptionPane.INFORMATION_MESSAGE, null, utilityOptions, utilityOptions[0]);

            if (utilityChoice == 2 || utilityChoice == JOptionPane.CLOSED_OPTION) {
                running = false;
                continue;
            }

            String utilityType = utilityOptions[utilityChoice];
            boolean activityLoop = true;

            while (activityLoop) {
                String[] activityOptions;
                if (isAdmin) {
                    activityOptions = new String[] { "New Registry", "View", "Edit", "Delete", "Back" };
                } else {
                    activityOptions = new String[] { "View Bill", "Pay Bill", "Back" };
                }

                int choice = JOptionPane.showOptionDialog(null, "Select Activity:", "Menu",
                        JOptionPane.DEFAULT_OPTION, JOptionPane.INFORMATION_MESSAGE, null, activityOptions,
                        activityOptions[0]);

                if (isAdmin) {
                    switch (choice) {
                        case 0:
                            addCustomer(utilityType);
                            break;
                        case 1:
                            viewCustomers(utilityType);
                            break;
                        case 2:
                            editCustomer();
                            break;
                        case 3:
                            deleteCustomer();
                            break;
                        case 4:
                        case JOptionPane.CLOSED_OPTION:
                            activityLoop = false;
                            break;
                        default:
                            JOptionPane.showMessageDialog(null, "Invalid option!");
                            break;
                    }
                } else { // Customer
                    switch (choice) {
                        case 0:
                            customerViewBill(utilityType);
                            break;
                        case 1:
                            customerPayBill(utilityType);
                            break;
                        case 2:
                        case JOptionPane.CLOSED_OPTION:
                            activityLoop = false;
                            break;
                        default:
                            JOptionPane.showMessageDialog(null, "Invalid option!");
                            break;
                    }
                }
            }
        }

        JOptionPane.showMessageDialog(null, "Thank you for using the app. Logged out.");
    }

    static void addCustomer(String utilityType) {
        String name = JOptionPane.showInputDialog("Enter Customer Name:");
        String accNo = JOptionPane.showInputDialog("Enter Account Number:");
        double due;
        try {
            due = Double.parseDouble(JOptionPane.showInputDialog("Enter Amount Due:"));
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(null, "Invalid amount.");
            return;
        }

        Customer c = new Customer(name, accNo, utilityType, due);
        customerList.add(c);
        saveToFile();
        JOptionPane.showMessageDialog(null, "Customer registered successfully!");
    }

    static void viewCustomers(String utilityType) {
        loadFromFile();
        StringBuilder sb = new StringBuilder();
        sb.append("--- ").append(utilityType).append(" Customers ---\n");
        for (Customer c : customerList) {
            if (c.utilityType.equalsIgnoreCase(utilityType)) {
                sb.append(c.toString()).append("\n");
            }
        }
        JTextArea area = new JTextArea(sb.toString());
        area.setEditable(false);
        JScrollPane scroll = new JScrollPane(area);
        scroll.setPreferredSize(new java.awt.Dimension(400, 250));
        JOptionPane.showMessageDialog(null, scroll, "Customer List", JOptionPane.INFORMATION_MESSAGE);
    }

    static void editCustomer() {
        String acc = JOptionPane.showInputDialog("Enter Account Number to Edit:");
        for (Customer c : customerList) {
            if (c.accountNo.equals(acc)) {
                c.name = JOptionPane.showInputDialog("Enter New Name:");
                try {
                    c.amountDue = Double.parseDouble(JOptionPane.showInputDialog("Enter New Amount Due:"));
                } catch (NumberFormatException e) {
                    JOptionPane.showMessageDialog(null, "Invalid amount.");
                    return;
                }
                saveToFile();
                JOptionPane.showMessageDialog(null, "Customer updated.");
                return;
            }
        }
        JOptionPane.showMessageDialog(null, "Customer not found.");
    }

    static void deleteCustomer() {
        String acc = JOptionPane.showInputDialog("Enter Account Number to Delete:");
        Iterator<Customer> it = customerList.iterator();
        while (it.hasNext()) {
            if (it.next().accountNo.equals(acc)) {
                it.remove();
                saveToFile();
                JOptionPane.showMessageDialog(null, "Customer deleted.");
                return;
            }
        }
        JOptionPane.showMessageDialog(null, "Customer not found.");
    }

    static void customerViewBill(String utilityType) {
        String acc = JOptionPane.showInputDialog("Enter Your Account Number:");
        for (Customer c : customerList) {
            if (c.accountNo.equals(acc) && c.utilityType.equalsIgnoreCase(utilityType)) {
                JOptionPane.showMessageDialog(null, c.toString());
                return;
            }
        }
        JOptionPane.showMessageDialog(null, "Bill not found.");
    }

    static void customerPayBill(String utilityType) {
        String acc = JOptionPane.showInputDialog("Enter Your Account Number:");
        for (Customer c : customerList) {
            if (c.accountNo.equals(acc) && c.utilityType.equalsIgnoreCase(utilityType)) {
                try {
                    double pay = Double.parseDouble(JOptionPane.showInputDialog("Enter Payment Amount:"));
                    c.amountDue -= pay;
                    saveToFile();
                    JOptionPane.showMessageDialog(null, "Payment successful. New amount due: RM"
                            + String.format("%.2f", c.amountDue));
                } catch (NumberFormatException e) {
                    JOptionPane.showMessageDialog(null, "Invalid amount.");
                }
                return;
            }
        }
        JOptionPane.showMessageDialog(null, "Account not found.");
    }

    static void saveToFile() {
        try {
            FileWriter writer = new FileWriter(FILE_NAME, false);
            for (Customer c : customerList) {
                writer.write(c.toString() + "\n");
            }
            writer.close();
        } catch (IOException e) {
            JOptionPane.showMessageDialog(null, "Error writing to file.");
        }
    }

    static void loadFromFile() {
        customerList.clear();
        try {
            File file = new File(FILE_NAME);
            if (!file.exists()) return;

            Scanner scanner = new Scanner(file);
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine().trim();
                if (line.isEmpty()) continue;

                String[] parts = line.split(", ");
                String name = parts[0].split(": ")[1];
                String accNo = parts[1].split(": ")[1];
                String type = parts[2].split(": ")[1];
                double due = Double.parseDouble(parts[3].split(": RM")[1]);

                customerList.add(new Customer(name, accNo, type, due));
            }
            scanner.close();
        } catch (IOException | ArrayIndexOutOfBoundsException | NumberFormatException e) {
            JOptionPane.showMessageDialog(null, "Error reading from file.");
        }
    }
}
