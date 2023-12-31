"""
THIS IS A BOOK MANAGEMENT SYSTEM THAT ALLOWS USERS TO ADD, VIEW, UPDATE AND DELETE BOOKS FROM A FILE
"""

import datetime
import os


# Create a function to get the write date format of the user's input
def get_valid_date_input(prompt, format_str):
    while True:
        user_input = input(prompt)
        try:
            date_obj = datetime.datetime.strptime(user_input, format_str)
            return date_obj
        except ValueError:
            print("Invalid input. Please enter the date in the specified format.")


Run = True


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
        proceed = input("Do you want to continue adding books? (y/n): ")
        if proceed.lower() == "n":
            os.system('cls')
            return

        while True:
            isbn = input("Enter book ISBN: ")
            if len(isbn) == 13 and isbn.isdigit():
                break  # Exit the loop if the ISBN is valid
            else:
                print("Invalid ISBN. ISBN must be exactly 13 digits and consist only of digits.")

        author = input("Enter book author: ")
        title = input("Enter book title: ")
        publisher = input("Enter book publisher: ")
        genre = input("Enter book genre: ")
        year_published = get_valid_date_input("Enter year published (DD-MM-YYYY): ", "%d-%m-%Y")
        date_purchased = get_valid_date_input("Enter date purchased (DD-MM-YYYY): ", "%d-%m-%Y")
        status = input("Enter book status: ")

        # Confirmation of the criteria of the user's input
        if isbn.isdigit() and author.isalpha() and genre.isalpha():
            book = Book(isbn, author, title, publisher, genre, year_published, date_purchased, status)
            books_to_add = [book]
            self.books.extend(books_to_add)
            self.write_to_file()
            print("The book has been successfully added.")
            os.system('cls')
        else:
            print("There are some invalid inputs, please try again.")

    Run = True

    def __len__(self):
        return len(self.books)

    # Create a function called view_books that takes in the following parameters:
    def view_books(self):

        print("Numbers of books:", len(self.books))
        if len(self.books) == 0:
            print("No books found.")
            input("Press Enter to continue...")
        else:
            for book in self.books:
                print(
                    f"ISBN: {book.isbn}, "
                    f"Author: {book.author}, "
                    f"Title: {book.title}, "
                    f"Publisher: {book.publisher}, "
                    f"Genre: {book.genre}, "
                    f"Year Published: {book.year_published}, "
                    f"Date Purchased: {book.date_purchased.strftime('%d-%m-%Y')}, "
                    f"Status: {book.status}"
                )
            input("Press Enter to continue...")
            os.system('cls')

    # Create a function called update_book that takes in the following parameters:
    def update_book(self):

        proceed = input("Do you want to continue updating books? (y/n): ")
        if proceed.lower() == "n":
            os.system('cls')
            return

        while True:
            isbn = input("Enter book ISBN: ")
            if len(isbn) == 13 and isbn.isdigit():
                break  # Exit the loop if the ISBN is valid
            else:
                print("Invalid ISBN. ISBN must be exactly 13 digits and consist only of digits.")

        books_found = False

        for book in self.books:
            if isbn == book.isbn:
                print(
                    f'ISBN: {book.isbn}, '
                    f'Author: {book.author}, '
                    f'Title: {book.title}, '
                    f'Publisher: {book.publisher}, '
                    f'Genre: {book.genre}, '
                    f'Year Published: {book.year_published}, '
                    f'Date Purchased: {book.date_purchased.strftime("%d-%m-%Y")}, '
                    f'Status: {book.status}'
                )
                books_found = True
        if not books_found:
            print("No books found.")

        print("1.Author Name")
        print("2.Book Title")
        print("3.Publisher")
        print("4.Genre")
        print("5.Year Published")
        print("6.Date Purchased")
        print("7.Status")
        update_choice = input("Enter the choice that you want to update:")

        if update_choice == "1":
            while True:
                new_book_author = input("Enter the new author name of the book: ")
                if new_book_author.replace(" ", "").isalpha():
                    for book in self.books:
                        if isbn == book.isbn:
                            book.author = new_book_author
                            print("Author name updated successfully.")
                            self.write_to_file()
                            os.system('cls')
                            return  # exit the loop after updating the author
                else:
                    print("Invalid input. Please enter the author using only letters.")

        elif update_choice == "2":
            while True:
                new_book_name = input("Enter the new title name of the book: ")
                for book in self.books:
                    if isbn == book.isbn:
                        book.title = new_book_name
                        print("Book title name updated successfully")
                        self.write_to_file()
                        os.system('cls')
                        return  # exit the loop after updating the author
        elif update_choice == "3":
            while True:
                new_publisher = input("Enter the new publisher of the book: ")
                for book in self.books:
                    if isbn == book.isbn:
                        book.publisher = new_publisher
                        print("Book publisher updated successfully")
                        self.write_to_file()
                        os.system('cls')
                        return  # exit the loop after updating the author
        elif update_choice == "4":
            while True:
                new_genre = input("Enter the new genre of the book: ")
                if new_genre.replace(" ", "").isalpha():
                    for book in self.books:
                        if isbn == book.isbn:
                            book.genre = new_genre
                            print("Book genre updated successfully")
                            self.write_to_file()
                            os.system('cls')
                            return  # exit the loop after updating the author
                else:
                    print("Invalid input. Please enter the genre using only letters.")
        elif update_choice == "5":
            while True:
                new_year_published = input("Enter the new year published of the book: ")
                if new_year_published.replace(" ", "").isdigit():
                    for book in self.books:
                        if isbn == book.isbn:
                            book.year_published = new_year_published
                            print("Book published updated successfully")
                            self.write_to_file()
                            os.system('cls')
                            return  # exit the loop after updating the author
                else:
                    print("Invalid input. Please enter the author using only number.")
        elif update_choice == "6":
            while True:
                new_date_purchased = get_valid_date_input("Enter date purchased (DD-MM-YYYY): ", "%d-%m-%Y")
                for book in self.books:
                    if isbn == book.isbn:
                        book.date_purchased = new_date_purchased
                        print("Book date purchased updated successfully")
                        self.write_to_file()
                        os.system('cls')
                        return  # exit the loop after updating the author
        elif update_choice == "7":
            while True:
                new_status = input("Enter the new status of the book: ")
                if all(c.isalpha() or c == '-' for c in new_status):
                    for book in self.books:
                        if isbn == book.isbn:
                            book.status = new_status
                            print("Book status updated successfully")
                            self.write_to_file()
                            os.system('cls')
                            return  # exit the loop after updating the author
                else:
                    print("Invalid input. Please enter the genre using only letters and hyphens.")
        else:
            print("Invalid choice. Please try again.")
    os.system('cls')

    # Create a function called delete_book that takes in the following parameters:
    def delete_book(self):
        proceed = input("Do you want to continue deleting books? (y/n): ")
        if proceed.lower() == "n":
            os.system('cls')
            return

        for book in self.books:
            print(
                f"ISBN: {book.isbn}, "
                f"Author: {book.author}, "
                f"Title: {book.title}, "
                f"Publisher: {book.publisher}, "
                f"Genre: {book.genre}, "
                f"Year Published: {book.year_published}, "
                f"Date Purchased: {book.date_purchased.strftime('%d-%m-%Y')}, "
                f"Status: {book.status}"
            )
        while True:
            isbn = input("Enter book ISBN to delete: ")
            if len(isbn) == 13 and isbn.isdigit():
                break  # Exit the loop if the ISBN is valid
            else:
                print("Invalid ISBN. ISBN must be exactly 13 digits and consist only of digits.")
        for book in self.books:
            if book.isbn == isbn:
                self.books.remove(book)
                print("Book deleted successfully")
                self.write_to_file()
                os.system('cls')
                return
        print("Book not found.")
        os.system('cls')

    # Create a function called write_to_file that takes in the following parameters:
    def write_to_file(self):
        with open(self.filename, 'w') as f:
            for book in self.books:
                f.write(
                    f"{book.isbn},{book.author},{book.title},{book.publisher},"
                    f"{book.genre},{book.year_published},{book.date_purchased.strftime('%d-%m-%Y')},"
                    f"{book.status}\n"
                )

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

    # Create a function to search for books

    def search_books(self):
        proceed = input("Do you want to continue searching books? (y/n): ")
        if proceed.lower() == "n":
            os.system('cls')
            return
        while True:
            isbn = input("Enter book ISBN: ")
            if len(isbn) == 13 and isbn.isdigit():
                break  # Exit the loop if the ISBN is valid
            else:
                print("Invalid ISBN. ISBN must be exactly 13 digits and consist only of digits.")

        while True:
            author = input("Enter book author: ")
            if author.replace(" ", "").isalpha():
                break
            else:
                print("Invalid input. Please enter the author using only letters.")

        while True:
            title = input("Enter book title: ")
            break

        books_found = False  # Flag to check if any books match the criteria

        for book in self.books:
            if book.isbn == isbn or (book.author == author and book.title == title):
                print(
                    f"ISBN: {book.isbn}, "
                    f"Author: {book.author}, "
                    f"Title: {book.title}, "
                    f"Publisher: {book.publisher}, "
                    f"Genre: {book.genre}, "
                    f"YearPublished: {book.year_published}, "
                    f"Date Purchased: {book.date_purchased.strftime('%d-%m-%Y')}, "
                    f"Status: {book.status}"
                )
                books_found = True

        if not books_found:
            print("No books found.")


# Create an instance of the BookManagementSystem class and call the functions
bms = BookManagementSystem('books.txt')
while Run:
    print("1. Search book")
    print("2. View books")
    print("3. Update book")
    print("4. Add book")
    print("5. Delete book")
    print("6. Exit")

    choice = input("Enter your choice: ")
    if choice == "1":
        bms.search_books()
    elif choice == "2":
        bms.view_books()
    elif choice == "3":
        bms.update_book()
    elif choice == "4":
        bms.add_book()
    elif choice == "5":
        bms.delete_book()
    elif choice == "6":
        confirm_exit = input("Are you sure you want to exit the program?(y/n)").lower()
        if confirm_exit == 'y':
            break
        if confirm_exit == 'n':
            continue
    else:
        print("Invalid choice. Please try again.")

        try_again = input("Do you wish to try again? (y/n): ").lower()

        while try_again != "n" and try_again != "y":
            try_again = input("Do you wish to try again? (y/n): ").lower()
        if try_again == "n":
            Run = False

print("Thank you for using the Book Management System. Goodbye!")

