---
layout: post
title: Calculate An Arithmetic Expression
date: 2020-02-17 15:27:56 +0700
categories: ['technique']
tags: ['C/C++']
---
## CALCULATE AN ARITHMETIC EXPRESS
---

  - Hello guys, have you ever heard of "Stack" and "Queue" before? They're basic data structure. Like an array and a linked list, Stack and Queue is a basic data structure that holds a sequence of elements. 

  - However, they're still different from array, linked list and each of them about mechanism of action:

	* Stack: stores and retrives items in a last-in-first-out manner(LIFO). That means: the last item
inserted in stack is the first one retrive. Simple applications: convert to another counting system, check a sequence of parentheses, ...

	* Queue: stores anf retrives items in a first-in-first-out manner(FIFO). That means: abcxyz, ... oh I'm lazy ...

  - "Stack" and "Queue" are helpful data structure, support us handling data.

  - Recently, I've found a interesting application of "Stack" and "Queue", calculate a arithmetic express using "Balan" algorithm.

  - About "Balan" algorithm, I won't concern with it, because you can search GG, key word is "Reverse Polish notation", I will just give you how to find result through source code.

  1. Procedure:
	
	* Initialize a stack stores operands, a queue stores digits.

	* Enter the express through a string chain.

	* Parsing the chain into a digit, a operand, a parentheses. 

	* Initialize the second stack calculates express.

  2. Source code:

{% highlight ruby %}

bool isDigit(char character) // Check if the character is a digit ([0..9])
{
	if (character >= '0' && character <= '9')
	{
		return true;
	};

	return false;
};

std::string getItem(std::string& str) // Get the digits, operands or parentheses
{
	std::string item = "";

	if (str[0] == '(' || str[0] == ')' || str[0] == '+' || // If the first character is a operand or
            str[0] == '-' || str[0] == '*' || str[0] == '/'))  // a parentheses
	{
		item += str[0]; 
		str.erase(0, 1); // Erase this position
	}
	else if (str[0] != '\0') // Else if the first character is a digit
	{
		item += str[0];
		str.erase(0, 1);
		while (!str.empty() && isDigit(str[0]) || str[0] == '.') // Check digit or a dot
		{
			item += str[0];
			str.erase(0, 1);
		};
	};

	return item;
};

int Classify(std::string item) // Check the type of the item(digit, operand, parentheses)
{
	if (item == "+" || item == "-" || item == "*" || item == "/")
	{
		return 1;
	}
	else if (item == "(" || item == ")")
	{
		return -1;
	}
	else 
	{
		return 0;
	};
};

int Priority(std::string item)
{
	if (item == "*" || item == "/")
	{
		return 1;
	};

	return 0;
};

void ParsingChain(std::string str, std::stackst, std::queue q)
{
	std::string item;

	while (!str.empty())
	{
		item = getItem(str);
	
		if (Classify(item) == 0)
		{
			q.push(item);
		}
		else if (Classify(item) == 1)
		{
			if (Priority(item) < Priority(st.top())
			{
				q.push(st.top());
				st.pop();
				while (!st.empty() && Priority(item) < Priority(st.top()))
				{
					q.push(st.top());
					st.pop();
				};

			st.push(item);
		}
		else if (item == ")")
		{
			while (!st.empty() && st.top != "(")
			{
				q.push(st.top());
				st.pop();
			};
			
			st.pop();
		};
		else
		{
			st.push(item);
		};
	};

	while (!st.empty())
	{
		q.push(st.top());
		st.pop()
	};
};

float Calculate(std::string operand, float a, float b)
{
	if (operand == "+")
	{
		return a+b;
	}
	else if (operand == "-")
	{
		return a-b;
	}
	else if (operand == "*")
	{
		return a*b;
	}
	else if (operand == "/")
	{
		return a/b;
	};
};

float Calculation(std::queue q)
{
	std::stack st;

	while (!q.empty())
	{
		std::string item = q.front();
		q.pop();

		if (classify(item) == 0)
		{
			st.push(item);
		}
		else if (classify(item) == 1)
		{
			if (st.size() < 2)
			{
				return -1;
			};

			float value1 = stof(q.front());
			q.pop();
			float value2 = stof(q.front());
			q.pop();
			float result = Calculate(item, value1, value2);
			q.push(result);
		};
	};

	return result;
};
{% endhighlight %}

  * Finally, let's check it out !

{% highlight ruby %}
int main()
{
	std::string expression;
	std::stack st;
	std::queue q;

	std::cout << "Enter the expression: ";
	std::cin >> expression;

	std::cout << "Result: " "<<" Calculation(q);

	return 0;
};
{% endhighlight %}

#### HEHE, GOOGBYE !
