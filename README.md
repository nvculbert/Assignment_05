#------------------------------------------#
# Title: CDInventory.py
# Desc: Starter Script for Assignment 05
# Change Log: (Who, When, What)
# DBiesinger, 2030-Jan-01, Created File
# nickculbert, 2021-Feb-14, edited main body
#------------------------------------------#

# Declare variables

strChoice = '' # User input
oldLstTbl = []
newLstTbl = []
lstTbl = []  # list of lists to hold data
lstRow = []  # list of data row
dictRow = {} # dict of list row
strDelChoice = ''
strFileName = 'CDInventory.txt'  # data storage file
objFile = None  # file object

objFile = open(strFileName, 'a')
objFile.close()

# Get user Input

print('The Magic CD Inventory\n')
while True:
    
    # Display menu allowing the user to choose:
    print('[l] load Inventory from file\n[a] Add CD\n[i] Display Current Inventory')
    print('[d] delete CD from Inventory\n[s] Save Inventory to file\n[x] exit')
    strChoice = input('l, a, i, d, s or x: ').lower()  # convert choice to lower case at time of input
    print()

    # 'x' Exit the program if the user chooses so
    if strChoice == 'x':
        break
    
    # 'l' Load existing data
    if strChoice == 'l':
        objFile = open(strFileName, 'r')
        for row in objFile:
            lstRow = row.split('|')
            dictRow = {'ID':int(lstRow[0].strip()), 'Title':lstRow[1].strip(), 'Artist':lstRow[2].strip()}
            oldLstTbl.append(dictRow)
        objFile.close()
        print('Data loaded!\n')
    
    # 'a' Add data to the table (2d-list) each time the user wants to add data
    elif strChoice == 'a':
        strID = input('Enter an ID: ')
        strTitle = input('Enter the CD\'s Title: ')
        strArtist = input('Enter the Artist\'s Name: ')
        intID = int(strID)
        dictRow = {'ID':intID, 'Title':strTitle, 'Artist':strArtist}
        newLstTbl.append(dictRow)
        print('\nData added!\n')
    
    # 'i' Display the current data to the user each time the user wants to display the data
    elif strChoice == 'i':
        print('ID | CD Title | Artist')
        print('----------------------\n')
        
        lstTbl = oldLstTbl + newLstTbl
        
        # Check to see if there is data first
        if not lstTbl:
            print('No data in queue!\n')
            
        for row in lstTbl:
            strRow = ''
            for key, val in row.items():
                strRow += str(val) + ' | '
            strRow = strRow[:-2] + '\n'
            print(strRow)
        print()
    
    # 'd' Deleting an entry
    elif strChoice == 'd':
        print()
        strDelChoice = input('Please type the ID of the entry you wish to delete: ')
        
        # Account for user error
        try:
            intDelChoice = int(strDelChoice)
        except:
            print('An error occured because you failed to enter an ID number.')
        
        for row in lstTbl:
            if row['ID'] == intDelChoice:
                row.clear()
                objFile = open(strFileName, 'r')
                fileData = objFile.readlines()
                objFile.close()
                objFile = open(strFileName, 'w')
                for row in fileData:
                    if row[0] != strDelChoice:
                        objFile.write(row)
                objFile.close()
                print('\nCD ' + strDelChoice + ' deleted!\n')
    
    # 's' Save the data to a text file CDInventory.txt if the user chooses so
    elif strChoice == 's':
        objFile = open(strFileName, 'a')
        for row in newLstTbl:
            strRow = ''
            for key, val in row.items():
                strRow += str(val) + ' | '
            strRow = strRow[:-2] + '\n'
            objFile.write(strRow)
        objFile.close()
        oldLstTbl.clear()
        newLstTbl.clear()
        lstTbl.clear()
        print('Data saved! Your queue is cleared!\n')
    
    # User policing
    else:
        print('Please choose either l, a, i, d, s or x!')

