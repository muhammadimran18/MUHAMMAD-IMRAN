# MUHAMMAD-IMRAN
SUPERMARKET BILLING SYSTEM
import java.util.*;

// Class to represent a product
class Product {
    private int id;
    private String name;
    private double price;

    public Product(int id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }
}

// Class to represent a customer
class Customer {
    private int id;
    private String name;

    public Customer(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}

// Class to represent a supermarket billing system
class BillingSystem {
    private List<Product> products;
    private List<Customer> customers;
    private Map<Integer, Integer> cart; // Product ID -> Quantity

    public BillingSystem() {
        products = new ArrayList<>();
        customers = new ArrayList<>();
        cart = new HashMap<>();
    }

    // Add a product to the supermarket
    public void addProduct(Product product) {
        products.add(product);
    }

    // Add a customer to the supermarket
    public void addCustomer(Customer customer) {
        customers.add(customer);
    }

    // Display all available products
    public void displayProducts() {
        System.out.println("Available Products:");
        for (Product product : products) {
            System.out.println(product.getId() + ". " + product.getName() + " - $" + product.getPrice());
        }
    }

    // Display all customers
    public void displayCustomers() {
        System.out.println("Registered Customers:");
        for (Customer customer : customers) {
            System.out.println(customer.getId() + ". " + customer.getName());
        }
    }

    // Add a product to the cart
    public void addToCart(int productId, int quantity) {
        if (quantity <= 0) {
            System.out.println("Quantity should be greater than zero.");
            return;
        }
        if (!productExists(productId)) {
            System.out.println("Product with ID " + productId + " does not exist.");
            return;
        }
        if (cart.containsKey(productId)) {
            cart.put(productId, cart.get(productId) + quantity);
        } else {
            cart.put(productId, quantity);
        }
        System.out.println(quantity + " " + getProductById(productId).getName() + "(s) added to cart.");
    }

    // Remove a product from the cart
    public void removeFromCart(int productId, int quantity) {
        if (!productExists(productId)) {
            System.out.println("Product with ID " + productId + " does not exist.");
            return;
        }
        if (!cart.containsKey(productId) || cart.get(productId) == 0) {
            System.out.println("No such product in cart.");
            return;
        }
        int currentQuantity = cart.get(productId);
        if (quantity >= currentQuantity) {
            cart.remove(productId);
        } else {
            cart.put(productId, currentQuantity - quantity);
        }
        System.out.println(quantity + " " + getProductById(productId).getName() + "(s) removed from cart.");
    }

    // Display items in the cart
    public void displayCart() {
        System.out.println("Items in Cart:");
        for (Map.Entry<Integer, Integer> entry : cart.entrySet()) {
            int productId = entry.getKey();
            int quantity = entry.getValue();
            Product product = getProductById(productId);
            System.out.println(product.getName() + " - Quantity: " + quantity + " - Total Price: $" + (product.getPrice() * quantity));
        }
    }

    // Checkout - calculate total bill
    public double checkout() {
        double totalBill = 0.0;
        for (Map.Entry<Integer, Integer> entry : cart.entrySet()) {
            int productId = entry.getKey();
            int quantity = entry.getValue();
            Product product = getProductById(productId);
            totalBill += product.getPrice() * quantity;
        }
        cart.clear(); // Clear the cart after checkout
        return totalBill;
    }

    // Helper method to check if product exists
    private boolean productExists(int productId) {
        for (Product product : products) {
            if (product.getId() == productId) {
                return true;
            }
        }
        return false;
    }

    // Helper method to get product by ID
    private Product getProductById(int productId) {
        for (Product product : products) {
            if (product.getId() == productId) {
                return product;
            }
        }
        return null;
    }
}

// Main class to test the supermarket billing system
public class SupermarketBillingSystem {
    public static void main(String[] args) {
        BillingSystem supermarket = new BillingSystem();

        // Adding products
        supermarket.addProduct(new Product(1, "Bread", 2.5));
        supermarket.addProduct(new Product(2, "Milk", 3.0));
        supermarket.addProduct(new Product(3, "Eggs", 1.75));

        // Adding customers
        supermarket.addCustomer(new Customer(1, "Alice"));
        supermarket.addCustomer(new Customer(2, "Bob"));

        // Display available products and customers
        supermarket.displayProducts();
        System.out.println();
        supermarket.displayCustomers();
        System.out.println();

        // Simulating customer adding products to cart
        supermarket.addToCart(1, 2); // Adding 2 breads to cart
        supermarket.addToCart(2, 1); // Adding 1 milk to cart
        supermarket.addToCart(4, 1); // Trying to add non-existent product

        // Display cart and total bill
        System.out.println();
        supermarket.displayCart();
        System.out.println("Total Bill: $" + supermarket.checkout());
    }
}
