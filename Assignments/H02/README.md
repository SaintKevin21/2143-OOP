|                              Understand Encapsulation                |
| :------------------------------------------------------------------------: |
| Part A: Conceptual Questions
1. Definition
* In your own words, define encapsulation. Include an example of how it can prevent unintended changes to data.
A principle that restricts direct access to an object's data by bundling it with the methods that operate on that data. Ex Student grade class where the grade can be access by anyone someone can direct changed their grade. This could lead to the student either go from passing to failing grades or failing to passing grades. But if the student grade class is encapsulation the the grads is private and must used getters and setters to control the student grades.
2. Visibility Modifiers
* Compare public, private, and protected access. For each, list one benefit and one potential drawback (in terms of code flexibility, safety, or maintainability).
Public - can be accessed and modified from outside the code.
        Benefits - Public allows access to an object's data and methods from other parts of the program
        Drawback - Since the code can be access from anywhere from the code it can lead to unintended or incorrect modifications data or information.
Private - members cannot be accessed (or viewed) from outside the class
        Benefits - Private member is protected and can not be interfered with other parts of the code.
        Drawbacks - Reduce flexibility access in other parts of the code
Protected - members cannot be accessed from outside the class, however, they can  be accessed in inherited classes. You will learn more about Inheritance later.
        Benefits - Protected and can be access by other class and can use inheritance
        Drawbacks - Although protected data member can be accessed and corrupt in other part in the code.
* Name a scenario in which you might intentionally use protected members instead of private members. 
The advantage of using a protected member instead of a private member when it comes to will be inherited by other classes. While both provide protection of the data member the protected is allowed to be access and work with other class by inheritance; when the private member can not.
3. Impact on Maintenance
* Explain why encapsulation can reduce debugging complexity when maintaining a large codebase.
Encapsulation can reduce debugging complexity when maintaining a large codebase by hiding the internal details of objects.
* Provide a brief example (no code needed, just a scenario) of how code could break if internal data is made public.
Student grade class where the grade can be access by anyone someone can direct changed their grade.
4. Real-World Analogy
* Think of a real-life object or system. How would you describe its “public interface” vs. its “private implementation”? Why is it helpful to keep the private side hidden?[Part A_ Conceptual Questions.txt](https://github.com/user-attachments/files/20002785/Part.A_.Conceptual.Questions.txt)
|
