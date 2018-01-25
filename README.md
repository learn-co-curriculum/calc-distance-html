
# Calculating Distance Lab

### Introduction

In this lab, you will write methods to calculate the distance of various students in a class from each other.  Once again, let's assume that the x coordinates represent avenues of a student, the y coordinates represent street numbers, and we will assume that the distance between each street and the distance between each avenue is the same.

In this lab we will write functions to calculate the distance between students.  We will work up to a function called `nearest_neighbors` that given a student, finds the other students who are closest.

### Getting Started

Let's declare a variable `students` and assign it to an array of dictionaries, each representing the location of a student.


```python
students = [{'name': 'fred', 'street': 4, 'avenue': 8}, {'name': 'daniel', 'street': 1, 'avenue': 9}, {'name': 'rachel', 'street': 2, 'avenue': 6}, {'name': 'steven', 'street': 4, 'avenue': 10}]
```


```python
students
```




    [{'avenue': 8, 'name': 'fred', 'street': 4},
     {'avenue': 5, 'name': 'daniel', 'street': 1},
     {'avenue': 6, 'name': 'rachel', 'street': 2},
     {'avenue': 10, 'name': 'steven', 'street': 4}]




```python
fred = students[0]
daniel = students[1]
```

### Calculating the sides of the triangle

Remember that to calculate the distance, we use the Pythagorean Theorem to calculate the two shorter sides of the right triangle, and from there can calculate the distance, that is the hypotenuse, of the triangle.  Let's start with calculating the shorter sides and then use that work to calculate the distance. 

Write a function called `street_distance` that calculates how far in streets two students are from each other.


```python

```


```python
street_distance(fred, daniel) #3
```




    3



Write a function called `avenue_distance` that calculates how far in avenues two students are from each other.


```python
def avenue_distance(first_student, second_student):
    return first_student['avenue'] - second_student['avenue']
```


```python
avenue_distance(fred, daniel) #  == 3
```




    3



### Calculating the distance

Now let's begin writing functions involved with calculating that hypotenuse of our right triangle.  Using the Pythagorean Theorem, write a function called `distance_between_students_squared` that calculates the length of the hypotenuse squared.


```python
def distance_between_students_squared(first_student, second_student):
    return street_distance(first_student, second_student)^2 + avenue_distance(first_student, second_student)^2
```


```python
distance_between_students_squared(fred, daniel) # 4
```




    4



Now take the next step, and write a function called `distance`, that given two students returns the distance between them.  


```python
import math
def distance(first_student, second_student):
    return math.sqrt(distance_between_students_squared(first_student, second_student))
```


```python
distance(fred, daniel)
```




    2.0



### Writing Our "Nearest Neighbors" Functions

This next section will work up to building a `nearest_neighbor` function.  This is a function that given one student, will tell us which students are closest to him.  How do we write something like this? Can we use our calculation of distance between two students, to figure out the closest students to an individual?

Sure, we first need to calculate the distances between one student and all of the others.  Next, we sort those students by their distance from the student.  Finally, we select a given number of the closest students.  Let's work through it.   

Note that we already have a function that calculates the distance between two students.  We may think we could simply use this function to loop through our students, but that would just return an array of distances.  


```python
distances = []
for student in students:
    distance_between = distance(fred, student)
    distances.append(distance_between)

distances
```




    [0.0, 2.0, 2.0, 1.4142135623730951]



The returned array from the above procedure isn't super helpful.  We need to know who each distance is associated with.  

So let's accomplish this by writing a function called `distance_with_student` that works like our distance function but instead of returning an integer, returns a dictionary representing the `second_student`, and also adds in the a key value pair indicating distance from the `first_student`.


```python
def distance_with_student(first_student, second_student):
    student_with_distance = second_student.copy()
    distance = math.sqrt(distance_between_students_squared(first_student, second_student))
    student_with_distance['distance'] = distance
    return student_with_distance

```


```python
distance_with_student(fred, daniel)
# {'avenue': 5, 'distance': 2.0, 'name': 'daniel', 'street': 1}
```




    {'avenue': 5, 'distance': 2.0, 'name': 'daniel', 'street': 1}



Now write a function called `distance_all` that returns an array representing the distances between a `first_student` and the rest of the students.  The array should not return the `first_student` in its collection of students. 


```python
def distance_all(first_student, students):
    remaining_students = filter(lambda student: student != first_student, students)
    return list(map(lambda student: distance_with_student(first_student, student), remaining_students))
```


```python
distance_all(fred, students)
# [{'avenue': 5, 'distance': 2.0, 'name': 'daniel', 'street': 1},
#  {'avenue': 6, 'distance': 2.0, 'name': 'rachel', 'street': 2},
#  {'avenue': 10, 'distance': 1.4142135623730951, 'name': 'steven', 'street': 4}]
```




    [{'avenue': 5, 'distance': 2.0, 'name': 'daniel', 'street': 1},
     {'avenue': 6, 'distance': 2.0, 'name': 'rachel', 'street': 2},
     {'avenue': 10, 'distance': 1.4142135623730951, 'name': 'steven', 'street': 4}]



Finally, write a function called `nearest_neighbors` given a student, returns an array of students, ordered from closest to furthest from the student.  The function should take an optional third argument that specifies how many "nearest" students are returned.


```python
def nearest_neighbors(first_student, students, number = None):
    number = number or len(students) - 1
    student_distances = distance_all(first_student, students)
    sorted_students = sorted(student_distances, key=lambda student: student['distance'])
    return sorted_students[:number]
```


```python
nearest_neighbors(fred, students, 2)
# [{'avenue': 10, 'distance': 1.4142135623730951, 'name': 'steven', 'street': 4},
#  {'avenue': 5, 'distance': 2.0, 'name': 'daniel', 'street': 1}]
```




    [{'avenue': 10, 'distance': 1.4142135623730951, 'name': 'steven', 'street': 4},
     {'avenue': 5, 'distance': 2.0, 'name': 'daniel', 'street': 1}]




```python

```
