# Table of Contents
1. [Challenge Summary](README.md#challenge-summary)
2. [Details of Implementation](README.md#details-of-implementation)
3. [Anomalous Purchases](README.md#anomalous-purchases)
4. [Sample Data](README.md#sample-data)
5. [Writing Clean, Scalable, and Well-tested Code](README.md#writing-clean-scalable-and-well-tested-code)
6. [Repo Directory Structure](README.md#repo-directory-structure)
7. [Testing your Directory Structure and Output Format](README.md#testing-your-directory-structure-and-output-format)
8. [Instructions to Submit your Solution](README.md#instructions-to-submit-your-solution)
9. [FAQ](README.md#faq)


# Challenge Summary

Picture yourself as a backend engineer of Market-ter, an e-commerce website where besides shopping users can build their social network and get recommendations based on what their friends are buying. Your challenge is to build a streaming application which flags anomalous purchases.

## Details of Implementation
With this coding challenge, you should demonstrate a strong understanding of computer science fundamentals. We won't be wowed by your knowledge of various available software libraries but will be impressed by your ability to pick and use the best data structures and algorithms for the job.

We're looking for clean, well-thought-out code that correctly implements the desired feature in an optimized way and highlights your ability to write production-quality code and clear documentation.

### Anomalous Purchases

A user purchase is considered anomalous if the amount spent is higher than the `mean+3*sd` of the last T purchases in the user’s Dth degree social network.

D >= 1: D=1 means that we consider only the friends of the user, D=2 corresponds to friends and friends of friends, and so on so forth

T >= 2: the last T purchases are the T purchases in the user's network (not including the user's own purchases) with the highest timestamps (if two events have the same timestamp the one entering the system first is considered to be earlier). If there is 0 or 1 purchase in the user's network we don't have enough information to detect the anomaly so the purchase shouldn't be flagged. If the user's network has at least 2 but not T purchases so far than the calculation should be done using the current number of purchases.

For simplicity we can assume that the mean and standard deviation of the purchase amounts can be calculated based on the formulas below:

FORMULA - MEAN

FORMULA - STD

### Input Data
Ideally, the input data would come from a real-time, streaming API, but we don't want this challenge to focus on the relatively uninteresting "DevOps" of connecting to an API.

As a result, you may assume that collecting the purchases and social network events has been done for you and the data resides in two files (in the log_input directory) which we can replay to mimic the data stream.

The first file, batch_log.json, contains past data that should be used to build the initial state of the entire user network as well as the purchase history of the users.

Data in the second file, stream_log.json, should be used to determine whether a purchase is anomalous. If a purchase is flagged anomalous the whole json record should be logged in the flagged_purchases.json file. As events come in both the social network and the purchase history of users get updated.

The first line of batch_log.json contains a json object with the degree (D) and number of purchases (T) to consider for the calculation.
The rest of the events both in batch_log.json and in stream_log.json fall into to following 3 categories:
 - purchase
 - befriend
 - unfriend
 
Purchase events have a timestamp, user id and the amount paid. Befriend events represent two users becoming friends while unfriend events correspond to users deleting their connections (all friendships are considered bidirectional).

e.g., `batch_log.json`:

    {"D":"3", "T":"50"}
    {"event_type":"purchase", "timestamp":"2017-06-13 11:33:01", "id": "1", "amount": "16.83"}
    {"event_type":"purchase", "timestamp":"2017-06-13 11:33:01", "id": "1", "amount": "59.28"}
    {"event_type":"befriend", "timestamp":"2017-06-13 11:33:01", "id1": "1", "id2": "2"}
    {"event_type":"purchase", "timestamp":"2017-06-13 11:33:01", "id": "1", "amount": "11.20"}
    {"event_type":"unfriend", "timestamp":"2017-06-13 11:33:01", "id1": "1", "id2": "3"}


e.g., `stream_log.json`:

    {"event_type":"purchase", "timestamp":"2017-06-13 11:33:02", "id": "2", "amount": "1601.83"}

### Output Data
Write to a file, named `flagged_purchases.json`, all the anomalous purchase events (in their original order). Flagged events are still valid and can contribute to other users' baseline.

e.g., `flagged_purchases.json`:

    {"event_type":"purchase", "timestamp":"2017-06-13 11:33:02", "id": "2", "amount": "1601.83"}

### Sample Data
You can download a medium sized sample data set here: XXX

### Optional Features
Feel free to implement additional features that might be useful to derive further metrics or prevent harmful activity. These features will be considered as bonus while evaluating your submission. If you choose to add extras please document them in your `README` and make sure that they don't interfere with the core feature (e.g. don't alter the output of flagged_purchases.json).

## Writing Clean, Scalable, and Well-tested Code

As a data engineer, it’s important that you write clean, well-documented code that scales for large amounts of data. For this reason, it’s important to ensure that your solution works well for a huge number of logged events, rather than just the simple examples above.

For example, your solution should be able to account for a large number of events coming in over a short period of time, and need to keep up with the input (i.e. need to process a minute worth of events in less than a minute).

It's also important to use software engineering best practices like unit tests, especially since public data is not clean and predictable. For more details about the implementation, please refer to the FAQ below. If further clarification is necessary, email us at <cc@insightdataengineering.com>

Before submitting your solution you should summerize your approach, dependencies and run instructions (if any) in your `README`.  
You may write your solution in any mainstream programming language such as C, C++, C#, Clojure, Erlang, Go, Haskell, Java, Python, Ruby, or Scala. Once completed, submit a link to a Github repo with your source code.

In addition to the source code, the top-most directory of your repo must include the `log_input` and `log_output` directories, and a shell script named `run.sh` that compiles and runs the program(s) that implement the required feature.

If your solution requires additional libraries, environments, or dependencies, you must specify these in your `README` documentation. See the figure below for the required structure of the top-most directory in your repo, or simply clone this repo.

## Repo Directory Structure

The directory structure for your repo should look like this:

    ├── README.md 
    ├── run.sh
    ├── src
    │   └── process_log.py
    ├── log_input
    │   └── batch_log.json
    │   └── stream_log.json
    ├── log_output
    |   └── flagged_purchases.json
    ├── insight_testsuite
        └── run_tests.sh
        └── tests
            └── test_1
            |   ├── log_input
            |   │   └── batch_log.json
            |   │   └── stream_log.json
            |   |__ log_output
            |   │   └── flagged_purchases.json
            ├── your-own-test
                ├── log_input
                │   └── your-own-log.txt
                |__ log_output
                    └── flagged_purchases.json

<b>Please don't fork</b> our repo and don't use this `README` instead of your own.
The contents of `src` do not have to contain a single file called `process_log.py`, you are free to include one or more files and name them as you wish.

## Testing your Directory Structure and Output Format

To make sure that your code has the correct directory structure and the format of the output files are correct, we included a test script, called `run_tests.sh` in the `insight_testsuite` folder.

The tests are stored simply as text files under the `insight_testsuite/tests` folder. Each test should have a separate folder and within should have a `log_input` folder for `batch_log.json` and `stream_log.json` and a `log_output` folder for output corresponding to the current test.

You can run the test with the following from the `insight_testsuite` folder:

    insight_testsuite~$ ./run_tests.sh 

On a failed test, the output of `run_tests.sh` should look like:

    [FAIL]: test_1
    [Thu Mar 30 16:28:01 PDT 2017] 0 of 1 tests passed

On success:

    [PASS]: test_1
    [Thu Mar 30 16:25:57 PDT 2017] 1 of 1 tests passed



One test has been provided as a way to check your formatting and simulate how we will be running tests when you submit your solution. We urge you to write your own additional tests here as well as for your own programming language. `run_tests.sh` should alert you if the directory structure is incorrect.

Your submission must pass at least the provided test in order to pass the coding challenge.

## Instructions to Submit your Solution
* To submit your entry please use the link you receieved in your coding challenge invite email
* You will only be able to submit through the link one time 
* Do NOT attach a file - we will not admit solutions which are attached files 
* Use the submission box to enter the link to your github repo or bitbucket ONLY
* Link to the specific repo for this project, not your general repo
* Put any comments in the RADME File inside your Project repo, not in the submission box
* We are unable to accept coding challenges that are emailed to us 

# FAQ

Here are some common questions we've received. If you have additional questions, please email us at `cc@insightdataengineering.com` and we'll answer your questions as quickly as we can (during PST business hours), and update this FAQ.

### Which Github link should I submit?
You should submit the URL for the top-level root of your repository. For example, this repo would be submitted by copying the URL `https://github.com/InsightDataScience/fansite-analytics-challenge` into the appropriate field on the application. Do NOT try to submit your coding challenge using a pull request, which would make your source code publicly available.

### Do I need a private Github repo?
No, you may use a public repo, there is no need to purchase a private repo. You may also submit a link to a Bitbucket repo if you prefer.

### May I use R, Matlab, or other analytics programming languages to solve the challenge?
It's important that your implementation scales to handle large amounts of data. While many of our Fellows have experience with R and Matlab, applicants have found that these languages are unable to process data in a scalable fashion, so you should consider another language.

### May I use distributed technologies like Hadoop or Spark?
While you're welcome to do so, your code will be tested on a single machine so there may not be a significant benefit to using these technologies prior to the program. With that said, learning about distributed systems is a valuable skill for all data engineers.

### What sort of system should I use to run my program on (Windows, Linux, Mac)?
You may write your solution on any system, but your source code should be portable and work on all systems. Additionally, your run.sh must be able to run on either Unix or Linux, as that's the system that will be used for testing. Linux machines are the industry standard for most data engineering teams, so it is helpful to be familiar with this. If you're currently using Windows, we recommend using tools like Cygwin or Docker, or a free online IDE such as Cloud9.

### How fast should my program run?
While there are no strict performance guidelines to this coding challenge, we will take the amount of time your program takes into consideration in grading the challenge. Therefore, you should design and develop your program in the most optimal way. 

### Can I use pre-built packages, modules, or libraries?
This coding challenge can be completed without any "exotic" packages. While you may use publicly available packages, modules, or libraries, you must document any dependencies in your accompanying README file. When we review your submission, we will download these libraries and attempt to run your program. If you do use a package, you should always ensure that the module you're using works efficiently for the specific use-case in the challenge, since many libraries are not designed for large amounts of data.

### Can I use a database engine?
This coding challenge can be completed without the use of a database. However, if you must use one, it must be a publicly available one that can be easily installed with minimal configuration.

### Will you email me if my code doesn't run?
Unfortunately, we receive hundreds of submissions in a very short time and are unable to email individuals if code doesn't compile or run. This is why it's so important to document any dependencies you have, as described in the previous question. We will do everything we can to properly test your code, but this requires good documentation. More so, we have provided a test suite so you can confirm that your directory structure and format are correct.

### Do I need to use multi-threading?
No, your solution doesn't necessarily need to include multi-threading - there are many solutions that don't require multiple threads/cores or any distributed systems, but instead use efficient data structures.

### What should the format of the output be?
In order to be tested correctly, you must use the format described above. You can ensure that you have the correct format by using the testing suite we've included. If you are still unable to get the correct format from the debugging messages in the suite, please email us at `cc@insightdataengineering.com`.

### How should I handle ties in the data for feature 1-3? Should I list all the hosts/resources, or only 10? If only 10, how do I decide which 10?
In the event of ties for features, please only list 10 entries, using lexicographical order to sort them.

### Should I check if the files in the input directory are text files or non-text files(binary)?
No, for simplicity you may assume that all of the files in the input directory are text files, with the format as described above.

### Can I use an IDE like Eclipse or IntelliJ to write my program?
Yes, you can use what ever tools you want - as long as your run.sh script correctly runs the relevant target files and creates the `hosts.txt`, `hours.txt`, `resources.txt`, `blocked.txt` files in the `log_output` directory.

### What should be in the log_input directory?
You can put any text file you want in the directory since our testing suite will replace it. Indeed, using your own input files would be quite useful for testing. The file size limit on Github is 100 MB so you won't be able to include the provided input file in your log_input directory.

### How will the coding challenge be evaluated?
Generally, we will evaluate your coding challenge with a testing suite that provides a variety of inputs and checks the corresponding output. This suite will attempt to use your `run.sh` and is fairly tolerant to different runtime environments. Of course, there are many aspects (e.g. clean code, documentation) that cannot be tested by our suite, so each submission will also be reviewed manually by a data engineer.

### How long will it take for me to hear back from you about my submission?
We receive hundreds of submissions and try to evaluate them all in a timely manner. We try to get back to all applicants within two or three weeks of submission, but if you have a specific deadline that requires expedited review, you may email us at `cc@insightdataengineering.com`.
