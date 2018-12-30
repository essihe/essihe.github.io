---
layout: default
---

The command-line course is an introductory course to command-line environments. During the 
course each week a new theme has been presented with relevant exercises. Main purposes were 
to get used to UNIX enviroment, install programs, write scripts and process corpora.

Week 1: Introduction to Command-Line Environments

We were introduced to text editor (nano) and to basic commands such as cat, mv and echo. Also 
the difference between text files and binary files was made clear. I started to make notes of 
all the commands that had appeared and also made a short explanation of what each command does. 
I found this helpful during the course.  
Example commands:
```
touch text.txt
echo 123 > text.txt
```

Week 2: Navigating a UNIX system

This week continued with new commands and especially commands concerning files. In addition, 
we learned about changing into root user and working with sudo su and su commands. Personally 
for me interesting was learning about parent processes since that was a whole new concept that I 
wasn't familiar with. It was very helpful to realize that all processes, except for init, form 
kind of a tree structure which enables tracing the processes.
Example of command setting file permissions:
```
chmod 777 /path/to/file
```

Week 3: Corpus Processing

On week 3 we were working with files and to be more specific, with the Life of Bee textfile. 
This week was about learning to use grep and nano better. I found the technical videos confusing 
and not very clear so I decided to use my own regex program for the quiz. My program counts word 
frequencies and highlights the matches of the words I want use based on given Regex pattern. I 
think the instructions to making .freq and .sent files weren't made clear and specific enough. 
Example code of the regex program I used:
```
import re
import pprint

def regex_search(string, pattern):
	# Luodaan frekvenssi dictionary
	osumat = re.findall(pattern, string)
	frequency = {}
	for osuma in osumat:
		frequency[osuma] = frequency.setdefault(osuma, 0) + 1

	# frequency = {
	# 	'big':12,
	# 	'great':23,
	# 	' big':4,
	# 	..... jne
	# }

	lines = string.splitlines()
	# lines = re.split(r'(?<=\.) ', string)

	return frequency, lines	

def search_cli(string):
	pattern = input('Syötä RegEx kaava: \n')
	if not pattern:
		quit()

	frequency, lines = regex_search(string, pattern)
	hits = 0
	re_pattern = re.compile(pattern)
	for line in lines:
		if re_pattern.search(line):
			for hit in re_pattern.findall(line):
				if type(hit) == tuple:
					for group in hit:
						hits += 1
						line = line.replace(group, '\033[95m' + group 
+ '\033[0m')
				else:
					hits += 1
					line = line.replace(hit, '\033[95m' + hit + '\033[0m')
			print("{}:  {}".format(hits, line))
	pp.pprint(sorted(frequency.items(), key=lambda t: t[1]))
	print("Summa: {}".format(sum(frequency.values())))
	print("Highlightatut matchit: {}".format(hits))
	print("Lauseitten kokonaismäärä: {}".format(len(lines)))
	print("Haettiin kaavalla '{}'".format(pattern))


if __name__ == "__main__":
	import readline
	import sys
	import os
	import atexit

	pp = pprint.PrettyPrinter(indent=4)

	# Tallennetaan haku-pattern historia.
	historyPath = os.path.expanduser('~/.haku_history')

	if os.path.exists(historyPath):
		readline.read_history_file(historyPath)

	def save_history():
		readline.write_history_file(historyPath)

	atexit.register(save_history)


	if len(sys.argv) != 2:
		print("Anna luettavan tiedoston nimi tai polku kansioon jonka sisältä 
olevista tiedostoista haluat etsiä esim: python3 haku.py dictionary.txt")
		quit()
	elif os.path.isfile(sys.argv[1]):
		# Jos annettu parametri on tiedosto:
		with open(sys.argv[1]) as f:
			search_cli(f.read())
	elif os.path.isdir(sys.argv[1]):
		# Jos annettu parametri on kansio, luetaan kaikki tiedostot ja yhdistetään 
ne yhdeksi merkkijonoksi
		# ennen haku funktioon syöttämistä
		texts = []
		for root, dirs, files in os.walk(sys.argv[1]):
			for file in files:
				with open(os.path.join(root, file)) as f:
					texts.append(f.read())
		search_cli(r'\n'.join(texts))
	else:
		print("Polkua '{}' ei löytynyt. Siirrä luettava tiedosto 
kotikansioon.".format(sys.argv[1]))
		quit()
```

Week 4: Scripting and UNIX Configuration Files

This week we were supposed to learn about scripts and learn to make our own simple scripts. It 
was quite challenging because in the technical videos it wasn't always clear that I should have 
created some file or something and then later on suddenly the instructor had some files that I
hadn't even clue that I should have too or used commands that wouldn't work for me because it 
hadn't been said that I should have done something to make them work. 
Example of my script: 
```
echo "You are: $LOGNAME"
echo "Today is:" $(date)
echo "User is:" $(whoami)
echo "Current directory: $PWD"
``` 

Week 5: Installing and Running Programs

This week was about learning to install for example Python modules using pip. Also we learned 
about Makefiles and were supposed to learn about parsing. However, the bllipparser module did 
not work on my Macbook for some reason and I googled a while for an answer and a suggestion for 
this problem was to create aliases for the gcc. That seemed a bit too challenging for me so I 
got to use another computer to go through the exercises.
Example of installed module:
```
pip3 install guess_language-spirit
```


Week 6: Version Control

This week's main themes were git repositories and git commands. This was nice to have just 
before the final assignment which is about creating a Git page. I think creating the branches 
and going throught all the basic stuff was very useful in order to get straight away started 
with the course final work. 
Example of git commands:
```
git add .
git commit -m "Initial commmit"
git push
```

Week 7: Final Assignment

To pass the course students are supposed to build their own websites and publish them on Github 
pages. I started to work with creating a template repository and installing Jekyll. Jekyll tried 
to cause some problems but when I slept through the night it magically started working in the 
morning. 
