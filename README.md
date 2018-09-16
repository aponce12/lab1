# lab1
Lab1A-Recursion
##CS 2302 Data Structures
##Instructor:Diego Aguirre
##Project 1 Option A
##Modified and submitted by Andres Ponce 80518680
##Date of last modification 09/16/2018
##Purpose: Use recursion to traverse through directories, grab files and classify
##the files according to type (Dog or cat).
##Extra Credit: receives dog and cat lists, select one to adopt and delete it from list

import os
import random


def get_dirs_and_files(path):
    dir_list = [directory for directory in os.listdir(path) if os.path.isdir(path + '/' + directory)]
    file_list = [directory for directory in os.listdir(path) if not os.path.isdir(path + '/' + directory)]

    return dir_list, file_list


def classify_pic(path):
    # To be implemented by Diego: Replace with ML model
    if "dog" in path:
        return 0.5 + random.random() / 2

    return random.random() / 2

#Method receives a path from-list user, traverse directories from path using recursion, search for files and classify by kind (dog or cat)
#using classify_pic method. Returns two array lists dog_list and cat_list
def process_dir(path):

    dir_list, file_list = get_dirs_and_files(path)
    dog_list=[]
    cat_list=[]
    
    # Your code goes here
    i=0
    #Recursion step to traverse directories
    if i<len(dir_list):
        for i in range (len (dir_list)):
            process_dir(path +"\\"+dir_list[i])
    #loop to classify pictures into arrays
    for j in range (len(file_list)):
        value=classify_pic(file_list[j])
        if value>0.5:
            dog_list=(makeDog_array(file_list[j]))
        else:
            cat_list=(makeCat_array(file_list[j]))

    return dog_list, cat_list

#Method that receives picture files and store them into dog array
tempDog_list=[]
def makeDog_array(file):
    if file not in tempDog_list:
        tempDog_list.append(file)
    
    return tempDog_list
#Method that receives picture files and store them into cat array
tempCat_list=[]
def makeCat_array(file):
    if file not in tempCat_list:
        tempCat_list.append(file)
    
    return tempCat_list

#Method to adopt any animal from the two arrays and remove selected from array
def get_any_animal_type(dogList,catList):
    adoptAny=[]
    adoptAny=dogList+catList
    date=0
    #loop to traverse array and look for the highest number, 
    for i in range(len(adoptAny)):
        #Split is used to catch the number, compare and look for highest number
        splitString=adoptAny[i].split(".")
        if int(splitString[1])>int(date):
            date=splitString[1]
            adopted=adoptAny[i]
    #loops are used to search and remove selected item from array
    for i in range (len(dogList)):
        if adopted in dogList[i]:
            dogList.remove(dogList[i])
            break
        else:
            for j in range(len(catList)):
                if adopted in catList[j]:
                    catList.remove(catList[j])
                    break
    #returns adopted item, updated dogList and catList
    return adopted, dogList, catList

#This method is used if only cat is selected for adoption    
def get_cat (catList):
    date=0
    #loop search for highest number in array
    for i in range(len(catList)):
        #split is used to search by the highest number
        splitString=catList[i].split(".")
        if int(splitString[1])>int(date):
            date=splitString[1]
            adopted=catList[i]
    #loop is used to remove the selected item from array
    for j in range(len(catList)):
        if adopted in catList[j]:
            catList.remove(catList[j])
            break
    #Method returns the adopted item and updated catList
    return adopted, catList

#This method is used if only dog is selected for adoption
def get_dog(dogList):
    date=0
    #loop search for highest number in array
    for i in range(len(dogList)):
        #split is used to search by highest number
        splitString=dogList[i].split(".")
        if int(splitString[1])>int(date):
            date=splitString[1]
            adopted=dogList[i]
    #loop is used to remove the selected item from array
    for i in range (len(dogList)):
        if adopted in dogList[i]:
            dogList.remove(dogList[i])
            break
    #Method returns the adopted item and updated dogList
    return adopted, dogList
    
def main():
    path=input("Enter path: ")
#Received path is adjusted for the method entry
    path=path[2:]
#path is assigned to start_path
    start_path = (r""+path+"")#'./' current directory
#process_dir() method is called with the start_path variable, returned items are assigned to dog_list and cat_list
    dog_list,cat_list=process_dir(start_path)
    print("These are the list of dogs and cats available: ")
    print(dog_list,"\n")
    print(cat_list,"\n")
#answ variable receives the user input desired for adoption
    answ=input("Do you want to adopt? Any, Dog, Cat: \n")
#conditionals are used to call methods for adoption
    if answ in "Any":
        adoptedAny,dog_list,cat_list=get_any_animal_type(dog_list, cat_list)
        print("You adopted ",adoptedAny)
    if answ in "Cat":
        adoptedCat,cat_list=get_cat(cat_list)
        print("You adopted ",adoptedCat)
    if answ in "Dog":
        adoptedDog,dog_list=get_dog(dog_list)
        print("You adopted ",adoptedDog)
    
    
main()

