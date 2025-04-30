# Traveling Salesperson Problem -- Local Search

This exercise is about the Traveling Salesperson Problem I mentioned in the
lecture on NP-hard problems -- given a set of cities, determine the length of
the shortest tour that visits all of them. We can get from any city to any other
city, i.e. the graph of cities is completely connected. We consider the version
of the Traveling Salesperson Problem that finds the shortest tour to visit $n$
cities, starting at a city and ending at the $n$ th city; it *does not* go
back to the start. The start city may be any of the cities. Remember that the
graph for a TSP is undirected, i.e. the cost is the same in either direction.

The 2-opt algorithm for solving the Traveling Salesperson Problem is a
randomized local search algorithm that, at each iteration, reverses part of the
route. It starts with a random route (this is the randomized part), and changes
part of the route in each step (this is the local search part, sprinkled with
more randomness). The pseudocode for one iteration is as follows:

```javascript
2optSwap(route, i, k)
  cities 1 to i-1 stay in the order they are
  cities i to k are reversed
  cities k + 1 to n stay in the order they are
```

For example, if I call the above function with route A--B--C--D--E--F, $i=2$,
$k=4$, the resulting route is A--B--E--D--C--F.

The algorithm starts with a random route; if the new route at the end of an
iteration decreases the total length, it is retained as the current incumbent.
The incumbent after the final iteration is returned as the solution.

Implement the 2-opt algorithm, which repeatedly runs the above steps. Your
implementation needs to fix two design parameters that I have left open. First,
you need to design a stopping criterion -- when would it make sense to stop and
return the shortest route found so far rather than trying another iteration?
Second, design a way to choose $i$ and $k$ -- note that they need to be
different in subsequent iterations, as one iteration would simply undo what
the previous one did otherwise. Start with the template I provided in `code.js`.
Describe in your code how you designed your stopping criterion and ways of
choosing $i$ and $k$ and why.

The function takes a distance matrix (the adjacency matrix for the graph where
the values in the cells are the distances between the corresponding cities) and
returns the length of the shortest tour (not the tour itself).

Test your new function; I've provided some basic testing code in `code.test.js`.

## Runtime Analysis

What is the worst-case asymptotic time complexity of your implementation? What
is the worst-case asymptotic memory complexity? Add your answer, including your
reasoning, to this markdown file.

The algorithm must go through the local search function at least once all the 
way through, which has two for loops, both from roughly 0 to $|V|$. Since the twoOptSwap 
function is only called once per local search call when an improvement is found, and only 
has to iterate from 0 to n, that won't impact the complexity. This means that the complexity 
of the local search function is $\Theta(|V|^2)$.

However, this function is called repeatedly in a while loop until there are no more 
improvements that can be made. This makes this very difficult to analyze. This has 
a complexity of $\Theta(x \cdot |V|^2)$, where $x$ is the number of local minimums 
that can be found with the route and distance matrix. Getting a concrete complexity 
for $x$ is very difficult. It is very easy to give an upper bound of $O(|V|!)$ because 
it is impossible to make more improvements after all permutations have been processed. 
That said, not all permutations will probably be checked, as a local minimum should 
be found well before this happens. 

Beyond this, I didn't know where to start. This implementation is fairly optimized. It 
would be easier to build a local search that ran in a complexity of $\Theta(|V|^3)$, 
and would be much easy to analyze. This would come at the cost of precision, however. 
For this implementation, the best thing I felt like I could do was do research on it. As 
it turns out, it seems that this problem has given many scholars troubles as well. "Worst 
Case and Probabilistic Analysis of the 2-Opt Algorithm for the TSP" states that, "2-Opt 
can take an exponential number of steps before it finds a locally optimal solution." If 
this is the case, then the asymptotic complexity would be $\Theta(|V|^2 2^{|V|})$, which 
would make this about as slow as held-karp in the absolute worst case. 

The memory complexity is much simpler. The only variable with non-constant memory is the route, 
which has a memory complexity of $\Theta(|V|)$. No copies are ever made, so the memory complexity 
doesn't really change once the algorithm gets going, rather, that same route is just updated. 

## Extra Help

I didn't really understand what local search was, so I used https://en.wikipedia.org/wiki/2-opt 
to give me a better idea of what I needed to do. 

Additionally, because I wanted to make sure that I was actually shuffling the path, I used 
https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle to get the Fisher-Yates shuffle 
algorithm. 

I had a lot of trouble undersntading the time complexity, mainly because of the while loop. 
I did some research to help me out. The source I used used is: 
https://link.springer.com/article/10.1007/s00453-013-9801-4

"I certify that I have listed all sources used to complete this exercise, including the use of any Large Language Models. 
All of the work is my own, except where stated otherwise. I am aware that plagiarism carries severe penalties and that 
if plagiarism is suspected, charges may be filed against me without prior notice." 
