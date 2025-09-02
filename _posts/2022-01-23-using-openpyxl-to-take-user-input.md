openpyxl is a Python library to read/write Excel 2010 xlsx/xlsm/xltx/xltm files.

I recently purchased a road bicycle as a means of getting outside and increasing my cardiovascular stamina. I use the built-in WatchOS Activity app to track my rides and health data but, I wanted a way to get that data into a locally stored excel sheet for data collection and reference as my health improves. Python was the way!

Started reading documentation and stackoverflow threads on openpyxl but, it wasn’t very clear how you would take user input and append it to an existing excel file. Most use cases for openpyxl involve creating an excel document and copying over data to the newly created file, or inputting the data values yourself.

So how do you get it to write to an existing file?

You need to use workbook_obj = openpyxl.load_workbook to load the existing file.

Use sheet_obj to set the existing and active sheet. If you only have one sheet, this is what you would use.

otherwise it would look something like sheet_obj = [‘SheetName1’]

and you’d have to segment your code further to clean up the data you want entered.

How do you take user input and store it?

Name the input variable. That simple. VAR = input(“Input: “)

In order to sort this data and get it into a column:

col1, col2, col3, so on and so forth.

```
# Date/Time data
DATE = input("Date")
col1 = DATE

# Miles data
MILES = input("Miles")
col2 = MILES

# Time length data
TIME = input("How long did you ride?")
col3 = TIME

# Heart rate data
HTRATE = input("Average Heartrate")
col4 = HTRATE

```
I can now input the date, how many miles I rode, how long I rode for and my average heart rate throughout the ride. All data collected through my Apple watch and presented to me at the end of my ride.

Now, how does this user input data get appended to the existing file and sorted into those columns we set earlier?

`sheet_obj.append([col1, col2, col3, col4])`

Easily. Now format the columns as shown, and this will append all data to the first available row in each column. This makes it easy to keep adding data to the excel file.

to save:

`Workbook_obj.save(<"path">)`

