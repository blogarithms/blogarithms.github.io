---
layout: post
title: "Getting Game Theory (Part 1)"
excerpt: "Grundy Numbers, Nim Game, Minimum Excludants, Math and Logic!"
categories: [algorithm]
comments: true
---
<i>Minimal Excludants, Partisan games, Impartial games, Sprague-Grundy Theorem, Nim game, Combinatorial Game Theory, Minimax algorithms, and Grundy Numbers.</i>
<br><br>
<i>All of 'em represent a field of mathematics and computer science, titled "Game Theory".</i>
<br><br>
<i>(No, not the Economics - Nobel Prize - A Beautiful Mind - Russel Crowe movie but a different "Combinatorial" Game Theory.)</i>
<br><br>
<i>Let's just jump in!</i>

<hr>

Firstly, let's discuss games. Not the console/PC type (<i>PC Master R</i><i>ace &lt;3 tho), </i>but let's discuss the board games type.
<br><br>
Consider <b>the Nim Game</b>.
<br><br>
The rules are as follows -
<ul>
<li>There are 'n' piles of stones in front of you and your opponent.</li>
<li>The person whose move it is, has the right to choose any pile of stones.</li>
<li>After choosing a pile, this person may remove any number (&gt;=1) of stones from the pile.</li>
<li>The last person to make a move (i.e. Remove stone(s) from the final pile) wins.</li>
<li>In other words, the first person who can't make a move loses.</li>
</ul>
Such games are called as <b>Impartial Games, </b>games where there is no partiality or bias toward either players. A game where all the information is evident and present to all participants of the game, unlike a game such as Poker or Blackjack.
<br><br>
The concept of Game Theory, mex, and grundy numbers apply only to such full information state games.
<br><br>
Here's a simulation of Nim Game. Consider 3 piles, each pile having 3, 4 and 5 stones respectively, and A &amp; B are playing against one another.
<br><br>
<img class="aligncenter" src="http://www.geeksforgeeks.org/wp-content/uploads/Set2_Nim_Game_start_with_A.jpg" alt="Set2_Nim_Game_start_with_A">
<br><br>
(Note: A started the match in the above simulation)
<br><br>
The fact that all the information is open to both players raises the question -- <b>Is it possible to leverage this and make an educated first move? Can the outcome of the game be decided before it even begins? Is that even possible?</b>
<br><br>
<i>Sounds too anime-ish, I agree.</i>
<br><br>
<b>Yes. </b>
<br><br>
<i>Maximum anime cliche achieved. </i>
<br><br>
Consider this alternate scenario where B played first -
<br><br>
<img class="aligncenter" src="http://www.geeksforgeeks.org/wp-content/uploads/gameofnim11.jpg" alt="gameofnim11">
<br><br>
B won. Coincidentally here too, B made the first move.
<br><br>
So does this mean, the starting player unconditionally wins? Can we generalize this observation to that extent?
<br><br>
As it turns out, to a certain degree, <b>yep. </b>The starting player has the upper hand. By that, what I mean is that, he can control the outcome if he plays without making an error.
<br><br>
Here's the trick to the Nim Game. <b>The cumulative "nim-sum" must be zero."</b>
<br><br>
The nim-sum operation (more commonly known as the XOR operator), is the key to breaking this game.
<br><br>
There are two observations to be made here -
<ul>
<li>Any move made from a losing state, HAS to lead to a winning state. i.e. - notice that any move can only change one of the A[i]. If you change a single bit in an XOR of bits, you will change the final result. So if you start with an XOR total of 0, you must end up with an XOR total of that is unequal to 0.</li>
</ul>
<ul>
<li>There exists SOME move that can bring a winning state into a losing state. i.e. - Find the cumulative XOR of number of stones across all piles, this value has to be lesser than or equal to the number of stones in the largest pile. Thus, the same amount of stones can be removed from the largest pile to lead to a losing state as the other player now has to make a move in a game where the cumulative nim-sum is 0.</li>
</ul>
The reason XOR, fits in so brilliantly is because of such desirable properties which support our observations.
<br><br>
Thus the key to Nim Game is -
<br><br>
If the cumulative XOR across the board is equal to zero, the player whose turn it is loses. If the cumulative XOR across the board is NON-zero, the player making the turn is guaranteed to win if he plays optimally.
<br><br>
An easy way to understand and even visualize this for faster future recall, is to consider this example, you and I playing this game with 2 piles, of the same number of stones. Say you start, my strategy would be <b>to precisely mirror your move on the other pile</b>. You'll realize, this way, I'll always end up making the last move, as you started.
<br><br>
Frankly, up until now before I researched more of Game Theory and Theorems and current algorithms and approaches in the field, this was my primary method of solving. Identify some potential parameters that may play a role in changing the state from a winning to a losing state or vice versa, and then analyse deeply to observe what is the "equivalent number" I need to cumulative XOR over, or perform another such relevant operation on.
<br><br>
Of course, this has its limitations but my success with it gives me some solace in considering it as my last resort.
<br><br>
<h2>Coming up next - Grundy Numbers and Mex</h2>
This is a number that can define the state of the game. 
<br><br> 
For example, in the nim game, the grundy number was the stones in a pile (A[i]). Grundy numbers are the numbers associated with the Sprague-Grundy Theorem. 
<br><br>
Next post, we'll be dealing with them and their prerequisite -- <b>mex</b>, short for "Minimal Excludant".
<hr>
I hope you enjoyed the post, and stick around for part 2 of Game Theory. 
<br><br>
<i>Aditya Ramesh</i>