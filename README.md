# Library-check-In
The LibrarySystem class represents a simple library management system.  * It provides functionalities to add, display, search, and delete books,  * as well as write the library's contents to a file.
/**
 * The Book class represents a book in a library system.
 * It stores details such as the book's name, ID, and author.
 *
 * @author (Bernard Ginn)
 * @version (1.0)
 */
public class Book {
    private String name; // Name of the book
    private long id;     // Unique identifier for the book
    private String author; // Author of the book

    /**
     * Constructor for creating a new Book object.
     *
     * @param name   The name of the book.
     * @param id     The unique identifier for the book.
     * @param author The author of the book.
     */
    public Book(String name, long id, String author) {
        this.name = name;
        this.id = id;
        this.author = author;
    }

    /**
     * Gets the name of the book.
     *
     * @return The name of the book.
     */
    public String getName() {
        return name;
    }

    /**
     * Sets the name of the book.
     *
     * @param name The new name of the book.
     */
    public void setName(String name) {
        this.name = name;
    }

    /**
     * Gets the ID of the book.
     *
     * @return The ID of the book.
     */
    public long getId() {
        return id;
    }

    /**
     * Sets the ID of the book.
     *
     * @param id The new ID of the book.
     */
    public void setId(long id) {
        this.id = id;
    }

    /**
     * Gets the author of the book.
     *
     * @return The author of the book.
     */
    public String getAuthor() {
        return author;
    }

    /**
     * Sets the author of the book.
     *
     * @param author The new author of the book.
     */
    public void setAuthor(String author) {
        this.author = author;
    }

    /**
     * Returns a string representation of the Book object.
     * This includes the name, author, and ID of the book.
     *
     * @return A string representation of the book.
     */
    @Override
    public String toString() {
        return "Name: " + name + "\nAuthor: " + author + "\nID: " + id;
    }
}
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Scanner;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
/**
 * The LibrarySystem class represents a simple library management system.
 * It provides functionalities to add, display, search, and delete books,
 * as well as write the library's contents to a file.
 *
 * @author (Bernard Ginn)
 * @version (1.0)
 */
    public class LibrarySystem
    {
        public static void main (String[] args){
            /**
             * The main method serves as the entry point of the program.
             * It provides a menu for the user to interact with the library system.
             *
             * @param args Command line arguments (not used in this application).
             */
            ArrayList<Book> library = new ArrayList<>();
            HashMap<String,Book> searchByID = new HashMap<>();
            Scanner scan = new Scanner(System.in);
            System.out.println("Welcome to our Libray,");
            int option;
            do{
                menu();
                System.out.print("\nPlease choose from the above actions: ");
                option = scan.nextInt();
                System.out.println(" You Entered:" + option);
                switch(option){
                    case 1 -> {
                        Book book = addBook(scan);
                        library.add(book);
                        searchByID.put(String.valueOf(book.getId()), book);


                    }
                    case 2 -> {
                        displayBook(library,scan);

                    }
                    case 3->  {
                        displayAllBook(library,scan);

                    }
                    case 4 -> {
                        writeLibraryToFile(library);

                    }
                    case 5->{
                        SearchBookByID(searchByID,library,scan);


                    }
                    case 6->{
                        deleteBook(library,scan);

                    }
                    case 7 ->{
                        System.out.println(" thank you");
                    }
                    default -> System.out.printf("Sorry," +option+ "is not a valid option.");
                }
            } while (option != 7);
        }
        /**
         * Prompts the user to input details for a new book and adds it to the library.
         *
         * @param scan The scanner object for user input.
         * @return The newly created Book object.
         */
        public static Book addBook(Scanner scan) {
            scan.nextLine(); // Clear the newline from the buffer
            String name = "";
            while (true){
                // Get the book's name from the user
                System.out.println("Please enter the name of the book:");
                name = scan.nextLine();
                if (noNumbers(name)){
                    System.out.println(" no numbers please");
                } else{
                    break;
                }
            }
            System.out.println("You entered: " + name);
            String id;
            String idStr;
            while (true) {
                System.out.println("Please enter the book ID (must be 10 digits long):");
                idStr = scan.nextLine();
                if (idStr.matches("\\d{10}")) {
                    break; // ID is valid, break out of the loop
                } else {
                    System.out.println("Invalid ID. The ID must be exactly 10 digits long.");
                }
            }
            String author = "";
            while(true){
                System.out.println("Please enter the book author:");
                 author = scan.nextLine();
                 if(noNumbers(author)){
                     System.out.println(" no Number please");
                 }else{
                     break;
                 }
            }
            System.out.println("You entered: " + author);
            long longId = Long.parseLong(idStr);
            Book newBook = new Book(name, longId, author);
            // Return the new book
            return newBook;
        }
        /**
         * Displays a specific book based on user input.
         *
         * @param library The list of books in the library.
         * @param scan The scanner object for user input.
         */
        public static void displayBook(ArrayList<Book> library,Scanner scan) {
            scan.nextLine();
            System.out.println("please enter the name of the book you want to display");
            String userBook = scan.nextLine();
            System.out.println(" you entered" + userBook);
            boolean isFound = false;
            for (int i = 0; i < library.size(); i++) {
                if (library.get(i).getName().equals(userBook)) {
                    System.out.println(library.get(i).toString());
                    isFound = true;
                }
            }
            if (!isFound) {
                System.out.println(" sorry i could not find" + userBook);
            }
        }

        /**
         * Displays all books in the library.
         *
         * @param library The list of books in the library.
         */
        public static void displayAllBook(ArrayList<Book> library, Scanner scan){
            System.out.println("Displaying"+ library.size() + "book");
            if(library.size() > 0){
                for(Book books : library){
                    System.out.println(books);
                }
            }
        }
        /**
         * Writes the current state of the library to a text file.
         *
         * @param library The list of books to be written to the file.
         */
        public static void writeLibraryToFile(ArrayList<Book> library){
            LocalDateTime now = LocalDateTime.now();
            DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss");
            String timeStamp = now.format(formatter);
            String fileName = "library_" +timeStamp+ ".txt";
            try (BufferedWriter writer = new BufferedWriter((new FileWriter(fileName)))){
                for (Book book : library){
                    writer.write(book.toString());
                    writer.newLine();

                }
                System.out.println("Library written to " + fileName);
            } catch (IOException e){
                System.out.println(" a error have occured");
            }
        }
        /**
         * Searches for a book by its ID and displays it.
         *
         * @param searchById The map containing books indexed by their ID.
         * @param scan The scanner object for user input.
         */
        public static void SearchBookByID(HashMap<String, Book> searchById, ArrayList<Book> Library, Scanner scan) {
            System.out.println("Please enter the book ID to search:");
            long id = scan.nextLong();
            String idStr = String.valueOf(id); // Convert long ID to String
            Book foundBook = searchById.get(idStr); // Use String ID for searching

            if (foundBook != null) {
                System.out.println("Book found: " + foundBook);
            } else {
                System.out.println("No book is found with ID: " + idStr);
            }
        }

        /**
         * Removes a book from the library based on user input.
         *
         * @param library The list of books in the library.
         * @param scan The scanner object for user input.
         */
        public static void deleteBook(ArrayList<Book> library, Scanner scan){
            scan.nextLine();
            System.out.println("please enter the name or ID of the book you want to delete:");
            String userInput = scan.nextLine();
            boolean isFound = false;
            for(int i = 0 ; i< library.size();i++){
                Book book = library.get(i);
                if(book.getName().equals(userInput)|| String.valueOf(book.getId()).equals(userInput)){
                    library.remove(i);
                    System.out.println("Book removed:" + book);
                    isFound = true;
                    break;
                }
            }
            if (!isFound){
                System.out.println("book not found");
            }
}
        /**
         * Checks if a string contains any numeric values.
         *
         * @param str The string to be checked.
         * @return true if the string contains numbers, false otherwise.
         */
public static boolean noNumbers(String str){
            try {
                Double.parseDouble(str);
                return true;
            } catch (NumberFormatException e){
                return false;
            }
}

        /**
         * This method displays the main menu options for the library system.
         */
        public static void menu(){
            System.out.println("Action:");
            System.out.println("1) Add a book");
            System.out.println("2) Display an book");
            System.out.println("3) Display all books");
            System.out.println("4) Add the library to a file");
            System.out.println("5) search book by ID");
            System.out.println("6) delete book");
            System.out.println("7) Quit");
        }
    }

