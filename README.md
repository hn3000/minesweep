# Minesweeper-like game in HTML

## Running

Since it's only a single page with some external img references you can run the game from the filesystem.

Or use this version on [github.io].

## Installation

Just put it in a folder on a webserver somewhere.


## Dependencies

This project uses [vanilla JS]. Because it's so simple, it doesn't even need all the features, e.g. Animations, Prototype-based Object System and AJAX are probably not used. There are no unit tests at the moment.

## Motivation

I saw a link to this [article] by @rkoutnik about interviewing candidates on [Hackernews].

Reading it, I found myself agreeing with most of the ideas, especially
the part about candidates that were all talk and no action struck a nerve.

But I was surprised that an hour was deemed not nearly enough to
implement basic Minesweeper. I thought that it should be possible to implement
the basics of gameplay in that time if I had all the required assets.

The [jsbin] contained a convenient JS object with some imgur links and
as a first step I wrote some code to render a table full of img elements,
just so I could have a look at the images. Seeing the images after just a
minute or so seemed to confirm that it should be easy enough and I continued
noodling around with the jsbin for a while.

I was interrupted a few times and did not track the time exactly, that it took
me to get to the "1h" version, but it can't have been more than
about an hour and 10 minutes.

My implementation went like this:

1. create some code to render the board as a table full of TR, TD and IMG nodes
2. added a single style that made the img elements 32px wide
3. create an array of cells, using numbers as bitfields to hold cell states
4. generate some random bombs and note the number of neighbors in the cells array
5. display the generated data (no clicking and no hiding yet)
6. check the generated number of neighbors (in the first version I wrapped around on the left and right boundary because my cells array is one-dimensional, this problem was fixed using the "cell" function)
7. verify the generated data by looking at a few examples (the pattern was generated on load, browser reload was used to generate a new state)
8. change cell rendering to initially not show anything
9. attach a click handler to the table (so only one addEventListener was necessary)
10. handle the click by changing the state to "clicked" (or the cell value with 512, which is a single set bit since it's 2^9, I had decided on this representation earlier and did not find it hard to use)
11. update the cell using replaceChild (see note below), refactoring the actual cell-rendering code out of the renderView function
12. write the flood fill for the case where an empty cell was hit by the click, I used setTimeout because I wanted the fill to be animated
13. at this point I changed from jsbin to a local file in a Atom because the debugger started to annoy me while I edited code, every time the code became invalid the debugger stopped because I had turned on "stop on exception" and syntax errors in the error could cause uncaught exceptions
14. after a few corrected typos and type-conversion problems the code worked okay (I had to remember to make sure my array indices were turned into numbers when I read from node attributes -- I was lazy, I simply multiplied by 1; since I wanted to finish as quickly as possible I used the simplest coding strategies that I knew off the top of my head, and I wanted to avoid having to look up Number.parseInt vs parseInt or whatever that function is called)

Looking at the clock I noticed that the hour was certainly gone (I think I was about 10 minutes over time) but the game was already playable. I noticed that the flood-fill of empty cells often made it very easy to solve the game and identified two main reasons:

1. the probability for placing a bomb at some location was about 5% at first, which was way too low, and
2. the flood fill would continue if two cells with neighboring bombs sat diagonally from each other

I changed point 1 in the 1h version, but point 2 I addressed later by considering only the four non-diagonal neighbors in my flood-fill algorithm. I think this is different from the original minesweeper, but I did not have Windows at hand and did not check.

Much later I looked at the solution by [mishoo] and I was surprised that a
solution using CSS style classes did not occur to me -- it should have been
even less work to simply add and remove css style classes based on the cells
array contents. I think it was caused by "priming" -- simply by being given
a Javascript object with img URLs in it, I was pushed towards a solution
that references the images using the URLs instead of using CSS classes. (Part
of the surprise was caused by the fact that in a previous job I had been
the person to push people towards a CSS class-based solution.)


Note: I did have to look up the order of arguments for replaceChild when
my first guess turned out wrong and my second guess (although correct,
there aren't that many permutations) did not work because I had gone
one parentNode too far.





[article]: http://rkoutnik.com/articles/How-I-Interview.html
[jsbin]: http://jsbin.com/jucesisaki/2/edit?html,js,output
[vanilla JS]: http://vanilla-js.com
[Hackernews]: https://news.ycombinator.com/item?id=10752564
[github.io]: http://hn3000.github.io/minesweep/
