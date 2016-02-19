
# Reading (and Writing) Files without numpy

Sometimes you will want to read files without relying on external libraries. This can be achieved using the standard Python function `open`, which (surprise) opens a file so that you can either read or write it.  


    file_to_read = open("data/inflammation-01.csv","r") # this opens it with read-access
    file_to_write = open("data/inflammation-new.csv","w") # this opens it with write-access

If you `open` a file with write-access it will overwrite any existing files! If you want to append new things to an existing file use *`a`* rather than `w`. 

Since Python 3 there is also `x` as a possible mode, which writes to a file only if it doesn't exist already, this should prevent many tears that otherwise would have been shed over lost data.


    file_to_append = open("data/inflammation-new.csv","a") # append
    file_to_write = open("data/inflammation-new.csv","x") # write if not existing


    ---------------------------------------------------------------------------

    FileExistsError                           Traceback (most recent call last)

    <ipython-input-2-73e8b8c4ffe8> in <module>()
          1 file_to_append = open("data/inflammation-new.csv","a") # append
    ----> 2 file_to_write = open("data/inflammation-new.csv","x") # write if not existing
    

    FileExistsError: [Errno 17] File exists: 'data/inflammation-new.csv'



Now that we've opened our file, what can we do with it in order to read the contents. Python allows for two easy ways to read a file, line-by-line. Let's discuss the easiest way first:


    file_to_read = open("data/inflammation-01.csv","r")
    file_content = file_to_read.readlines()
    print(file_content[:2])

    ['0,0,1,3,1,2,4,7,8,3,3,3,10,5,7,4,7,7,12,18,6,13,11,11,7,7,4,6,8,8,4,4,5,7,3,4,2,3,0,0\n', '0,1,2,1,2,1,3,2,2,6,10,11,5,9,4,4,7,16,8,6,18,4,12,5,12,7,11,5,11,3,3,5,4,4,5,5,1,1,0,1\n']


Using the `readlines()` function of an open file you can read in the whole content of the file in one go. The output of this function is a list in which each list item represents a single line of your file. 


    print([file_content[0]])

    ['0,0,1,3,1,2,4,7,8,3,3,3,10,5,7,4,7,7,12,18,6,13,11,11,7,7,4,6,8,8,4,4,5,7,3,4,2,3,0,0\n']


If you look at a single line in your resulting list you will notice that they all end with a `\n`. This is a special character denoting the end of a line, also called *line break*. (Depending on whether you use MacOS or Windows this can also be either a `\r` or even a `\r\n`.

Python comes with a function to remove those trailing special characters, which you can use on strings, called `strip()`


    print([file_content[0]])
    print([file_content[0].strip()])

    ['0,0,1,3,1,2,4,7,8,3,3,3,10,5,7,4,7,7,12,18,6,13,11,11,7,7,4,6,8,8,4,4,5,7,3,4,2,3,0,0\n']
    ['0,0,1,3,1,2,4,7,8,3,3,3,10,5,7,4,7,7,12,18,6,13,11,11,7,7,4,6,8,8,4,4,5,7,3,4,2,3,0,0']


Unlike when using `numpy` we now have the trouble that our measurements for each patient are still a single string instead of separate, numeric entries. Python strings offers the function `split()` to deal with this. 


    line = file_content[0].strip()
    print(line)
    splitted_line = line.split(",")
    print(splitted_line)

    0,0,1,3,1,2,4,7,8,3,3,3,10,5,7,4,7,7,12,18,6,13,11,11,7,7,4,6,8,8,4,4,5,7,3,4,2,3,0,0
    ['0', '0', '1', '3', '1', '2', '4', '7', '8', '3', '3', '3', '10', '5', '7', '4', '7', '7', '12', '18', '6', '13', '11', '11', '7', '7', '4', '6', '8', '8', '4', '4', '5', '7', '3', '4', '2', '3', '0', '0']


Split accepts a single parameter, allowing you to specify the character on which the string should be split. (If you need to split at a `TAB` use `\t` as the special character). 

Unfortunately this still doesn't give us what we were looking for, as the individual measurements are still strings and not numbers. We now could loop over each item in `splitted_line` and change the type, but that would be cumbersome. Instead we can use a trick, called *list comprehensions*, which allows to perform operations on each element in a list:


    line = file_content[0].strip()
    converted_line = [float(element) for element in line.split(",")]
    print(converted_line)

    [0.0, 0.0, 1.0, 3.0, 1.0, 2.0, 4.0, 7.0, 8.0, 3.0, 3.0, 3.0, 10.0, 5.0, 7.0, 4.0, 7.0, 7.0, 12.0, 18.0, 6.0, 13.0, 11.0, 11.0, 7.0, 7.0, 4.0, 6.0, 8.0, 8.0, 4.0, 4.0, 5.0, 7.0, 3.0, 4.0, 2.0, 3.0, 0.0, 0.0]


Earlier we talked about there being two ways to read in a file. Using `.readlines()` will read in the complete file content at the same time. Depending on the file size this can be a problem for your computers memory and you would rather go line by line. To achieve this you can easily just create a `for` loop using your opened file.


    for line in open("data/small-01.csv","r"):
        print(line.strip())

    0,0,1
    0,1,2


Together with `open(filename,"w")` this will allow you to manipulate the content of a file and write the output of the manipulation without the need to store everything in the active memory at once. 

This will also allow you to perform operations on each line in a single go.


    for line in open("data/small-01.csv","r"):
        processed_line = [float(element) for element in line.strip().split(",")]
        print(processed_line)

    [0.0, 0.0, 1.0]
    [0.0, 1.0, 2.0]


If you didn't use the `for line in open()` structure but assigned a variable to your open file (e.g. `myfile = open("data/small-01.csv")`) you will have to close your file. 

For writing files this is also the point in time when your output is actually written to the file.


    my_file = open("data/small-01.csv","r")
    lines = my_file.readlines()
    my_file.close()

Now let's try writing something to a file and read it back in.


    file_to_write = open("data/inflammation-new.csv","w") 
    file_to_write.write("this is some text.\n and here is some more.")
    file_to_write.close()
    
    file_to_read = open("data/inflammation-new.csv","r")
    lines = file_to_read.readlines()
    print(lines)

    ['this is some text.\n', ' and here is some more.']


Ok, this worked just fine. But what happens if we try to write one of our processed lines to a new file?


    processed_line = [0.0, 0.0, 1.0]
    file_to_write = open("data/inflammation-new.csv","w") 
    file_to_write.write(processed_line)



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-25-b1ccf2d7b74e> in <module>()
          1 processed_line = [0.0, 0.0, 1.0]
          2 file_to_write = open("data/inflammation-new.csv","w")
    ----> 3 file_to_write.write(processed_line)
    

    TypeError: must be str, not list


We can only write strings to a file, this is why it does not work. In order to write our processed data we have to reconvert each element of the list into a string. We can again use list comprehensions to do this. 

To convert a list into a comma-separated string we can use the `join()` function.


    processed_line = [0.0, 0.0, 1.0]
    processed_line = [str(element) for element in processed_line]
    print(processed_line)
    
    processed_line = ",".join(processed_line)
    print(processed_line)
    
    # you can not only use ",". Every string is ok!
    print(" NEXTELEMENT ".join(["a","b","c"]))

    ['0.0', '0.0', '1.0']
    0.0,0.0,1.0
    a NEXTELEMENT b NEXTELEMENT c


## Questions / Tasks

 * What happens if you just write the lines generated by the following command into a new file? And how can you amend the problem?


    for line in open("data/small-01.csv","r"):
        processed_line = [float(element) for element in line.strip().split(",")]

* You run the following code, what happens? Is it what you expected?


    my_file = open("data/small-01.csv","r")
    file_content = my_file.readlines()
    print(file_content[0])
     
    for line in my_file:
        print(line)

    0,0,1
    



    
