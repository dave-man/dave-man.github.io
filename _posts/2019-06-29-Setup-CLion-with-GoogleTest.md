---
layout: home
title: "Setup CLion for GoogleTest"
date: 2019-06-29
categories:
  - Blog
  - Programming
tags:
  - GoogleTest
  - CLion
  - CMake
---


This will be a brief introduction to explain how to download, install 
and setup (meaning creating a small project to see if everything works "as intended")
GoogleTest with CLion.

## Before you start

What do you need?
<ul>
<li> Git </li>
<li> A C++ compiler </li>
<li> Make and CMake </li>
<li> CLion IDE from Jebtrains or whatever IDE or text editor you're used to: since I love Jebtrains products, I try to use them as much as possible ;)</li>
<li> A bit of time and patience! </li>
</ul>

I won't go too much in detail regarding the setup of what you need.. 
a brief warning though: I did all on a Mac, I installed Git and CMake using homebrew and CLion usind the Jetbrains toolbox. I also had to install Xcode Command Line Tools
(I used AppleClang on OsX).
If you're on a different platform, you should have those tools installed too: if you're on Linux you can get Git, CMake, make and the C++ compiler using your distro
package manager. If you're on Windows you have a couple of different choices: either use a package manager like [Chocolatey](https://chocolatey.org/) or [Scoop](https://scoop.sh/) 
or install [Visual Studio Community Edition](https://visualstudio.microsoft.com/vs/community/) (the VS installer will allow to install Git too if you don't want to download it separately). 
On Windoes you can also choose to install [MingW-w64](https://mingw-w64.org/doku.php) if you're so inclined
or try out [WLS](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
Of course, depending on your OS, the installation process described below will change and you'll have to make adjustements, in particular 
regarding to the paths for the final test project.

---

Now, let's start, step by step!

## Download the GoogleTest
GoogleTest is a C++ testing and mocking framework made by Google (doh!).
It's completely open source and it's hosted on GitHub right [here](https://github.com/google/googletest). 
We'll get the source directly from the GitHub repository using Git.
Create a new folder where you want to store the source code, cd into it and:

```git
git clone https://github.com/google/googletest
```

![Cloning the GoogleTest repo](/assets/images/posts/2019-06/01_GIT_CLONE.png)

## Generate the build files
Next, cd inside the newly cloned project and create a new folder ("install" or whatever you prefer).
Cd inside this new folder and:

```bash
cmake ../
```

![Generating the buildsystem: CMAKE](/assets/images/posts/2019-06/02_CMAKE.png)

## Build and install all the stuff 
In the same directory you're currently in, issue the command:
```bash
make
```

![Make all the stuff!](/assets/images/posts/2019-06/03_MAKE.png)

Now you'll likely have to use sudo/su if you're on a Unix-based system and provide your password when asked:

```bash
sudo make install
```

![Build and install](/assets/images/posts/2019-06/04_MAKE_INSTALL.png)
(The console output is actually a bit longer than the above picture shows..)
---

Assuming everything worked as intended ;)
you should now have all the necessary headers placed in /usr/local/include
and the related libraries in /usr/local/lib
You're ready to test wether everything works corretly!

## Let's try out

Pop open CLion and create a new project ("C++ Executable", CLion will set up for you
a CMake project by default).

In the default "main.cpp" file, paste this:

```cpp
#include "gmock/gmock.h"
//#include "gtest/gtest.h"
#include <iostream>

int main(int argc, char** argv)
{
    std::cout << "Ready to Test?" << std::endl;
    testing::InitGoogleMock(&argc, argv);
    return RUN_ALL_TESTS();
}
```

while in the "CMakeLists.txt" replace the last two lines (the ones after "project(...)" with:

```cmake
set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-std=c++11 -pthread")
include_directories(/usr/local/include)
link_directories(/usr/local/lib)

#Define your executable
add_executable(${PROJECT_NAME} main.cpp)

#Link with GoogleTest
target_link_libraries(${PROJECT_NAME} libgtest.a libgtest_main.a)
#Link with GoogleMock
target_link_libraries(${PROJECT_NAME} libgmock.a libgmock_main.a)
```

If you followed the instructions so far, everything is exactly where it's supposed 
to be, so hitting the run button will show in the console:

![Everything's working](/assets/images/posts/2019-06/05_TEST.png)

---

Congrats, you're done!

Time to do some more serious testing.. 
Although it's not really 'recent', if you enjoy reading or learning from a book, 
you should give a try to 
["Modern C++ Programming with Test-Driven Development by Jeff Langr"](https://pragprog.com/book/lotdd/modern-c-programming-with-test-driven-development) 
 from [The Pragmatice Bookshelf](https://pragprog.com/)! 