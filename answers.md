# CMPS 6610 Problem Set 03
## Answers

**Name:** Amin Mirfakhar


---

- **1a.** Use `iterate` to implement the `isearch` stub, and check that your
code passes the test cases given by `test_isearch`.
``` python

def isearch_v2(L, x):
    def f(prev_result, new_element):
        
        a = prev_result[0]
        c = False
        if prev_result[1] == new_element:
            c = True
        return ((a or c), prev_result[1])
        
    r = iterate(f, (False, x), L)
    return r[0]
```
   based on the definition at each iteration an array of a boolean with the key value (False, x) plus the new element to check whould pass through the function f. In the f function if key value and new value matches then it return True else it would be or function between prev results boolean and current one.

- **1b.** What is the work and span of this algorithm?
  
Since this function iterates sequentially over all elements of the list to check whether they are equal to the key value, it is clear that both the work and the span are **O(n)**.


- **1c.** Now, use `reduce` to implement the `rsearch` stub. Test it with `test_rsearch`.

``` python

def rsearch(L, x):
    def f(a, b, kv = x):
        if a == kv:
            return a
        elif b == kv:
            return b
        else:
            return 0
        
    r = reduce(f, 1, L)
    return r == x

```

- **1d.** What is the work and span of the resulting algorithm, assuming that `reduce` is implemented as specified in the lecture notes?

since we have to check the condition for every element of agian so the work would not change. we can also show this as $W(n) = 2W(n/2) + O(1)$ because we devide each list into two part and process them recursively. So work = $O(n)$.
for span, we can process those two recursion in parallel then $S(n) = S(n/2) + O(1)$. We saw this function ($S(n)$) multiple time and know it is and a balance with order of $O(logn)$.


- **1e.** Finally, let's consider another implementation of `reduce` as given
by `ureduce` in `main.py`. That is, if you replace `reduce` from part b) with
`ureduce` then there should be no difference in output. However, what
is the work and span of the resulting algorithm for `rsearch`?

In this case we divide or list into two part one with the size 1/3 of input (n) and the other 2/3 input, it is clear that the work would not change because we have to check each element agian. but for the span this time our tree is not balanced any more and at each layer it split the previous part into a 1/3 and 2/3 of it's size. so we can write $S(n) = max(S(n/3), S(2n/3)) + O(1) = S(2n/3) + O(1)$ and the longest run is equal to $log_{3/2}(n)$ with the o(1) work at each node, then we can say $S(n) \in log n $.

---

- **2a.** List deduplication** Suppose you are given a list $A$ of $n$ unsorted
elements with duplicates. Design an algorithm and provide a SPARC specification for a function `dedup` that
takes $A$ as an argument and returns the distinct elements of $A$
(preserving order). Analyze the work and span of your algorithm.


```
f(List, element):
    if element in List: (funcitons like isearch in first part)
        return List
    else:
        return List + element

dedup(A):
    iterate(f, [], A)
```

based on the definition of funciton dedup we have to iterate over all elements in List A (preserving order) and then check if that element is in the alraedy seen elements or not. in the worst case all the elements are unique then we are doing the most work at the last when we have to check the list of size n-1. so for i in range(len(A)) we have to check lists of size 0 to n-1 so it would be $W(n) = \sum_{i = 0} ^{n-1} O(i) \in O(n^2)$. since we have to do all these sequentially so span should be equal to work. if we just consider one element at the moment W(n) = S(n) = O(n) or the search could be done in parall (for example S(n) seach = log(n)) then S(n) = O(n log n).


- **2b.** Deduplication in a network** Imagine now that we have a
collection of lists $A_0, \ldots, A_m$, where each list has $n$
elements. In the distributed setting all we care about is identifying the unique
elements, without regard to the order in which they appear in the
input lists. Design an algorithm and provide a SPARC specification for a function `multi-dedup` that
takes $A$ as an argument and returns the distinct elements of $A$
(preserving order). Analyze the work and span of your algorithm and
compare it to the work and span from part a) above.

one way to solve this could be just flatten the inputs and consider it as a single input list:

```
f(List, element):
    if element in List: (funcitons like isearch in first part)
        return List
    else:
        return List + element

multi-dedup(A):
    iterate(f, [], flatten(A = [A_0, ..., A_m])
```

in the worst case the result is just like part a if we consider |flatted_A| = n then ( $W(n) \in O(n^2), $S(n) \in O(n log n)$ )


- **2c.** Sequence operations** Are any of our sequence operations useful
for either of these problem settings? If so, which operations are useful and
why? If not, why do they not help us?

we can use iterate for the fist one and based on the algorithm on the second part flatten, iterate, reduce could be usefull.

---

- **3a.** iterative solution** Implement `parens_match_iterative`, a solution to this problem using the `iterate` function. **Hint**: consider using a single counter variable to keep track of whether there are more open or closed parentheses. How can you update this value while iterating from left to right through the input? What must be true of this value at each step for the parentheses to be matched? To complete this, complete the `parens_update` function and the `parens_match_iterative` function. The `parens_update` function will be called in combination with `iterate` inside `parens_match_iterative`. Test your implementation with `test_parens_match_iterative`.


``` python
def parens_update(current_output:int , next_input):

    if current_output == -1:  return current_output
    elif next_input == "(": return current_output + 1
    elif next_input == ")": return current_output - 1
    else: return current_output

```

this function get a element as next_input to check if it is "(", ")" (an opened or closed parenthese. if it is opened it add 1 to the current_output and if it is closed it substract 1 from that element otherwise it would be onchanged. but, there is case just like 
``` python parens_match_scan(['(', 'a', ')', ')', '(']) ``` where the order of closing and opening doesn't match, to solve this we add ``` if current_output == -1:  return current_output ```. since each parentetheses should be opend and closed in order, then we can not have any negative value for the current_output and if it happens this shows the opening and closing is out of the order some where.


- **3b.** What are the recurrences and corresponding asymptotic
  expressions for the work and span of this solution?
  

- **3c.** Using `scan`** Implement `parens_match_scan` a solution to this problem using `scan`. **Hint**: We have given you the function `paren_map` which maps `(` to `1`, `)` to `-1` and everything else to `0`. How can you pass this function to `scan` to solve the problem? You may also find the `min_f` function useful here. Implement `parens_match_scan` and test with `test_parens_match_scan`


- **3d.** Assume that any `map`s are done in parallel, and that we use
the most efficient implementation of `scan` (that uses contraction) from class. What are the recurrences for the work and pan of this solution? 



- **3e.** A Divide-and-Conquer Solution** Implement
  `parens_match_dc_helper`, a divide and conquer solution to the
  problem. A key observation is that we *cannot* simply solve each
  subproblem using the above solutions and combine the results. E.g.,
  consider '((()))', which would be split into '(((' and ')))',
  neither of which is matched. Yet, the whole input is
  matched. Instead, we'll have to keep track of two numbers: the
  number of unmatched right parentheses (R), and the number of
  unmatched left parentheses (L). `parens_match_dc_helper` returns a
  tuple (R,L). So, if the input is just '(', then
  `parens_match_dc_helper` returns (0,1), indicating that there is 1
  unmatched left parens and 0 unmatched right parens. Analogously, if
  the input is just ')', then the result should be (1,0). The main
  difficulty is deciding how to merge the returned values for the two
  recursive calls. That is, if (i,j) is the result for the left half of the list, and (k,l) is the output of the right half of the list, how can we compute the proper return value (R,L) using only i,j,k,l? Try a few example inputs to guide your solution, then test with `test_parens_match_dc_helper`.




- **3f.** Assuming any recursive calls are done in parallel, what are
  the recurrences and corresponding asymptotic expressions for the work and span of this solution?

