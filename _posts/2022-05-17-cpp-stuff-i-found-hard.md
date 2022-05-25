---
layout: post
title: C++ - Static Data Members & Static Functions
categories: [coding, c++, cpp]
---

Long time ago, I found a really nice example to understand it and I'm writing this post to document it. It took me a long time to understand what a static member/function is, why and when I should use it. 


## Creating a toy class

Let's create small code where we will emulate a service with users. We want to know at any time how many users we have, how can we do that?

#### Code for the `user` class:

```c++
class User{

	public:
		std::string name;
		User(){ // Constr

		};
		~User(){  //Destr

		};
};
```

#### Counting the users in the main function:


```c++
int main(){
	int user_counter = 2 ;
	User user1, user2;

	user2.~User();
	user_counter--;
}
```

This is not a good solution, I would have to count every time I create/delete a new user by hand. Wouldn't it be nice to make c++ count for us? This is why we want to use a single variable inside the class to count: This is a static variable.


## Creating the `user` class with a static counter

```c++
class User{

	static int user_counter;
	public:
		std::string name;

		User(){ // Constr
			user_counter++;
		};

		~User(){  //Destr
			user_counter--;
		};
};
```

Now, the `user_counter` value goes up/down when the constructor/destructor is called.

```c++
int main(){
	User user1, user2; //user_counter = 2 
	user2.~User(); //user_counter = 1
}
```
## How do we access this static counter?: Static Functions


"Normal" functions can't access static variables, and it makes sense since these variables are global to every instance of the class, i.e. the number of users can't be accessed easily as it's shared by everyone.

```c++
class User{

	static int user_counter;
	public:
		std::string name;

		static int get_user_count() {return user_counter;}

		User(){ // Constr
			user_counter++;
		};

		~User(){  //Destr
			user_counter--;
		};
};
```

Great, but  how do i initiate the `user_counter` variable? It's a shared variable, so we can't simply write `static int user_counter = 0` inside the user class, so need to initiate outside of it!

```c++
//Below the User class
int User::user_counter = 0; 

int main(){
	std::cout << User::get_user_count() << "\n";
	User user1, user2; //user_counter = 2

	std::cout << User::get_user_count() << "\n";
	user2.~User(); //user_counter = 1

	std::cout << User::get_user_count() << "\n";	
}
Output:
0
2
1
```

The full working code:

```c++
#include <iostream>

class User{

	static int user_counter;
	public:
		std::string name;

		static int get_user_count() {return user_counter;}

		User(){ // Constr
			user_counter++;
		};

		~User(){  //Destr
			user_counter--;
		};
};

int User::user_counter = 0; 

int main(){
	std::cout << User::get_user_count() << "\n";
	User user1, user2; //user_counter = 2

	std::cout << User::get_user_count() << "\n";
	user2.~User(); //user_counter = 1

	std::cout << User::get_user_count() << "\n";	
}
```

