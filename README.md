import java.util.*;

class Product {
 int id;
 String name;
 int price;
 int stock;
 Product(int id, String name, int price, int stock) {
 this.id = id;
 this.name = name;
 this.price = price;
 this.stock = stock;
 }
}
public class SmartVendingMachine {
 static Scanner scanner = new Scanner(System.in);
 // Product stock loaded by admin
 static Product[] products = {
 new Product(1, "Chips", 25, 5),
 new Product(2, "Soda", 45, 3),
 new Product(3, "Chocolate", 30, 4),
 new Product(4, "Juice", 40, 2)
 };
 static int totalSales = 0; // Sales analytics tracker
 static int itemsSold = 0; // Total items sold
 public static void main(String[] args) {
 System.out.println("========== Welcome to the Smart Vending Machine
==========");
 String machineStatus = "Working";
 if (!machineStatus.equals("Working")) {
 System.out.println("Machine is currently out of service. Try
later!");
 return;
 }
 boolean continueShopping = true;
 while (continueShopping) {
 displayProducts();
 System.out.print("Enter product number (or 0 to Exit): ");
 int choice = scanner.nextInt();
 if (choice == 0) {
 System.out.println("Exiting... Thank you for visiting!");
 break;
 }
 Product selectedProduct = getProductById(choice);
 if (selectedProduct == null) {
 System.out.println("Invalid product selected!");
 } else if (selectedProduct.stock == 0) {
 System.out.println("Sorry! " + selectedProduct.name + " is out
of stock.");
 } else {
 System.out.printf("You selected: %s (Rs %d)%n",
selectedProduct.name, selectedProduct.price);
 // Payment processing
 System.out.println("Select Payment Method: 1. UPI 2. Card 3.
Cash");
 int payChoice = scanner.nextInt();
 int change = processPayment(selectedProduct.price, payChoice);
 if (change >= 0) { // means payment success
 selectedProduct.stock--;
 totalSales += selectedProduct.price;
 itemsSold++;
 System.out.printf("Dispensing %s...%n",
selectedProduct.name);
 System.out.println("Digital Receipt: Purchase successful!");
 if (change > 0) {
 System.out.printf("Change returned: Rs %d%n", change);
 }
 System.out.println("------------------------------------");
 if (selectedProduct.stock <= 1) {
 System.out.printf("Low stock alert: %s (Only %d left!)
%n",
 selectedProduct.name,
selectedProduct.stock);
 }
 } else {
 System.out.println("Payment failed. Transaction
cancelled.");
 }
 }
 // Ask for feedback (optional)
 System.out.print("Would you like to give feedback? (yes/no): ");
 String feedback = scanner.next();
 if (feedback.equalsIgnoreCase("yes")) {
 scanner.nextLine(); // consume newline
 System.out.print("Enter your feedback: ");
 String userFeedback = scanner.nextLine();
 System.out.println("Thank you for your feedback!");
 }
 System.out.print("Do you want to buy another product? (yes/no): ");
 String again = scanner.next();
 continueShopping = again.equalsIgnoreCase("yes");
 }
 // At exit: Show Sales Analytics
 System.out.println("\n========== Machine Report ==========");
 System.out.println("Total Items Sold: " + itemsSold);
 System.out.println("Total Sales: Rs " + totalSales);
 System.out.println("====================================");
 }
 // Display available products
 static void displayProducts() {
 System.out.println("\nAvailable Products:");
 for (Product p : products) {
 System.out.printf("%d. %s - Rs %d (Stock: %d)%n", p.id, p.name,
p.price, p.stock);
 }
 }
 // Find product by ID
 static Product getProductById(int id) {
 for (Product p : products) {
 if (p.id == id) {
 return p;
 }
 }
 return null;
 }
 // Payment simulation: returns change if cash, -1 if failed
 static int processPayment(int amount, int method) {
 switch (method) {
 case 1:
 System.out.println("UPI Payment of Rs " + amount + "
successful!");
 return 0; // no change in UPI
 case 2:
 System.out.println("Card Payment of Rs " + amount + "
successful!");
 return 0; // no change in Card
 case 3:
 System.out.print("Insert cash (Rs): ");
 int money = scanner.nextInt();
 if (money >= amount) {
 return money - amount; // return change
 } else {
 System.out.println("Insufficient cash provided.");
 return -1;
 }
 default:
 System.out.println("Ivalid payment method!");
 return -1;
 }

 }
}
