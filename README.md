Reto 4

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
    
    # Ejemplos de uso
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
