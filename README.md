# Hangman
import random

WORDLIST_FILENAME = "words.txt"

def loadWords():
    print("Loading word list from file...")
    inFile = open(WORDLIST_FILENAME, 'r')
    line = inFile.readline()
    wordlist = line.split()
    print("  ", len(wordlist), "words loaded.")
    return wordlist

def chooseWord(wordlist):
    return random.choice(wordlist)

wordlist = loadWords()

def isWordGuessed(secretWord, lettersGuessed):
    count = 0
    for i in lettersGuessed:
        if i in secretWord:
            count += 1
    return count == len(secretWord)



def getGuessedWord(secretWord, lettersGuessed):
    List = []
    s = "_ "
    for i in range(len(secretWord)):
        List.append(s)
    for j in range(len(lettersGuessed)):
        for k in range(len(secretWord)):
            if lettersGuessed[j] == secretWord[k]:
                List[k] = secretWord[k]
    result = ("")
    for m in range(len(secretWord)):
        result += List[m]
    return result         
        
        
def getAvailableLetters(lettersGuessed):
    s = 'abcdefghijklmnopqrstuvwxyz'
    List = []
    for i in s:
        List.append(i)
    for j in lettersGuessed:
        if j in List:
            List.remove(j)
    result = ("")
    for k in range(len(List)):
        result += List[k]
    return result
    

def hangman(secretWord):
    print("Welcome to the game Hangman!")
    print("I am thinking of a word that is " + str(len(secretWord))+" letters long.")
    numGuesses = 8
    lettersGuessed = []
    while True:
      print("-------------")
      print("You have " + str(numGuesses) + " guesses left.")
      print("Available letters: " + getAvailableLetters(lettersGuessed))
      print("Please guess a letter:",end = " ")
      a = input("")
      lettersGuessed.append(a)
      if a in secretWord and lettersGuessed.count(a) == 1 and not isWordGuessed(secretWord, lettersGuessed):
          print("Good guess: " + getGuessedWord(secretWord, lettersGuessed))
      elif lettersGuessed.count(a) > 1 and getGuessedWord(secretWord, lettersGuessed) != secretWord:
          lettersGuessed.remove(a)
          print("Oops! You've already guessed that letter: " + getGuessedWord(secretWord, lettersGuessed))
      elif a not in secretWord:
          print("Oops! That letter is not in my word: " + getGuessedWord(secretWord, lettersGuessed))
          numGuesses -= 1
          if numGuesses == 0:
              print("-------------")
              print("Sorry, you ran out of guesses. The word was " + secretWord)
              break
      elif a in secretWord and isWordGuessed(secretWord, lettersGuessed):
          print("Good guess: " + getGuessedWord(secretWord, lettersGuessed))
          print("-------------")
          print("Congratulations, you won!")
          break
secretWord = chooseWord(wordlist).lower()
hangman(secretWord)
