Class Exercise
1.0

    import math
    
    class Point:
        def __init__(self, x_val=0.0, y_val=0.0):
            self.x_val = float(x_val)
            self.y_val = float(y_val)
        
        def distance_to(self, other_point:"Point"):
            return ((self.x_val - other_point.x_val)**2 + (self.y_val - other_point.y_val)**2)**0.5
    
    class Line:
        def __init__(self, start_point: Point, end_point: Point):
            self.start_point = start_point
            self.end_point = end_point
        
        def length(self):
            self.len_ = round(Point.distance_to(self.start_point, self.end_point), 2)
            return self.len_
    
    class Shape:
        def __init__(self):
            self.is_regular = None
            self.vertices_ = []
            self.edges_ = []
            self.inner_angles_ = []
    
        def area(self):
            pass
    
        def perimeter(self):
            pass
    
        def inner_angles(self):
            pass
    
    class Rectangle(Shape):
        def __init__(self, bottom_left, upper_right):
            super().__init__()
            self.is_regular = False
            bottom_right = Point(upper_right.x_val, bottom_left.y_val)
            upper_left = Point(bottom_left.x_val, upper_right.y_val)
            self.vertices_.extend([bottom_left, upper_left, upper_right, bottom_right])
            self.edges_.extend([
                Line(bottom_left, upper_left),
                Line(upper_left, upper_right),
                Line(upper_right, bottom_right),
                Line(bottom_right, bottom_left)
            ])
        
        def area(self):
            return Line.length(self.edges_[0]) * Line.length(self.edges_[1])
    
        def perimeter(self):
            return (2 * Line.length(self.edges_[0])) + (2 * Line.length(self.edges_[1]))
    
        def inner_angles(self):
            self.inner_angles_ = [90] * 4
            return self.inner_angles_
    
    class Square(Rectangle):
        def __init__(self, bottom_left, upper_right):
            super().__init__(bottom_left, upper_right)
            self.is_regular = True
    
        def area(self):
            return super().area()
    
        def inner_angles(self):
            return super().inner_angles()
    
    class Triangle(Shape):
        def __init__(self, bottom_left, bottom_right, upper_vertex):
            super().__init__()
            self.is_regular = False
            self.vertices_.extend([bottom_left, bottom_right, upper_vertex])
            self.edges_.extend([
                Line(bottom_left, bottom_right),
                Line(bottom_right, upper_vertex),
                Line(upper_vertex, bottom_left)
            ])
        
        def perimeter(self):
            return sum(Line.length(edge) for edge in self.edges_)
    
    class Isosceles(Triangle):
        def __init__(self, bottom_left, bottom_right, upper_vertex):
            super().__init__(bottom_left, bottom_right, upper_vertex)
    
        def area(self):
            height_ = ((Line.length(self.edges_[1]))**2 + (Line.length(self.edges_[0])**2))**0.5
            return (Line.length(self.edges_[0]) * height_) / 2
    
        def inner_angles(self):
            angle_A = math.degrees(math.acos(((Line.length(self.edges_[1]))**2 + (Line.length(self.edges_[2]))**2 - (Line.length(self.edges_[0]))**2) / (2 * (Line.length(self.edges_[1])) * (Line.length(self.edges_[2])))))
            angle_B = math.degrees(math.acos(((Line.length(self.edges_[0]))**2 + (Line.length(self.edges_[2]))**2 - (Line.length(self.edges_[1]))**2) / (2 * (Line.length(self.edges_[0])) * (Line.length(self.edges_[2])))))
            angle_C = 180 - angle_A - angle_B
    
            self.inner_angles_.extend([angle_A, angle_B, angle_C])
            return self.inner_angles_
    
    class Equilateral(Triangle):
        def __init__(self, bottom_left, bottom_right, upper_vertex):
            super().__init__(bottom_left, bottom_right, upper_vertex)
            self.is_regular = True
    
        def area(self):
            return (((Line.length(self.edges_[0]))**2) * math.sqrt(3)) / 4
    
        def inner_angles(self):
            self.inner_angles_ = [60] * 3
            return self.inner_angles_
    
    class Scalene(Triangle):
        def __init__(self, bottom_left, bottom_right, upper_vertex):
            super().__init__(bottom_left, bottom_right, upper_vertex)
        
        def area(self):
            s = self.perimeter() / 2
            c_length = Line.length(self.edges_[2])
            return math.sqrt(s * (s - Line.length(self.edges_[0])) * (s - (Line.length(self.edges_[1]))) * (s - c_length))
    
        def inner_angles(self):
            angle_A = math.degrees(math.acos(((Line.length(self.edges_[1]))**2 + (Line.length(self.edges_[2]))**2 - (Line.length(self.edges_[0]))**2) / (2 * (Line.length(self.edges_[1])) * (Line.length(self.edges_[2])))))
            angle_B = math.degrees(math.acos(((Line.length(self.edges_[0]))**2 + (Line.length(self.edges_[2]))**2 - (Line.length(self.edges_[1]))**2) / (2 * (Line.length(self.edges_[0])) * (Line.length(self.edges_[2])))))
            angle_C = 180 - angle_A - angle_B
    
            self.inner_angles_.extend([angle_A, angle_B, angle_C])
            return self.inner_angles_
    
    class TriRectangle(Triangle):
        def __init__(self, bottom_left, bottom_right, upper_vertex):
            super().__init__(bottom_left, bottom_right, upper_vertex)
    
        def area(self):
            if self.vertices_[1].y_val == self.vertices_[2].y_val:
                return (Line.length(self.edges_[0]) * Line.length(self.edges_[1])) / 2
            else:
                return (Line.length(self.edges_[0]) * Line.length(self.edges_[2])) / 2
    
        def inner_angles(self):
            angle_A = 90
            if self.vertices_[1].y_val == self.vertices_[2].y_val:
                angle_B = math.degrees(math.atan(Line.length(self.edges_[1]) / Line.length(self.edges_[0])))
            else:
                angle_B = math.degrees(math.atan(Line.length(self.edges_[2]) / Line.length(self.edges_[0])))
            angle_C = angle_A - angle_B
    
            self.inner_angles_.extend([angle_A, angle_B, angle_C])
            return self.inner_angles_
    
    #Ejemplos de utilidad
    bottom_left_rect = Point(0, 0)
    upper_right_rect = Point(4, 3)
    
    bottom_left_square = Point(0, 0)
    upper_right_square = Point(3, 3)
    
    rect = Rectangle(bottom_left_rect, upper_right_rect)
    square = Square(bottom_left_square, upper_right_square)
    
    print(f"El rectángulo tiene área de {rect.area():.2f} y perímetro de {rect.perimeter():.2f}")
    print(f"Sus ángulos internos son {rect.inner_angles()}")
    
    print("\n")
    
    print(f"El cuadrado tiene área de {square.area():.2f} y perímetro de {square.perimeter():.2f}")
    print(f"Sus ángulos internos son {square.inner_angles()}")
    
    print("\n")
    
    p1 = Point(0, 0)
    p2 = Point(4, 0)
    p3 = Point(2, 4)
    p4 = Point(3, 0)
    p5 = Point(3, 3)
    
    triangle = Triangle(p1, p2, p3)
    isosceles = Isosceles(p1, p2, p3)
    equilateral = Equilateral(p1, Point(4, 0), Point(2, math.sqrt(12)))
    scalene = Scalene(p1, p4, p5)
    trirectangle = TriRectangle(p1, p2, Point(4, 3))

2.0

    class Shape:
        def __init__(self):
            pass
    
        def calculate_area(self):
            pass
    
        def calculate_perimeter(self):
            pass
    
    class Rectangle(Shape):
        def __init__(self, width_val, height_val):
            super().__init__()
            self.width_val = width_val
            self.height_val = height_val
    
        def calculate_area(self):
            return self.height_val * self.width_val
    
        def calculate_perimeter(self):
            return 2 * self.height_val + 2 * self.width_val
    
    class Square(Shape):
        def __init__(self, side_length):
            super().__init__()
            self.side_length = side_length
    
        def calculate_area(self):
            return self.side_length ** 2
    
        def calculate_perimeter(self):
            return 4 * self.side_length
    
    # Toca probar el codigo, aca estan los comandos con los que lo hice (Perdon no s eutilizar GitHub)
    rect = Rectangle(5, 10)
    square = Square(7)
    
    print(f"El Rectángulo tiene un ancho de {rect.width_val} y altura de {rect.height_val}")
    print(f"Su Área es de {rect.calculate_area()}")
    print(f"Su Perímetro es de {rect.calculate_perimeter()}")
    
    print(f"El Cuadrado tiene lados de {square.side_length}")
    print(f"Su Área es de {square.calculate_area()}")
    print(f"Su Perímetro es de {square.calculate_perimeter()}")

    print(f"El triángulo inicial tiene un perímetro de {triangle.perimeter():.2f}")
    
    print("\n")
    
    print(f"El triángulo isósceles tiene un área de {isosceles.area():.2f}, un perímetro de {isosceles.perimeter():.2f}")
    print(f"Sus ángulos internos son {isosceles.inner_angles()}")
    
    print("\n")
    
    print(f"El triángulo equilátero tiene un área de {equilateral.area():.2f}, un perímetro de {equilateral.perimeter():.2f}")
    print(f"Sus ángulos internos son {equilateral.inner_angles()}")
    
    print("\n")
    
    print(f"El triángulo escaleno tiene un área de {scalene.area():.2f}, un perímetro de {scalene.perimeter():.2f}")
    print(f"Sus ángulos internos son {scalene.inner_angles()}")
    
    print("\n")
    
    print(f"El triángulo rectángulo tiene un área de {trirectangle.area():.2f}, un perímetro de {trirectangle.perimeter():.2f}")
    print(f"Sus ángulos internos son {trirectangle.inner_angles()}")

Ejercicio del Restaurante con getters y setters

        class MenuItem:
        def __init__(self, name: str, price: float, type_: str):
            self._name = name
            self._price = price
            self._type = type_
    
        def get_name(self):
            return self._name
    
        def set_name(self, name: str):
            self._name = name
    
        def get_price(self):
            return self._price
    
        def set_price(self, price: float):
            self._price = price
    
        def get_type(self):
            return self._type
    
        def set_type(self, type_: str):
            self._type = type_
    
        def total_price(self, quantity) -> float:
            return quantity * self._price
    
    class Appetizer(MenuItem):
        def __init__(self, name: str, price: float, type_: str, serving: str):
            super().__init__(name, price, type_)
            self._serving = serving
    
        def get_serving(self):
            return self._serving
    
        def set_serving(self, serving: str):
            self._serving = serving
    
    class Beverage(MenuItem):
        def __init__(self, name: str, price: float, type_: str, size: str):
            super().__init__(name, price, type_)
            self._size = size
    
        def get_size(self):
            return self._size
    
        def set_size(self, size: str):
            self._size = size
    
    class Dessert(MenuItem):
        def __init__(self, name: str, price: float, type_: str, flavor: str):
            super().__init__(name, price, type_)
            self._flavor = flavor
    
        def get_flavor(self):
            return self._flavor
    
        def set_flavor(self, flavor: str):
            self._flavor = flavor
    
    class MainCourse(MenuItem):
        def __init__(self, name: str, price: float, type_: str, protein: str, vegetable: str, starch: str):
            super().__init__(name, price, type_)
            self._protein = protein
            self._vegetable = vegetable
            self._starch = starch
    
        def get_protein(self):
            return self._protein
    
        def set_protein(self, protein: str):
            self._protein = protein
    
        def get_vegetable(self):
            return self._vegetable
    
        def set_vegetable(self, vegetable: str):
            self._vegetable = vegetable
    
        def get_starch(self):
            return self._starch
    
        def set_starch(self, starch: str):
            self._starch = starch
    
    class Order:
        def __init__(self):
            self.items_ = []
    
        def add_item(self, item: MenuItem, quantity: int = 1):
            self.items_.append((item, quantity))
    
        def calculate_total(self) -> float:
            total = sum(item.total_price(quantity) for item, quantity in self.items_)
            return total
    
        def apply_discount(self, discount: float) -> float:
            total = self.calculate_total()
            return total * (1 - discount)
    
        def display_order(self):
            for item, quantity in self.items_:
                print(f'{quantity} x {item.get_name()} - {item.total_price(quantity):.2f} (unidad: {item.get_price():.2f})')
    
            total = self.calculate_total()
            print(f'Total: {total:.2f}')
    
        def pay(self, payment_method: "PaymentMethod"):
            total = self.calculate_total()
            payment_method.pay(total)
    
    class PaymentMethod:
        def __init__(self):
            pass
    
        def pay(self, amount):
            pass
    
    class Card(PaymentMethod):
        def __init__(self, number, cvv):
            super().__init__()
            self.number_ = number
            self.cvv_ = cvv
    
        def pay(self, amount):
            print(f"Paying {amount:.2f} with card ending in {self.number_[-4:]}")
    
    class Cash(PaymentMethod):
        def __init__(self, amount_given):
            super().__init__()
            self.amount_given_ = amount_given
    
        def pay(self, amount):
            if self.amount_given_ >= amount:
                print(f"Payment made in cash. Change: {self.amount_given_ - amount:.2f}")
            else:
                print(f"Insufficient funds. Missing {amount - self.amount_given_:.2f} to complete the payment.")


    #Aca lo mismo, instanciamos para probar
    Bebida = Beverage("water", 2.75, "still", "250 ml")
    Entradita = Appetizer("Chicharrones carnudos", 13.50, "beef", "Group")
    Elplatote = MainCourse("Dish of the day", 24.00, "daily", "beef", "salad", "rice")
    
    order = Order()
    order.add_item(Bebida, 2)
    order.add_item(Entradita, 1)
    order.add_item(Elplatote, 1)
    
    print("La orden antes del descuento:")
    order.display_order()
    
    discount_rate = 0.10
    total_after_discount = order.apply_discount(discount_rate)
    print(f"\nTotal con {discount_rate * 100}% de descuento: {total_after_discount:.2f}")
    
    print("\nPagando:")
    Tarjeta = Card("12345678901234568347643863", "123")
    Cash = Cash(50.00)
    order.pay(Tarjeta)  
    order.pay(Cash)
