    def hangman(secretWord):
        '''
        secretWord: string, the secret word to guess.

        Starts up an interactive game of Hangman.

        * At the start of the game, let the user know how many 
          letters the secretWord contains.

        * Ask the user to supply one guess (i.e. letter) per round.

        * The user should receive feedback immediately after each guess 
          about whether their guess appears in the computers word.

        * After each round, you should also display to the user the 
          partially guessed word so far, as well as letters that the 
          user has not yet guessed.

        Follows the other limitations detailed in the problem write-up.
        '''
        print("Welcome to the game Hangman!")
        print("I am thinking of a word that is {} letters long.".format(len(secretWord)))

        guesses = 8
        lettersGuessed = []
        while guesses > 0:
            print('-------------')
            print("You have {} guesses left.".format(guesses))
            print("Available letters: {}".format(getAvailableLetters(lettersGuessed)))
            letter = input("Please guess a letter: ") 
            if letter in lettersGuessed:
                print("Oops! You've already guessed that letter: {}".format(getGuessedWord(secretWord, lettersGuessed)))
            else:
                lettersGuessed += letter
            #print(lettersGuessed)
                if letter in secretWord:
                    print('Good guess: {}'.format(getGuessedWord(secretWord, lettersGuessed)))
                    #guesses -= 1
                    if isWordGuessed(secretWord, lettersGuessed) == True:
                        print('-------------')
                        print('Congratulations, you won!')
                else:
                    print('Oops! That letter is not in my word: {}'.format(getGuessedWord(secretWord, lettersGuessed)))
                    guesses -= 1
            #print('lettersGuessed is: {}'.format(lettersGuessed))

        if guesses == 0 and isWordGuessed(secretWord, lettersGuessed) != True:
            print('-------------')
            print("Sorry, you ran out of guesses. The word was {}. ".format(secretWord))



    # When you've completed your hangman function, uncomment these two lines
    # and run this file to test! (hint: you might want to pick your own
    # secretWord while you're testing)
