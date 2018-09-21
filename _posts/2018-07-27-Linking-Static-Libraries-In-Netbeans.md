---
layout: post
title: "Linking Static Libraries in Netbeans 8.2"
description: How to link a c++ project to a static library in netbeans 8.2.
date: 2018-07-27
comments: true
tags:
 - netbeans
 - c++
--- 
In this post I will detail how to link static libraries to your C++ projects in netbeans 8.2 (on Linux).  More specifically, linking static libraries that you yourself have written.  I spent the better part of my afternoon trying to figure this out, so I figure it deserves its own blog post for my (and others) future reference.

1. Create a static library and build it.
2. Locate path of static library header files.
3. Locate path of static library file (.a extension, usually in *project/directory/dist/Debug/GNU-Linux*).
4. In the project you wish to link to the static library, open up the project properties and do the following:
   a.)  Project Properties > Build > C++ Compiler > General > Include Directories : add the directory  from 2.
   b.)  Project Properties > Build > Linker > General > Additional Library Directories : add the directory from 3.
   c.)  Project Properties > Build > Linker > Libraries > Libraries : add the library in directory from 3.

Now your project should build and run without issue.