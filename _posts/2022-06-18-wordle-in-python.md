---
layout: post
title: Wordle in Python
subtitle: Play Wordle in your terminal
author: Luke
categories: projects
banner:
  video: https://stream.mux.com/RPcdshtnn5q54RsZAKRCTseOIL6YvORA/high.mp4?token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6InQ5UHZucm9ZY0hQNjhYSmlRQnRHTEVVSkVSSXJ0UXhKIn0.eyJleHAiOjE2NTU3NjI3NDYsImF1ZCI6InYiLCJzdWIiOiJSUGNkc2h0bm41cTU0UnNaQUtSQ1RzZU9JTDZZdk9SQSJ9.HC94KysO8rvv7AuE6ib-Fva3oZaXuwKJiXd2VcfjBfg1EQiIADf-gLJlR0G2Bw2jmvIluIReUNACjd0PQ9d5WanSCcv2vOuTNQWKIbzcAQit8dFpQUIavkmJgYi_0FtVLYDD5E3u8s2lTsT_nxrpAT0OaCAzArreD-pFhB9wJc2my_4wQCPe-Df44Xq61y8z5XowxrrjNm8dk1c_gAttJNS6PZxVHvwZ3Tk2XjNFHHH7sU2wvHx3Z-bw1e_RZaBg4yYy-lbS1uVDCp-CkCaAJ1Dc91-HTcRWldZ8YWsFTWXfAi1tAXwfNUdELZ3j9xSvLpPAoWLpryITvpYdLAgRRA
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
tags: projects python wordle
sidebar: []
---

This simple and fun game captivated the internet for weeks during 2022. Now, you can play the game from your own terminal, all with some simple python scripts.

The game is both rudimentary and simplistic, yet matches the design and style of the original internet sensation ([here][original-wordle] ↗). Feel free to modiify and change the game as you see fit!

This game runs perfectly just as a python file, and only requires the additional `wordle-answers-alphabetical.txt` file to be stored in the same directory as your `wordle.py` file, unless you want to reference the `.txt` file another way.

> Each of these files can also be downloaded from my GitHub account ([here][github-wordle] ↗)

## Python Wordle

Required modules: `random`

#### Preliminary importing and organization of our words

Let's also establish a class of colors, and some score keeping metrics

```python
import random
words_5 = list(open('wordle-answers-alphabetical.txt', 'r'))
words_5 = list(map(lambda s: s.strip(), words_5))
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
count = 0
guesses = 0
score = 0
streak = 0
```

#### Define the Wordle game itself

We will loop over this many times as the user plays the game

```python
def wordleGame(): 
    print(color.BOLD + color.DARKCYAN + '\nWORDLE\n' + color.END)
    word = (words_5[random.randrange(0,len(words_5))]).lower()
    def guessingFunction():
        global count, guesses, score, streak       
        while True:
            try:
                word_guess = (input('Guess: ')).lower()
                if len(word_guess) != 5:
                    raise ValueError
                elif word_guess not in words_5:
                    raise ValueError
                break
            except ValueError:
                print(color.YELLOW + 'Guess must be 5 letters long & in the English dictionary' + color.END)
        if word_guess == word:
            print(color.GREEN + color.BOLD + word + '\n' + 'You Win!' + color.END)
            score += 1
            print(color.PURPLE + '\nScore: ' + color.BOLD + str(score) + color.END)
            streak += 1
            print(color.PURPLE + '\nStreak: ' + color.BOLD + str(streak) + color.END)
            count = 0
            guesses = 0
        elif word_guess != word:
            word_index = 0
            word_guess_ouput = []
            for letter in word_guess:
                if letter not in word:
                    word_index += 1
                    word_guess_ouput.append(letter)
                elif letter == word[word_index]:
                    word_index += 1
                    word_guess_ouput.append(color.GREEN + letter + color.END)
                    continue
                else:
                    word_guess_ouput.append(color.BLUE + letter + color.END)
                    word_index += 1
                    continue         
            count += 1
            guesses += 1
            print('Guess',str(guesses)+'/6:',"".join(word_guess_ouput))
            if count == 6:
                score += 0
                print(color.PURPLE + '\nScore: ' + color.BOLD + str(score) + color.END)
                streak = 0
                print(color.PURPLE + '\nStreak: ' + color.BOLD + str(streak) + color.END)
                print('\n' + color.RED + color.BOLD + 'You lose...\nThe correct word was:', color.UNDERLINE + color.YELLOW + word + color.END)
                count = 0
                guesses = 0
            else:
                guessingFunction()   
    guessingFunction()
    rerun = input('\nWould you like to play again?''\n(y/n): ')
    if rerun == 'y':
        wordleGame()
    else:
        print('Goodbye!\n')
        exit()
wordleGame()
```

### Conclusion

Thank you for taking the time to check out this very simple version of worlde, all built in python, and playable in your terminal!

If you're interested in more, checkout my other projects on this site or visit my GitHub account ([here][github-account] ↗)


[original-wordle]: https://www.nytimes.com/games/wordle/index.html
[github-wordle]: https://github.com/LukeNelsn/pyworlde
[github-account]: https://github.com/LukeNelsn/