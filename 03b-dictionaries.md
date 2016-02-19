
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
    list
    another_dictionary?


Often you want to have both, the key and value at the same time. For this you can use the dictionary function `.items()`: 


    for key,value in example_dictionary.items():
        print(key,value)

    (1.0, '1,0')
    (2, 1)
    ('list', ['yes', 'this', 'is', 'possible'])
    ('another_dictionary?', {'yes': 'that', 'works': 'as well'})


## Questions / Tasks

* How does your dictionary look like when you fill it like this: `dictionary = {1.0 = "a", 1="b"}`?
* You have two files, `data/merge-1.csv` and `data/merge-2.csv`. Each line in the file looks like this: 

File 1
```
patient_a,1,0,2,3,4
patient_b,2,3,1,0,23
…
```
File 2
```
patient_c,3,0,1,1,2
patient_a,4,5,3,0,2
…
```

Each patient is represented in both files, but they are not ordered. You want to merge the data for each patient from the two files into a single one and save the output.


    
