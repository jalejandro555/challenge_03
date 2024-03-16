# challenge_03
## Class exercise 
```python
import math

class Point:
    
    definition: str = "Entidad geométrica abstracta que representa una ubicación en un espacio."

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
        self.top_left = top_left
        self.bottom_right = bottom_right
        self.top_right = Point(bottom_right.x, top_left.y)
        self.bottom_left = Point(top_left.x, bottom_right.y)
        self.lines = [Line(self.top_left, self.top_right), Line(self.top_right, self.bottom_right),
                      Line(self.bottom_right, self.bottom_left), Line(self.bottom_left, self.top_left)]

    def init_from_lines(self, top_line, bottom_line, left_line, right_line):
        self.lines = [top_line, bottom_line, left_line, right_line]
        self.top_left = top_line.start
        self.bottom_right = bottom_line.end
        self.top_right = top_line.end
        self.bottom_left = left_line.start

    def init_from_point_and_dimensions(self, top_left, width, height):
        self.top_left = top_left
        self.bottom_right = Point(top_left.x + width, top_left.y - height)
        self.init_from_points(self.top_left, self.bottom_right)

    def init_from_center_and_dimensions(self, center, width, height):
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
