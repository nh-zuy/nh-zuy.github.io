---
layout: post
title: Simple Snake Game
date: 2020-02-15 14:49:45 +0700
categories: ['C/C++']
tags: ['C/C++']
---
## Simple SNAKE Game
---
   Long time no C ... guys ^^.

   The whole world is shaking with Corona. nCov is very scary, right? But I'm in long vacation because of it, so thanks so much, nCoV.
 
   Because of all my day is waste, I'm back with a new ... hmmm ... very very simple project.

   There are two common programming methods in practice today: procedural programming and object-oriented programming(OOP).

   Today, I'm going to show you how to write a familiar game, Snake eats fruit (with OOP).

>Requests: A brief understanding of classes, objects, ..., OOP in general.

1. Design:

- Our game's having 5 classes: Snake(main program), Fruit, Setting, Score, Menu.
 
2. Beginning:

  \* Notice: * In this program, I used some graphic function in header file "Graphic.h"
	     * This program I wrote in Window 7.
{% highlight ruby %}
#ifndef GRAPHIC_H
#define GRAPHIC_H
#include <iostream>
#include <conio.h>
#include <Windows.h>
using namespace std;
void resizeConsole(int width, int height)
{
	HWND console = GetConsoleWindow();
	RECT r;
	GetWindowRect(console, &r);
	MoveWindow(console, r.left, r.top, width, height, TRUE);
}

void textcolor(int x)
{
	HANDLE mau;
	mau=GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleTextAttribute(mau,x);
}

void gotoxy(int x,int y)
{
	HANDLE hConsoleOutput;
	COORD Cursor_an_Pos = {x-1,y-1};
	hConsoleOutput = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleCursorPosition(hConsoleOutput , Cursor_an_Pos);
}

void XoaManHinh()
{
	HANDLE hOut;
	COORD Position;
	hOut = GetStdHandle(STD_OUTPUT_HANDLE);
	Position.X = 0;
	Position.Y = 0;
	SetConsoleCursorPosition(hOut, Position);
}

// Ham to mau text tai toa do (x, y)
void ToMau(int x, int y, string a, int color) 
{
	gotoxy(x, y);
	textcolor(color);
	cout << \a;
	textcolor(7);
};
#endif
{% endhighlight %}

  * "Setting" class:

  - This class's managing data about Snake game's parameter such as : console's width, height, snake's speed, speed increase, level, the status of the game.

  - Accessor methods: GetWidth(), GetHeight(), GetLevel(), GetSpeedSnake().

  - Mutator methods: EndGame(), UpradeLevel(), DrawLevel().

  - Here is code:

{% highlight ruby %}
#ifndef SETTINGS_H
#define SETTINGS_H
class Setting
{
	private:
		int WidthBoard; // = 58
		int HeightBoard; // = 35
		int SpeedSnake; // THe defaule speed of the snake is 100 and can increase
		int IncreSpeed; //This's the speed of inscreasing speed snake
		bool GameStatus;
		int Level; // That's level of the game
	public:
		Setting()
		{
			WidthBoard = 58;
			HeightBoard = 35;
			SpeedSnake = 100;
			IncreSpeed = 10;
			Level = 1;
			GameStatus = true;
		};
		bool EndGame()
		{
			GameStatus = false;
			return GameStatus;
		};
		int GetWidth() const
		{
			return WidthBoard;
		}
		int GetHeight() const
		{
			return HeightBoard;
		};
		int GetSpeedSnake() const
		{
			return SpeedSnake;
		};
		int GetLevel() const
		{
			return Level;
		};
		void UpradeLevel()
		{
			Level++;
		};
		void DrawLevel()
		{
			gotoxy(63, 10);
			std::cout << "LEVEL";
			gotoxy(65, 11);
			std::cout << Level;
		};
		void UpradeSpeed()
		{
			SpeedSnake -= IncreSpeed * Level;
		};
};
#endif
{% endhighlight %}

  * ""Point" class:
   
{% highlight ruby %}
#ifndef POINT_H
#define POINT_H
#include <iostream>
#include "Graphic.h"
#include "Setting.h"
class Point
{
	private:
		int x;
		int y;
		Setting setting;
	public:
		//Set default position = 0, 0
		Point()
		{
			x = y = 10;
		};
		
		//Initialize the position
		Point(int x, int y)
		{
			this->x = x;
			this->y = y;
		};
		
		//Get x, y
		int GetX() const
		{
			return x;
		};
		int GetY() const
		{
			return y;
		};
		
		//Set the position
		void SetPoint(int x, int y)
		{
			this->x = x;
			this->y = y;
		};
		
		//Take the positon of the previous cell
		void TakePoint(Point* p)
		{
			p->x = x;
			p->y = y;
		};
		
		//Draw cell and Erase
		void Draw()
		{
			gotoxy(x, y);
			std::cout << char(254);
		}
		void Erase()
		{
			gotoxy(x, y);
			std::cout << " ";
		}
		
		//Functions for moving
		void MoveUp()
		{
			if (y == 2)
			{
				y = setting.GetHeight();
			};
			y--;
		};
		void MoveDown()
		{
			if (y == setting.GetHeight() - 1)
			{
				y = 1;
			}
			y++;
		};
		void MoveLeft()
		{
			if (x == 2)
			{
				x = setting.GetWidth();
			}
			x--;
		};
		void MoveRight()
		{
			if (x == setting.GetWidth() - 1)
			{
				x = 1;
			}
			x++;
		};
};
#endif
{% endhighlight %}

  * "Fruit" class:

{% highlight ruby %}
#ifndef FRUIT_H
#define FRUIT_H

class Fruit
{
	private:
		int x;
		int y;
	public:
		void SetNewFruit(int x, int y)
		{
			this->x = x;
			this->y = y;
		};
		int GetFruitX() const
		{
			return x;
		};
		int GetFruitY() const
		{
			return y;
		}
		void DrawFruit()
		{
			gotoxy(x, y);
			std::cout << "*";
		};
		void EraseFruit()
		{
			gotoxy(x, y);
			std::cout << " ";
		};
};
#endif
{% endhighlight %}

  * "Score" class:
{% highlight ruby %}
#ifndef SCORE_H
#define SCORE_H

class Score
{
	private:
		int x;
		int y;
		int value;
	public:
		Score()
		{
			value = 0;
			x = 65;
			y = 3;
		};
		void UpradeScore()
		{
			value++;
		};
		int GetScore() const
		{
			return value;
		}
		void DrawScore()
		{
			gotoxy(x - 2, y - 1);
			std::cout << "SCORE";
			gotoxy(x, y);
			std::cout << value;
		};
};

#endif
{% endhighlight %}

  * "Snake" class: Main class
   - Idea: We're saving the location of each Snake's cell into pointer array.

{% highlight ruby %}
#ifndef SNAKE_H
#define SNAKE_H
#include "Graphic.h"
#include "Point.h"
#include "Fruit.h"
#include "Score.h"
#include "Setting.h"
#include <conio.h>
#include <dos.h>
#include <ctime>

#define MAX_SNAKE_SIZE 69
enum Direction{UP, DOWN, LEFT, RIGHT, NONE};

class Snake
{
	private:
		Point* cell[MAX_SNAKE_SIZE];
		int size;
		Direction dir;
		Fruit fruit;
		Score score;
		Setting setting;
	public:
		Snake()
		{
			size = 1;
			cell[0] = new Point(20, 20);
			//No moving
			dir = NONE;
			//Set the body is NULL
			for (int i = 1; i < MAX_SNAKE_SIZE; i++)
			{
				cell[i] = NULL;
			};
			
			//Set the first fruit on the board
			srand((unsigned)time(NULL));
			fruit.SetNewFruit(rand()%(setting.GetWidth() - 2) + 2, rand()% (setting.GetHeight() - 3) + 3);
		};
		//Destructor
		~Snake()
		{
			delete[] cell;
		}
		//Add a cell at the end of the snake when he get a fruit
		void AddCell(int x, int y)
		{
			cell[size++] = new Point(x, y); //Set a new sell at the last of the snake
		}
		//Turn the snake when hit a keydown
		void TurnUp()
		{
			if (dir != DOWN) 
			{
				dir = UP;                               //    W
			}                                           // A     D
		};                                              //    s
		void TurnDown()
		{
			if (dir != UP)
			{
				dir = DOWN;
			}
		};
		void TurnLeft()
		{
			if (dir != RIGHT)
			{
				dir = LEFT;
			}
		};
		void TurnRight()
		{
			if (dir != LEFT)
			{
				dir = RIGHT;
			}
		};
		//Operate the snake 
		//   w
		//a     d
		//   s
		//Check direction -> check moving
		void OperateSnake()
		{
			if (kbhit())
			{
				char c = getch();
				switch(c)
				{
					case 'w': TurnUp();
								break;
					case 's': TurnDown();
								break;
					case 'a': TurnLeft();
								break;
					case 'd': TurnRight();
								break;
				};
				
			};
		};
		//In this function, we will show the snake moving on the board
		void Moving()
		{
			//Erasing the old body after moving
			//Idea: we will erase the all the cell and update the new positon 
			//      of each cell 
			for (int i = 0; i < size; i++)
			{
				cell[i]->Erase();
			};
			
			//The body following the head
			//The next cell will be equivalent to the previous cell
			for (int i = size - 1; i > 0; i--)
			{
				cell[i-1]->TakePoint(cell[i]); // cell i <- cell (i - 1);
			};
			
			//Check the direction of the snake
			//Idea: We just move the head and the body will be follow it
			switch(dir)
			{
				case UP: cell[0]->MoveUp();
							break;
				case DOWN: cell[0]->MoveDown();
							break;
				case LEFT: cell[0]->MoveLeft();
							break;
				case RIGHT: cell[0]->MoveRight();
							break;
			};
			
			//If a collision fruit and snake occurs
			//Add a new cell -> Update score -> Update level -> Update speed -> Update fruit
			if (fruit.GetFruitX() == cell[0]->GetX() && fruit.GetFruitY() == cell[0]->GetY())
			{
				AddCell(0,0); //Add a new cell
				score.UpradeScore(); //Update the score with the score increment 1
				//We will raise the difficult of the game if player get 10 score
				if (score.GetScore() % 10 == 0)
				{
					setting.UpradeLevel(); // Uprade the level
					setting.UpradeSpeed(); // and after, uprade the speed
				};
				
				fruit.EraseFruit(); //Erase the eaten fruit
				srand((unsigned)time(NULL));
				do
				{
					fruit.SetNewFruit(rand()%(setting.GetWidth() - 2) + 2,
									  rand()%(setting.GetHeight() - 3) + 3); 
									  //Set a new fruit.
									  //It will be not similar to the postion of the snake.
				} while ( fruit.GetFruitX() == cell[0]->GetX() && 
						  fruit.GetFruitY() == cell[0]->GetY() );
			};
			
			//Check if the head collide with the body
			//And the game over.
			//I'm disappointed with that ending sence because i don't know how to end the game better.
			for (int i = 1; i < size; i++)
			{
				if (cell[0]->GetX() == cell[i]->GetX() && cell[0]->GetY() == cell[i]->GetY())
				{
					system("cls");
					gotoxy(setting.GetWidth()/2, setting.GetHeight()/2);
					std::cout << "END";
					exit(0);
				};
			};
		
			//Now we print the snake, the fruit, the score and the level		
			for (int i = 0; i < size; i++)
			{
				cell[i]->Draw();
			};
			score.DrawScore();
			setting.DrawLevel();
			fruit.DrawFruit();
			
			Sleep(setting.GetSpeedSnake()); //Easilly realizing the difference
		}
};
#endif
{% endhighlight %}

  * "Menu" class: Make a menu to "make color" a bit :))))

{% highlight ruby %}
#ifndef MENU_H
#define MENU_H
#include "Graphic.h"
#include "Setting.h"

#define MAX_LIST 3

class Menu
{
	private:
		std::string List[MAX_LIST];
		int Pointer;
		Setting setting;
	public:
		Menu()
		{
			List[0] = "START";
			List[1] = "ABOUT";
			List[2] = "EXIT";
			Pointer = 0;
		}
		void DrawMenu()
		{
			for (int Option = 0; Option < MAX_LIST; Option++)
			{
				if (Pointer == Option)
				{
					ToMau(setting.GetWidth()/2, setting.GetHeight()/3 + Option * 4, List[Option], 28);
				}
				else
				{
					gotoxy(setting.GetWidth()/2, setting.GetHeight()/3 + Option * 4);
					std::cout << List[Option];
				};
			};
		};
		void ABOUT()
		{
			system("cls");
			gotoxy(setting.GetWidth()/2 - 18, setting.GetHeight()/2);
			std::cout << "PROJECT: SIMPLE SNAKE";
			gotoxy(setting.GetWidth()/2 - 18, setting.GetHeight()/2 - 3);
			std::cout << "GITHUB: https://github.com/nh-zuy";
		};
		void MoveUp()
		{
			if (Pointer == 0)
			{
				Pointer = MAX_LIST - 1;
			}
			else
			{
				Pointer--;
			};
		};
		void MoveDown()
		{
			if (Pointer == MAX_LIST - 1)
			{
				Pointer = 0;
			}
			else
			{
				Pointer++;
			}
		};
		void ControlMenu()
		{
			int flag = -1;
			while (true) //THE MAIN LOOP
			{
				system("cls");
				DrawMenu();
				while (true)
				{
					if (kbhit())
					{
						char c = getch();
						if (c == 'w')
						{
							MoveUp();
							break;
						}
						else if (c == 's')
						{
							MoveDown();
							break;
						}
						else if (c == 13)
						{
							if (Pointer == 0)
							{
								flag = 0;
								break;
							}
							else if (Pointer == 1)
							{
								ABOUT();
								getch();
								break;
							}
							else if (Pointer == 2)
							{
								flag = 2;
								break;
							};
						};
					};
				};
				
				if (flag == 0)
				{
					break;
				}
				else if (flag == 2)
				{
					system("cls");
					gotoxy(setting.GetWidth()/2, setting.GetHeight()/2);
					std::cout << "END";
					exit(0);
				};
			};
		};
};
#endif
{% endhighlight %}

  * Main program: 

   - Let's play game ^^

{% highlight ruby %}
#include "Snake.h"
#include "Board.h"
#include "Menu.h"

int main()
{
	resizeConsole(600, 500);
	Snake snake;
	Menu menu;
	
	menu.ControlMenu();
	system("cls");
	DrawTheBoard();
	while (true)
	{
		snake.OperateSnake();
		snake.Moving();
	};

	return 0;
}
{% endhighlight %}

#### Hmmm ... We've completed a quiet simple and familiar console game. If you want to check the source code, you can vist [MySimpleSnake](https://github.com/nh-zuy/MySimpleSnake)

## Hope you will enjoy, Goodbye ^^

