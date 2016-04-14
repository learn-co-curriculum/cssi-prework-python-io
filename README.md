# File Input/Output
Python has handy methods for manipulating files. These methods allows us to access other files, process them and create new files. These methods are helpful when wanting to use non-Pythonic data or when we want to store our output somewhere other than the terminal screen.

## Objectives

1. Open files from a Python script
2. Close files from a Python script
3. Write to files from a Python script
4. Read files from a Python script
5. Use the 'with' Keyword

## Opening Files
Python has a built in `open()` function that allows us to access files. Each time this function is used, it returns a file object, which allows us to further manipulate that file.
```
my_file_object=open(filename,mode)
```

Notice the second parameter of the `open()` function specifies a mode:

+ `'r'` - read only
+ `'w'` - write, will overwrite the previous file (if it exists) otherwise by using 'w' **a new file will be created** (you'll do this below)
+ `'w+'` -allows you to read the data that was just written to the file.
+ `'a'` - appends to existing file or creates a new one
+ `'r+'` - read and write

Once you've figured out what you want to do with the file, call the `open()` function with the name of the file you are trying to read, create or append to, along with that mode.

In the case below, let's say we have a file called movies.txt that has a list of all of my favorite movies.

```python
#read the contents of movies.txt (starts at the top)
my_file_obj=open('movies.txt', 'r')

#read the contents of movies.txt and then write additional data to the movies.txt file (starts at the top)
my_file_obj=open('movies.txt', 'r+')

#overwrite or create favorite_movies.txt (starts at the top)
my_file_obj=open('favorite_movies.txt', 'w')

#adds to the end of favorite_movies.txt
my_file_obj=open('favorite_movies.txt', 'a')

```
Whenever the `open()` function is used, a number is temporarily assigned to that file object. This number is called a "file handle" and helps the operating system understand how many files it needs to keep track of.

## Closing Files
Python will eventually close any file objects that were used, but it is best practice to explicitly close the files *after your script is done with them*. This guarantees that your file will be saved. It also makes sure you can open the same object later on in the script. Finally, using `close()` eliminates extra memory being wasted on file handles.

The syntax is pretty simple, just the name of the file object chained with the .close() method.
```python
my_file_obj.close()
```

## Writing to Files

Let's make a new file, movies.txt and add some data to it - this is the **output** part of the lesson. You can code along by creating a new python script `create_write_files.py` and running it from your console with the `python` keyword. 

Our first step is to create a file object using the `open()` function. Since we know we want to add data to this new file, we can use the `w` argument (which will also create the file itself).

```python
>>movie_file = open("movies.txt", "w")
```
Notice that we chose to assign the returned file object to the variable `movie_file`, but this could of course have any name you want.

If you run the script now, you'll see that a new file, `movies.txt`, has been created in whichever directory you ran the script from.

Now that the file object has been created with the mode `w`, we can write to our file using the write() method. This method writes strings to our file. Remember that since `write()` is a method, it is called by  chaining it to our file object.

```python
>>movie_file.write("Finding Nemo\nInside Out\nToy Story\nWallE")
```
(The '\n' character is a newline, meaning that each time you see it you're placing the remaining content on a new line (like hitting return))

When you run your script now, you should see Pixar's hits appear in your `movies.txt` file.

We always close the file when we're done with it.
```python
>>movie_file.close()
```

Your whole script should look like this:
```
movie_file = open("movies.txt", 'w')
movie_file.write("Finding Nemo\nInside Out\nToy Story\nWallE")
movie_file.close()
```

Try creating a new file and using the open, write, and close functions to add three more Pixar films to the list. Use the 'a' mode (instead of 'w') to append to an already existing list.


## Reading Files

Let's now *read* a file in a new script. Create the python script `reading_files.py` and open it up in your text editor. We'll use the file we created above (movies.txt) to read from.

In order to use data in a file, we will need to load it into our script as **input**. The `read()` method starts at the start of a file. This method takes an optional parameter to indicate the number of characters to be read. If the parameter is omitted, the entire file is read.

To read our movies.text file, we need to create a new file object in read mode: `r`. Then we'll print whatever read() returns so we can see it. 

```python
>>movie_file=open('movies.txt', 'r')
>>print movie_file.read(15) #Read the first 15 characters.
Finding Nemo
In
>>print movie_file.read()
side Out
Toy Story
WallE
movie_file.close()
```
Huh, what happened? Since we called two `read()` methods in a row on the same file object, the file pointer never gets reset. After the first print statement, the file pointer (like a cursor) is in between the `n` and `s` in `Inside Out`. When that second print statement is called, the rest of the file is read starting at the file pointer.

### Reseting the Cursor
To change where in a file to start reading from or writing to, the seek() operation can be used. The syntax is below
```python
my_file_object.seek(offset, from_where)
```
The second parameter, `from_where` decides where the reference point is. 0 is the default value and represents the start of a file, 1 is the current cursor position and 2 sets the reference point to the end of the file. The first parameter, `offset`, determines how many characters to move from the reference point.

```python
>>movie_file=open('movies.txt', 'r')
>>print movie_file.read(15)
Finding Nemo
In
>>movie_file.seek(0)
>>print movie_file.read()
Finding Nemo
Inside Out
Toy Story
WallE
movie_file.close()
```
This example was more illustrative than practical. Ideally, if you are just reading a file multiple times, you should just write those contents to a variable within your script.
```python
>>> movies=movie_file.read()
>>> movie_file.close()
>>> print movies[0:15]
Finding Nemo
In
>>> print movies
Finding Nemo
Inside Out
Toy Story
WallE
```

### Iterating Through Lines
Python is clever when it comes to iterating through files. You can simply use the for/in syntax and the file will automatically be parsed by line.

```python
>>> for x in movie_file:
>>>      print x.upper()
FINDING NEMO
INSIDE OUT
TOY STORY
WALLE
```

## The Magic of the With Keyword
It is important to close a file object each time. Python has a keyword, `with` which always closes a file, even if an error is raised in processing that file.

When using `with`, you can name the file object after the `open()` function by using the keyword `as`:
```python
with open('movies.txt', 'r') as movie_file:
  print movie_file.read()
```

In the example above, the movies.txt file is opened and saved to a variable called movie_file. We can then manipulate the movie_file object in the code block. When we exit the code block the file is closed.

The `with` keyword is a great way to open a file, process its contents, and to ensure that it will be closed.

## Conclusion
Knowing how to create file objects in Python allows us to process or create files. This expands the scope of what we can do within our scripts.

## Resources
[Python Documentation on IO](https://docs.python.org/2/tutorial/inputoutput.html)
[Python for You and Me - File IO](http://pymbook.readthedocs.org/en/latest/file.html)
