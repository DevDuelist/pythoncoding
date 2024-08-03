# pythoncoding
coding
import json
import os

class Contact:
    def __init__(self, name, phone, email):
        self.name = name
        self.phone = phone
        self.email = email

    def __str__(self):
        return f"Name: {self.name}, Phone: {self.phone}, Email: {self.email}"

class ContactBook:
    def __init__(self, filename="contacts.json"):
        self.filename = filename
        self.contacts = []
        self.load_contacts()

    def load_contacts(self):
        if os.path.exists(self.filename):
            with open(self.filename, "r") as file:
                contacts_data = json.load(file)
                self.contacts = [Contact(**data) for data in contacts_data]
        else:
            self.contacts = []

    def save_contacts(self):
        with open(self.filename, "w") as file:
            json.dump([contact.__dict__ for contact in self.contacts], file, indent=4)

    def add_contact(self, name, phone, email):
        new_contact = Contact(name, phone, email)
        self.contacts.append(new_contact)
        self.save_contacts()

    def remove_contact(self, name):
        self.contacts = [contact for contact in self.contacts if contact.name != name]
        self.save_contacts()

    def edit_contact(self, old_name, new_name, new_phone, new_email):
        for contact in self.contacts:
            if contact.name == old_name:
                contact.name = new_name
                contact.phone = new_phone
                contact.email = new_email
                self.save_contacts()
                break

    def display_contacts(self):
        if not self.contacts:
            print("No contacts found.")
        else:
            for contact in self.contacts:
                print(contact)
                print("-" * 30)

def main():
    contact_book = ContactBook()

    while True:
        print("\nContact Book Menu")
        print("1. Add contact")
        print("2. Remove contact")
        print("3. Edit contact")
        print("4. View contacts")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            name = input("Enter contact name: ")
            phone = input("Enter contact phone: ")
            email = input("Enter contact email: ")
            contact_book.add_contact(name, phone, email)
        elif choice == "2":
            name = input("Enter the name of the contact to remove: ")
            contact_book.remove_contact(name)
        elif choice == "3":
            old_name = input("Enter the name of the contact to edit: ")
            new_name = input("Enter new contact name: ")
            new_phone = input("Enter new contact phone: ")
            new_email = input("Enter new contact email: ")
            contact_book.edit_contact(old_name, new_name, new_phone, new_email)
        elif choice == "4":
            contact_book.display_contacts()
        elif choice == "5":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
