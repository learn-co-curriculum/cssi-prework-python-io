# File I/O
Python has handy methods for manipulating file types besides other Python scripts. These methods allows us to access other files, manipulate them or create new files. These methods are helpful when wanting to use non-Pythonic data or when we want to store our output somewhere other than the terminal screen.

## Objectives

1. Opening Files
2. Closing Files
3. Writing to Files
4. Reading Files

## Opening Files
Python has a built in `open()` function that allows us to access files. Each time this function is used, it returns a file object, which allows us to further manipulate that file.
```
my_file_object=open(filename,mode)
```

Notice the second parameter of the `open()` function specifies a mode:
`r` - read only
`w` - write, will overwrite the previous file (if it exists) otherwise a new file will be created
`w+` -allows you to read the data that was just written to the file
`a` - appends to existing file or creates a new one
`r+` - read and write

Once the appropriate mode is determined, call the `open()` function with the name of the file you are trying to read, create or append to, along with that mode.
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
Whenever the `open()` function is used a number is temporarily assigned to that file object. This number is called a file handle and helps the operating system understand how many files it needs to keep track of.

The `open()` method primes the file to be used in Python. Once the file is used, the file should immediately be closed, which saves the file, allows it to be opened again and will also erase the file handle.

## Closing Files
Python will eventually close any file objects that were used, but it is best practice to explicitly close the files after your script is done with them. This guarantees that your file will be saved. It also makes sure you can open the same object later on in the script. Finally, using `close()` eliminates extra memory being wasted on file handles.

The syntax is pretty simple, just the name of the file object chained with the .close() method.
```python
my_file_obj.close()
```


## Writing to Files

Let's make a new file, movies.txt and add some data to it. You can follow along by opening up the Python console in terminal. You can navigate to any directory that you don't mind using as a playground.

The first step is to create a file object using the `open()` function. Since we know we want to add data to this new file, we can use the `w` method.

```python
>>movie_file = open("movies.txt", "w")
```
Notice that we chose to assign the returned file object to the variable `movie_file`, but this could of course have any name you want.

After calling the open() function, you should see a new file, `movies.txt` has been created in whichever directory you opened the Python terminal in.

Now that the file object has been created with the mode `w`, we can write to our file using the write() method. This method writes strings to our file. Remember that since `write()` is a method, it is called by  chaining it to our file object.

```python
>>movie_file.write("Finding Nemo\nInside Out\nToy Story\nWallE")
```

When you call this command in the Python terminal, you should see Pixar's hits appear in your `movies.txt` file.

We always close the file when we're done with it.
```python
>>movie_file.close()
```

## Reading Files

The `read()` method starts at the start of a file. This method takes an optional parameter to indicate the number of characters to be read. If the parameter is omitted, the entire file is read.

To read our movies.text file, we need to create a new file object in read mode: `r`. Then we'll print whatever read() returns so we can see it.
```python
>>movie_file=open('movies.txt', 'r')
>>print movie_file.read(15)
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
The second parameter, `from_where` decides where the reference point is.0 is the default value and represents the start of a file, 1 is the current cursor position and 2 sets the reference point to the end of the file. The first parameter, `offset`, determines how many characters to move from the reference point.

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
This example was more illustrative than practical. Ideally, if you are reading a file multiple times, you should just write those contents to a variable within your script.
```python
movies=movie_file.read()
movie_file.close()
print movies[0:15]
Finding Nemo
In
print movies
Finding Nemo
Inside Out
Toy Story
WallE
```

### Iterating Through Lines
Python is clever when it comes to iterating through files. You can simply use the for/in syntax and the file will automatically be parsed by line.

for x in movie_file:
     print x

## The Magic of the With Keyword
It is important to close a file object each time. Python has a keyword, `with` which always closes a file, even if an error is raised in processing that file.

When using with, you can name the file object after the `open()` function by using the keyword `as`:
```python
with open('movies.txt', 'r') as movie_file:
  print movie_file.read()

```

The `with` keyword is a great way to open a file, process its contents, and to ensure that it will be closed.
