---
title: Bite Size Bash and things to keep in mind
author: Zeroc00I
categories: bash
tags: ["bash","sh","shellscript"]
math: true
---

 For those that have not already purchased Bite Size Bash book from Julia, I would like to suggest you do it in order to improve your knowledge in Unix Shells =)

 ![Bite Size Bash Book](/assets/img/bitesizebash.jpg)

 Go to her website:
 [https://wizardzines.com/](https://wizardzines.com/)
 
 Or directly from book sale on: 
 [https://gumroad.com/l/bite-size-linux](https://gumroad.com/l/bite-size-linux)

#  Command Substitution vs Pipeline
 (If you don't understand the concepts between both, check out the examples bellow)

### Pipeline seems like

	ls | head

### Otherwise Command Substitution

	head <(ls)

 The benefits of using command substitution instead pipeline are that you have more control of the output, just like named pipelines using MKFIFO.

	wget -O - http://example.com/dvd.iso | tee >(sha1sum > dvd.sha1) >(md5sum > dvd.md5) > dvd.iso

# Parameter expansion ${} - There are some very usefull tricks

### Similar to sed functionality, we can do the following replacing patterns to another string

 Declaring out variable

	soOutput=`cat /etc/lsb-release`

 Will return

	DISTRIB_ID=Ubuntu DISTRIB_RELEASE=19.10 DISTRIB_CODENAME=eoan DISTRIB_DESCRIPTION="Ubuntu 19.10"

 Now, we can replace some patters from our output with another string, for example, Kali

	echo ${soOutput//Ubuntu/Kali}
	DISTRIB_ID=Kali DISTRIB_RELEASE=19.10 DISTRIB_CODENAME=eoan DISTRIB_DESCRIPTION="Kali 19.10"


### Similar to wc -c

	echo $soOutput | wc -c
	96
 
 is similar to

	echo ${#soOutput}
	96

### Checking unset/null variable

	idunno="echo hi"

	${thisvariabledoesntexist:-$idunno}

 Output: 

	hi

### Taking better control of exceptions

	${nothingwasset:?this variable doesnt exist}

 Output:

	bash: nothingwasset: this variable doesnt exist

# Trap - Calling a command when some intended event ocurr

### Printing currently day when Ctrl + C is pressed

	trap '$(date +%d)' EXIT

# Set -euo and erros in bash

### Stops the script on erros

	set -e

### Stops on unset variables

	set -u

### Stops the script when any command fail

	set -o pipefail

### Bonus: Debugging every line with all variables expanded

	set -x or bash -x script.sh
