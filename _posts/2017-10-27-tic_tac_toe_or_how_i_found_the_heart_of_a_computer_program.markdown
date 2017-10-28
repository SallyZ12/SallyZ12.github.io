---
layout: post
title:      "Tic Tac Toe or How I Found the Heart of a Computer Program"
date:       2017-10-28 00:10:11 +0000
permalink:  tic_tac_toe_or_how_i_found_the_heart_of_a_computer_program
---


On my second blog posting, I discussed how Tic Tac Toe, while a game, is so much more than that.
Well, now that I've completed the Tic Tac Toe with Artificial Intelligence project, I can say that I've gone from competing with humans to competing with a computer to watching computers battle it out.

How have I accomplished this task?  I took a bunch of coding from the lntro to Ruby section of the program, refactored for Objective Oriented Ruby and added a  dash of CLI code to reach my goal.

While the first basic Tic Tac Toe game seemed like a tremendous accomplishment, the Tic Tac Toe with AI was mind shattering.  Refactor after refactor to get the code to work, multiple "Ask A Question" sessions with a Tech Coach, walking away for hours or a day at a time to refresh my brain, and then back at the task at hand.

The most difficult parts of the process were figuring out a computer move method and then designing the CLI component--actually a call method in a different file than the CLI file to have a nice clean file with very little code.  I think of the computer player move method as part of brains of the operation (logic after logic-- is a cell blocked what token has populated that cell, should the computer move to an empty cell to block the other player's move, etc.) while the call method used in the CLI is the heart of the operation.   A class TicTacToeCLI is created that allows you to model its behavior in the call method.

But, why the heart?

Let's look at the code:

```
class TicTacToeCLI

  def initialize
  end

  def call

    puts "Welcome to Tic Tac Toe!"

    puts "What Kind of Game Do You Want to Play?  0, 1, or 2 Player(s)"

    puts "Enter Number of Player(s):"


    players = " "

    players = gets.strip.to_i
    case players

      when 0
        puts "2 Computers Playing"
        game = Game.new(player_1 = Players::Computer.new("X"),player_2 = Players::Computer.new("O"))
        game.play

      when 1
        puts "1 Human and 1 Computer Playing"
        game = Game.new(player_1 = Players::Human.new("X"),player_2 = Players::Computer.new("O"))
        game.play


      when 2
        puts "2 Humans Playing"
        game = Game.new
        game.play


        play_again = " "
        until play_again == "N"
          puts "To stop play enter N, otherwise enter Y to keep playing!"
          play_again = gets.strip

          if play_again == "Y"
            game = Game.new
            game.play
          end

        end
      end
    end
  end
```
While there is some conditional logic, this call method allows you, as the human, to shape how you want the computer program to interact with you.  You greet the user, ask the user what kind of game to play, have the user respond to the question, confirm to the user you're now playing the game with: "2 Computers", "1 Human and 1 Computer" or "2 Computers".  You as the creator of this program can craft whatever interface you want--if an incorrect move is made--you can shout it out "Invalid Move", etc.  You can keep it simple or make it complex, but you control the feel and attitude of this program.  It all culminates in the bin folder tictactoe file by using a simple call of the class TicTacToeCLI.

```
require_relative '../config/environment'


TicTacToeCLI.new.call
```

So, to start the game in the Terminal it's a simple as ruby bin/tictactoe and you're off to the races with any kind of game you want to play.

As I've progressed through the Full Stack Online Web Development Program, I'm becoming more aware of how coding begins to reflect the personality of the coder.  It's an exciting time to be part of this experience.

