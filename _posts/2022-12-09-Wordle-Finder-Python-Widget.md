---
layout: post
title: Wordle Finder Python Widget
gh-repo: sherr3h/sherr3h.github.io
tags: [Wordle, game, Python]
comments: true
---
I created a Python widget to find 5-letter word candidates for the New York Times game [Wordle](https://www.nytimes.com/games/wordle/index.html), using simple regular expressions and ipywidgets. Wordle is a 5-letter word guessing game:

![WordleRule](/img/Wordle.png)


I tried to put the widget online. Unfortunately, it's not easy to host a live jupyter notebook to display the widget on Github. Tried using Binder, but it's really slow. So here's the Python code for anyone interested. Copy them into a jupyter notebook and play Wordle with fewer guesses!

### Data Source
- [sgb-words](https://www-cs-faculty.stanford.edu/~knuth/sgb-words.txt), a 5,000 5-letter common words corpus on Stanford CS website
- [Wordle Valid Words](https://www.kaggle.com/datasets/bcruise/wordle-valid-words), a 10,000 words corpus on Kaggle


### Dependencies

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from collections import defaultdict
import re
from urllib.request import urlopen
%matplotlib widget
import ipywidgets as widgets
from ipywidgets import Layout, Button, Box, Textarea, Label, GridBox, ButtonStyle
```

### The Wordle Finder Class
```python
class Wordle():
    def __init__(self):
        self.confirmed = set()
        self.greens = defaultdict(list)
        self.yellows = defaultdict(list)
        self.greys = set()
        self.readStartWords()
    
    def readStartWords(self):
        url1 = r'https://www-cs-faculty.stanford.edu/~knuth/sgb-words.txt'
        data = urlopen(url1)
        words = [w.strip().decode("utf-8")  for w in list(data)]
        url2 = r"https://raw.githubusercontent.com/sherr3h/sherr3h.github.io/master/data/valid_guesses.csv"
        words2 = list(pd.read_csv(url2)["word"])
        self.possibleWords = list(set(words + words2))
        self.possibleWords.sort()
        

    def updateGuess(self, greens, yellows, greys):
        self.matchGreen(greens)
        self.matchYellow(yellows)
        self.matchGrey(greys)
        
    def matchGreen(self, greens):
        for i in greens.keys():
            if greens[i]:
                pattern = re.compile("^.{"+str(i)+"}"+greens[i][0].lower()+".*")
                self.possibleWords = list(filter(pattern.match, self.possibleWords))
                self.confirmed.add(i)
                #print(possibleWords)

    def matchYellow(self, yellows):
        for i in yellows.keys():
            if yellows[i] and i not in self.confirmed:
                for j in range(len(yellows[i])):
                    pattern = re.compile("\w*"+yellows[i][j].lower()+"\w*")# or (".*"+yellows[i][0]+".*")
                    self.possibleWords = list(filter(pattern.match, self.possibleWords))
                    pattern = re.compile("^.{"+str(i)+"}[^"+yellows[i][j].lower()+"].*")
                    self.possibleWords = list(filter(pattern.match, self.possibleWords))
                    
    def matchGrey(self, greys):
        if greys:
            excludes = ''.join(list([g.lower() for g in greys]))
            pattern = re.compile("[^" + excludes +"]{5}")
            self.possibleWords = list(filter(pattern.match, self.possibleWords))
        
```

### Some HTML styles to use Wordle colors 
```python
%%html
<style>
.green{
    width:auto;
    background-color:#6ca965;
    color: white;
}
.yellow{
    width:auto;
    background-color:#c8b653;
    color: white;
}
.grey{
    width:auto;
    background-color:#787c7f;
    color: white;
}
</style>
```

### Setting up the widget
```python
green_fields = [Textarea(layout=Layout(width='100%', grid_area='green' + str(i))) for i in range(1,6)]
yellow_fields = [Textarea(layout=Layout(width='100%', grid_area='yellow' + str(i))) for i in range(1,6)]
grey_field = Textarea(layout=Layout(width='100%', grid_area='grey'))
green_label  = widgets.Label(value= ' Correct letter & position:', 
                             layout=Layout(width='100%', grid_area='greenLabel'))
yellow_label  = widgets.Label(value= ' Correct letter, incorrect position:', 
                              layout=Layout(width='100%', grid_area='yellowLabel'))
grey_label  = widgets.Label(value= ' Incorrect letter & position:', 
                              layout=Layout(width='100%', grid_area='greyLabel'))
yellow_label.add_class('yellow')
green_label.add_class('green')
grey_label.add_class('grey')

submit  = Button(description='Update',
                 layout=Layout(width='auto', grid_area='header'),
                 style=ButtonStyle(button_color='lightblue'))
clear    = Button(description='Clear All',
                 layout=Layout(width='auto', grid_area='clear'),
                 style=ButtonStyle(button_color='moccasin'))

greens = defaultdict(list)
yellows = defaultdict(list)
guess = Wordle()
```

### Setting up two buttons, Submit and Clear 
```python
def onSubmitButtonClicked(b):
    for i in range(5):
        greens[i] = [green_fields[i].value] if green_fields[i].value  else []
        yellows[i] = list(yellow_fields[i].value) if yellow_fields[i].value else [] 

    greys = set(list(grey_field.value))
    print("Greens:", greens)
    print("Yellows:", yellows)
    print("Greys:", greys)
    guess.updateGuess(greens, yellows, greys)
    print(guess.possibleWords)
    
def onClearButtonClicked(b):
    for i in range(5):
        green_fields[i].value = ''
        yellow_fields[i].value = ''
    grey_field.value = ''
    greens = defaultdict(list)
    yellows = defaultdict(list)
    greys = set()
    print("Greens:", greens)
    print("Yellows:", yellows)
    print("Greys:", greys)
    guess.greens = greens
    guess.yellows = yellows
    guess.greys = greys
    guess.readStartWords()
    
submit.on_click(onSubmitButtonClicked)
clear.on_click(onClearButtonClicked)
```

### Displaying everything
```python
sub = GridBox(children=[green_label,
                        green_fields[0], green_fields[1], green_fields[2], green_fields[3], green_fields[4], 
                        yellow_label,
                        yellow_fields[0], yellow_fields[1], yellow_fields[2], yellow_fields[3], yellow_fields[4],
                        grey_label,
                        grey_field,
                        submit,
                        clear],
        layout=Layout(
            width='50%',
            grid_template_rows='auto auto auto auto auto auto auto auto', # auto auto',
            grid_template_columns='20% 20% 20% 20% 20%',
            grid_template_areas='''
            "greenLabel greenLabel . . ."
            "green1 green2 green3 green4 green5"
            "yellowLabel yellowLabel . . ."
            "yellow1 yellow2 yellow3 yellow4 yellow5"
            "greyLabel greyLabel . . ."
            "grey grey . . ."
            "header header header header header"
            "clear clear clear clear clear"
            ''')
       )
display(sub)
```

![WordleWidget](/img/WordleFinder.png)
