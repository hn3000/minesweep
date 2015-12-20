Minesweeper-like game.

I saw a link to this [article] about interviewing candidates on Hackernews.

Reading it, I found myself agreeing with most of the ideas, especially
the part about candidates that were all talk and no action struck a nerve.

But I was surprised that an hour was deemed not nearly enough to
implement basic Minesweeper. I thought that it should be possible to implement
the basics of gameplay in that time if I had all the required assets.

The [jsbin] contained a convenient JS object with some imgur links and
as a first step I wrote some code to render a table full of img elements,
just so I could have a look at the images. Seeing the images after just a
minute or so seemed to confirm that it should be easy enought and I continued
noodling around with the jsbin.

I was interrupted a few times and did not track the time it took me to
get to the "1h" version exactly, but it can't have been more than
an hour and 10 minutes.

My implementation went like this:
1) create some code to render the board as a table full of TR, TD and IMG nodes
1a) added a single style that made the img elements 32px wide
2) create an array of cells, using numbers as bitfields to hold cell states
3) generate some random bombs and note the number of neighbors in the cells array
4) display the generated data (no clicking and no hiding yet)
5) check the generated number of neighbors (in the first version I wrapped around on the left and right boundary because my cells array is one-dimensional, this problem was fixied using the "cell" function)
6) verify the generated data by looking at a few examples (the pattern was generated on load, browser reload was used to generate a new state)
7) change cell rendering to initially not show anything
8) attach a click handler to the table (so only one addEventListener was necessary)
9) handle the click by changing the state to "clicked" (or the cell value with 512, which is a single set bit since it's 2^9, I had decided on this representation earlier and did not find it hard to use)
10) update the cell using replaceChild (see note below), refactoring the actual cell-rendering code out of the renderView function
11) write the flood fill for the case where an empty cell was hit by the click, I used setTimeout because I wanted the fill to be animated
12) at this point I changed from jsbin to a local file in a Atom because the debugger started to annoy me while I edited code, every time the code became invalid the debugger stopped because I had turned on "stop on execption" and syntax errors in the error could cause uncaught exceptions
13) after a few corrected typos and type-conversion problems the code worked okay (I had to remember to make sure my array indices were turned into numbers when I read from node attributes -- I was lazy, I simply multiplied by 1; since I wanted to finish as quickly as possible I used the simplest coding strategies that I knew off the top of my head, and I wanted to avoid having to look up Number.parseInt vs parsetInt or whatever that function is called)

At this point I noticed that hour was certainly gone (I think I was about 10 minutes over time) but the game was already playable. I noticed that the flood-fill of empty cells often made it very easy to solve the game and identified two main reasons:
1) the probability for placing a bomb at some location was about 5% at first, which was way too low, and
2) the flood fill would continue if two cells with neighboring bombs sat diagonally from each other

I changed point 1 in the 1h version, but point 2 I addressed later by considering only the four non-diagonal neighbors in my flood-fill algorithm. I think this is different from the original minesweeper, but I did not have Windows at hand and did not check.



Note: I did have to look up the order of arguments for replaceChild when
my first guess turned out wrong and my second guess (although correct,
there aren't that many permutations) did not work because I had gone
one parentNode too far.





[article]: http://rkoutnik.com/articles/How-I-Interview.html
[jsbin]: http://jsbin.com/jucesisaki/2/edit?html,js,output