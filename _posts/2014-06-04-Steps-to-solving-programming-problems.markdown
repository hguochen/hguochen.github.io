---
title: "Steps to solving programming problems"
layout: post
date: 2014-06-04 16:07:39
comments: true
categories: [programming, web development]
---

If you are looking for a systematic approach to solving programming problems, i have one for you here. This is my variation from the [Problem analysis coding execution cycle](http://www.coursehero.com/file/7263502/Programing-with-the-problem-Analysis-Coding-Execution-Cycle/) that typical computer science class will teach. Hope it will be of use to you!

## Problem analysis coding execution cycle

### 1. Analyse the problem

##### a. Problem specification
	
*What is the problem asking for?*

##### b. Requirements

*What are the constraints and requirements?*

##### c. Assumptions

*What are the underlying assumptions on the question?*

*Is there any assumptions which could be challenged?*

##### d. Sample test cases

*Identify at least 1 sample test case.*

*How many test cases should there be minimally?*

##### e. Determine input & output

*Determine the input and output and their corresponding data types*

##### f. Identify input boundary cases

*List out all boundary cases for inputs and logic flows.*

### 2. Implement the algorithm

##### a. Algorithm structure

*What is the best data structure and data types for this problem?*

##### b. Design Algorithm

*Write algorithm in plain english or pseudo code.*

*Identify repeated patterns if any.*

##### c. Testing algorithm correctness

*Verbally run through the algrithm with pre-defined test cases*

##### d. Write code

*Write code incrementally with adequete tests at each step.*

##### e. Test code correctness

*Run the code with sample inputs to test for desired outputs.*

##### f. Testing boundary cases

*Run the code with boundary cases and lookout for unexpected behaviors.*

##### d. Verify program correctness

*Re-visit the question and and lookout for inconsistencies  and redundancies.*

### 3. Program maintenance

##### a. Determine time complexity

*What is the average case complexity?*

*What is the worst case complexity?*

##### b. Determine space complexity

*Are there redundant or unused variables?*

##### c. Refactoring

*Can the code be refactored to improve readability?*

*Can the algorithm be redesigned to improve efficiency?*

##### d. Documentation

*Leave comments where the abstraction logic is unclear.*

*Are there any trivial comments?*

*Write formal documentations if the program is expected to be used by people.*