
### main_menu.py
```
import time
import os.path

from LocalFileProgram import file_output, large_file_search, parse_csv
from MongoFiles import mongo_crud, mongo_file_export, start_kill_mongod

if __name__ == "__main__":
    system_choice = 0

    while system_choice is not 9:
        print("Load from local file or from the database")
        print("  1. Local File")
        print("  2. Database")
        print("  9. Exit")
        try:
            system_choice = int(input("Enter your choice: "))
        except:
            print("Please enter a valid input...\n")


        if system_choice is 1:
            bst = large_file_search.BinarySearchTree()
            csv_path = input("Enter the file name including extension: ")
            
            # Validate that the file is in the currenty working directory.
            if os.path.isfile(csv_path) is not True:
                print("No file found...\n")
                continue

            user_choice = 0

            while user_choice is not 9:
                print("Menu:")
                print("  1. Load Bids")
                print("  2. Display all Bids")
                print("  3. Find Bid")
                print("  4. Remove Bid")
                print("  5. Create File")
                print("  9. Exit")
                try:
                    user_choice = int(input("Enter your choice: "))
                except:
                    print("Please enter a valid input...\n")
                    continue
    
                if user_choice is 1:
                    bst.load_bids(csv_path)
                elif user_choice is 2:
                    node = bst.root
                    bst.display_all_bids(node)
                elif user_choice is 3:
                    try:
                        search_bid = int(input("Enter a Bid ID: "))
                    except:
                        print("Invalid input...\n")
                        continue
                    found_bid = bst.search_tree(search_bid)

                    if found_bid is not None:
                        parse_csv.print_bid(found_bid.bid)
                    else:
                        print("No bid found...")
                elif user_choice is 4:
                    try:
                        deleted_bid = int(input("Enter a Bid ID: "))
                    except:
                        print("Invalid input...\n")
                    
                    if bst.search_tree(deleted_bid) is None:
                        print("No bid found for deletion...")
                        continue

                    bst.root = bst.delete_bid(bst.root, deleted_bid)
                    print("Bid successfully deleted...")
         
                elif user_choice is 5:
                    file_name = input("Enter a file name: ")
                    labels = ["Auction ID", "Auction Title", "Fund", "Auction Fee Total"]
                    
                    file_output.create_csv(file_name, bst, labels)
                    print("File successfully created...")


        elif system_choice is 2:
            start_kill_mongod.start_mongod()
            time.sleep(0.25)

            # Ensures index for AuctionID is created
            mongo_crud.create_index('localhost', 27017, 'CityData', 'bids', 'AuctionID')

            mongo_choice = 0

            while mongo_choice is not 9:
                print("Menu:")
                print("  1. Display all Bids")
                print("  2. Find Bid")
                print("  3. Remove Bid")
                print("  4. Export Database to CSV")
                print("  9. Exit")
                try:
                    mongo_choice = int(input("Enter your choice: "))
                except:
                    print("Please enter a valid input...\n")
                    continue  
                if mongo_choice == 2:
                    try:
                        search_bid = int(input("Enter a Bid ID: "))
                    except:
                        print("Invalid input...\n")
                        continue
                    found_bid = mongo_crud.find_bid('localhost', 27017, 'CityData', 'bids', search_bid)
                    mongo_crud.print_results(found_bid)
                if mongo_choice == 4:
                    new_file_name = input("Enter a file name fore exporting: ")
                    mongo_file_export.export_collection(new_file_name)
                    time.sleep(2)

            # Killing the MongoDB database
            print("Shutting down MongdoDB database...\n")
            start_kill_mongod.kill_mongod()
print("")
```

### parse_csv.py
```
import csv


# Opening file to be read and creating a list that contains each bid. DictReader is used to store each bid value with
# its associated key value.
def open_file(file):
    bid_list = []
    with open(file, 'r') as csv_file:
        csv_reader = csv.DictReader(csv_file)

        for bid in csv_reader:
            bid_list.append(bid)

        return bid_list


def print_bid(bid):
print("%s | %s | %s | %s" % (bid.bid_id, bid.title, bid.fund, bid.amount))
```

### large_file_search.py
```
from operator import itemgetter
from LocalFileProgram import parse_csv


class Bid():
    def __init__(self, bid):
        self.bid_id = int(bid["AuctionID"])
        self.title = bid["AuctionTitle"]
        self.fund = bid["Fund"]
        self.amount = bid["AuctionFeeTotal"]
        
class Node():
    def __init__(self, bid):
        self.right = None
        self.left = None
        self.bid = bid
        
# Count created for the purpose of testing in its current state.
# TODO create an option for the user to get a count of how many entries were taken in
# from the CSV file.
class BinarySearchTree():
    def __init__(self):
        self.root = None
        self.node_count = 0
        
    
    def load_bids(self, csv_path):
        print("Loading CSV file...")   
        list_bids = parse_csv.open_file(csv_path)
        
        # Sorting the entries taken in from the CSV file so that the BST is balanced. This results
        # in the most efficient traversal of the BST when searching or deleting an entry.
        sorted_list = sorted(list_bids, key=itemgetter("AuctionID"))
        start = 0
        end = len(sorted_list) - 1
        print("%s files loaded..." % len(sorted_list))
    
        
        try:
            self.root = self.create_tree(sorted_list, start, end)
            print("Loading Complete...")
        except IOError:
            print("An error occurred when trying to read the file")
       
    # Recursive function below handles the creation of the BST. This is called in the load_bids function
    # directly above. We will also handle a count for node numbers in this function.  
    def create_tree(self, bid_list, start, end):
        if start > end:
            return None
        
        # Code below sets the root as the middle most bid based on ID values. This process creates a balanced tree
        # making the most efficient traversal.
        try:
            self.node_count += 1
            midpoint = int((start + end) / 2)
            root = Node(Bid(bid_list[midpoint]))
            root.left = self.create_tree(bid_list, start, midpoint-1)
            root.right = self.create_tree(bid_list, midpoint+1, end)
            
            return root
        except:
            print("Out of bounds thrown. Midpoint is %s and end is %s" % (midpoint, end))
        

    def display_all_bids(self, node):
        if node is not None:
            self.display_all_bids(node.left)
            parse_csv.print_bid(node.bid)        
            self.display_all_bids(node.right)
        
    
    def search_tree(self, search_bid):
        if self.root is None:
            print("No file loaded...")
            return
        current_node = self.root
        found = False
        while not found:
            # This is statement handles if a bid has been deleted.
            if current_node.bid == None:
                return None
            if search_bid == current_node.bid.bid_id:
                return current_node
            elif search_bid < current_node.bid.bid_id:
                current_node = current_node.left
            else:
                current_node = current_node.right
                
            if current_node == None:
                return None
    
    # Method used in delete_bid 
    def minimum_value_bid(self, bid):
        current_bid = bid
        while current_bid.left is not None:
            current_bid = current_bid.left

        return current_bid
            

    # This function runs in check_bid_for_deletion method.    
    def delete_bid(self, bid, bid_id):
        # Setting up base case
        if bid is None:
            return None

        # If bid_id passed in is smaller than root.
        if bid_id < bid.bid.bid_id:
            bid.left = self.delete_bid(bid.left, bid_id)
        # If bid_id passed in is larger than root.
        elif bid_id > bid.bid.bid_id:
            bid.right = self.delete_bid(bid.right, bid_id)
        # If bid_id matches the bid_id of the bid passed into the function
        else:
            # Two conditions handle if there is one or no children nodes to the root
            if bid.left is None:
                temp = bid.right
                bid = None
                return temp
            elif bid.right is None:
                temp = bid.left
                bid = None
                return temp
            
            # if there are two children, the following code traverses the code to
            # the smallest successor

            temp = self.minimum_value_bid(bid.right)
          
            bid.bid = temp.bid
            bid.right = self.delete_bid(bid.right, temp.bid.bid_id)
return bid
```

### file_output.py
```
import csv


# Assistance with building function to create CSV files from https://pythonspot.com/files-spreadsheets-csv/

# code takes in a file name, binary search tree, and labels when called. Uses Python's csv module
# to open and close the the file writer. The program takes a file name, a BinarySearchTree and the labels
# for each column in the outputted file. 
def create_csv(file_name, source_code, labels):
    with open(file_name + '.csv', 'w', newline = '') as csv_file:
        file_writer = csv.writer(csv_file, delimiter = ',',
                                 quotechar = '|', quoting = csv.QUOTE_MINIMAL)
        file_writer.writerow(labels)

        node = source_code.root

        def export_bids(node):
            if node is not None:
                export_bids(node.left)
                file_writer.writerow([node.bid.bid_id, node.bid.title, node.bid.fund, node.bid.amount])
                export_bids(node.right)
        
export_bids(node)
```

### mongo_crud.py
```
from os import abort
from pymongo import MongoClient

def print_results(values):
    print("{0} | {1} | {2} | {3}".format(values[0], values[1], values[2], values[3]))

# creates an index based on the field passed in and puts it in ascending order.
def create_index(host, port, udb, ucollection, field):
    connection = MongoClient(host, port)
    db = connection [udb]
    collection = db[ucollection]

    collection.create_index(field)


def find_bid(host, port, udb, ucollection, bid_id):
    connection = MongoClient(host, port)
    db = connection [udb]
    collection = db[ucollection]

    pipeline = [
        {"$match" : {"AuctionID" : bid_id}},
        {"$project" : {"AuctionID" : 1, "AuctionTitle" : 1, "Fund" : 1, "AuctionFeeTotal" : 1, "_id" : 0}}
        ]
    
    # Convert the pipline values to a printable format
    cursor = collection.aggregate(pipeline)
    results = list(cursor)

    if not results:
        print("No bid found....")
        return None

    # Pull the dictionary object out of the list
    results_string = results[0]
    listed_values = []

    for i in results_string:
        value = results_string[i]
        listed_values.append(value)
   
    return listed_values


def create_bid(host, port, udb, ucollection):
    connection = MongoClient(host, port)
    db = connection [udb]
    collection = db[ucollection]

    field_list = ["AuctionID", "AuctionTitle", "Fund", "AuctionFeeTotal"]
    new_bid = []

    for field in field_list:
        user_value = input("Enter {0}:  ".format(field))
        new_bid.append(user_value)

        # Check if bid exists with AuctionID and stop creation if it does.
        check_bid = find_bid(host, port, udb, ucollection, int(user_value))

        if check_bid:
            print("Bid with that AuctionID already exists...")
            return None

    bid = {"AuctionID" : int(new_bid[0]),
           "AuctionTitle" : new_bid[1],
           "Fund" : new_bid[2],
           "AuctionFeeTotal" : new_bid[3]}

    try:
        collection.insert_one(bid).inserted_id
        find_bid(host, port, udb, ucollection, int(new_bid[0]))
    except:
        abort(400, str("Error in process. Aborted."))


def update_bid(host, port, udb, ucollection, bid_id):
    connection = MongoClient(host, port)
    db = connection [udb]
    collection = db[ucollection]

    value = {"AuctionID" : bid_id}
    bid = collection.find_one(value)

    field_list = ["AuctionTitle", "Fund", "AuctionFeeTotal"]
    
    # Check if bid exists before trying to update it.
    check_bid = find_bid(host, port, udb, ucollection, int(bid_id))
    if not check_bid:
        return None

    for field in field_list:
        # Loop here is for input validation.
        loop_condition = True
        while loop_condition:
            user_choice = input("Update {0}... (Y/N) ".format(field))
            if user_choice is 'Y' or user_choice is 'y':
                try:
                    user_value = input("Enter a new value: ")
                    update = {field : user_value}
                    collection.update_one({"AuctionID" : bid_id} , {"$set" : update})
                except:
                    abort(400, str("Error in process. Aborted."))

                loop_condition = False
            elif user_choice is 'N' or user_choice is 'n':
                loop_condition = False
            else:
                print("Invalid input, please enter valid input")

def delete_bid(host, port, udb, ucollection, bid_id):
    connection = MongoClient(host, port)
    db = connection [udb]
    collection = db[ucollection]

    value = {"AuctionID" : bid_id}
    bid = collection.find_one(value)

    try:
        collection.delete_one({'AuctionID' : bid_id})
        print("Bid successfully removed from the database")
    except:
abort(400, str("Error in process. Aborted."))
```

### start_kill_mongod.py
```
from pathlib import Path
import subprocess
import time

def start_mongod():
    # Code below starts up MongoDB database for the user.
    try:
        path_loop = True
        while(path_loop):
            print("Is your mongod.exe C:\\Program Files\\MongoDB\\Server\\4.0\\bin\\mongod.exe?")
            default_path = input("Y/N: ")
    
            if default_path is 'Y' or default_path is 'y':
                exe_location = Path("C:/Program Files/MongoDB/Server/4.0/bin/mongod.exe")
                path_loop = False
            else:
                if default_path is 'N' or default_path is 'n':
                    print("Enter the location of mongod.exe: (use '/'not '\\')")
                    exe_location = Path(input())
                    path_loop = False
                else:
                    print("Invalid input...")

        directory_loop = True
        while(directory_loop):
            print("Is the database directory located at C:\\data\\db?")
            default_directory = input("Y/N: ")

            if default_directory is 'Y' or  default_directory is'y':
                directory_location = Path("C:/data/db")
                directory_loop = False
            else:
                if default_directory is 'N' or 'n':
                    print("Enter the database directory location: (use '/ 'not '\\')")
                    directory_location = Path(input())
                    directory_loop = False
                else:
                    print("Invalid input...")

        command = ("{0} --dbpath={1}".format(exe_location, directory_location))

        # Creating a process that allows other processes to continue while this runs. 
        # mongod.exe runs in a seperate console.
        subprocess.Popen(command, creationflags = subprocess.CREATE_NEW_CONSOLE)
        
        
        
    except:
        print("Database failed to start...")

# Function to kill the mongod.exe and stop the MongoDB batabase
def kill_mongod():
subprocess.run("taskkill /f /im mongod.exe")
```

### mongo_file_export.py
```
import subprocess
import os
from pathlib import Path


def export_collection(file_name):
    # change the path to point to where mongoexport.exe is located.
    path = ("C:\\Program Files\\MongoDB\\Server\\4.0\\bin\\")
    os.chdir(path)

    # Set the file name passed on what the user passes in.
    file = "C:\\data\\" + file_name + ".csv"
    command = ("mongoexport --db CityData --collection bids --type=csv --fields AuctionID,AuctionTitle,Fund,AuctionFeeTotal --out %s" % (file))

subprocess.run(command, creationflags = subprocess.CREATE_NEW_CONSOLE)
```




