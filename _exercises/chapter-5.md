---
layout: page
title: Chapter 5 Homework
description: Homework answers for Chapter 5
---



```python
#5-1

car = 'tesla'
truck = 'ford'
inventor = 'tesla'



print("Is car == 'tesla'? I predict True.")
print(car == 'tesla')

print("\nIs truck == 'ford'? I predict True.")
print(truck == 'ford')

print("\nIs car == inventor? I predict True.")
print(car == inventor)

print("\nIs truck == inventor? I predict False.")
print(truck == inventor)

print("\nIs truck == car? I predict False.")
print(truck == car)

print("\nIs inventor == car and truck? I predict False.")
print(inventor == car and inventor == truck)

print("\nIs car == 'mazda'? I predict False.")
print(truck == 'mazda')

print("\nIs inventor == 'ford'? I predict False.")
print(inventor == 'ford')

print("\nIs 'tesla' == inventor and car? I predict True.")
print('tesla' == inventor and 'tesla' == car)

print("\nIs truck == 'ford' or truck == 'tesla'? I predict True.")
print(truck == 'ford' or truck == 'tesla')
```

    Is car == 'tesla'? I predict True.
    True
    
    Is truck == 'ford'? I predict True.
    True
    
    Is car == inventor? I predict True.
    True
    
    Is truck == inventor? I predict False.
    False
    
    Is truck == car? I predict False.
    False
    
    Is inventor == car and truck? I predict False.
    False
    
    Is car == 'mazda'? I predict False.
    False
    
    Is inventor == 'ford'? I predict False.
    False
    
    Is 'tesla' == inventor and car? I predict True.
    True
    
    Is truck == 'ford' or truck == 'tesla'? I predict True.
    True



```python
#5-2


presidents = ['obama', 'trump', 'biden']

print("\nIs 'Truck'.lower() == 'truck'? I predict True.")
print('Truck'.lower() == 'truck')

print("\nIs 'ford' == 'truck'? I predict False.")
print('truck' == 'ford')

print("\nIs 4 > 3? I predict True.")
print(4 > 3)

print("\nIs 4 == 2 + 2 and 4 == 3 + 1? I predict True.")
print(4 == (2+2) and 4 == (3+1))

print("\nIs 'clooney' in presidents? I predict False.")
print('clooney' in presidents)

print("\nIs 'biden' not in presidents? I predict False.")
print('biden' not in presidents)

```

    
    Is 'Truck'.lower() == 'truck'? I predict True.
    True
    
    Is 'ford' == 'truck'? I predict False.
    False
    
    Is 4 > 3? I predict True.
    True
    
    Is 4 == 2 + 2 and 4 == 3 + 1? I predict True.
    True
    
    Is 'clooney' in presidents? I predict False.
    False
    
    Is 'biden' not in presidents? I predict False.
    False



```python
#5-6

age = 6

if(age <= 2):
    print("Baby!")
elif(age >= 2 and age < 4):
    print('Toddler!')
elif(age >= 4 and age < 13):
    print('Kid!')
elif(age >= 13 and age < 20):
    print("Teen!")
elif(age >= 20 and age < 65):
    print("Adult!")
elif(age >= 65):
    print("Old!")
```

    Kid!



```python
#5-7
#used for loop bc easier

fav_fruits = ["Strawberry", "Apple", "Pineapple"]


test_fruits = ["Melon", "Strawberry", "Banana", "Orange", "Apple"]


for fruit in test_fruits:
    if(fruit in fav_fruits):
        print(f"You really like {fruit}s!")
```

    You really like Strawberrys!
    You really like Apples!



```python
#5-8

usernames = ['admin', 'masongoldberg', 'goober123', 'johnny1978', 'cupcakestomper']

for username in usernames:
    if(username == 'admin'):
        print("Hello admin, would you like to see the reports?")
    else:
        print(f"Hello {username}, welcome back!")



```

    Hello admin, would you like to see the reports?
    Hello masongoldberg, welcome back!
    Hello goober123, welcome back!
    Hello johnny1978, welcome back!
    Hello cupcakestomper, welcome back!



```python
usernames = []

if(usernames == []):
    print("Oh no! We have no users.")
else:
    for username in usernames:
        if(username == 'admin'):
            print("Hello admin, would you like to see the reports?")
        else:
            print(f"Hello {username}, welcome back!")
```

    Oh no! We have no users.



```python
#5-10

current_users = ['joemama', 'heftycubism', 'davidbeats', 'Lolla29', 'tanchy']
new_users = ['Jarjar23', 'Tanchy', 'elchuPacabRa2009', 'yungluve', 'joemama']

for user in current_users:
    user = user.lower()

for new_user in new_users:
    if (new_user.lower() in current_users):
        print(f"Your username is taken, {new_user}")
    else:
        print("Your username is available!")

    


```

    Your username is available!
    Your username is taken, Tanchy
    Your username is available!
    Your username is available!
    Your username is taken, joemama

