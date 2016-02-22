
# Using Dictionaries

So far we dealt with some different standard data types of Python, including numerical values (integers and floats), strings and lists. Additionally there were the arrays provided by *numpy*. There is one last, very important data type: Dictionaries (which can be though of as hashes, if you come from another programming language).

The name *Dictionary* already gives you an idea of what this data type does: It stores values associated with a given key, just as a foreign language dictionary would do:


    dictionary = {"my":"mein",
                  "hovercraft":"Luftkissenboot",
                  "is":"ist","full":"voll","of":"von",}
    print(dictionary["hovercraft"])

    Luftkissenboot


Accessing the values of a dictionary is similar to accessing a given position in a list, you pass it to the dictionary using square brackets. You can also use this to create new entries in your dictionary.


    dictionary["eels"]


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-2-e059547cb67b> in <module>()
    ----> 1 dictionary["eels"]


    KeyError: 'eels'


The key *eels* does not exist yet, this is why we are getting a key error. So we create it.


    dictionary["eels"] = "Aale"
    print(dictionary["eels"])

    Aale


To initialize an empty dictionary you can use `dictionary = {}`. You can use strings, integers and floats as your keys for the dictionary. Your values can be basically everything. It can also be a list or even a dictionary.


    example_dictionary = {}

    example_dictionary[1.0] = "1,0"
    example_dictionary[2] = 1
    example_dictionary["list"] = ["yes","this","is","possible"]
    example_dictionary["another_dictionary?"] = {"yes":"that","works":"as well"}

    print(example_dictionary["another_dictionary?"])

    {'yes': 'that', 'works': 'as well'}


**Unlike lists, dictionaries are not ordered. So if you loop over them you are not guaranteed that they will always have the same order.**

By default the loop will iterate over the keys.


    for key in example_dictionary:
        print(key)

    1.0
    2
    another_dictionary?
    list


Often you want to have both, the key and value at the same time. For this you can use the dictionary function `.items()`:


    for key,value in example_dictionary.items():
        print(key,value)

    1.0 1,0
    2 1
    another_dictionary? {'yes': 'that', 'works': 'as well'}
    list ['yes', 'this', 'is', 'possible']


## Questions / Tasks

* How does your dictionary look like when you fill it like this: `dictionary = {1.0 = "a", 1="b"}`?

* You have two dictionaries, `shopping_list` and `prices`. Use it to calculate the total cost of your shopping trip.

```
shopping_list = {"banana":2,
    "apple": 4,
    "orange": 1,
    "pear": 3,
    "chocolate": 1}
```

```
prices = { "banana":0.39,
    "apple": 0.20,
    "orange": 0.30,
    "pear":0.20,
    "chocolate":0.90
    }
```
