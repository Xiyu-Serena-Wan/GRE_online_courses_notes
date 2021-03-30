    def getWordScore(word, n):
        """
        Returns the score for a word. Assumes the word is a valid word.

        The score for a word is the sum of the points for letters in the
        word, multiplied by the length of the word, PLUS 50 points if all n
        letters are used on the first turn.

        Letters are scored as in Scrabble; A is worth 1, B is worth 3, C is
        worth 3, D is worth 2, E is worth 1, and so on (see SCRABBLE_LETTER_VALUES)

        word: string (lowercase letters)
        n: integer (HAND_SIZE; i.e., hand size required for additional points)
        returns: int >= 0
        """
        sum = 0
        if len(word) == 0:
            return sum
        else:
            for item in word:
                sum += SCRABBLE_LETTER_VALUES[item]
            sum *= len(word)
            if len(word) == n:
                sum += 50
            return sum

    #
    # Problem #2: Update a hand by removing letters
    #
    def updateHand(hand, word):
        """
        Assumes that 'hand' has all the letters in word.
        In other words, this assumes that however many times
        a letter appears in 'word', 'hand' has at least as
        many of that letter in it. 

        Updates the hand: uses up the letters in the given word
        and returns the new hand, without those letters in it.

        Has no side effects: does not modify hand.

        word: string
        hand: dictionary (string -> int)    
        returns: dictionary (string -> int)
        """
        handy = hand.copy()
        handkeys = hand.keys()
        for item in word:
            if item in handkeys and item in handy.keys():
                handy[item] -= 1
        return (handy)
    
    #
    # Problem #3: Test word validity
    #
    def isValidWord(word, hand, wordList):
        """
        Returns True if word is in the wordList and is entirely
        composed of letters in the hand. Otherwise, returns False.

        Does not mutate hand or wordList.

        word: string
        hand: dictionary (string -> int)
        wordList: list of lowercase strings
        """
        handyy=hand.copy()
        if word in wordList:
            for letter in getFrequencyDict(word).keys():
                if letter in hand.keys():
                    if getFrequencyDict(word)[letter] > hand[letter]:
                        return False
                    else:
                        continue
                else:
                    return False
            return True
        else:
            return False
         
         
    def calculateHandlen(hand):
        """ 
        Returns the length (number of letters) in the current hand.

        hand: dictionary (string-> int)
        returns: integer
        """
        sum = 0

        for item in hand.keys():
            sum += hand[item]
        return sum
        
    def playHand(hand, wordList, n):
        """
        Allows the user to play the given hand, as follows:

        * The hand is displayed.
        * The user may input a word or a single period (the string ".") 
          to indicate they're done playing
        * Invalid words are rejected, and a message is displayed asking
          the user to choose another word until they enter a valid word or "."
        * When a valid word is entered, it uses up letters from the hand.
        * After every valid word: the score for that word is displayed,
          the remaining letters in the hand are displayed, and the user
          is asked to input another word.
        * The sum of the word scores is displayed when the hand finishes.
        * The hand finishes when there are no more unused letters or the user
          inputs a "."

          hand: dictionary (string -> int)
          wordList: list of lowercase strings
          n: integer (HAND_SIZE; i.e., hand size required for additional points)

        """
        # BEGIN PSEUDOCODE <-- Remove this comment when you code this function; do your coding within the pseudocode (leaving those comments in-place!)
        # Keep track of the total score
        point = 0
        # As long as there are still letters left in the hand:
        while calculateHandlen(hand) != 0:
            # Display the hand
            handys = ''
            for letter in hand.keys():
                for j in range(hand[letter]):
                    handys += letter
                    handys += ' '
            print('Current Hand: ',handys)

            # Ask user for input
            word = input("Enter word, or a \".\" to indicate that you are finished: ") 
            # If the input is a single period:
            if word == '.':
                break
                # End the game (break out of the loop) 
            # Otherwise (the input is not a single period):
            else:
                # If the word is not valid:
                if not isValidWord(word, hand, wordList):
                    # Reject invalid word (print a message followed by a blank line)
                    print('Invalid word, please try again.')
                    print()
                else:
                # Otherwise (the word is valid):
                    point += getWordScore(word, n)
                    print('\"{}\" earned {} points. Total: {} points '.format(word,getWordScore(word, n),point))
                    print()
                    # Tell the user how many points the word earned, and the updated total score, in one line followed by a blank line
                    hand = updateHand(hand, word)

                    # Update the hand 


        # Game is over (user entered a '.' or ran out of letters), so tell user the total score
        print('Goodbye! Total score: {} points.'.format(point))
        
        def playGame(wordList):
    """
    Allow the user to play an arbitrary number of hands.

    1) Asks the user to input 'n' or 'r' or 'e'.
      * If the user inputs 'n', let the user play a new (random) hand.
      * If the user inputs 'r', let the user play the last hand again.
      * If the user inputs 'e', exit the game.
      * If the user inputs anything else, tell them their input was invalid.
 
    2) When done playing the hand, repeat from step 1    
    """
    n = HAND_SIZE
    val = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')
    if val == 'n' or val == 'r' or val == 'e':
        if val == 'r':
            print('You have not played a hand yet. Please play a new hand first!')
            return playGame(wordList)
        if val == 'n':
            dealdic = dealHand(n)
            playHand(dealdic, wordList, n)
            val_n = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')
            while val_n == 'r':
                playHand(dealdic, wordList, n)
                val_n = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')
                if val_n != 'r':
                    
                    break
            while val_n == 'n':
                playHand(dealHand(n), wordList, n)
                return playGame(wordList)
            if val_n == 'e':
                return None
            else:
                print('Invalid command.')
                return playGame(wordList)

        if val == 'e':
            return None
    else:
        print('Invalid command.')
        return playGame(wordList)
        
        #
    # Problem #6: Playing a game
    #
    #
    def playGame(wordList):
        """
        Allow the user to play an arbitrary number of hands.

        1) Asks the user to input 'n' or 'r' or 'e'.
            * If the user inputs 'e', immediately exit the game.
            * If the user inputs anything that's not 'n', 'r', or 'e', keep asking them again.

        2) Asks the user to input a 'u' or a 'c'.
            * If the user inputs anything that's not 'c' or 'u', keep asking them again.

        3) Switch functionality based on the above choices:
            * If the user inputted 'n', play a new (random) hand.
            * Else, if the user inputted 'r', play the last hand again.

            * If the user inputted 'u', let the user play the game
              with the selected hand, using playHand.
            * If the user inputted 'c', let the computer play the 
              game with the selected hand, using compPlayHand.

        4) After the computer or user has played the hand, repeat from step 1

        wordList: list (string)
        """
        n = HAND_SIZE
        val = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')

        if val == 'n' or val == 'r' or val == 'e':
            if val == 'e':
                return None

            if val == 'r':

                print('You have not played a hand yet. Please play a new hand first!')

                return playGame(wordList)
            choice = input('Enter u to have yourself play, c to have the computer play: ')
            while choice != 'u' and choice != 'c':
                print('Invalid command.')
                print()
                choice = input('Enter u to have yourself play, c to have the computer play: ')
            if choice == 'u':
                if val == 'n':
                    dealdic = dealHand(n)
                    playHand(dealdic, wordList, n)
                    val_n = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')
                    while val_n == 'r':
                        choice = input('Enter u to have yourself play, c to have the computer play: ')
                        if choice == 'u':
                            playHand(dealdic, wordList, n)
                            val_n = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')
                            if val_n != 'r':

                                break
                        if choice == 'c':

                            compPlayHand(dealdic, wordList, n) 
                            val_n = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')
                            if val_n != 'r':

                                break

                    while val_n == 'n':
                        choice = input('Enter u to have yourself play, c to have the computer play: ')
                        if choice == 'u':
                            playHand(dealHand(n), wordList, n)
                            return playGame(wordList)
                        if choice == 'c':
                            hand = dealHand(n)
                            compPlayHand(hand, wordList, n) 
                            return playGame(wordList)


                    if val_n == 'e':
                        return None
                    else:
                        print('Invalid command.')
                        return playGame(wordList)

                if val == 'e':
                    return None

            if choice == 'c':
                dealdid = dealHand(n)
                compPlayHand(dealdid, wordList, n) 

                val_n = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')
                while val_n == 'r':
                    choice = input('Enter u to have yourself play, c to have the computer play: ')
                    if choice == 'u':
                        playHand(dealdid, wordList, n)
                    val_n = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')
                    if val_n != 'r':
                    
                        break
                if choice == 'c':

                    compPlayHand(dealdid, wordList, n) 
                    val_n = input('Enter n to deal a new hand, r to replay the last hand, or e to end game: ')
                    if val_n != 'r':
                    
                        break
                    
            while val_n == 'n':
                choice = input('Enter u to have yourself play, c to have the computer play: ')
                if choice == 'u':
                    playHand(dealHand(n), wordList, n)
                    return playGame(wordList)
                if choice == 'c':
                    hand = dealHand(n)
                    compPlayHand(hand, wordList, n) 
                    return playGame(wordList)


            if val_n == 'e':
                return None
            else:
                print('Invalid command.')
                return playGame(wordList)


        else:
            print('Invalid command.')
            return playGame(wordList)
       
    else:
        print('Invalid command.')
        return playGame(wordList)
   
