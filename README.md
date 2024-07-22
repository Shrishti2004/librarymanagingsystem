# librarymanagingsystem
class LibraryItem:
    def _init_(self, title, author, category):
        self.title = title
        self.author = author
        self.category = category
        self.checked_out = False
        self.due_date = None

    def _str_(self):
        return f"{self.title} by {self.author} - {self.category}"

class Book(LibraryItem):
    pass

class Magazine(LibraryItem):
    pass

class DVD(LibraryItem):
    pass
from datetime import datetime, timedelta

class Library:
    def _init_(self):
        self.items = []
        self.overdue_fines_per_day = 1.0

    def add_item(self, item):
        self.items.append(item)

    def checkout_item(self, title):
        for item in self.items:
            if item.title == title and not item.checked_out:
                item.checked_out = True
                item.due_date = datetime.now() + timedelta(days=14)  # 2 weeks checkout period
                print(f"{title} has been checked out. Due date: {item.due_date}")
                return
        print(f"{title} is not available for checkout.")

    def return_item(self, title):
        for item in self.items:
            if item.title == title and item.checked_out:
                item.checked_out = False
                if datetime.now() > item.due_date:
                    overdue_days = (datetime.now() - item.due_date).days
                    fine = overdue_days * self.overdue_fines_per_day
                    print(f"{title} is overdue. Fine: ${fine:.2f}")
                else:
                    print(f"{title} has been returned on time.")
                item.due_date = None
                return
        print(f"{title} is not currently checked out.")

    def search_items(self, keyword, search_by='title'):
        results = []
        for item in self.items:
            if search_by == 'title' and keyword.lower() in item.title.lower():
                results.append(item)
            elif search_by == 'author' and keyword.lower() in item.author.lower():
                results.append(item)
            elif search_by == 'category' and keyword.lower() in item.category.lower():
                results.append(item)
        return results
def main():
    library = Library()
    
    while True:
        print("\nLibrary Management System")
        print("1. Add Item")
        print("2. Checkout Item")
        print("3. Return Item")
        print("4. Search Items")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            item_type = input("Enter item type (Book, Magazine, DVD): ")
            title = input("Enter title: ")
            author = input("Enter author: ")
            category = input("Enter category: ")
            
            if item_type.lower() == 'book':
                library.add_item(Book(title, author, category))
            elif item_type.lower() == 'magazine':
                library.add_item(Magazine(title, author, category))
            elif item_type.lower() == 'dvd':
                library.add_item(DVD(title, author, category))
            else:
                print("Invalid item type.")
        
        elif choice == '2':
            title = input("Enter the title of the item to checkout: ")
            library.checkout_item(title)
        
        elif choice == '3':
            title = input("Enter the title of the item to return: ")
            library.return_item(title)
        
        elif choice == '4':
            search_by = input("Search by (title, author, category): ")
            keyword = input(f"Enter {search_by}: ")
            results = library.search_items(keyword, search_by)
            if results:
                print("Search Results:")
                for item in results:
                    print(item)
            else:
                print("No items found.")
        
        elif choice == '5':
            print("Exiting the system.")
            break
        
        else:
            print("Invalid choice. Please try again.")

if _name_ == "_main_":
    main()
