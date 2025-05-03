# Online_app_project
# Initialize data
users = {}
transactions = []

# Helper function to display users
def display_users():
    for username, info in users.items():
        print(f"{username} | Balance: ₹{info['balance']}")

def register_user(username, password):
    if username in users:
        print("Username already exists.")
        return False
    users[username] = {
        "password": password,
        "balance": 0.0,
    }
    print(f"User '{username}' registered successfully.")
    return True

def login(username, password):
    user = users.get(username)
    if user and user['password'] == password:
        print(f"Login successful. Welcome, {username}!")
        return True
    else:
        print("Invalid username or password.")
        return False

def add_funds(username, amount):
    if amount <= 0:
        print("Amount must be greater than zero.")
        return

    users[username]['balance'] += amount
    transactions.append((username, "ADD_FUNDS", amount))
    print(f"₹{amount} added to {username}'s wallet.")

def transfer_funds(sender, receiver, amount):
    if sender not in users or receiver not in users:
        print("Sender or receiver does not exist.")
        return

    if users[sender]['balance'] < amount:
        print("Insufficient balance.")
        return
    users[sender]['balance'] -= amount
    users[receiver]['balance'] += amount
    transactions.append((sender, f"TRANSFER_TO_{receiver}", -amount))
    transactions.append((receiver, f"RECEIVED_FROM_{sender}", amount))
    print(f"₹{amount} transferred from {sender} to {receiver}.")

def view_transactions(username):
    print(f"Transaction history for {username}:")
    for txn in transactions:
        if txn[0] == username:
            print(f"Type: {txn[1]}, Amount: ₹{txn[2]}")

# Register users
register_user("alice", "1234")
register_user("bob", "5678")

# Login
login("alice", "1234")

# Add funds
add_funds("alice", 1000)

# Transfer
transfer_funds("alice", "bob", 300)

# Display balances
display_users()

# View transaction history
view_transactions("alice")
view_transactions("bob")
