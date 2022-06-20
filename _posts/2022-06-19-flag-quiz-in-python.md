---
layout: post
title: Flags of the World Quiz in Python
subtitle: Quiz your flag knowledge in your terminal
author: Luke
categories: projects
banner:
  video: https://stream.mux.com/fg8NLVP2j6ao4CsN013Lc5p01LgglByEZW/high.mp4?token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6InQ5UHZucm9ZY0hQNjhYSmlRQnRHTEVVSkVSSXJ0UXhKIn0.eyJleHAiOjE2NTU3NjI4MDUsImF1ZCI6InYiLCJzdWIiOiJmZzhOTFZQMmo2YW80Q3NOMDEzTGM1cDAxTGdnbEJ5RVpXIn0.cRAGROu7oTnN42pv1GOMHkiznl64aDAQ9TrhKO-iykcgaHJ6KrdCOTuJvYnOV1I0Z3UIZthYhfL-TQSsbHxgRqSNLHOrRK2tbSjmloM787TWUJmwijkNH9YoUzLExsKw_zs5UqBi9QeAoSePBpSVc50YYiF_xb3kIA-KmmoBl6KnD8MoIMjczIOE3gsKz4X2V1rq-RkRfFJR0yMBqwPLnYjK6l9bV2IaizYk4_IpJ0ymUi3cYUxanzcw_R9aOQmV5rhlbkqwP29iVKyUjxswTyqLyyuWDYb9RkDHuBU7cQxl5UCmU-OjEAyuVuZn1Nm0p2rDr2IXf9s2e9gt3659iw
  #https://stream.mux.com/dUWi6ycKHlN58ue01b4P025rr6fpfBUVNS/high.mp4?token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6InQ5UHZucm9ZY0hQNjhYSmlRQnRHTEVVSkVSSXJ0UXhKIn0.eyJleHAiOjE2NTU3NjI0NTMsImF1ZCI6InYiLCJzdWIiOiJkVVdpNnljS0hsTjU4dWUwMWI0UDAyNXJyNmZwZkJVVk5TIn0.DrbtKu57DxB8k8A5bntV2IPUfguAyieIsNGh7oUJxEz9-4IWnkrghjHI3xLcF2SFJJkWcdddNWtcV-qfbDKdQtD4LC1061telKEdALk-LQC4U7XtEyLnEJ_3IFiFSYWKTfai1Zs0UmSO8Ojz0aOIe3lXkVzzGEtwuX9q8qjoOvmP6d4glVjI-LFQ7TacoL_2eUwnmq6v7pKQfP1pi9c38KSCMsR1I7gNQ0yh0JHNKPJnFZVH_SgYj9k23P7I3AWWujb8G14Di0rWrR8k4pP6C7Io79za3pQ8l4Dj3THqZcrcYD60QFyGC1iRsn_hV8TbMaeGdxaYykR26eephZumEw
  loop: true
  volume: 0
  start_at: 8.5
  image: "" #https://bit.ly/3xTmdUP
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: white"
tags: projects python jupyter flagquiz
sidebar: []
---

This simple and fun game can be played from your own terminal, all with some simple python scripts.

The game is both rudimentary and simplistic, yet matches the design and style of this online flag quiz ([here][original-quiz] ↗). Feel free to modiify and change the game as you see fit!

This game runs perfectly just as a python file, but even better when separated and ran in a Jupyter notebook. This quiz only requires the additional `png1000px` and `countries.json` files to be stored in the same directory as your `FlagQuiz.py` or `FlagQuizJupyter.ipynb` file, unless you want to reference the aforementioned files another way.

The `png1000px` and `countries.json` files can be downloaded from hampusborgos's GitHub page ([here][countries-source] ↗) 

> Each of these files can also be downloaded from my GitHub account ([here][github-flagquiz] ↗)

## Flags of the World Quiz in Python

Required modules: `random` `json` `PIL` `glob` `matplotlib`

#### Preliminary importing and organization of our figure size

Let's also import all country names, establish a class of colors, some score keeping metrics, and create a clean list of each country flag

```python
import random 
import json
from PIL import Image
import glob
import matplotlib.pyplot as plt
from matplotlib.pyplot import figure
import matplotlib.image as mpimg
figure(figsize=(1,1), dpi=300)
countriesDICT = json.load(open('countries.json'))
class color:
   PURPLE = '\033[95m'
   CYAN = '\033[96m'
   DARKCYAN = '\033[36m'
   BLUE = '\033[94m'
   GREEN = '\033[92m'
   YELLOW = '\033[93m'
   RED = '\033[91m'
   BOLD = '\033[1m'
   UNDERLINE = '\033[4m'
   END = '\033[0m'
score = 0
count = 0 
image_list = [] 
for image in glob.glob('png1000px/*.png'):
    i = Image.open(image)
    image_list.append(i) 
print(color.BOLD + color.DARKCYAN + '\nFLAG QUIZ\n' + color.END)
```

#### Define the Flag Quiz itself

We will loop over this many times as the user plays the game
```python
def flagQuiz():
    global score, count
    uniqueCountries = random.sample(list(countriesDICT.items()),4)
    randList = random.sample([0,1,2,3], 4)
    print('1.',uniqueCountries[randList[0]][1],'\n'
            '2.',uniqueCountries[randList[1]][1],'\n'
            '3.',uniqueCountries[randList[2]][1],'\n'
            '4.',uniqueCountries[randList[3]][1],'\n')
    optionsDict = {1:uniqueCountries[randList[0]][1],2:uniqueCountries[randList[1]][1],3:uniqueCountries[randList[2]][1],4:uniqueCountries[randList[3]][1]}
    imgplot = plt.imshow(mpimg.imread('png1000px/'+str(uniqueCountries[0][0].lower())+'.png'))    
    plt.axis('off')
    plt.show()
    while True:
        try:
            playerGuess = input('Guess (enter "n" to exit): ')
            if playerGuess == 'n':
                print('\n' + color.BOLD + color.BLUE + 'Final Score: ' + str(score) + '/' + str(count) + color.END)
                exit()
            elif int(playerGuess) < 0:
                raise ValueError
            elif int(playerGuess) > 4:
                raise ValueError
            count += 1
            break
        except ValueError:
            print(color.YELLOW + 'Guess must be between 1 & 4' + color.END)
    if optionsDict[int(playerGuess)] == uniqueCountries[0][1]:
        score += 1     
        print(color.GREEN + color.BOLD + '\n' + uniqueCountries[0][1] + color.END, '\n' + 
        color.BLUE + 'Score: ' + str(score) + '/' + str(count) + color.END)
    else:
        print(color.RED + color.BOLD + '\n' + optionsDict[int(playerGuess)] + color.END, '\n' +
        color.GREEN + color.BOLD + uniqueCountries[0][1] + color.END, '\n' + 
        color.BLUE + 'Score: ' + str(score) + '/' + str(count) + color.END)
    flagQuiz()        
flagQuiz()
```

## Flags of the World Quiz in Python (Jupyter Notebook)

Required modules: `random` `json` `PIL` `glob` `matplotlib`
Additionally required modules for Jupyter version: `IPython` `time`

#### Preliminary importing and organization of our figure size

Let's also import all country names, establish a class of colors, some score keeping metrics, and create a clean list of each country flag

```python
import random 
import json
from PIL import Image
import glob
import matplotlib.pyplot as plt
from matplotlib.pyplot import figure
import matplotlib.image as mpimg
from IPython.display import clear_output
import time
figure(figsize=(1,1), dpi=300)
countriesDICT = json.load(open('countries.json'))
class color:
   PURPLE = '\033[95m'
   CYAN = '\033[96m'
   DARKCYAN = '\033[36m'
   BLUE = '\033[94m'
   GREEN = '\033[92m'
   YELLOW = '\033[93m'
   RED = '\033[91m'
   BOLD = '\033[1m'
   UNDERLINE = '\033[4m'
   END = '\033[0m'
score = 0
count = 0 
image_list = [] 
for image in glob.glob('png1000px/*.png'):
    i = Image.open(image)
    image_list.append(i) 
print(color.BOLD + color.DARKCYAN + '\nFLAG QUIZ\n' + color.END)
```

#### Define the Flag Quiz itself

We will loop over this many times as the user plays the game

```python
def flagQuiz():
    clear_output(wait=True)
    global score, count
    print(color.BLUE + 'Score: ' + str(score) + '/' + str(count) + color.END)
    uniqueCountries = random.sample(list(countriesDICT.items()),4)
    randList = random.sample([0,1,2,3], 4)
    print('1.',uniqueCountries[randList[0]][1],'\n'
            '2.',uniqueCountries[randList[1]][1],'\n'
            '3.',uniqueCountries[randList[2]][1],'\n'
            '4.',uniqueCountries[randList[3]][1],'\n')
    optionsDict = {1:uniqueCountries[randList[0]][1],2:uniqueCountries[randList[1]][1],3:uniqueCountries[randList[2]][1],4:uniqueCountries[randList[3]][1]}
    imgplot = plt.imshow(mpimg.imread('png1000px/'+str(uniqueCountries[0][0].lower())+'.png'))    
    plt.axis('off')
    plt.show()
    while True:
        try:
            playerGuess = input('Guess (enter "n" to exit): ')
            if playerGuess == 'n':
                print('\n' + color.BOLD + color.BLUE + 'Final Score: ' + str(score) + '/' + str(count) + color.END)
                exit()
            elif int(playerGuess) < 0:
                raise ValueError
            elif int(playerGuess) > 4:
                raise ValueError
            count += 1
            break
        except ValueError:
            print(color.YELLOW + 'Guess must be between 1 & 4' + color.END)
    if optionsDict[int(playerGuess)] == uniqueCountries[0][1]:
        score += 1     
        print(color.GREEN + color.BOLD + '\n' + uniqueCountries[0][1] + color.END)
    else:
        print(color.RED + color.BOLD + '\n' + optionsDict[int(playerGuess)] + color.END, '\n' +
        color.GREEN + color.BOLD + uniqueCountries[0][1] + color.END, '\n')
    time.sleep(3)
    flagQuiz()        
flagQuiz()
```

### Conclusion

Thank you for taking the time to check out this very simple flag quiz, all built in python, and playable in your terminal!

If you're interested in more, checkout my other projects on this site or visit my GitHub account ([here][github-account] ↗)

[countries-source]: https://github.com/hampusborgos/country-flags
[original-quiz]: https://world-geography-games.com/en/flags_world.html
[github-flagquiz]: https://github.com/LukeNelsn/pyflagquiz
[github-account]: https://github.com/LukeNelsn/