# SOLID Principles:

## Single Responsibility Principle (SRP):

A class or module should have only one reason to change, meaning it should only have one responsibility or job.

In Angular, it's important that components, services, and modules adhere to SRP. For example, an Angular component should only handle UI logic (like rendering HTML, handling user interactions) and leave other tasks (like fetching data or processing business logic) to services. This separation ensures that if UI logic changes, we only need to modify the component without touching other parts of the app.

For instance, an AuthService should be responsible exclusively for authentication. This fosters a clear separation of concerns and facilitates maintenance and testing.

In this example, the Single Responsibility Principle (SRP) is applied through the design of the AuthService class, which is focused on a single responsibility: handling user authentication.
  
* AuthService:
This service is dedicated solely to authentication-related tasks. It includes methods for:
  * login(username: string, password: string): This method handles user login, verifying credentials through an API and returning a user object as an observable.
  * logout(): This method manages user logout, clearing session data or notifying the server.

By following SRP, AuthService has a clear purpose, making it easier to maintain and test. If changes are needed in authentication (like updating the login process), you only modify this service, reducing the risk of bugs elsewhere. SRP helps keep the code organized and easier to understand.

    @Injectable()
    export class AuthService {
      constructor(private httpClient: HttpClient) {}
    
      login(username: string, password: string): Observable<User> {
        // Implementation of login logic
      }
    
      logout(): void {
        // Implementation of logout logic
      }
    }

### Real-world use cases:

*Imagine a baker who is responsible for baking bread. The baker’s role is to focus on the task of baking bread, ensuring that the bread is of high quality, properly baked, and meets the bakery’s standards.*

• However, if the baker is also responsible for managing the inventory, ordering supplies, serving customers, and cleaning the bakery, this would violate the SRP.

• Each of these tasks represents a separate responsibility, and by combining them, the baker’s focus and effectiveness in baking bread could be compromised.

• To adhere to the SRP, the bakery could assign different roles to different individuals or teams. For example, there could be a separate person or team responsible for managing the inventory, another for ordering supplies, another for serving customers, and another for cleaning the bakery.

## Open/Closed Principle (OCP):

Software entities (classes, modules, functions) should be open for extension but closed for modification.

This principle encourages writing code that can be extended without changing the existing code. In Angular, this can be achieved by using inheritance or by creating flexible services and injecting different implementations through Angular's dependency injection system.

An Angular LoggerService could implement various logger strategies, which can be swapped by Dependency Injection, without altering the class itself. This boosts the flexibility and reusability of code.

In this example, the Open/Closed Principle (OCP) is applied, meaning that the LoggerService class is open for extension but closed for modification.

* LoggerService:
This class uses a LoggerStrategy to handle logging. By default, it uses ConsoleLogger, but it allows for the logging strategy to be changed at runtime using the setStrategy() method.

* LoggerStrategy (Interface):
This interface defines a contract for logging, allowing different logging implementations (strategies) to be used.

* ConsoleLogger (Concrete Implementation):
This class implements the LoggerStrategy interface and logs messages to the console.

The LoggerService is open for extension because you can add new logging strategies (e.g., FileLogger, CloudLogger) without modifying the existing LoggerService class. This adheres to the Open/Closed Principle because the service can be extended by introducing new strategies without altering its core functionality, keeping the code flexible and maintainable.

    @Injectable()
    export class LoggerService {
      private loggerStrategy: LoggerStrategy;
    
      constructor() {
        this.loggerStrategy = new ConsoleLogger(); // Default Strategy
      }
    
      setStrategy(strategy: LoggerStrategy) {
        this.loggerStrategy = strategy;
      }
    
      log(message: string) {
        this.loggerStrategy.log(message);
      }
    }
    
    interface LoggerStrategy {
      log(message: string): void;
    }
    
    class ConsoleLogger implements LoggerStrategy {
      log(message: string) {
        console.log(message);
      }
    }

### Real-world use cases:

*Imagine you have a class called PaymentProcessor that processes payments for an online store. Initially, the PaymentProcessor class only supports processing payments using credit cards. However, you want to extend its functionality to also support processing payments using PayPal.*

• Instead of modifying the existing PaymentProcessor class to add PayPal support, you can create a new class called PayPalPaymentProcessor that extends the PaymentProcessor class. This way, the PaymentProcessor class remains closed for modification but open for extension, adhering to the Open-Closed Principle. Let’s understand this through the code implementation.

## Liskov Substitution Principle (LSP):

Objects of a superclass should be replaceable with objects of a subclass without affecting the functionality of the program.

When using inheritance in Angular services or components, it's important to ensure that subclasses can replace their parent classes without breaking the application. In other words, derived classes should work in any place where the parent class is expected.

In Angular, an ExtendedService that inherits from a BaseService can provide additional functions while preserving the basic functionality.

In this example, the Liskov Substitution Principle (LSP) is applied by ensuring that the subclass (ExtendedService) can replace the base class (BaseService) without altering the correctness of the program.

* BaseService:
This class provides a basic implementation of the getData() method, which returns some data.

* ExtendedService:
This subclass extends BaseService and overrides the getData() method to add extra logic. However, it still calls the super.getData() method to retain the behavior of the base class, ensuring that ExtendedService can be used wherever BaseService is expected without breaking functionality.

By following LSP, any instance of ExtendedService can replace BaseService in the application without causing issues. The overridden getData() method maintains the expected behavior, ensuring that both the base class and subclass can be used interchangeably. This promotes consistency and ensures that the code is reliable and extensible.

    class BaseService {
      getData(): any {
        // Basic implementation
      }
    }
    
    @Injectable()
    export class ExtendedService extends BaseService {
      getData(): any {
        let data = super.getData();
        // Extended Logic
        return data;
      }
    }

### Real-world use cases:

*One of the classic examples of this principle is a rectangle having four sides. A rectangle’s height can be any value and width can be any value. A square is a rectangle with equal width and height. So we can say that we can extend the properties of the rectangle class into square class.*

• In order to do that you need to swap the child (square) class with parent (rectangle) class to fit the definition of a square having four equal sides but a derived class does not affect the behavior of the parent class so if you will do that it will violate the Liskov Substitution Principle.

## Interface Segregation Principle (ISP):

Clients should not be forced to depend on methods they do not use. It's better to have multiple, smaller interfaces than a large, "fat" interface.

This principle can be applied when creating Angular services or interfaces. Instead of having one large service or interface that includes many unrelated methods, it’s better to break them into smaller, more specific ones.

An example would be a UserService split into specific interfaces like UserAuthentication and UserDataManagement.

In this example, the Interface Segregation Principle (ISP) is applied by separating concerns into two smaller interfaces: UserAuthentication and UserDataManagement. Each interface has a specific responsibility:

* UserAuthentication:
This interface defines the login() method, focusing only on user authentication (i.e., logging in users).

* UserDataManagement:
This interface defines the getUserData() method, focusing on retrieving user-related data after authentication.

* UserService:
This class implements both interfaces (UserAuthentication and UserDataManagement), meaning it provides both the login functionality and the ability to fetch user data.

By splitting the functionality into smaller interfaces, the Interface Segregation Principle ensures that classes only implement the methods they need. This way, clients that only care about user authentication don't have to depend on the methods for managing user data, and vice versa. This promotes cleaner, more modular, and maintainable code.

    interface UserAuthentication {
      login(user: string, password: string): Observable<User>;
    }
    
    interface UserDataManagement {
      getUserData(userId: number): Observable<UserData>;
    }
    
    @Injectable()
    export class UserService implements UserAuthentication, UserDataManagement {
      login(user: string, password: string): Observable<User> {
        // Login logic
      }
    
      getUserData(userId: number): Observable<UserData> {
        // Retrieve user data
      }
    }

### Real-world use cases:

*Suppose if you enter a restaurant and you are pure vegetarian. The waiter in that restaurant gave you the menu card which includes vegetarian items, non-vegetarian items, drinks, and sweets.*

• In this case, as a customer, you should have a menu card which includes only vegetarian items, not everything which you don’t eat in your food. Here the menu should be different for different types of customers.

• The common or general menu card for everyone can be divided into multiple cards instead of just one. Using this principle helps in reducing the side effects and frequency of required changes.

## Dependency Inversion Principle (DIP):

High-level modules should not depend on low-level modules. Both should depend on abstractions. Also, abstractions should not depend on details. Details should depend on abstractions. It dictates that dependencies should be based on abstractions and not on concretizations.

Angular’s dependency injection system makes it easy to follow this principle. Instead of tightly coupling components and services, Angular allows you to inject services into components. This way, the component only relies on an abstract interface, and you can easily swap out different implementations of the service without modifying the component.

A BookComponent should depend on an abstract BookService rather than a specific implementation, reducing coupling and enhancing testability.

In this example, the Dependency Inversion Principle (DIP) is applied by having the BookComponent depend on an abstraction (BookService) rather than a concrete implementation (ConcreteBookService).

* BookService (Abstraction):
This is an abstract class that defines a contract with the getBooks() method, ensuring that any implementation will provide an observable stream of book data.

* ConcreteBookService (Implementation):
This class extends BookService and provides the actual implementation of the getBooks() method, which might fetch books from an API or another source.

* BookComponent (High-level Module):
The BookComponent depends on the abstract BookService, not the specific implementation. This allows it to work with any service that implements BookService.

By applying DIP, BookComponent doesn’t need to know about ConcreteBookService. If you want to switch to a different book service (e.g., MockBookService for testing), you can inject it without changing the component. This makes the code more flexible and easier to maintain.

abstract class BookService {
  abstract getBooks(): Observable<Book[]>;
}

    @Injectable()
    export class ConcreteBookService extends BookService {
      getBooks(): Observable<Book[]> {
        // Specific implementation
      }
    }
    
    @Component({
      selector: 'app-book',
      template: `...`
    })
    export class BookComponent {
      books: Book[];
    
      constructor(private bookService: BookService) {
        this.bookService.getBooks().subscribe(books => this.books = books);
      }
    }

### Real-world use cases:

*In a software development team, developers depend on an abstract version control system (e.g., Git) to manage and track changes to the codebase. They don’t depend on specific details of how Git works internally.*

• This allows developers to focus on writing code without needing to understand the intricacies of version control implementation.

### References:

https://angular.love/angular-and-solid-principles

https://newcubator.com/blog/web/solid-principles-in-angular

https://www.geeksforgeeks.org/solid-principle-in-programming-understand-with-real-life-examples/
