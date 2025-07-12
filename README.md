import json
class Account:
  def __init__(self , id , name , balance = 0):
    self.id = id
    self.name = name
    self.balance = balance
    self.transactions = []

  def show_details(self):
        print("Account Details:")
        print("   Account Number:", self.id)
        print("   Holder Name:", self.name)
        print("   Balance: ₹", self.balance)


  def deposit(self , amount):
    try:
      amount = float(amount)
      if (amount > 0):
        self.balance += amount
        self.transactions.append(f"Deposited Amount ₹{amount}")
        print(f"The {amount} amount deposited successfully!!!")
      else:
        print("Invalid amount")
    except ValueError:
      print("Please enter a valid number.")


  def withdraw(self , amount):
    try:
      amount = float(amount)
      if (amount <= 0):
        print("The amount should be greater than 0")
      elif (amount > self.balance):
        print("Insufficient balance")
      else:
        self.balance -= amount
        self.transactions.append(f"Withdrawn amount ₹{amount}")
        print(f"The {amount} is withdrawn successfully!!!")
    except ValueError:
      print("Please enter a valid number.")


  def get_balance(self):
    print(f"The Balance in your account is {self.balance}")
    return self.balance


  def get_history(self):
    if len(self.transactions) <= 0:
      print("No Transactions left")
    else:
      print("Transactions History")
      for transaction in self.transactions:
        print(transaction)

  def to_dict(self):
    return {"id" : self.id,
            "name" : self.name,
            "balance" : self.balance,
            "transactions" : self.transactions
            }
  @classmethod
  def from_dict(cls,data):
    account = cls(data["id"], data["name"], data["balance"])
    account.transactions = data["transactions"]
    return account


  def save_to_file(self,filename):
    with open( filename , "w" ) as file:
      json.dump(self.to_dict() , file)
    print("Account save successfully")

  @classmethod
  def load_the_file(cls,filename):
    with open(filename , "r") as file:
      data = json.load(file)
      return cls.from_dict(data)



acc1 = Account(1 , "Karuppaiya" , 1000)
acc1.save_to_file("account_data.json")

acc2 = Account(101, "ravi", 1500)
acc2.deposit(200)
acc2.deposit(1000)
acc2.withdraw(500)
acc2.save_to_file("ravi_data.json")


load1 = Account.load_the_file("account_data.json")
load2 = Account.load_the_file("ravi_data.json")

print(load2.get_history())
