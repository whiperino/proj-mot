## Proj-Motion and Code Dissection

Documenting how to modify the code for project members. None of this is imporant for the grading to feel free to ignore this.

First up, grab a code IDE (Integrated Dev. Environment) that's worth its salt. Jupyter is fine, and no ill will to those who use the browser program, but it's Ju(py)ter, and if you want to code in any other language you'll have to use something else.

VSC (Visual Studio Code) is my gold standard, with VS (Visual Studio) being my specific IDE if you're writing anything in C#. But as far as I know, you'll only be needing VSC for this class. 

And, of course, we need to install python itself. Grab it from <a href="https://python.org/downloads">python.org/downloads<a>. Or just click there.

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024185949598535730/unknown.png" alt="python download" width="500"/>

<hr>
<h2>Extensions</h2>

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024169561915920464/unknown.png" alt="extensions to install" width="500"/>

Just grab Python and the rest auto install. I wont detail them all, but they'll cover auto fill suggestions, pointing out errors, and I'm pretty sure one of these makes it a static typed language in VSC.


<hr>
<h2>The Actual Code</h2>

Dump the .py (or .ipynb) into your app window, it'll open up, then go on and scrawl it for a second
<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024175933692854292/unknown.png" alt="code" width="400"/>


Look frightening? Don't worry, we only care about these 3 variables

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024176279181865000/unknown.png" alt="code" width="400"/>

Creating these arrays is pretty easy. Let's break it down. First off, we want to make an array of our angles, our average heights, and... yerr values?
First off, I don't know what "yerr" is, so I'll have to check out what yerr could mean.
A short investigation leads me to the line `c,b,a=np.polynomial.polynomial.polyfit(x,y,2,w=dy)`

I scroll up to the imports, find out `np` is the prefix for `numpy`'s library, and search `numpy.polynomial.polynomial.polyfit` to see what that code does.
Looking at the documentation, we see `w=` sets the weighting of the graph, likely for measurement variation! 
Now we know what `yerr_values` means, and we can fill out the arrays

<h2>Creating Variables</h2>

<h3> Theta/Angle </h3>
You /could/ just put them in as normal. ie 29, 32, 35-- BLEUGH. It's slow. To me. Maybe you like putting them in one by one. And you know what? It's more legible in some cases. However we can actually input equations to save effort. Python takes it the same. We could do 35-6, 35-3, 35 and it would be equally valid.

But maybe I'm getting ahead of myself. If you're wondering what an array is, it's a very common concept and one we'll probably use the rest of the year. It's a list of values-- strings, floats, integers, whatever. You know a variable is an array if it uses parantheses, not braces or brackets. Let's see this in action.

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024180908498092062/unknown.png" alt="code" width="400"/>

Oh, and make sure to go from low to high. This isn't specifically a python thing, but I'm sure inversing the order would probably mess with the plotting function later on. It might not, but I'd rather not test.

<h3> Y Mean Values </h3>
Again, you could calculate the mean values yourself. But let's not, ok? I wont even make an example with putting it manually like with Theta because it's too much.
Lets make python do it. This is purely a bonus excersize, as you can do it manually, but I'm putting this in to teach extra.

First, we want a function to calculate the mean. I already know the `statistics` library has what we need, so I import mean from statistics with `from statistics import mean`. We could write just `import statistics` and that'd give us everything in there, but let's learn how to be specific.

Then, we assign various variables to each mean.

Lastly, we push those inside the Y array just like we did with Theta. __BE SURE TO MATCH ANGLES AND THE Y VALUES, OTHERWISE THE COORDINATES WILL BE OFF.__

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024183188999917618/unknown.png" alt="code" width="400"/>

<h3>Yerr/Variance</h3>

Last, we just have to fill out the uncertainty-- or what I'm ASSUMING is the uncertainty. Despite these all being the same values, don't just put them in once. You need to put it in the full 5 times.

We know the uncertainty is .1 cm, as the smallest tick marks were... well, .1 cm or 1 milimeter. To keep consistant, we have to put in .1 since the heights were recorded as cm too.

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024185427156008960/unknown.png" alt="code" width="400"/>

<h2>Testing, and why runtimes hurt</h2>

So we have our code, and we still need to test it to see the plot. Go to Jupyter and open a python notebook, or click <a href="https://jupyter.org/try-jupyter/retro/notebooks/?path=notebooks/Intro.ipynb">here<a>

Aaaaand... oh. Error.

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024190984302637056/unknown.png" alt="runtime error" width="400"/>

Specifically a "runtime" error, one not caught by the debugger and only now seen during-- well, run time. We see the issue is with the function `mean`, and that there are 5 arguments given, when it should be only 1. And that's what we did! Or so we thought. 
`Mean` didn't work because it takes 1 argument, such as `Mean(value list)`, but what we actually gave it was `Mean(value, another, and another, and another value)`, which doesn't work. So what we have to do is put the list in a list. 

In short, put those values in another group of parentheses. 

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024191869208510514/unknown.png" alt="runtime error" width="400"/>

Wham! Let's try again.

Voila! We get a graph this time. But no line of best fit?
Alas, an error persists.

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024198586252144660/unknown.png" alt="runtime error" width="400"/>

This one is tricker, and took a couple minutes to re-read the documentation for `matplotlib.pyplot`, until I found it needs a /specific/ type of array. `np.array()` is what we're looking for. Put that around our x, which is Theta, which is our theta array. You'll have this

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024199271978901554/unknown.png" alt="fixed theta array" width="400"/>

And run again...

<hr></hr>
<h3> Final Graph </h3>
We now have a completed python script, giving us this pretty scatter plot with a line of best fit.

<img src="https://cdn.discordapp.com/attachments/872160747306754058/1024199642424021072/unknown.png" alt="graph" width="400"/>

For any further questions, I'm always best reached via discord. proj-mot
