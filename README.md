
import tkinter as tk
from tkinter import messagebox # Import messagebox module from the tkinter library
from PIL import Image, ImageTk
import os

class McDonaldsApp:    # Class to create the McDonald's application
    def __init__(self, root): # Constructor to initialize the class
        """
        Initializes the McDonald's App with the given root window.

        Args:
            root (Tk): The root window of the application.

        Attributes:
            root (Tk): The root window of the application.
            cart (list): A list to store items added to the cart.
            total_price (float): The total price of items in the cart.
            customer_first_name (str): The first name of the customer.
            customer_last_name (str): The last name of the customer.
            customer_email (str): The email address of the customer.

        Methods:
            create_welcome_window: Initializes and displays the welcome window.
        """
        self.root = root
        self.root.title("McDonald's App")
        self.cart = []
        self.total_price = 0.0
        self.customer_first_name = ""
        self.customer_last_name = ""
        self.customer_email = ""

        self.create_welcome_window()

    def create_welcome_window(self): #Method to create the welcome Window
        """
        Creates the welcome window for the application.
        This method clears the current window and sets up the welcome window with
        the McDonald's logo and a welcome message. It also includes a button to 
        start an order.
        The method attempts to load and display the McDonald's logo from a specified
        file path. If the image file is not found, an error message is displayed.
        Raises:
            FileNotFoundError: If the image file specified by `image_path` is not found.
        """
        self.clear_window()
        
        # Load and display the McDonald's logo
        try:
            # Use an absolute path or ensure the relative path is correct
            image_path = "/Users/user/Library/Mobile Documents/com~apple~CloudDocs/BRYAN SDEV 140/M08/mcdonalds_logo.png"
            if not os.path.exists(image_path):
                raise FileNotFoundError(f"Image file '{image_path}' not found.")
            
            logo_image = Image.open(image_path) # Open the image file
            logo_image = logo_image.resize((200, 200), Image.LANCZOS) # Resize the image
            logo_photo = ImageTk.PhotoImage(logo_image) # Create a PhotoImage object
            logo_label = tk.Label(self.root, image=logo_photo)
            logo_label.image = logo_photo  # Keep a reference to avoid garbage collection
            logo_label.pack(pady=20)
        except FileNotFoundError as e:
            messagebox.showerror("Error", str(e))

        welcome_label = tk.Label(self.root, text="Welcome to McDonald's", font=("Helvetica", 16))
        welcome_label.pack(pady=20) # Pack the welcome label

        tour_button = tk.Button(self.root, text="Start an order", command=self.create_menu_window)
        tour_button.pack(pady=10)

    def create_menu_window(self):
        """
        Creates the menu window for the application.
        This method clears the current window and sets up the menu window with 
        various buttons and labels. It includes a back button to return to the 
        welcome window, a label displaying "Menu", and buttons for each menu 
        item to add them to the cart. Each menu item button displays the item 
        name and price. Additionally, there is a next button to proceed to the 
        order window.
        Menu items:
            - Burger: $5.99
            - Fries: $2.99
            - Coke: $1.49
            - Salad: $4.99
        The buttons for menu items call the `add_to_cart` method with the item 
        name and price as arguments.
        The back button calls the `create_welcome_window` method.
        The next button calls the `create_order_window` method.
        """
        self.clear_window()
        
        back_button = tk.Button(self.root, text="Back", command=self.create_welcome_window)
        back_button.pack(anchor='nw', padx=10, pady=10)

        menu_label = tk.Label(self.root, text="Menu", font=("Helvetica", 16))
        menu_label.pack(pady=20)

        menu_items = [("Burger", 5.99), ("Fries", 2.99), ("Coke", 1.49), ("Salad", 4.99)]
        for item, price in menu_items:
            button = tk.Button(self.root, text=f"Add {item} (${price:.2f}) to Cart", command=lambda item=item, price=price: self.add_to_cart(item, price))
            button.pack(pady=5)

        next_button = tk.Button(self.root, text="Next", command=self.create_order_window)
        next_button.pack(pady=10)

    def add_to_cart(self, item, price): # Method to add itmes to the cart
        """
        Adds an item to the cart and updates the total price.

        Args:
            item (str): The name of the item to add to the cart.
            price (float): The price of the item to add to the cart.

        Returns:
            None
        """
        self.cart.append((item, price)) # Append the item and price to the cart
        self.total_price += price
        messagebox.showinfo("Cart", f"{item} added to cart. Total price: ${self.total_price:.2f}")

    def create_order_window(self): # Method to create the order window
        """
        Creates the order window where the user can review their cart, enter customer information, 
        and choose to place an order or go back to the main menu.
        This method performs the following actions:
        - Clears the current window.
        - Displays a "Back" button to return to the main menu.
        - Displays a label asking if the user wants to place an order.
        - Lists the items in the user's cart along with their prices.
        - Displays the total price of the items in the cart.
        - Provides entry fields for users to input their first name, last name, and email address.
        - Displays "Yes" and "No" buttons for the user to confirm or cancel the order.
        Attributes:
            self.root (tk.Tk): The root window of the application.
            self.cart (list): A list of tuples containing items and their prices.
            self.total_price (float): The total price of the items in the cart.
            self.first_name_entry (tk.Entry): Entry widget for the user's first name.
            self.last_name_entry (tk.Entry): Entry widget for the user's last name.
            self.email_entry (tk.Entry): Entry widget for the user's email address.
        """
        self.clear_window()
        
        back_button = tk.Button(self.root, text="Back", command=self.create_menu_window)
        back_button.pack(anchor='nw', padx=10, pady=10) # Pack the back button

        order_label = tk.Label(self.root, text="Do you want to place an order?", font=("Helvetica", 16))
        order_label.pack(pady=20) # Pack the order label

        cart_label = tk.Label(self.root, text=f"Items in your cart:", font=("Helvetica", 12))
        cart_label.pack(pady=10) # Pack the cart label

        for item, price in self.cart: # Loop through the cart
            item_label = tk.Label(self.root, text=f"{item}: ${price:.2f}", font=("Helvetica", 12))
            item_label.pack() # Pack the item label

        total_label = tk.Label(self.root, text=f"Total price: ${self.total_price:.2f}", font=("Helvetica", 12))
        total_label.pack(pady=10)

        # Customer information
        first_name_label = tk.Label(self.root, text="First Name:", font=("Helvetica", 12))
        first_name_label.pack(pady=5)
        self.first_name_entry = tk.Entry(self.root)
        self.first_name_entry.pack(pady=5)

        last_name_label = tk.Label(self.root, text="Last Name:", font=("Helvetica", 12))
        last_name_label.pack(pady=5)
        self.last_name_entry = tk.Entry(self.root)
        self.last_name_entry.pack(pady=5) #Pack the last name entry

        email_label = tk.Label(self.root, text="Email Address:", font=("Helvetica", 12))
        email_label.pack(pady=5)
        self.email_entry = tk.Entry(self.root)
        self.email_entry.pack(pady=5) # Pack the email entry

        yes_button = tk.Button(self.root, text="Yes", command=self.create_confirmation_window)
        yes_button.pack(pady=10) # Pack the yes button

        no_button = tk.Button(self.root, text="No", command=self.create_welcome_window)
        no_button.pack(pady=10) # Pack the no button

    def create_confirmation_window(self): # Method to create the confirmation window
        """
        Creates a confirmation window displaying the order details and a thank you message.

        This method retrieves the customer's first name, last name, and email from the entry fields.
        If any of these fields are empty, it shows an error message and reopens the order window.
        Otherwise, it generates an order ID, clears the current window, and displays the confirmation
        details including the items in the cart, total price, and a thank you message.

        Raises:
            messagebox.showerror: If any customer information is missing.

        Returns:
            None
        """
        self.customer_first_name = self.first_name_entry.get()
        self.customer_last_name = self.last_name_entry.get()
        self.customer_email = self.email_entry.get()

        if not self.customer_first_name or not self.customer_last_name or not self.customer_email:
            messagebox.showerror("Error", "Please enter all customer information.")
            self.create_order_window()
            return

        order_id = self.customer_first_name[:2].upper() + self.customer_last_name[-2:].upper()

        self.clear_window()

        back_button = tk.Button(self.root, text="Back", command=self.create_order_window)
        back_button.pack(anchor='nw', padx=10, pady=10)

        confirmation_label = tk.Label(self.root, text="Your order has been placed successfully!", font=("Helvetica", 16))
        confirmation_label.pack(pady=20) # Pack the confirmation label

        cart_label = tk.Label(self.root, text=f"Items in your cart: {', '.join(item for item, _ in self.cart)}", font=("Helvetica", 12))
        cart_label.pack(pady=10) # pack the cart label

        total_label = tk.Label(self.root, text=f"Total price: ${self.total_price:.2f}", font=("Helvetica", 12))
        total_label.pack(pady=10) # Pack the total label

        order_id_label = tk.Label(self.root, text=f"Order ID: {order_id}", font=("Helvetica", 12))
        order_id_label.pack(pady=10) # Pack the order ID label

        thank_you_label = tk.Label(self.root, text=f"Thank you {self.customer_first_name} for using our mobile app. Please wait patiently while we work on your order.", font=("Helvetica", 12))
        thank_you_label.pack(pady=10) # Pack the thank you label

    def clear_window(self): #Method to clear the window
        for widget in self.root.winfo_children():
            widget.destroy()

if __name__ == "__main__": # Main method
    root = tk.Tk()
    app = McDonaldsApp(root)
    root.mainloop()
