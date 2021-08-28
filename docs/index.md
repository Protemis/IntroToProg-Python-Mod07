# Assignment 07

## Introduction

In this assignment, I will demonstrate two concepts of python: Structured Exception Handling and Pickling. The script builds upon my previous knowledge with using structured exception handling and pickling.

I found this [link](https://www.datacamp.com/community/tutorials/exception-handling-python) to be an excellent resource due to its robust explanations of all things exceptions. This [link](https://stackabuse.com/file-handling-in-python/) was also helpful for handling file modes.

## Structured Exception Handling

I created a math quiz to demonstrate Structured Exception Handling. Structured Exception Handling allows coders to make code-bases that are more robust and communicate issues in a more user-friendly way. The math quiz that I created covers the try-except method and defining custom exceptions utilizing an if statement.

The try-except method works in two steps. The first, the try part, looks for exceptions within a given block of code. The except part then allows the coder to determine what the code should do next whether it's print an error message, ignore and continue, etc.

### Writing the code

Defining custom exceptions allows the user to customize exception types and messages. This can be useful if you have an exception situation that isn’t necessarily covered by python’s built-in exceptions.

I started writing the code by capturing user’s responses to three math problems. I then used an if/elif to validate the answers and printing messages before adding exception handling (figure 1-1).

```
if answer1 != '50:
  print('You have gotten question 1 right')
```
*Figure 1-1: Part of if statement before adding exception handling*

I then defined the exceptions if the user got an answer wrong (figure 1-2). 
```
class question_1_check(Exception):
    """  Project name must be input as lower case  """
    def __str__(self):
        return 'You have gotten question 1 wrong. Please retake the quiz.\n'
```
*Figure 1-2: Defining question 1 exception*

Finally, I added the try exception method, replacing the printed messages by raising the custom exceptions if the user got an answer wrong, and printing the exception message. I added an if statement to allow the user to retake the quiz or exit. See Figure 1-3 for the completed code. I am not sure that exception handling is the best way to create a quiz like this because if a user got more than one question wrong my script would just inform them of the first question that they got wrong.

```
# Error Handling
class question_1_check(Exception):
    """  Project name must be input as lower case  """
    def __str__(self):
        return 'You have gotten question 1 wrong. Please retake the quiz.\n'
class question_2_check(Exception):
    """  Project name must be input as lower case  """
    def __str__(self):
        return 'You have gotten question 2 wrong. Please retake the quiz.\n'
class question_3_check(Exception):
    """  Project name must be input as lower case  """
    def __str__(self):
        return 'You have gotten question 3 wrong. Please retake the quiz.\n'

while(True):
    try:
        print('           Quick Quiz\n'+'*'*30+'\n Press enter to continue')
        input()
        answer1 = input('Question 1\nWhat is 24+26?\n')
        answer2 = input('Question 2\nWhat is 5*10?\n')
        answer3 = input('Question 3\nWhat is 100/2?\n')
        if answer1 != '50':
            raise question_1_check()
        elif answer2 != '50':
            raise question_2_check()
        elif answer3 != '50':
            raise question_3_check()
        else:
            print('You have won!')
            strchoice = input('Enter 1 to retake the quiz or 2 to exit')
            if strchoice == '2':
                break
            elif strchoice == '1':
                continue
    except question_1_check as e:
        print(e)
    except question_2_check as e:
        print(e)
    except question_3_check as e:
        print(e)
```
*Figure 1-3: Completed math quiz code*

### Running the script

I ran the script in PyCharm (figure 1-4) and then opened the command prompt on my windows computer and copied the file path. 

I then used the python.exe command with the file path to run my script, Assigment07_Error_Handling.py. The script ran as expected (figure 1-5)

![Figure 1-4](/docs/Figure%201-4.png "Figure 1-4") 

*Figure 1-4: Screenshot showing commands to locate the proper folder and run Assigment07_Error_Handling.py in PyCharm*

![Figure 1-5](/docs/Figure%201-5.png "Figure 1-5") 


*Figure 1-5: Screenshot showing commands to locate the proper folder and run Assigment07_Error_Handling.py in windows command prompt*

## Pickling

In order to not waste good code, I updated my script from assignment 6 to demonstrate pickling. Pickling allows users to save objects in binary files and later retrieve those objects intact. This is different from writing an object to a txt file as you must format the object into a csv format when writing and format the txt file back into the object when reading. The other advantage to pickling when writing or reading a pickle is that the syntax is more succinct than when writing or reading to a txt file.

### Writing the code

In order to not waste good code, I updated my script from assignment 6 to demonstrate pickling. Pickling allows users to save objects in binary files and later retrieve those objects intact. This is different from writing an object to a txt file as you must format the object into a csv format when writing and format the txt file back into the object when reading. The other advantage to pickling when writing or reading a pickle is that the syntax is more succinct than when writing or reading to a txt file.

To update the assignment 6 code to incorporate pickling, I had to update two custom functions, read_data_from_file() and write_data_to_file(). 

I began by importing the pickle module so that I would have access to the relevant pickle functions. I then updated the strFileName variable to "ToDoFile.dat" so that the code would save the todo list in a .dat file instead of a .txt file.

I updated the read_data_from_file() function to open the specified file in rb mode which opens the file as read-only in binary format. I used the pickle.load() function to load the contents into a the list_of_rows variable. I returned the list_of_rows variable after closing the file (figure 2-1).

```
    @staticmethod
    def read_data_from_file(file_name, list_of_rows):
        """ Reads data from a file into a list of dictionary rows

        :param file_name: (string) with name of file:
        :param list_of_rows: (list) you want filled with file data:
        :return: (list) of dictionary rows
        """
        try:
            list_of_rows.clear()  # clear current data
            objFile = open(file_name, "rb")
            list_of_rows = pickle.load(objFile)
            objFile.close()
            return list_of_rows, 'Success data reloaded from file'
        except:
            print()
```

*Figure 2-1: Screenshot showing the updated read_data_from_file() function*

```
    @staticmethod
    def write_data_to_file(objFile, list_of_rows):
        """ Write list of dictionary rows to .dat file

        :param objFile: (string) name of data file:
        :param list_of_rows: (list) you want to save to file:
        :return: (list) of dictionary rows
        """
        objFile = open(objFile, "wb")
        for row in lstTable:
            pickle.dump(list_of_rows, objFile)
        objFile.close()
        return list_of_rows, 'Success, Your data has been saved'
```

*Figure 2-2: Screenshot showing the updated write_data_to_file() function*

I also updated the script to address an issue highlighted in the Assignment06 feedback. If a user selected option 2 (remove task) but did not enter a task that was in the task list the code still printed a message that a task was removed. To fix this, I checked if the user provided a task that was in the task list. If they did, the code removed that task if not then the code would print a message and direct the user to the main menu.

```
    elif strChoice == '2':  # Remove an existing Task
        strtaskremove = IO.input_task_to_remove() #get task to remove from user
        removecounter = 0
        for row in lstTable:
            if row['Task'] == strtaskremove:
                removecounter = removecounter + 1
        if removecounter > 0:
            lstTable, strStatus = Processor.remove_data_from_list(strtaskremove, lstTable) #remove task from lstTable
            IO.input_press_to_continue(strStatus) #display status
        else:
            input('\nTask was not removed as it is not in the list. Press Enter to return to the main menu\n')
        continue  # to show the menu
```

*Figure 2-3: Screenshot showing the updated remove task code block*

I did run into an issue where after running the Processor.read_data_from_file() and updating the list_of_rows variable with the contents of the .dat file, the list_of_rows variable in the IO.print_current_Tasks_in_list() function was blank and so was not able to print the current tasks. I fixed it by unpacking the tuple returned by Processor.read_data_from_file() but was confused as to why I hadn’t needed to do so in Assignment 6. Maybe importing the pickle module changed how the variables behaved.

### Running the script
I ran the script in PyCharm (figure 2-4) and then opened the command prompt on my windows computer and copied the file path. 
I then used the python.exe command with the file path to run my script, Assigment07_Pickling.py. The script ran as expected (figure 2-5).

![Figure 2-4](/docs/Figure%202-4.png "Figure 2-4") 

*Figure 2-4: Screenshot showing commands to locate the proper folder and run Assigment07_Pickling.py in PyCharm*

![Figure 2-5](/docs/Figure%202-5.png "Figure 2-5") 

*Figure 2-5: Screenshot showing commands to locate the proper folder and run Assigment07_Pickling.py in windows command prompt*

![Figure 2-6](/docs/Figure%202-6.png "Figure 2-6") 

*Figure 2-6: Screenshot of ToDoFile.dat directory location and contents*

## Summary

Using what I learned in Module 07, I was able to successfully create two scripts demonstrating structured exception handling and pickling.
