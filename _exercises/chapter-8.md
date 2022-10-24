---
layout: page
title: Chapter 8 Homework
description: Homework answers for Chapter 8
---





```python
#8-1
def display_message():
    print("I am learning about functions!")

display_message()
```

    I am learning about functions!



```python
#8-2
def favorite_book(title):
    print(f"One of my favorite books is {title}.")

favorite_book("The Magus")
```

    One of my favorite books is The Magus.



```python
#8-3

def make_shirt(size, text):
    print(f"The shirt is size {size} and says {text}.")

make_shirt('M', '"Hello there!"')
make_shirt(text = 'TGIF', size = 'L')
```

    The shirt is size M and says "Hello there!".
    The shirt is size L and says TGIF.



```python
#8-5


def describe_city(city, country = 'Canada'):
    print(f"{city} is in {country}.")

describe_city('Ottawa')
describe_city('Winnipeg')
describe_city('Paris', 'France')

```

    Ottawa is in Canada.
    Winnipeg is in Canada.
    Paris is in France.



```python
#8-6

def city_country(city, country):
    location = f"{city}, {country}"
    return location

loc1 = city_country('Santiago', 'Chile')
loc2 = city_country('Beijing', 'China')
loc3 = city_country('Cairo', 'Egypt')

locations = [loc1, loc2, loc3]

for loc in locations:
    print(loc)
```

    Santiago, Chile
    Beijing, China
    Cairo, Egypt



```python
#8-7

def make_album(artist, title):
    album = {'artist':artist, 'title':title}
    return album

Pet_Sounds = make_album('The Beach Boys', 'Pet Sounds')
print(Pet_Sounds)

OffTheWall = make_album('Off the Wall', 'Michael Jackson')
print(OffTheWall)

VoulezVous = make_album('Voulez Vous', 'ABBA')
print(VoulezVous)

def make_album(artist, title, num = None):
    album = {'artist':artist, 'title':title, 'number of songs':num}
    return album

OncleJazz = make_album('Men I Trust', 'Oncle Jazz', 24)
print(OncleJazz)
```

    {'artist': 'The Beach Boys', 'title': 'Pet Sounds'}
    {'artist': 'Off the Wall', 'title': 'Michael Jackson'}
    {'artist': 'Voulez Vous', 'title': 'ABBA'}
    {'artist': 'Men I Trust', 'title': 'Oncle Jazz', 'number of songs': 24}

