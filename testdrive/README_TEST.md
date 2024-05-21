# Testing Experience

This scenario is a test drive of how testing experience looks like when you use Test4z framework to unit test your application.
The application under test is DOGGOS, which should look familliar if you went through one of our Developer Experience workshops before.
We are going to go through some common tasks developers meet in their daily life.
Also we are going to see how Test4z extends the VSCode functionality so dealing with unit tests is easier.

## Run tests and generate Test Coverage Report 

Let's start with the positive scenario when tests are there, they are passing, and the only thing developer has to worry about is reporting the level of code coverage.

Right click on the "test" folder and select “Test4z Run All Tests with Coverage”. Reference screenshot: 

<img src='images/devx/devx3.png' width='65%'>

This will run tests and generate the report.
Take your time and look through the data presented in the report.

<img src='images/devx/devx4.png' width='65%'>

<img src='images/devx/devx5.png' width='65%'>

From the Explorer tab, Open the src folder.       
Open the source code file "ZTPDOGOS.cbl".            
VSCode Coverage Gutters in the source code file are observed.
When test results are available, Test4z is going to present the test coverage info as you navigate through the code.

<img src='images/devx/devx6.png' width='65%'>

## Failing the test 

We have seen what happens when things go well and tests are passing, but what if they fail? 
What kind of information developer is going to see in that case?
Let's see.

Open the ZTTDOGWS.cbl file under "test" folder

<img src='images/devx/devx7.png' width='65%'>

Go to line number 192.
Locate a code block which looks like the one on the screenshot. 
Change the value from 9 to 900.
Save the file.
Reference screenshot: 

<img src='images/devx/devx8.png' width='65%'>

From the command line, run the “t4z” command.
As you can see, tests can be executed not only from UI, but also via command line, handy when you want to integrate them with something like Jenkins pipeline.

Reference screenshot: 

<img src='images/devx/devx9.png' width='65%'>

<img src='images/devx/devx10.png' width='65%'>

You will observe that the test run fails. The reason is that expected Record count is 9 whereas we edited the value to be 900.

## Add statements to Test File

Now let's see how Test4z extends VSCode intellisense to enhance the test editing experience.

Open the ZTTDOGWS.cbl file under "test" folder

Go to line 51 (Be under the PROCEDURE DIVISION section). Reference Screenshot: 

<img src='images/devx/devx11.png' width='65%'>

Move the cursor to column 12.       
Type "t4z me".      
Note: It will start filling out the intellisense.      
Select “t4z Message write”.     

Reference Screenshots: 

<img src='images/devx/devx12.png' width='65%'>

This will fill in the code for the user. 

<img src='images/devx/devx13.png' width='65%'>

Replace “Your Message” with “Hello Test4z” and save the file. 

<img src='images/devx/devx14.png' width='65%'>

Let's fix the test so it is not failing again.

Go to line number 192.      
Locate a code block which looks like the one on the screenshot. 
Change the value back from 900 to 9.
Save the file.
Reference screenshot: 

<img src='images/devx/devx15.png' width='65%'>

From the command line, Run “t4z”. Reference screenshot of command line output.

<img src='images/devx/devx16.png' width='65%'>

Select “ZLMSG.txt” file from the “test-out” folder. This file contains the statements that are added to the test files. You will see the “Hello Test4z” statement that was added.

<img src='images/devx/devx17.png' width='65%'>         


#### Conclusion

This demo scenario shows how to generate a test coverage report, edit, fail a test case and add statements to a test file. 


