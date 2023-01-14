---
title: Linux Bash Operators
date: 2023-01-14 20:58:00 -0500
categories: [Scripting, Walkthrough, Bash]
tags: [Bash, Walkthrough, Scripting, Linux, operators,]
---

## Operators in Shell Scripts

There are five basic operators in bash/shell scripting:

1. Arithmetic Operators
2. Relational Operators
3. Boolean Operators
4. Bitwise Operators
5. File Test Operators

## Arithmetic Operators

These operators are used to perform normal arithmetics/mathematical operations. There are seven arithmetic operators:

- Addition `+`: Binary operation used to add two operands.
- Subtraction `-`: Binary operation used to subtract two operands.
- Multiplication `*`: Binary operation used to multiply two operands.
- Division `/`: Binary operation used to divide two operands.
- Modulus `%`: Binary operation used to find remainder of two operands.
- Increment Operator `++`: Unary operator used to increase the value of operand by one.
- Decrement Operator `–`: Unary operator used to decrease the value of a operand by one

## Relational Operators

Relational operators are those operators which define the relation between two operands. They give either true or false depending upon the relation. There are six types:

- `==` Operator: Double equal to operator compares the two operands. It returns true if they are equal otherwise returns false.
- `!=` Operator: Not equal to operator returns true if the two operands are not equal otherwise it returns false.
- `<` Operator: Less than operator returns true if first operand is less than second operand otherwise returns false.
- `<=` Operator: Less than or equal to operator returns true if first operand is less than or equal to second operand otherwise returns false
- `>` Operator: Greater than operator returns true if the first operand is greater han the second operand otherwise returns false.
- `>=` Operator: Greater than or equal to operator returns true if first operand is greater than or equal to second operand otherwise returns false.

## Logical Operators

They are also known as boolean operators. These are used to perform logical operations. There are three types:

- Logical AND `&&`: This is a binary operator, which returns true if both the operands are true otherwise returns false.
- Logical OR `||`: This is a binary operator, which returns true if either of the operands is true or both the operands are true and returns false if none of them is false.
- Not Equal to `!`: This is a unary operator which returns true if the operand is false and returns false if the operand is true.

## Bitwise Operators
    
A bitwise operator is an operator used to perform bitwise operations on bit patterns. There are six types.

- Bitwise And `&`: Bitwise & operator performs binary AND operation bit by bit on the operands.
- Bitwise OR `|`: Bitwise | operator performs binary OR operation bit by bit on the operands.
- Bitwise XOR `^`: Bitwise ^ operator performs binary XOR operation bit by bit on the operands.
- Bitwise compliment `~`: Bitwise ~ operator performs binary NOT operation bit by bit on the operand.
- Left Shift `<<`: This operator shifts the bits of the left operand to left by number of times specified by right operand.
- Right Shift `>>`: This operator shifts the bits of the left operand to right by number of times specified by right operand.

## File Test Operator

These operators are used to test a particular property of a file.

- `-b` operator: This operator checks whether a file is a block special file or not. It returns true if the file is a block special file, otherwise returns false.
- `-c` operator: This operator checks whether a file is a character special file or not. It returns true if it is a character special file otherwise returns false.
- `-d` operator: This operator checks if the given directory exists or not. If it exists then operators returns true otherwise returns false.
- `-e` operator: This operator checks whether the given file exists or not. If it exists this operator returns true otherwise returns false.
- `-r` operator: This operator checks whether the given file has read access or not. If it has read access then it returns true otherwise returns false.
- `-w` operator: This operator checks whether the given file has write access or not. If it has write then it returns true otherwise returns false.
- `-x` operator: This operator checks whether the given file has execute access or not. If it has execute access then it returns true otherwise returns false.
- `-s` operator: This operator checks the size of the given file. If the size of given file is greater than 0 then it returns true otherwise it returns false.
