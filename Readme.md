# Mastermind
## A terminal based game.

This is documentation is a guide on how to install and use the application. The following topics are covered:

- Instllation
- Technologies used
- Challenge Requirements
- Attempted extra features.

## Installation
This application is built with Java 11 and MySQL server Version 5. A part of the applicaiton includes JUnit testing farmework to test the curial or the core function of the application. The instruciton provided here is based on the IntelliJ IDEA community edition IDE. Finally, there is a need for an API key (provided for this purpose) for random.org's API to access their random generator API. The two JAR files one for MySQL dependency and another for API is also provided with needs to be added into the module before being apple to use the application.

- To Download Java 11, please referen to this link https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html
- To install Java 11, please refer to this link https://docs.oracle.com/en/java/javase/11/install/
- To install MySQL Server community edition, please follow the following link https://dev.mysql.com/downloads/mysql/
- For help on how to use MySQL, click on the following link https://dev.mysql.com/doc/
- This link shows how to add a dependce/jar file into a java project in IntelliJ https://stackoverflow.com/questions/30651830/use-jdbc-mysql-connector-in-intellij-idea
- For insturction on how to use the random.org API, please visit this https://github.com/iarks/random_org-api-example
- Note: you don't need mysql server, if ou prefer to read and write guess history in the RAM. MySQL is used for presistance purposes.

To make sure you have Java 11 installed, type in the terminal 
- java -version
- Check Project File Structure to make sure depencies are added. [Markdown site][df1]

This text you see here is *actually- written in Markdown! To get a feel
for Markdown's syntax, type some text into the left window and
watch the results in the right.

## Technlogy
Familiarity with the following technologies can help.
- Java, C++
- SQL, PL/SQL, TSQL
- GitHub, for access repository
- MySQL Workbench, MySQL terminal App.


```sh
Java
node app
```
For production environments...

## Dependencies

Dillinger is currently extended with the following plugins.
Instructions on how to use them in your own application are linked below.

| Dependency | README |
| ------ | ------ |
| MySQL DBC Driver  | [plugins/dropbox/README.md][PlDb] |
| Random.org API | [plugins/github/README.md][PlGh] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Libraries insdie IDE
- import java.util.*;

## Development

Want to contribute? Great!

Dillinger uses Gulp + Webpack for fast developing.
Make a change in your file and instantaneously see your updates!

Open your favorite Terminal and run these commands.

First Tab:

```sh
node app
```
Second Tab:
```sh
gulp watch
```
(optional) Third:
```sh
karma test
```
## Desing of the app
The following variables are used at global scope.
- [computerPoints], initialized with 10. On Each correct guess, these points are deducted
- [userPoints], initialized with the value of 10. On each wrong guess, user loses points.
- [-difficultyLevelValue], There are 3 levels in this game. If user enters 1, the value of this variable changes to 4. Which means, the random number will consist of an array integer with length 4. If user's choice 2, the array length will be 6, and 3 will consist of 8 elements. The user's input also reduce the maximum number of tries to from 10, 8, 5 respectively. 
- [numberOfAttemps], the default value is 10. But it will change when user chooses a difficult level and if user keep guessing the number. It will reset when user agrees to play again.
- [computerFeedback], type String, this is a 2 Dimension array. A player's feedback is copied here to the player can review either after failling the game or winning the game. This method doesn't automatically push the data to MySQL. It is just in memory access data.
- [guessHistory], type String, will hold the information about the users guesses only to keep the record cleaner for review.
- [numberOfSavedFeedback]
- [numberSavedHistory]
- The last two variables are used to keep track of the saved results and feedbacks to avoid unncessary loops for null values. 
- [userName], This will recod a player's name on the initial start and will be saved with each player's game cycle.
- [isWin], keep status of the while loop to avoid unncessary loops. It is delcared at global scope to provide flexibilty for the other methods.

The following Methods are designed to either communit with the user or serve as a helper method.

- chooseDifficultyLevel(), asks the user for an integer input, then changes the numberOfRandomNumber, max, min limit for the random.org/api generator.
- generateRandomNumbers(), makes a get request to random.org/api to generate a random array of integers. Then it return the array for future use.
- isValid(String userGuess), will accept a userValue, then checks if the input has numberic values between 0 and 9, and also check if the userInput value's length is equal to difficulty level. if userInput is size 4, then the random array length must 4 too. Otherwise, it fails. It returns truthy/falsy.
- convertStringToArrayOfInteger(), accepts a string, in this case user's input, then converts the charcters to integer, so it can be compared.
- readUserInput(), in the terminal, it asks for the user to enter values.
- start(), this function job is to record user's name at global scoope. Then starts the game.
- play(), the engine for this application. This method/function triggers other functions such as chooseDifficultyLevel(), generateRandomNumbers(), wile loop, and calculate. ...
- calculate(), this method takes two arrays, then compares them to determin if their values match. For instance, arrayA = {1,2,3,1}, arrayB = {1,2,3,4}. There will be thee correct number on the correct location, One value 4, is incorrect because is not in the list. Since arrayA has two 1s, the exact location takes precedence over the wrong location. If arrayA = {1,2,4,3}, an arrayB = {2,1,3,4}, this will produce 4 correct numbers in the in the wrong positions. There is JUnit test written for this method, you can run your test. The difficult part in this challenge was to find the result for (true, false, false), if all matched, other two conditions will fail, if the second matched the first and last will fail, if the was true, the first two would fail. Perhaps, there many ways to solve this, but I ended up using multiple arrays then comparing the index values in both. if a match found, then replaced with a value out of the domain range in this case {x>=0}, since negative values are not not part of the domain, I keep replacing the tmpArray to keep testing. The Otherway, I thought to work on it was to use a string, if a number found the first match in the list, then increment the counter, if not, added to the string, so at the end, the string will cosist of not matched? true: false,
- guessedCorrectAndNumberLocation, it increments when two values at exact indices match in the two arrays.
- guessedCorrectNumber, it increments when a value doesn't have an exact position match but still exist in the list. It is only match with one value, if there are two {1,1}, {1,2}, only the first correct match increments.
- 
- incorrectGuess, after find the correct and incorrectValueAtWrongPost, then add them then subtract from the array.length;
- writeFeedbackIntoDB, this method takes an object with the result of numbers and saves in the database.
- saveFeedback(), saves the feedback in the RAM. 
- -getFeedback() reads from RAM. If the program stop, it loses all of its data.
- result(), just prints you won!
- saveGuessHistory(String guessed), save entered values into RAM
- -getHisotry prints them.
-
The following parts are manual. It is user's choice to install with the application or not. This app can work with mysql. To work without MySQL connection delete all methods with "DB" letters in their signature methods.
- writeInputHistoryINtoDB(String userName, String guess), write the user input with its in the database for future access. 
- I was tempted to use one -> manay relationship. A user can have many guesses, but due to the time constraints, I left it behind.
- There is no relationship model in this app. It just saves data and if need can delete them too.
- hint(int[] array), if a user guess 3 times, and still can find the number, it gives them a range of the minimum value and the maximum value in the array.
- resetHistory(), it sets all variables to inital state. If a player wants to play again, then this methods wipes out the told numberOfAttempts, history and feedback to start new.

## Conclusion
There couple lessons that learned by doing this coding challenge. Since this was my first one, I spent sometimes on deciding what I should use. Will just a web-page with css, html, and JavaScript suffice, will Nodejs + MySQL + React will be a good choice? I started doing some of each, then revisited my notes and saw that the recruiter had emphasized to use a technology based on the job you applied. At that memoment, I realized that I wasted sometime. Immediately, started planing on Java. The first thing I thought was to build a spring-boot api, but I noticed that a player can't interact directly without the api without prior knowldge of postman, insome and etc. At end decided a CLI app should be enough. Once the application was built, then wondered should I save the history into a file or a database, and finally decided to use MySQL because it runs on MacOS, Windows and Linux. This chanlleged thoght me that best way to learn is to build. I had nevere tried to access mysql from a just a plain java application without a middleware and a server.
