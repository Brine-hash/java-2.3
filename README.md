import java.util.*;
import java.util.stream.*;

class Employee {
    private String name;
    private int age;
    private double salary;

    public Employee(String name, int age, double salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }
    public String getName() { return name; }
    public int getAge() { return age; }
    public double getSalary() { return salary; }
    @Override
    public String toString() {
        return name + " - Age: " + age + ", Salary: " + salary;
    }
}

class Student {
    private String name;
    private double marks;

    public Student(String name, double marks) {
        this.name = name;
        this.marks = marks;
    }
    public String getName() { return name; }
    public double getMarks() { return marks; }
    @Override
    public String toString() {
        return name + " - Marks: " + marks;
    }
}

class Product {
    private String name;
    private double price;
    private String category;

    public Product(String name, double price, String category) {
        this.name = name;
        this.price = price;
        this.category = category;
    }
    public String getName() { return name; }
    public double getPrice() { return price; }
    public String getCategory() { return category; }
    @Override
    public String toString() {
        return name + " - " + category + " - " + price;
    }
}

public class LambdaAndStreamTasks {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("Choose Task:");
        System.out.println("1. Sort Employees (Lambda Expressions)");
        System.out.println("2. Filter and Sort Students (Streams)");
        System.out.println("3. Product Stream Operations");
        System.out.print("Enter choice: ");
        int choice = sc.nextInt();

        switch (choice) {
            case 1:
                sortEmployees();
                break;
            case 2:
                filterAndSortStudents();
                break;
            case 3:
                productStreamOperations();
                break;
            default:
                System.out.println("Invalid choice.");
        }
        sc.close();
    }

    // ---------- PART (a) ----------
    public static void sortEmployees() {
        List<Employee> employees = new ArrayList<>();
        employees.add(new Employee("Ravi", 28, 45000));
        employees.add(new Employee("Anita", 32, 55000));
        employees.add(new Employee("Karan", 25, 40000));
        employees.add(new Employee("Meena", 29, 60000));

        System.out.println("\nSort by Name (Alphabetically):");
        employees.sort((e1, e2) -> e1.getName().compareTo(e2.getName()));
        employees.forEach(System.out::println);

        System.out.println("\nSort by Age (Ascending):");
        employees.sort((e1, e2) -> Integer.compare(e1.getAge(), e2.getAge()));
        employees.forEach(System.out::println);

        System.out.println("\nSort by Salary (Descending):");
        employees.sort((e1, e2) -> Double.compare(e2.getSalary(), e1.getSalary()));
        employees.forEach(System.out::println);
    }

    // ---------- PART (b) ----------
    public static void filterAndSortStudents() {
        List<Student> students = Arrays.asList(
            new Student("Ravi", 82),
            new Student("Anita", 65),
            new Student("Karan", 91),
            new Student("Meena", 74),
            new Student("Priya", 88)
        );

        System.out.println("\nStudents with Marks > 75 (Sorted by Marks):");
        students.stream()
                .filter(s -> s.getMarks() > 75)
                .sorted(Comparator.comparingDouble(Student::getMarks))
                .map(Student::getName)
                .forEach(System.out::println);
    }

    // ---------- PART (c) ----------
    public static void productStreamOperations() {
        List<Product> products = Arrays.asList(
            new Product("Laptop", 75000, "Electronics"),
            new Product("Phone", 50000, "Electronics"),
            new Product("Shirt", 1500, "Clothing"),
            new Product("Jeans", 2000, "Clothing"),
            new Product("Watch", 8000, "Accessories"),
            new Product("Bag", 2500, "Accessories")
        );

        Map<String, List<Product>> groupedByCategory =
                products.stream().collect(Collectors.groupingBy(Product::getCategory));

        System.out.println("\nProducts Grouped by Category:");
        groupedByCategory.forEach((cat, list) -> System.out.println(cat + ": " + list));

        Map<String, Optional<Product>> maxPriceByCategory =
                products.stream().collect(Collectors.groupingBy(
                        Product::getCategory,
                        Collectors.maxBy(Comparator.comparingDouble(Product::getPrice))
                ));

        System.out.println("\nMost Expensive Product in Each Category:");
        maxPriceByCategory.forEach((cat, prod) ->
            System.out.println(cat + ": " + prod.get().getName() + " - " + prod.get().getPrice())
        );

        double avgPrice = products.stream()
                                  .collect(Collectors.averagingDouble(Product::getPrice));

        System.out.println("\nAverage Price of All Products: " + avgPrice);
    }
}
