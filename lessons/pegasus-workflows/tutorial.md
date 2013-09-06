---
layout: lesson
root: ../..
github_username: vahi
bootcamp_slug:  2013-09-16-ISI
title: Executing Workflows using Pegasus WMS
---
**Based on material by Karan Vahi and Gideon Juve**

## Introduction
Use a browser to open the tutorial on github, located at:
    http://github.com/{{page.github_username}}/{{page.bootcamp_slug}}

This tutorial will take you through the steps of creating and running
a simple workflow using Pegasus. This tutorial is intended for new
users who want to get a quick overview of Pegasus concepts and
usage. The tutorial covers the creating, planning, submitting,
monitoring, debugging, and generating statistics for a simple
diamond-shaped workflow. More information about the topics covered in
this tutorial can be found in later chapters of this user's guide. 

All of the steps in this tutorial are performed on the
command-line. The convention we will use for command-line input and
output is to put things that you should type in bold, monospace font,
and to put the output you should get in a normal weight, monospace
font, like this:

```
    [user@host dir]$ you type this
    you get this
```

Where *[user@host dir]$* is the terminal prompt, the text you should
type is “you type this”, and the output you should get is "you get
this". The terminal prompt will be abbreviated as $. Because some of
the outputs are long, we don’t always include everything. Where the
output is truncated we will add an ellipsis '...' to indicate the
omitted output. 

**If you are having trouble with this tutorial, or anything else related
to Pegasus, you can contact the Pegasus Users mailing list at
<pegasus-users@isi.edu> to get help.** 

## Getting started

In order to reduce the amount of work required to get started we have
provided several virtual machines that contain all of the software
required for this tutorial. Virtual machine images are provided for
VirtualBox. Information about deploying the
tutorial VM on these platforms is in the appendix. Please go to the
appendix for the platform you are using and follow the instructions
for starting the VM found there before continuing with this tutorial. 

## Generating the workflow

We will be creating and running a simple diamond-shaped workflow that
looks like this: 

![Diamond Workflow](./images/concepts-diamond.jpg )

The *shell* is a program that presents a command line interface
which allows you to control your computer using commands entered
with a keyboard instead of controlling graphical user interfaces
(GUIs) with a mouse/keyboard combination.

In the above diagram, the ovals represent computational jobs, the
dog-eared squares are files, and the arrows are dependencies. 

Pegasus reads workflow descriptions from DAX files. The term “DAX” is
short for “Directed Acyclic Graph in XML”. DAX is an XML file format
that has syntax for expressing jobs, arguments, files, and
dependencies. 

In order to create a DAX it is necessary to write code for a DAX
generator. Pegasus comes with Perl, Java, and Python libraries for
writing DAX generators. In this tutorial we will show how to use the
Python library. 

The DAX generator for the diamond workflow is in the file
generate_dax.py. Look at the file by typing: 

```
more generate_dax.py
...
```

