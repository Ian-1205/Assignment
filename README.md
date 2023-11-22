"""
THIS IS A BOOK MANAGEMENT SYSTEM THAT ALLOWS USERS TO ADD, VIEW, UPDATE AND DELETE BOOKS FROM A FILE
"""
import datetime

# Create a Book class with the following attributes:
class Book:
    def __init__(self, isbn, title, author, publisher, genre, year_published, date_purchased, status):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.publisher = publisher
        self.genre = genre
        self.year_published = year_published
        self.date_purchased = date_purchased
        self.status = status

# Create a BookManagementSystem class with the following attributes:
class BookManagementSystem:

    # initializes the following attributes:
    def __init__(self, filename):
        self.books = []
        self.filename = filename
        self.read_from_file()


    # Create a function called add_book that takes in the following parameters:
    def add_book(self):
        isbn = input("Enter book ISBN: ")
        author = input("Enter book author: ")
        title = input("Enter book title: ")
        publisher = input("Enter book publisher: ")
        genre = input("Enter book genre: ")
        year_published = input("Enter year published: ")
        date_purchased_str = input("Enter date purchased (DD-MM-YYYY): ")
        date_purchased = datetime.datetime.strptime(date_purchased_str, "%d-%m-%Y")
        status = input("Enter book status: ")
        book = Book(isbn, author, title, publisher, genre, year_published, date_purchased, status)
        self.books.append(book)
        self.write_to_file()
        self.clear_screen()

    def __len__(self):
        return len(self.books)

    # Create a function called view_books that takes in the following parameters:
    def view_books(self):
        print("Numbers of books:", len(self.books) )
        if len(self.books) == 0:
            print("No books found")
            input("Press Enter to continue...")
        else:
            for book in self.books:
                print(f"ISBN: {book.isbn}, Author: {book.author}, Title: {book.title}, Publisher: {book.publisher}, Genre: {book.genre}, Year Published: {book.year_published}, Date Purchased: {book.date_purchased.strftime('%d-%m-%Y')}, Status: {book.status}")
            input("Press Enter to continue...")
            self.clear_screen()

# Create a function called update_book that takes in the following parameters:
    def update_book(self):
        current_book_name = input("Enter the current name of the book you want to update: ")
        current_book_author = input("Enter the current author of the book you want to update: ")
        for book in self.books:
            if book.title == current_book_name and book.author == current_book_author:
                new_book_name = input("Enter the new name of the book: ")
                new_book_author = input("Enter the new author of the book: ")
                book.title = new_book_name
                book.author = new_book_author
                print("Book updated successfully.")
                self.write_to_file()
                self.clear_screen()
                return
        print("Book not found.")
        self.clear_screen()

# Create a function called delete_book that takes in the following parameters:
    def delete_book(self):
        isbn = input("Enter book ISBN to delete: ")
        for book in self.books:
            if book.isbn == isbn:
                self.books.remove(book)
                print("Book deleted successfully")
                self.write_to_file()
                self.clear_screen()
                return
        print("Book not found")
        self.clear_screen()

# Create a function called write_to_file that takes in the following parameters:
    def write_to_file(self):
        with open(self.filename, 'w') as f:
            for book in self.books:
                f.write(f"{book.isbn},{book.author},{book.title},{book.publisher},{book.genre},{book.year_published},{book.date_purchased.strftime('%d-%m-%Y')},{book.status}\n")

# Create a function called read_from_file that takes in the following parameters:
    def read_from_file(self):
        self.books = []
        try:
            with open(self.filename, 'r') as f:
                for line in f:
                    data = line.strip().split(',')
                    if len(data) == 8:
                        isbn, author, title, publisher, genre, year_published_str, date_purchased_str, status = data
                        year_published = int(year_published_str)
                        date_purchased = datetime.datetime.strptime(date_purchased_str, "%d-%m-%Y")
                        book = Book(isbn, title, author, publisher, genre, year_published, date_purchased, status)
                        self.books.append(book)
                    else:
                        print(f"Skipping invalid line: {line}")
        except FileNotFoundError:
            print("Error: File not found")

    # Create a function called clear_screen that takes in the following parameters:
    def clear_screen(self):
        print("\033", end="")


# Create an instance of the BookManagementSystem class and call the functions
bms = BookManagementSystem('books.txt')
while True:
    print("1. Add book")
    print("2. View books")
    print("3. Delete book")
    print("4. Update book")
    print("5. Exit")

    choice = input("Enter your choice: ")
    if choice == "1":
        bms.add_book()
    elif choice == "2":
        bms.view_books()
    elif choice == "3":
        bms.delete_book()
    elif choice == "4":
        bms.update_book()
    elif choice == "5":
        print("Goodbye!")
        break
    else:
        print("Invalid choice. Please try again.")

        try_again = input("Do you wish to try again? (y/n): ").lower()

        while try_again != "n" and try_again != "y":
            try_again = input("Do you wish to try again? (y/n): ").lower()

        if try_again == "n":
            Run = False
            break
            
