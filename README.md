
# shell-script-read-csv-and-sum-values

## Introduction

In this document we will discuss a simple task to run a shell script that reads a file from a user and print out the header of the file, and the lines one by one. It also performs a calculation over a measure in the file and print the result to an output file. 
This is done using Ubuntu Version 22 VM using AWS, and PuTTy to connect to the VM. Also using csvkit and awk commands in the shell script for processing the data.


## Connecting to VM

I have provided with some machine details like hostname, username, security key, and machine version. I was asked to connect to it using ssh, so I used PuTTy, which is an SSH and telnet client, developed originally by Simon Tatham for the Windows platform.

### First, I specified the Public key location in the Auth Section as showed:

 ![image](https://user-images.githubusercontent.com/108706173/221866681-a4c10fe5-1c59-4d33-8dfe-9d829f0cea05.png)


### Second, I filled the session section with the host name and saved the connection info for later use as showed:

![image](https://user-images.githubusercontent.com/108706173/221868577-c6303af0-52a3-4eef-9306-326a750ae695.png)

 
And after opening the terminal I was asked to provide the user name “ubuntu” and the I connected successfully as showed:
 
![image](https://user-images.githubusercontent.com/108706173/221866728-9c686923-e797-4130-a7d0-476a51d95783.png)


## Downloading a sample File

I created a directory called CSV_Files used command “curl” to download the sample csv file to the directory and called it sample.csv as Showed:
 
![image](https://user-images.githubusercontent.com/108706173/221866749-14a636d2-5b88-44cc-be93-37684af1f3e6.png)


curl -o sample.csv https://stats.govt.nz/assets/Uploads/Annual-enterprise-survey/Annual-enterprise-survey-2021-financial-year-provisional/Download-data/annual-enterprise-survey-2021-financial-year-provisional-size-bands-csv.csv 


## Developing The script


### First, I needed to install csvkit using command: “sudo apt install csvkit”.


### Second, I created a bash file called task_1.sh using “nano” command to create and open it. Then filled the file with the following code:

“””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””

head -n 1 $@ #print the header line

while read line  #begin the while loop with read command and line variable
do
   echo "Record is : $line"  #printing the line
done < $1 #taking the source file path as variable


csvformat -D "^" $1 | awk -F^ '{sum+=$6}END{print "The Sum of Values is:" sum}' > $2 #using csvformat from csvkit , we change the delimiter for the csv file from , to ^ because some columns contain a comma
#and this may cause erros, after that we pipe to awk so sum the values in the 6th column and pipe the result to the target file path 

“””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””””

 ![image](https://user-images.githubusercontent.com/108706173/221866772-5b805fca-9c0e-45fb-98f3-708e109491bb.png)

And saved and closed the file using ctrl+o >> enter >> ctrl+x


## Running the Script	

The final part was running the script, I used the following command:

‘’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’

bash task_1.sh CSV_Files/sample.csv Results/Task_1.txt

‘’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’’

Which read the data from the sample.csv file in the CSV_Files directory and print the output as showed:

![image](https://user-images.githubusercontent.com/108706173/221866791-41caf282-5e2c-43bc-b766-172d6072a291.png)
 

And the results will be printed in the Task_1.txt file in the Results Directory as showed:

![image](https://user-images.githubusercontent.com/108706173/221866807-88929555-5749-4592-9843-bbc2ae100acc.png)



