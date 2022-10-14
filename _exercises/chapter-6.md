---
layout: page
title: Chapter 6 Homework
description: Homework answers for Chapter 6
---





```python
#6-1

Me = {'firstname':'Mason', 'lastname':'Goldberg', 'age':21, 'birthday':'5/22/2001', 'city':'San Francisco'}
print(Me)
```

    {'firstname': 'Mason', 'lastname': 'Goldberg', 'age': 21, 'birthday': '5/22/2001', 'city': 'San Francisco'}



```python
#6-2

Favorite_Numbers = {'Michael Jordan':23, 'Jackie Robinson':42, 'Chad Ochocinco':85, 'Stephen Curry':30, 'Klay Thompson':11}
print(Favorite_Numbers)
```

    {'Michael Jordan': 23, 'Jackie Robinson': 42, 'Chad Ochocinco': 85, 'Stephen Curry': 30, 'Klay Thompson': 11}



```python
#6-3

Glossary = {'Variable':'Thing that stores a changable piece of data', 'Dictionary':'Variable that stores keys and values', 'Boolean':'True or False variable', 'Initialize':'To call a variable into existence in memory', 'Append':'Add something to a list'}

for key in Glossary:
    print(f"The term {key} means {Glossary[key]}")
```

    The term Variable means Thing that stores a changable piece of data
    The term Dictionary means Variable that stores keys and values
    The term Boolean means True or False variable
    The term Initialize means To call a variable into existence in memory
    The term Append means Add something to a list



```python
#6-4 
#already used a loop in 6-3

Glossary = {'Variable':'Thing that stores a changable piece of data', 'Dictionary':'Variable that stores keys and values', 'Boolean':'True or False variable', 'Initialize':'To call a variable into existence in memory', 'Append':'Add something to a list'}

for key in Glossary:
    print(f"The term {key} means {Glossary[key]}")

New_Words = {'Float':'Number with decimal places', 'Int':'Whole number', 'List':'A variable that holds a list of values', 'Sort':'Put a list into an order', 'Delete':'Remove from a list'}


for key in Glossary:
    print(f"The term {key} means {Glossary[key]}")
```

    The term Variable means Thing that stores a changable piece of data
    The term Dictionary means Variable that stores keys and values
    The term Boolean means True or False variable
    The term Initialize means To call a variable into existence in memory
    The term Append means Add something to a list
    The term Variable means Thing that stores a changable piece of data
    The term Dictionary means Variable that stores keys and values
    The term Boolean means True or False variable
    The term Initialize means To call a variable into existence in memory
    The term Append means Add something to a list



```python
#6-5

Rivers = {'Nile':'Egypt', 'Amazon':'Brazil', 'Mississippi':'the United States'}

for key, value in Rivers.items():
    print(f"The {key} runs through {value}.")


print("The rivers are: ")
for key in Rivers:
    print(key)

print("The countries are: ")
for key in Rivers:
    print(Rivers[key])
```

    The Nile runs through Egypt.
    The Amazon runs through Brazil.
    The Mississippi runs through the United States.
    The rivers are: 
    Nile
    Amazon
    Mississippi
    The countries are: 
    Egypt
    Brazil
    the United States



```python
#6-7

Mason = {'firstname':'Mason', 'lastname':'Goldberg', 'age':21, 'birthday':'5/22/2001', 'city':'San Francisco'}
Steph = {'firstname':'Stephen', 'lastname':'Curry', 'age':34, 'birthday':'3/14/1988', 'city':'Akron'}
Joe = {'firstname':'Joseph', 'lastname':'Biden', 'age':79, 'birthday':'11/20/1942', 'city':'Scranton'}

people = [Mason, Joe, Steph]

for person in people:
    for key, value in person.items():
        print(f"{person['firstname']}'s {key} is {value}.")
```

    Mason's firstname is Mason.
    Mason's lastname is Goldberg.
    Mason's age is 21.
    Mason's birthday is 5/22/2001.
    Mason's city is San Francisco.
    Joseph's firstname is Joseph.
    Joseph's lastname is Biden.
    Joseph's age is 79.
    Joseph's birthday is 11/20/1942.
    Joseph's city is Scranton.
    Stephen's firstname is Stephen.
    Stephen's lastname is Curry.
    Stephen's age is 34.
    Stephen's birthday is 3/14/1988.
    Stephen's city is Akron.



```python
#6-8

Quincy = {'name':'quincy', 'animal':'dog', 'owner':'jen',}
Bear = {'name':'bear', 'animal':'dog', 'owner':'ilene'}
Ginger = {'name':'ginger', 'animal':'snake', 'owner':'allen'}

pets = [Quincy, Bear, Ginger]


for pet in pets:
    print(f"{pet['name']} is a {pet['animal']} owned by {pet['owner']}.")

```

    quincy is a dog owned by jen.
    bear is a dog owned by ilene.
    ginger is a snake owned by allen.



```python
#6-9

Favorite_Places = {'Mason':['Big Sur', 'Italy', 'Mount Rainier'], 'Mark':['Arizona', 'Switzerland'], 'Sam':['New York']}

for key in Favorite_Places:
    for item in Favorite_Places[key]:
        print(f"One of {key}'s favorite places is {item}.")
    
```

    One of Mason's favorite places is Big Sur.
    One of Mason's favorite places is Italy.
    One of Mason's favorite places is Mount Rainier.
    One of Mark's favorite places is Arizona.
    One of Mark's favorite places is Switzerland.
    One of Sam's favorite places is New York.



```python
#6-11

San_Francisco = {'City':'San Francisco', 'Country':'USA', 'Population':875000, 'Mayor':'London Breed'}
Los_Angeles = {'City':'Los Angeles', 'Country':'USA', 'Population':3930000, 'Mayor':'Eric Garcetti'}
London = {'City':'London', 'Country':'UK', 'Population':8982000, 'Mayor':'Sadiq Khan'}

Cities = {'San Francisco':San_Francisco, 'Los Angeles':Los_Angeles, 'London':London}

for key, value in Cities.items():
    for k, v in value.items():
        print(f"{k}: {v}")
    print("\n")
```

    City: San Francisco
    Country: USA
    Population: 875000
    Mayor: London Breed
    
    
    City: Los Angeles
    Country: USA
    Population: 3930000
    Mayor: Eric Garcetti
    
    
    City: London
    Country: UK
    Population: 8982000
    Mayor: Sadiq Khan
    
    



```python

```
