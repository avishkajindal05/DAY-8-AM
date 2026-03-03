Q1: Conceptual
Explain the difference between elif and multiple if statements.
Give one example where:
multiple if produces a different output than elif
Clearly state:
Input
Output using if
Output using elif
Why the difference happens

----

Ans1: Multiple if statements operate as independent conditional checks. Each statement is evaluated sequentially, regardless of whether preceding conditions are true or false. Consequently, all conditions that evaluate to true will execute their corresponding code blocks.
In contrast, elif (short for "else if") statements create a dependent chain of conditions. They are evaluated sequentially only until the first true condition is encountered. Once a condition evaluates to true, its associated code block executes and the entire conditional structure terminates—subsequent elif or else statements are skipped. 

with only if statements
input 
a = 20
b = 8
if a > 10:
    print("a is greater than 10")
if a > 8:
    print("a is greater than 8") 
print("end")
output:
a is greater than 10
a is greater than 8
end

with elif statements
input 
a = 20
b = 8
if a > 10:
    print("a is greater than 10")
elif a > 8:
    print("a is greater than 8") 
print("end')
output:
a is greater than 10
end

Explanation of the Difference:
The divergence in output occurs due to the fundamental structural difference between independent and dependent conditional evaluation:
With multiple if statements: Both conditions are evaluated independently. Since a > 10 evaluates to true, the first message prints. However, the program continues to evaluate the second if statement because there is no logical dependency between the two conditions. Since a > 8 is also true, the second message prints as well.
With if-elif: The conditions form a mutually exclusive decision tree. The interpreter evaluates a > 10 first, finds it true, executes the corresponding code block, and immediately exits the entire conditional structure. The elif statement is never evaluated because the preceding if condition was satisfied. This design ensures that only one code block executes within the if-elif chain, prioritizing the first matching condition.

-----

Q2: Coding
Write a function:

def classify_triangle(a, b, c):
    ...
Return:

Not a triangle → if any side ≥ sum of other two
Equilateral → all sides equal
Isosceles → two sides equal
Scalene → all sides different
Edge cases to handle

Zero values
Negative values
Invalid triangle conditions

-----

ans 2:
def classify_triangle(a, b, c):
    if a >= b+c or b >= a+c or c >= b+a:
        return "Not a triangle"
    elif a<= 0 or b <= 0 or c<= 0:
        return "invalid values"
    else:
        if a == b == c :
            return "equilateral Triangle"
        else:
            if a == b or b == c or c == a:
                print "isosceles triangle"
            else:
                print "Scalene"

----------

Q3: Debug / Analyze
score = 85

if score >= 60:
    grade = 'D'
if score >= 70:
    grade = 'C'
if score >= 80:
    grade = 'B'
if score >= 90:
    grade = 'A'

print(grade)
Answer the following:
----- 
What is the bug?
-> The code uses a series of independent if statements rather than a conditional chain (if-elif-else). This causes all conditions to be evaluated sequentially, with each subsequent true condition overwriting the previous assignment to grade.

Why does this code give the wrong output?
-> Because all if statements are independent, the program does not exit after finding the appropriate grade range. Instead, it continues evaluating each condition, and the final true condition (score >= 80) determines the output.

While this happens to produce the correct letter grade for this specific input, the logic is fundamentally flawed. For example, with score = 95, the output would incorrectly be 'B' (from the >= 80 check) rather than 'A', because the >= 90 condition would never have a chance to execute after the earlier assignments. Similarly, any score would always result in 'D', 'C', 'B', or 'A' being the final assignment based solely on the highest threshold crossed, not the correct letter grade.

What should be the correct fix?
-> Reconstruct using if-elif statements
score = 85

if score >= 90:
    grade = 'A'
elif score >= 80:
    grade = 'B'
elif score >= 70:
    grade = 'C'
elif score >= 60:
    grade = 'D'
else:
    grade = 'F'

print(grade)


