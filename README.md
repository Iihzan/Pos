# Pos




from tkinter import *
import mysql.connector 

# Connect to MySQL database
mydb = mysql.connector.connect(
    host="localhost",
    user="yourusername",
    password="yourpassword",
    database="yourdatabase"
) 

# Define functions for login and calculate bill
def login():
    username = entry_username.get()
    password = entry_password.get()
    mycursor = mydb.cursor()
    mycursor.execute("SELECT * FROM users WHERE username=%s AND password=%s", (username, password))
    result = mycursor.fetchone()
    if result:
        frame_login.pack_forget()
        frame_bill.pack()
    else:
        label_login_error.config(text="Invalid username or password") 

def calculate_bill():
    item = entry_item.get()
    price = float(entry_price.get())
    quantity = int(entry_quantity.get())
    tax = 0
    if check_tax.get():
        tax = 0.1
    total_cost = price * quantity * (1 + tax)
    label_result.config(text="Total Cost: $%.2f" % total_cost)
    mycursor = mydb.cursor()
    sql = "INSERT INTO bills (item, price, quantity, tax, total_cost) VALUES (%s, %s, %s, %s, %s)"
    val = (item, price, quantity, tax, total_cost)
    mycursor.execute(sql, val)
    mydb.commit() 

# Create GUI window
window = Tk()
window.title("Bill System") 

# Create frame for login
frame_login = Frame(window) 

label_username = Label(frame_login, text="Username:")
label_username.grid(row=0, column=0, padx=10, pady=10)
entry_username = Entry(frame_login)
entry_username.grid(row=0, column=1, padx=10, pady=10) 

label_password = Label(frame_login, text="Password:")
label_password.grid(row=1, column=0, padx=10, pady=10)
entry_password = Entry(frame_login, show="*")
entry_password.grid(row=1, column=1, padx=10, pady=10) 

button_login = Button(frame_login, text="Login", command=login)
button_login.grid(row=2, column=0, columnspan=2, padx=10, pady=10) 

label_login_error = Label(frame_login, fg="red")
label_login_error.grid(row=3, column=0, columnspan=2, padx=10, pady=10) 

# Create frame for bill calculation
frame_bill = Frame(window) 

label_item = Label(frame_bill, text="Item:")
label_item.grid(row=0, column=0, padx=10, pady=10)
entry_item = Entry(frame_bill)
entry_item.grid(row=0, column=1, padx=10, pady=10) 

label_price = Label(frame_bill, text="Price:")
label_price.grid(row=1, column=0, padx=10, pady=10)
entry_price = Entry(frame_bill)
entry_price.grid(row=1, column=1, padx=10, pady=10) 

label_quantity = Label(frame_bill, text="Quantity:")
label_quantity.grid(row=2, column=0, padx=10, pady=10)
entry_quantity = Entry(frame_bill)
entry_quantity.grid(row=2, column=1, padx=10, pady=10) 

check_tax = BooleanVar()
check_tax.set(False)
checkbox_tax = Checkbutton(frame_bill, text="Include tax", variable=check_tax)
checkbox_tax.grid(row=3, column=0, padx=10, pady=10) 

button_calculate =
