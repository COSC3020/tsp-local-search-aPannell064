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

The algorithm has to go through the local search function at least once all the 
way through, which has two for loops, both from roughly 0 to $|V|$. Since the twoOptSwap 
function is only called once per local search call and only has to iterate from 
0 to n, that won't impact the complexity. However, this happens for as long as 
there is improvement. In the worst case, this would happen on every possible 
set of swaps that can be made, or basically however many ways we can chose two 
elements from $|V|$ to swap. This is equivelant to $\chose{|V|}{2} = \frac{|V|!}{2! \cdot (|V|-2)!}$.
We can reduce this to $\frac{|V|(|V|-1)}{2}$, which is asymptotically the same as 
$|V|^2$. Therefore the asymptotic compolexity is going to be $\Theta(|V|^4)$ for the 
worst case.

The memory complexity is very easy. The largest element in the entire algorithm 
is the distance matrix, which has a complexity of $|V|^2$. No copies are every made, 
so the memory complexity doesn't really change once the algorithm gets going. Therefore, 
the asymptotic memory complexity is $\Theta(|V|^2)$