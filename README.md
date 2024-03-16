# Challenge_03
## Class exercise 
1. Create class Line.
length, slope, start, end: Instance attributes, two of them being points (so a line is composed at least of two points).
compute_length(): should return the line´s length
compute_slope(): should return the slope of the line from the horizontal in deg.
compute_horizontal_cross(): should return if exists the intersection with x-axis
compute_vertical_cross(): should return if exists the intersection with y-axis

```python
import math
class Point:
    
    def __init__(self, x: float=0, y: float=0):
        self.x = x 
        self.y = y  

    def move(self, new_x: float, new_y: float):
        self.x = new_x
        self.y = new_y
    
    def reset(self):
        self.x = 0
        self.y = 0

class Line: 
    def __init__(self, start: Point, end: Point): 
        self.start = start
        self.end = end
        self.length = self.compute_distance(self.start, self.end)
        self.slope = self.compute_slope(self.start, self.end)

    def calculate_length(self):
        return math.sqrt((self.end.x - self.start.x)**2 + (self.end.y - self.start.y)**2)

    def compute_distance(self, start:Point, end:Point)-> float:
        self.lenght = ((((((end.x) - (start.x))**2))+(((end.y)-(start.y))**2))**0.5)
        return self.lenght
    def compute_slope(self, start:Point, end:Point)-> float:
        self.radslope = ((end.y)-(start.y)/(end.x)-(start.x))
        self.degslope = ((self.radslope * 180) / 3.141592653)
        return self.degslope
    def compute_horizontal_cross(self, start:Point, end:Point)-> bool: 
        if (end.y * start.y) <= 0: #if the values on "y" have opposite signs we have a negative number 
            return True #there is a horizontal cross
        else: 
            return False 
    def compute_vertical_cross(self, start:Point, end:Point)-> bool:
        if (end.x * start.x) <= 0: 
            return True
        else: 
            return False
```
## Explanation
The code begins by importing the math module, which provides mathematical functions defined by the C standard. Then, a Point class is defined to represent a point in a 2D space with x and y coordinates. The Point class has an __init__ method to initialize a point, a move method to change the coordinates of the point, and a reset method to set the coordinates back to the origin (0, 0). After the Point class, a Line class is defined to represent a line segment in a 2D space. The Line class has an __init__ method that initializes a line with a start point and an end point, and also calculates the length and slope of the line. The Line class also has methods to calculate the length of the line, compute the distance between the start and end points, compute the slope of the line in degrees, and check if the line crosses the horizontal or vertical axis.

2. Redefine the class Rectangle, adding a new method of initialization using 4 Lines (composition at its best, a rectangle is compose of 4 lines).
```python
##
class Rectangle:
    def __init__(self, method, *args):
      
        if method == 'points':
            self.init_from_points(*args)
        elif method == 'lines':
            self.init_from_lines(*args)
        elif method == 'point_and_dimensions':
            self.init_from_point_and_dimensions(*args)
        elif method == 'center_and_dimensions':
            self.init_from_center_and_dimensions(*args)
        else:
            raise ValueError("Método de inicialización no reconocido.")

    def init_from_points(self, top_left, bottom_right):
        if not (top_left.x < bottom_right.x and top_left.y > bottom_right.y): #A rectangle is valid if the top left point is above and to the left of the bottom right point.
            raise ValueError("The provided points cannot form a valid rectangle.")
        self.top_left = top_left
        self.bottom_right = bottom_right
        self.top_right = Point(bottom_right.x, top_left.y)
        self.bottom_left = Point(top_left.x, bottom_right.y)
        self.lines = [Line(self.top_left, self.top_right), Line(self.top_right, self.bottom_right),
                      Line(self.bottom_right, self.bottom_left), Line(self.bottom_left, self.top_left)]

    def init_from_lines(self, top_line, bottom_line, left_line, right_line):
        if not (top_line.start.x < top_line.end.x and left_line.start.y > left_line.end.y): #A rectangle is valid if the top line is horizontal and the left line is vertical.
            raise ValueError("The provided lines cannot form a valid rectangle.")
        self.lines = [top_line, bottom_line, left_line, right_line]
        self.top_left = top_line.start
        self.bottom_right = bottom_line.end
        self.top_right = top_line.end
        self.bottom_left = left_line.start

    def init_from_point_and_dimensions(self, top_left, width, height):
        if not (width > 0 and height > 0): #A rectangle is valid if both the width and height are greater than zero.
            raise ValueError("The provided dimensions cannot form a valid rectangle.")
        self.top_left = top_left
        self.bottom_right = Point(top_left.x + width, top_left.y - height)
        self.init_from_points(self.top_left, self.bottom_right)

    def init_from_center_and_dimensions(self, center, width, height):
        if not (width > 0 and height > 0): #A rectangle is valid if both the width and height are greater than zero.
            raise ValueError("The provided dimensions cannot form a valid rectangle.")
        half_width = width / 2
        half_height = height / 2
        top_left = Point(center.x - half_width, center.y + half_height)
        bottom_right = Point(center.x + half_width, center.y - half_height)
        self.init_from_points(top_left, bottom_right)

    def compute_area(self):
        width = abs(self.top_left.x - self.bottom_right.x)
        height = abs(self.top_left.y - self.bottom_right.y)
        return width * height

    def compute_perimeter(self):
        width = abs(self.top_left.x - self.bottom_right.x)
        height = abs(self.top_left.y - self.bottom_right.y)
        return 2 * (width + height)

def create_rectangle():
    print("Choose a method to create a rectangle:")
    print("1. From points")
    print("2. From lines")
    print("3. From point and dimensions")
    print("4. From center and dimensions")

    valid_choice = False
    while not valid_choice:
        try:
            method = int(input("Enter your choice (1-4): "))
            if method in [1, 2, 3, 4]:
                valid_choice = True
            else:
                print("Invalid choice. Please enter a number between 1 and 4.")
        except ValueError:
            print("Invalid input. Please enter a number.")

    if method == 1:
        print("Enter the coordinates for the top left and bottom right points:")
        top_left = Point(float(input("Top left x: ")), float(input("Top left y: ")))
        bottom_right = Point(float(input("Bottom right x: ")), float(input("Bottom right y: ")))
        rectangle = Rectangle('points', top_left, bottom_right)
        rectangle.points = [top_left, bottom_right]
    elif method == 2:
        print("Enter the coordinates for the start and end points of each line:")
        top_line = Line(Point(float(input("Top line start x: ")), float(input("Top line start y: "))),
                        Point(float(input("Top line end x: ")), float(input("Top line end y: "))))
        bottom_line = Line(Point(float(input("Bottom line start x: ")), float(input("Bottom line start y: "))),
                           Point(float(input("Bottom line end x: ")), float(input("Bottom line end y: "))))
        left_line = Line(Point(float(input("Left line start x: ")), float(input("Left line start y: "))),
                         Point(float(input("Left line end x: ")), float(input("Left line end y: "))))
        right_line = Line(Point(float(input("Right line start x: ")), float(input("Right line start y: "))),
                          Point(float(input("Right line end x: ")), float(input("Right line end y: "))))
        rectangle = Rectangle('lines', top_line, bottom_line, left_line, right_line)
    elif method == 3:
        print("Enter the coordinates for the top left point and the dimensions:")
        top_left = Point(float(input("Top left x: ")), float(input("Top left y: ")))
        width = float(input("Width: "))
        height = float(input("Height: "))
        rectangle = Rectangle('point_and_dimensions', top_left, width, height)
    elif method == 4:
        print("Enter the coordinates for the center point and the dimensions:")
        center = Point(float(input("Center x: ")), float(input("Center y: ")))
        width = float(input("Width: "))
        height = float(input("Height: "))
        rectangle = Rectangle('center_and_dimensions', center, width, height)
    else:
        print("Invalid choice.")
        return None
    

    print(f"Area: {rectangle.compute_area()}")
    print(f"Perimeter: {rectangle.compute_perimeter()}")

    return rectangle

if __name__ == "__main__":
    create_rectangle()
```
## Explanation 
The Rectangle class is defined to represent a rectangle in a 2D space. It has several methods to initialize a rectangle from different types of input: init_from_points, init_from_lines, init_from_point_and_dimensions, and init_from_center_and_dimensions. Each of these methods sets the four corners of the rectangle and the four lines that form the rectangle. The Rectangle class also has a compute_area method to calculate the area of the rectangle, a compute_perimeter method to calculate the perimeter.

The create_rectangle function is a user-interactive function that creates a Rectangle object based on user input. It first prompts the user to choose a method for creating a rectangle. Depending on the user's choice, it then asks for further inputs:
If the user chooses '1', it asks for the coordinates of two points (top left and bottom right), and creates a rectangle using these points.
If the user chooses '2', it asks for the coordinates of four lines (top, bottom, left, right), and creates a rectangle using these lines.
If the user chooses '3', it asks for the coordinates of a point (top left) and two dimensions (width and height), and creates a rectangle using this point and dimensions.
If the user chooses '4', it asks for the coordinates of a point (center) and two dimensions (width and height), and creates a rectangle using this center point and dimensions.
After creating the rectangle, it prints the area and perimeter of the rectangle by calling the compute_area and compute_perimeter methods of the Rectangle class.
The if __name__ == "__main__": line checks if this script is being run directly (as opposed to being imported as a module). If it is, it calls the create_rectangle function.

# WELCOME TO THE PYTHON'S PSYCHIATRIC RESTAURANT!
Restaurant scenario: You want to design a program to calculate the bill for a customer's order in a restaurant.
Define a base class MenuItem: This class should have attributes like name, price, and a method to calculate the total price.
```python
# Define the base class for menu items
class MenuItem:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def calculate_total_price(self, quantity):
        return self.price * quantity
```
The base class is MenuItem. This class represents a general item that can be ordered from the menu in a restaurant. It has two attributes: name and price.

__init__(self, name, price): This is the constructor method that's called when you create a new instance of the class. It takes two parameters: name and price. These parameters are used to set the name and price attributes of the MenuItem instance.

calculate_total_price(self, quantity): This method calculates the total price for a given quantity of the menu item. It multiplies the price attribute of the MenuItem instance by the quantity parameter and returns the result.

2. Create subclasses for different types of menu items: Inherit from MenuItem and define properties specific to each type (e.g., Beverage, Appetizer, MainCourse).

```python
# Define subclasses for different types of menu items

class FoodItem:
    def __init__(self, name, price, category):
        self.name = name
        self.price = price
        self.category = category

class Starters(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Starters")

class Soupes(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Soupes")

class MainCourse(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Main Course")

class Drinks(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Drinks")

class Dessert(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Dessert")
```
### Explamation 
FoodItem: This is the base class for all items that can be ordered in the restaurant. It has three attributes: name, price, and category.

Starters, Soupes, MainCourse, Drinks, Dessert: These classes inherit from FoodItem. They represent specific types of menu items. Each of these classes calls the FoodItem constructor (super().__init__(name, price, category)) to set the name, price, and category attributes. The category is hard-coded for each class (e.g., "Soupes" for the Soupes class).

3. Define an Order class: This class should have a list of MenuItem objects and methods to add items, calculate the total bill amount, and potentially apply specific discounts based on the order composition.
```python
# Define the Order class
class Order:
    def __init__(self):
        self.items = []

    def add_item(self, item, quantity):
        self.items.append((item, quantity))

    def calculate_total_price(self):
        total = 0
        for item, quantity in self.items:
            total += item.price * quantity
        return total
    
    def ask_for_discount(self):
        categories = ["programmer", "mentally_ill", "student", "senior_citizen", "veteran", "unemployed", "homeless", "refugee", "disabled"]
        print("Please enter your population category. if you are not in any of these categories, please press enter")
        print("Available categories are:")
        for category in categories:
            print(category)
        user_category = input()
        if user_category in categories:
            return 10
        else:
            print("Sorry, you are not eligible for a discount.")
            return 0


    def apply_discount(self):
        discount = self.ask_for_discount()
        total_bill = self.calculate_total_price()
        return total_bill - (total_bill * discount / 100)
```
### Explanation 
The Order class represents a customer's order in the restaurant. _init__(self): the constructor method initializes an empty list self.items that will hold the items in the order.

add_item(self, item, quantity): This method adds an item to the order. The item is a FoodItem instance, and quantity is the number of that item to add. The item and quantity are stored as a tuple in the self.items list.

calculate_total_price(self): This method calculates the total price of the order by iterating over the items in self.items and adding up the price of each item multiplied by its quantity.

ask_for_discount(self): This method asks the user to input their population category and checks if it's one of the categories that are eligible for a discount. If it is, the method returns a discount of 10%. If it's not, the method prints a message and returns a discount of 0%.

apply_discount(self): This method calls ask_for_discount to get the discount percentage. 

4. Menu for interaction with the user
```python
# Create instances of menu items
menu_items = [
    Starters("Diazepam, ONLY_FOR_SALE_WITH_PRESCRIPTION", 1.99,),
    Starters("Xanax, ONLY_FOR_SALE_WITH_PRESCRIPTION", 0.99),
    Starters("Propanonol, ONLY_FOR_SALE_WITH_PRESCRIPTION", 0.99), 
    Soupes("Programmer's tears with Mexican tortilla", 5.99),
    Soupes("Spinach with tomato", 3.99),
    Soupes("Lentil with bacon", 4.99),
    MainCourse("Korean boy beef burger", 201.99),
    MainCourse("Chicken fetus crepe", 122.99),
    MainCourse("Vegan salad with tuna", 7.99),
    MainCourse("Turkey pizza", 100),
    MainCourse("Venezuelan Pasta", 0.99),
    MainCourse("Fish and Chips with cornflakes", 12.99),
    Drinks("Fanta", 1.99),
    Drinks("Taylor Swift's sweat", 900.99),
    Drinks("Coca Cola", 1.99),
    Drinks("Pepsi", 1.99),
    Drinks("Water from arctic glaciers", 300.99),
    Dessert("Ice cream with an american sausage", 2.99),
    Dessert("Fruit salad without fruit or transgenic fruit", 2.99),
    Dessert("Fruit salad with organic fruit", 200.99),
    Dessert("Snow white apple, REQUIRES_SIGNED_CONSENT", 6.99),
]
# User interaction functions
def show_menu():
    print("Menu:")
    for index, item in enumerate(menu_items):
        print(f"{index + 1}. {item.name} - ${item.price} - Category: {item.category}")
def user_order():
    show_menu()
    order = Order()
    while True:
        choice = input("Enter the menu item number (or 'done' to finish): ")
        if choice.lower() == 'done':
            break
        quantity = int(input("Enter the quantity: "))
        order.add_item(menu_items[int(choice) - 1], quantity)
    return order

# Main program
if __name__ == "__main__":
    print("WELCOME TO THE PYTHON PSYCHIATRIC RESTAURANT!")
    customer_order = user_order()
    print(f"Your total bill is: ${customer_order.calculate_total_price():.2f}")
    print(f"Your total bill after discount is: ${customer_order.apply_discount():.2f}")
    print("Thank you for dining with us!")

```
### Explanation 
menu_items: This is a list of instances of various menu item classes (Starters, Soupes, MainCourse, Drinks, Dessert). Each instance represents a specific menu item, with a name, price, and category.

show_menu(): This function prints the menu to the console. It iterates over the menu_items list and prints the name, price, and category of each item.

user_order(): This function handles the user's order. It first shows the menu by calling show_menu(). Then it enters a loop where it asks the user to input the number of the menu item they want to order and the quantity. The selected menu item and quantity are added to the order. The loop continues until the user enters 'done', at which point the function returns the order.

The main program: It welcomes the user, calls user_order() to take the user's order, calculates and prints the total bill, applies any discount and prints the discounted total, and thanks the user for dining.
