class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}
        self.courses_attached = []

    def rate_hws(self, lecturer, course, grade):
        if isinstance(lecturer, Lecturer) and course in self.courses_attached and course \
                in lecturer.courses_in_progress:
            if course in lecturer.grades:
                lecturer.grades[course] += [grade]
            else:
                lecturer.grades[course] = [grade]
        else:
            return 'error'

    def _average_rating(self):
        _sum = 0
        counter = 0
        for value in self.grades.values():
            for i in value:
                _sum += i
                counter += 1
        return _sum / counter

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname} \nСредняя оценка за домашние задания: ' \
              f'{self._average_rating()} \nКурсы в процессе изучения: {self.courses_in_progress} ' \
              f'\nЗавершенные курсы: {self.finished_courses}'
        return res

    def __lt__(self, other):
        if not isinstance(other, Student):
            print('не является студентом !')
            return
        return self.grades < other.grades


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}
        self.courses_in_progress = []

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname} \nСредняя оценка за лекции: {self._average_rating}'
        return res

    def _average_rating(self):
        _sum = 0
        counter = 0
        for value in self.grades.values():
            for i in value:
                _sum += i
                counter += 1
        return _sum / counter

    def __lt__(self, other):
        if not isinstance(other, Lecturer):
            print('не является лектором !')
            return
        return self.grades < other.grades


class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname}'
        return res


first_student = Student('Ruoy', 'Eman', 'male')
first_student.courses_in_progress += ['Python', 'JS', 'C++']
first_student.finished_courses += ['GIT']
second_student = Student('Julia', 'Roberts', 'woman')
second_student.courses_in_progress += ['Python', 'C#']
second_student.finished_courses += ['GIT']

cool_reviewer = Reviewer('Spider', 'Man')
cool_reviewer.courses_attached += ['Python']
cool_reviewer.rate_hw(first_student, 'Python', 8)
cool_reviewer.rate_hw(first_student, 'Python', 7)
cool_reviewer.rate_hw(second_student, 'Python', 5)
cool_reviewer.rate_hw(second_student, 'Python', 7)

first_lecturer = Lecturer('Петров', 'Петр')
first_lecturer.courses_attached += ['Python']
first_lecturer.courses_in_progress += ['Python']
second_lecturer = Lecturer('Иванов', 'Иван')
second_lecturer.courses_attached += ['Python']
second_lecturer.courses_in_progress += ['Python']
first_student.rate_hws(first_lecturer, 'Python', 8)
first_student.rate_hws(second_lecturer, 'Python', 8)
second_student.rate_hws(first_lecturer, 'Python', 10)
second_student.rate_hws(second_lecturer, 'Python', 10)

print(first_student)
print(second_student)
print(first_lecturer)
print(second_lecturer)
