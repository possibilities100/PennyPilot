import pandas as pd
from tkinter import *
from tkinter import ttk
from tkinter import messagebox
import json
import os

CATEGORY_FILE = 'categories.json'

# Load existing data or create a new DataFrame
def load_data(file_path='expenses.csv'):
    try:
        return pd.read_csv(file_path)
    except FileNotFoundError:
        return pd.DataFrame(columns=['Date', 'Category', 'Amount'])

# Save data to CSV
def save_data(df, file_path='expenses.csv'):
    df.to_csv(file_path, index=False)

# Load categories or initialize with default categories
def load_categories():
    if os.path.exists(CATEGORY_FILE):
        with open(CATEGORY_FILE, 'r') as file:
            return json.load(file)
    else:
        return ["Food", "Transport", "Entertainment"]

# Save categories to file
def save_categories(categories):
    with open(CATEGORY_FILE, 'w') as file:
        json.dump(categories, file)

# Add an expense
def add_expense():
    date = date_entry.get()
    category = category_var.get()
    amount = amount_entry.get()
    
    if not date or not category or not amount:
        messagebox.showwarning("Input Error", "Please fill out all fields")
        return

    df = load_data()
    new_expense = pd.DataFrame([[date, category, amount]], columns=['Date', 'Category', 'Amount'])
    df = pd.concat([df, new_expense], ignore_index=True)
    save_data(df)

    date_entry.delete(0, END)
    amount_entry.delete(0, END)
    messagebox.showinfo("Success", "Expense added successfully")

# View expenses
def view_expenses():
    df = load_data()
    text.delete(1.0, END)
    text.insert(INSERT, df.to_string(index=False))

# Add custom category
def add_custom_category():
    new_category = custom_category_entry.get()
    if new_category:
        categories = load_categories()
        if new_category not in categories:
            categories.append(new_category)
            save_categories(categories)
            category_menu['values'] = categories
            category_var.set(new_category)
            custom_category_entry.delete(0, END)
            messagebox.showinfo("Success", f"Category '{new_category}' added successfully")
        else:
            messagebox.showinfo("Info", "Category already exists")
    else:
        messagebox.showwarning("Input Error", "Please enter a category")

# Calculator Functions
def calculate(expression):
    try:
        result = eval(expression)
        calc_entry.delete(0, END)
        calc_entry.insert(END, result)
    except Exception as e:
        calc_entry.delete(0, END)
        calc_entry.insert(END, "Error")

# Initialize categories
categories = load_categories()

# Create GUI
root = Tk()
root.title("Expense Tracker")
root.geometry("400x450")
root.resizable(False, False)
font_style = ("Arial", 12)

# Input fields
frame1 = Frame(root)
frame1.pack(padx=10, pady=10)

date_label = Label(frame1, text="Date (dd-mm-yyyy):", font=font_style)
date_label.grid(row=0, column=0, padx=5, pady=5, sticky=E)
date_entry = Entry(frame1, font=font_style)
date_entry.grid(row=0, column=1, padx=5, pady=5)

category_label = Label(frame1, text="Category:", font=font_style)
category_label.grid(row=1, column=0, padx=5, pady=5, sticky=E)
category_var = StringVar(value=categories[0])
category_menu = ttk.Combobox(frame1, textvariable=category_var, values=categories, font=font_style)
category_menu.grid(row=1, column=1, padx=5, pady=5)

custom_category_label = Label(frame1, text="Add Custom Category:", font=font_style)
custom_category_label.grid(row=2, column=0, padx=5, pady=5, sticky=E)
custom_category_entry = Entry(frame1, font=font_style)
custom_category_entry.grid(row=2, column=1, padx=5, pady=5)

add_category_button = Button(frame1, text="Add Custom Category", command=add_custom_category, font=font_style)
add_category_button.grid(row=3, columnspan=2, pady=5)

amount_label = Label(frame1, text="Amount:", font=font_style)
amount_label.grid(row=4, column=0, padx=5, pady=5, sticky=E)
amount_entry = Entry(frame1, font=font_style)
amount_entry.grid(row=4, column=1, padx=5, pady=5)

add_button = Button(frame1, text="Add Expense", command=add_expense, font=font_style)
add_button.grid(row=5, columnspan=2, pady=10)

# Calculator
calc_frame = Frame(root)
calc_frame.pack(pady=10)

calc_label = Label(calc_frame, text="Calculator:", font=font_style)
calc_label.grid(row=0, column=0, columnspan=2)

calc_entry = Entry(calc_frame, font=font_style, width=20)
calc_entry.grid(row=1, column=0, columnspan=2, pady=5)

calc_button = Button(calc_frame, text="Calculate", command=lambda: calculate(calc_entry.get()), font=font_style)
calc_button.grid(row=2, column=0, columnspan=2, pady=5)

# View expenses
view_button = Button(root, text="View Expenses", command=view_expenses, font=font_style)
view_button.pack(pady=10)

root.mainloop()
