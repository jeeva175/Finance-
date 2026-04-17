# Finance-
Mini project 

import java.sql.*;
import java.util.Scanner;

public class StockPortfolio {
    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {
        while (true) {
            System.out.println("\n--- STOCK PORTFOLIO MENU ---");
            System.out.println("1. Add Stock");
            System.out.println("2. View Portfolio");
            System.out.println("3. Update Stock");
            System.out.println("4. Delete Stock");
            System.out.println("5. Exit");
            System.out.print("Enter choice: ");

            int choice = sc.nextInt();

            switch (choice) {
                case 1: addStock(); break;
                case 2: viewPortfolio(); break;
                case 3: updateStock(); break;
                case 4: deleteStock(); break;
                case 5: System.exit(0);
                default: System.out.println("Invalid choice!");
            }
        }
    }

    // Add Stock
    public static void addStock() {
        try {
            Connection con = DBConnection.getConnection();

            System.out.print("Stock Name: ");
            String name = sc.next();

            System.out.print("Quantity: ");
            int qty = sc.nextInt();

            System.out.print("Price: ");
            double price = sc.nextDouble();

            String query = "INSERT INTO portfolio(stock_name, quantity, price) VALUES (?, ?, ?)";
            PreparedStatement ps = con.prepareStatement(query);

            ps.setString(1, name);
            ps.setInt(2, qty);
            ps.setDouble(3, price);

            ps.executeUpdate();
            System.out.println("Stock Added Successfully!");

        } catch (Exception e) {
            System.out.println(e);
        }
    }

    // View Portfolio
    public static void viewPortfolio() {
        try {
            Connection con = DBConnection.getConnection();
            String query = "SELECT * FROM portfolio";
            Statement st = con.createStatement();
            ResultSet rs = st.executeQuery(query);

            System.out.println("\nID | Name | Quantity | Price");

            while (rs.next()) {
                System.out.println(
                        rs.getInt("stock_id") + " | " +
                        rs.getString("stock_name") + " | " +
                        rs.getInt("quantity") + " | " +
                        rs.getDouble("price")
                );
            }

        } catch (Exception e) {
            System.out.println(e);
        }
    }

    // Update Stock
    public static void updateStock() {
        try {
            Connection con = DBConnection.getConnection();

            System.out.print("Enter Stock ID: ");
            int id = sc.nextInt();

            System.out.print("New Quantity: ");
            int qty = sc.nextInt();

            String query = "UPDATE portfolio SET quantity=? WHERE stock_id=?";
            PreparedStatement ps = con.prepareStatement(query);

            ps.setInt(1, qty);
            ps.setInt(2, id);

            ps.executeUpdate();
            System.out.println("Stock Updated!");

        } catch (Exception e) {
            System.out.println(e);
        }
    }

    // Delete Stock
    public static void deleteStock() {
        try {
            Connection con = DBConnection.getConnection();

            System.out.print("Enter Stock ID: ");
            int id = sc.nextInt();

            String query = "DELETE FROM portfolio WHERE stock_id=?";
            PreparedStatement ps = con.prepareStatement(query);

            ps.setInt(1, id);
            ps.executeUpdate();

            System.out.println("Stock Deleted!");

        } catch (Exception e) {
            System.out.println(e);
        }
    }
}