# New-Inventory-Management-System
import tkinter as tk
from tkinter import ttk
import sqlite3

class InventoryManagementSystem:
    def _init_(self):
        self.db_file = "inventory.db"
        self.create_db()
        self.root = tk.Tk()
        self.root.title("Inventory Management System")

        self.setup_authentication()
        self.setup_product_management()
        self.setup_inventory_tracking()
        self.setup_reports()

        self.layout_widgets()
        self.root.mainloop()

    def create_db(self):
        with sqlite3.connect(self.db_file) as conn:
            cursor = conn.cursor()
            cursor.execute('''
                CREATE TABLE IF NOT EXISTS products (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    product TEXT,
                    quantity INTEGER,
                    price REAL
                )
            ''')

    def setup_authentication(self):
        self.username_label = tk.Label(self.root, text="Username:")
        self.username_entry = tk.Entry(self.root)
        self.password_label = tk.Label(self.root, text="Password:")
        self.password_entry = tk.Entry(self.root, show="*")
        self.login_button = tk.Button(self.root, text="Login", command=self.login)

    def setup_product_management(self):
        self.product_label = tk.Label(self.root, text="Product Name:")
        self.product_entry = tk.Entry(self.root)
        self.quantity_label = tk.Label(self.root, text="Quantity:")
        self.quantity_entry = tk.Entry(self.root)
        self.price_label = tk.Label(self.root, text="Price:")
        self.price_entry = tk.Entry(self.root)
        self.add_button = tk.Button(self.root, text="Add Product", command=self.add_product)
        self.edit_button = tk.Button(self.root, text="Edit Product", command=self.edit_product)
        self.delete_button = tk.Button(self.root, text="Delete Product", command=self.delete_product)

    def setup_inventory_tracking(self):
        self.inventory_table = ttk.Treeview(self.root, columns=("Product", "Quantity", "Price"), show='headings')
        for col in ("Product", "Quantity", "Price"):
            self.inventory_table.heading(col, text=col)
        self.refresh_inventory_table()

    def setup_reports(self):
        self.low_stock_button = tk.Button(self.root, text="Low Stock Alert", command=self.generate_low_stock_report)
        self.sales_summary_button = tk.Button(self.root, text="Sales Summary", command=self.generate_sales_summary)

    def layout_widgets(self):
        self.username_label.grid(row=0, column=0)
        self.username_entry.grid(row=0, column=1)
        self.password_label.grid(row=1, column=0)
        self.password_entry.grid(row=1, column=1)
        self.login_button.grid(row=2, column=0, columnspan=2)

        self.product_label.grid(row=3, column=0)
        self.product_entry.grid(row=3, column=1)
        self.quantity_label.grid(row=4, column=0)
        self.quantity_entry.grid(row=4, column=1)
        self.price_label.grid(row=5, column=0)
        self.price_entry.grid(row=5, column=1)
        self.add_button.grid(row=6, column=0)
        self.edit_button.grid(row=6, column=1)
        self.delete_button.grid(row=7, column=0, columnspan=2)
        self.low_stock_button.grid(row=8, column=0)
        self.sales_summary_button.grid(row=8, column=1)
        self.inventory_table.grid(row=9, column=0, columnspan=2, sticky="nsew")

        self.root.grid_rowconfigure(9, weight=1)

    def login(self):
        # Implement user authentication logic
        pass

    def add_product(self):
        product_name = self.product_entry.get()
        quantity = self._get_integer(self.quantity_entry.get())
        price = self._get_float(self.price_entry.get())
        if product_name and quantity is not None and price is not None:
            with sqlite3.connect(self.db_file) as conn:
                cursor = conn.cursor()
                cursor.execute("INSERT INTO products (product, quantity, price) VALUES (?, ?, ?)", (product_name, quantity, price))
            self.refresh_inventory_table()

    def edit_product(self):
        # Implement logic to edit existing products
        pass

    def delete_product(self):
        # Implement logic to delete products
        pass

    def generate_low_stock_report(self):
        # Implement logic to generate low stock alerts
        pass

    def generate_sales_summary(self):
        # Implement logic to generate sales summary reports
        pass

    def refresh_inventory_table(self):
        self.inventory_table.delete(*self.inventory_table.get_children())
        with sqlite3.connect(self.db_file) as conn:
            cursor = conn.cursor()
            cursor.execute("SELECT product, quantity, price FROM products")
            for row in cursor.fetchall():
                self.inventory_table.insert("", "end", values=row)

    def _get_integer(self, value):
        try:
            return int(value)
        except ValueError:
            return None

    def _get_float(self, value):
        try:
            return float(value)
        except ValueError:
            return None

if _name_ == "_main_":
    app = InventoryManagementSystem()
