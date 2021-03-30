### Problem 3

0.0/15.0 points (graded)

Assume s is a string of lower case characters.

Write a program that prints the longest substring of s in which the letters occur in alphabetical order. For example, if s = 'azcbobobegghakl', then your program should print

Longest substring in alphabetical order is: beggh
In the case of ties, print the first substring. For example, if s = 'abcbcd', then your program should print

Longest substring in alphabetical order is: abc
Note: This problem may be challenging. We encourage you to work smart. If you've spent more than a few hours on this problem, we suggest that you move on to a different part of the course. If you have time, come back to this problem after you've had a break and cleared your head.

````
i = 0;
s_app = list()
for i in range(len(s)-2):
    j = 0;
    while ord(s[i+j]) <= ord(s[i+j+1]):
        j+=1;
        if i+j == len(s)-1:
            break
    s_app.append(s[i:i+j+1]);
print ("Longest substring in alphabetical order is: ", max(s_app, key=len))
````
