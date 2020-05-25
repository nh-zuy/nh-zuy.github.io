---
layout: post
title: Exhaustive Algorithm
date: 2020-02-16 16:14:45 +0700
categories: ['Algorithm']
tags: ['C/C++']
---
## EXHAUSTIVE ALGORITHM
---

  - Hello everyone, "Data structure and algorithm" is an important subject in college. It includes a lots things about algorithm, ..., and exhaustive algorithm is one of all.

  - An exhaustive algorithm is one that finds a best combination of items by looking all the posible combination.

  - Recursion is helpful if you need to examine many posible combination and identify the best combination. 

  * Problem: How many different ways you can make change for 7,700 VND(by coins) using our system of coins?
	* Ex: 7,700 = 5,000 + 2,000 + 500 + 200 = 5,000 + 1,000 + 1,000 + 500 + 200 = ...
  * Your task: Find the number of posible combination and the best combination.

  * Procedure:
	1. Initialize **coinPos** mark the coin value we used.

	2. **coinsUsed[]** is a array stores the used coins.

	3. Make a call to recursive function with **amount** subtract to **coinsValue[coinPos]**.

	3. If **amount** equal to 0, store all the used coins in **coinsUsed[]** into **bestCoins[]**,
	   assign **numBestCoins** by **numCoinsUsed**.

	4. If **amount** is negative, return( reject this combination).

	5. Repeat untill **coinsLeft** equal to zero.

  * Here is code:

{% highlight ruby %}
#include <iostream>
#include <climits>

const int NO_SOLUTIONS = INT_MAX;
const int MAX_COINS_CHANGES = 100;
const int MAX_COINS_VALUES = 4;

int bestCoins[MAX_COINS_CHANGE]; // Array stores the consequence of the best coins combination

int coinsValue[MAX_COINS_VALUES] = {5000, 2000, 1000, 500};

int numBestCoins = NO_SOLUTIONS,
    numSolutions = 0,
    numCoins;

void findBestOfCoinsCombination(int coinsLeft, int amount, int coinsUsed, int numCoinsUsed)
{
	int coinPos;

	if (coinsLeft == 0)
	{
		return;
	}
	else if (amount smaller than zero)
	{
		return;
	}
	else if (amount == 0)
	{
		if (numCoinsUsed < numBestCoins)
		{
			for (int  i = 0; i < numCoinsUsed; i++)
			{
				bestCoins[i] = coinsUsed[i];
			};
		
			numBestCoins = numCoinsUsed;
		};
	
		numSolutions++;
		return;
	};

	coinPos = numCoins - coinsLeft;
	coinsUsed[numCoinsUsed] = coinsValue[coinPos];
	numCoinsUsed++;

	findBestOfCoinsCombination(coinsLeft, amount - coinsValue[coinPos], coinsUsed, numCoinsUsed);

	numCoinsUsed--;

	findBestOfCoinsCombination(coinsLeft - 1, amount, coinsUsed, numCoinsUsed);
};
{% endhighlight %}

#### Cre: Starting out with C++(Eight edition) - Tony Gaddis
