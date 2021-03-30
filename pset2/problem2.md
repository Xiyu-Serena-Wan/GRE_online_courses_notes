### Problem 2 - Paying Debt Off in a Year

15.0/15.0 points (graded)
16.
Now write a program that calculates the minimum fixed monthly payment needed in order pay off a credit card balance within 12 months. By a fixed monthly payment, we mean a single number which does not change each month, but instead is a constant amount that will be paid each month.

In this problem, we will not be dealing with a minimum monthly payment rate.

The following variables contain values as described below:

1. balance - the outstanding balance on the credit card
2. annualInterestRate - annual interest rate as a decimal

The program should print out one line: the lowest monthly payment that will pay off all debt in under 1 year, for example:

Lowest Payment: 180 

Assume that the interest is compounded monthly according to the balance at the end of the month (after the payment for that month is made). The monthly payment must be a multiple of $10 and is the same for all months. Notice that it is possible for the balance to become negative using this payment scheme, which is okay. A summary of the required math is found below:

    Monthly interest rate = (Annual interest rate) / 12.0
    Monthly unpaid balance = (Previous balance) - (Minimum fixed monthly payment)
    Updated balance each month = (Monthly unpaid balance) + (Monthly interest rate x Monthly unpaid balance)
    
````

from math import *

def round_10( n ):
 
    # Smaller multiple
    a = (n // 10) * 10
     
    # Larger multiple
    b = a + 10
     
    # Return of closest of two
    return (b if n - a > b - n else a)

balance_fix_0 = round_10(balance*0.2 / 12)

def credebt(balance,annualInterestRate,balance_fix):
    
    month = 0

    balance_res = balance
    
    while month < 12:
       
        balance_res = (balance_res - balance_fix) * (1 + annualInterestRate / 12)
        month += 1
        #print(month,balance_res)
    
    if balance_res >= 0:
        return credebt(balance,annualInterestRate,balance_fix+10)
    else:
        return balance_fix
        
print('Lowest Payment: ', credebt(balance,annualInterestRate,balance_fix_0))
````
