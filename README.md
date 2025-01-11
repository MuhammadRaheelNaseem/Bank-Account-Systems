# Bank-Account-Systems
For Students Only

## Code # 1 - With Function
```python
# Bank Account Management System

users = {}


def signup(username, password):
    if username in users:
        return "Username already exists. Please choose a different one."
    users[username] = {
        "password": password,
        "balance": 0.0,
        "transaction_history": []
    }
    return f"User {username} successfully registered."


def login(username, password):
    if username in users and users[username]["password"] == password:
        return username
    return None


def deposit(username, amount):
    if amount > 0:
        users[username]["balance"] += amount
        users[username]["transaction_history"].append(f"Deposited: ${amount}")
        return f"Successfully deposited ${amount}. Current balance: ${users[username]['balance']}"
    else:
        return "Deposit amount must be greater than zero."


def withdraw(username, amount):
    if amount > 0 and amount <= users[username]["balance"]:
        users[username]["balance"] -= amount
        users[username]["transaction_history"].append(f"Withdrew: ${amount}")
        return f"Successfully withdrew ${amount}. Current balance: ${users[username]['balance']}"
    elif amount <= 0:
        return "Withdrawal amount must be greater than zero."
    else:
        return "Insufficient balance."


def view_balance(username):
    return f"Your current balance is: ${users[username]['balance']}"


def transfer(sender_username, recipient_username, amount):
    if recipient_username in users:
        if amount > 0 and amount <= users[sender_username]["balance"]:
            users[sender_username]["balance"] -= amount
            users[recipient_username]["balance"] += amount
            users[sender_username]["transaction_history"].append(f"Transferred ${amount} to {recipient_username}")
            users[recipient_username]["transaction_history"].append(f"Received ${amount} from {sender_username}")
            return f"Successfully transferred ${amount} to {recipient_username}."
        elif amount <= 0:
            return "Transfer amount must be greater than zero."
        else:
            return "Insufficient balance."
    else:
        return "Recipient username not found."


def view_transaction_history(username):
    transactions = users[username]["transaction_history"]
    return "\n".join(transactions) if transactions else "No transactions yet."


def main():
    while True:
        print("\n====== Bank Account Management System ======")
        print("1. Sign Up")
        print("2. Log In")
        print("3. Exit")
        choice = input("Enter your choice: ")

        if choice == "1":
            username = input("Enter a username: ")
            password = input("Enter a password: ")
            print(signup(username, password))

        elif choice == "2":
            username = input("Enter your username: ")
            password = input("Enter your password: ")
            logged_in_user = login(username, password)

            if logged_in_user:
                print(f"\nWelcome {logged_in_user}!")

                while True:
                    print("\n1. Deposit Funds")
                    print("2. Withdraw Funds")
                    print("3. View Balance")
                    print("4. Transfer Funds")
                    print("5. View Transaction History")
                    print("6. Log Out")
                    user_choice = input("Enter your choice: ")

                    if user_choice == "1":
                        try:
                            amount = float(input("Enter amount to deposit: "))
                            print(deposit(logged_in_user, amount))
                        except ValueError:
                            print("Invalid amount. Please enter a number.")

                    elif user_choice == "2":
                        try:
                            amount = float(input("Enter amount to withdraw: "))
                            print(withdraw(logged_in_user, amount))
                        except ValueError:
                            print("Invalid amount. Please enter a number.")

                    elif user_choice == "3":
                        print(view_balance(logged_in_user))

                    elif user_choice == "4":
                        recipient_username = input("Enter recipient's username: ")
                        try:
                            amount = float(input("Enter amount to transfer: "))
                            print(transfer(logged_in_user, recipient_username, amount))
                        except ValueError:
                            print("Invalid amount. Please enter a number.")

                    elif user_choice == "5":
                        print("\nTransaction History:")
                        print(view_transaction_history(logged_in_user))

                    elif user_choice == "6":
                        print("Logged out successfully.")
                        break

                    else:
                        print("Invalid choice. Please try again.")

            else:
                print("Invalid username or password.")

        elif choice == "3":
            print("Thank you for using the Bank Account Management System. Goodbye!")
            break

        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
```

## Code # 2 - With Classes
```python
import getpass
import uuid

# Bank Account Management System

class User:
    def __init__(self, username, password):
        self.username = username
        self.password = password
        self.balance = 0.0
        self.transaction_history = []
        self.id = str(uuid.uuid4())

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            self.transaction_history.append(f"Deposited: ${amount}")
            return f"Successfully deposited ${amount}. Current balance: ${self.balance}"
        else:
            return "Deposit amount must be greater than zero."

    def withdraw(self, amount):
        if amount > 0 and amount <= self.balance:
            self.balance -= amount
            self.transaction_history.append(f"Withdrew: ${amount}")
            return f"Successfully withdrew ${amount}. Current balance: ${self.balance}"
        elif amount <= 0:
            return "Withdrawal amount must be greater than zero."
        else:
            return "Insufficient balance."

    def view_balance(self):
        return f"Your current balance is: ${self.balance}"

    def transfer(self, recipient, amount):
        if amount > 0 and amount <= self.balance:
            self.balance -= amount
            recipient.balance += amount
            self.transaction_history.append(f"Transferred ${amount} to {recipient.username}")
            recipient.transaction_history.append(f"Received ${amount} from {self.username}")
            return f"Successfully transferred ${amount} to {recipient.username}."
        elif amount <= 0:
            return "Transfer amount must be greater than zero."
        else:
            return "Insufficient balance."

    def view_transaction_history(self):
        return "\n".join(self.transaction_history) if self.transaction_history else "No transactions yet."


class BankSystem:
    def __init__(self):
        self.users = {}

    def signup(self, username, password):
        if username in self.users:
            return "Username already exists. Please choose a different one."
        self.users[username] = User(username, password)
        return f"User {username} successfully registered."

    def login(self, username, password):
        if username in self.users and self.users[username].password == password:
            return self.users[username]
        return None


def main():
    bank_system = BankSystem()

    while True:
        print("\n====== Bank Account Management System ======")
        print("1. Sign Up")
        print("2. Log In")
        print("3. Exit")
        choice = input("Enter your choice: ")

        if choice == "1":
            username = input("Enter a username: ")
            password = getpass.getpass("Enter a password: ")
            print(bank_system.signup(username, password))

        elif choice == "2":
            username = input("Enter your username: ")
            password = getpass.getpass("Enter your password: ")
            user = bank_system.login(username, password)

            if user:
                print(f"\nWelcome {user.username}!")

                while True:
                    print("\n1. Deposit Funds")
                    print("2. Withdraw Funds")
                    print("3. View Balance")
                    print("4. Transfer Funds")
                    print("5. View Transaction History")
                    print("6. Log Out")
                    user_choice = input("Enter your choice: ")

                    if user_choice == "1":
                        try:
                            amount = float(input("Enter amount to deposit: "))
                            print(user.deposit(amount))
                        except ValueError:
                            print("Invalid amount. Please enter a number.")

                    elif user_choice == "2":
                        try:
                            amount = float(input("Enter amount to withdraw: "))
                            print(user.withdraw(amount))
                        except ValueError:
                            print("Invalid amount. Please enter a number.")

                    elif user_choice == "3":
                        print(user.view_balance())

                    elif user_choice == "4":
                        recipient_username = input("Enter recipient's username: ")
                        if recipient_username in bank_system.users:
                            try:
                                amount = float(input("Enter amount to transfer: "))
                                recipient = bank_system.users[recipient_username]
                                print(user.transfer(recipient, amount))
                            except ValueError:
                                print("Invalid amount. Please enter a number.")
                        else:
                            print("Recipient username not found.")

                    elif user_choice == "5":
                        print("\nTransaction History:")
                        print(user.view_transaction_history())

                    elif user_choice == "6":
                        print("Logged out successfully.")
                        break

                    else:
                        print("Invalid choice. Please try again.")

            else:
                print("Invalid username or password.")

        elif choice == "3":
            print("Thank you for using the Bank Account Management System. Goodbye!")
            break

        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
```
