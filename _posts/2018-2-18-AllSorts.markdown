---
layout: post
title:  "It Doesn't Take All Sorts"
date: 2018-2-18 00:00:00 -0600
categories: [Algorithms, Sorts]
---

>Computer Science, actually that's a terrible way to start, first of all its not a science,
> it might be engineering or it might be art. Its also not very much about computers -Harold Abelson

For some reason or another, a common starting place for computer science education is sorting algorithms.
Everyone has to learn them, but before getting to the [fun](https://en.wikipedia.org/wiki/Sorting_algorithm#Efficient_sorts) sorts, it is common to teach the $$O(n^2)$$ ones.
The Three being Insertion Sort,
```c++
for( int i = 1; i < arr.size(); i++ )
{
   j = i;
   while( j > 0 )
   {
      if( arr[j] > arr[j-1] )
      {
         swap(arr[k], arr[k-1]);
      }
      j--;
   }
}
``` 
Selection Sort, 
```c++
for( int i = 1; i < arr.size(); i++ )
{
   int mindex = i;

   for( int j = i+1; j < arr.size(); j++ )
   {
      if( arr[j] > arr[mindex] )
      {
         mindex = j
      }
   }

   if( mindex != i )
   {
      swap(arr[k], arr[k-1]);
   }
}
```
and Bubble sort.
```c++
for( int i = 0; i < arr.size(); i++ )
{
   for( int j = i; j < arr.size(); i++ )
      if( arr[j] > arr[j-1] )
      {
         swap(arr[k], arr[k-1]);
      }
}
```

But what if I told you there were not three diffrent sorts here, but instead a single mechanism with 3 diffrent ways of abstracting it?

Imagine, if you will, a node that takes in two inputs and returns two outputs:

<center>
<img class="centered" style="background-color: gray; border-radius: 25px; padding: 20px" src="{{ "/assets/posts/sorts/node.svg" | prepend: site.baseurl }}" alt="The Sorting Machine">
</center>

with output 1 less than output 2.

We can think of this node as a simple machine that sorts two numbers.
It is also possible to connect these nodes into a network in such a way as to sort any number of numbers.

<center>
<img class="centered" style="background-color: gray; border-radius: 25px; padding: 20px" src="{{ "/assets/posts/sorts/machine.svg" | prepend: site.baseurl }}" alt="The Sorting Machine">
</center>

It is easy to see that this collection of nodes provides a sorted list, since after node $$A$$ has activated the inputs to nodes $$B_0$$ and $$B_1$$ will be a sorted list containing of the first two $$i$$'s.
This will continue until the network outputs the $$o$$ vector.

Now, even though the output of each sorting algorithm is a sorted list, depending on our interpretation of the network we find any one of the individual algorithms.

Take the list of numbers: 54321. Sorting with a Selection sort

$$\begin{array} {|r|r}
543\underline{21} & \\
54\underline{31}2 & \\
5\underline{41}32 & \\
\underline{51}432 & \\
{\bf 1}54\underline{32} & \\
{\bf 1}5\underline{42}3 & \\
{\bf 1}\underline{52}43 & \\
{\bf 12}5\underline{43} & \\
{\bf 12}\underline{53}4 & \\
{\bf 123}\underline{54} & \\
{\bf 12345} & \\
\end{array}$$

We can see similar behaviour from our machine if the nodes are evaluated horizontally with the first smallest subscripts first the network performs a Selection sort.
This is sensible since the selection sort finds the smallest element and places it at the front of the list.
Since our machine outputs a sorted list, the first column $$C_0$$'s output must be the first element, $$C_1$$ will be the second and so on.

$$\begin{array} {|r|r}
{\bf 1}5432 & C_0 \\
{\bf 12}543 & C_1\\
{\bf 123}54 & C_2 \\
{\bf 12345} & C_3\\
\end{array}$$

What if instead of interpreting the layers vertically, we evaluate them horizontally? 
It turns out to be an insertion sort.

$$\begin{array} {|r|r}
\underline{5 4} 3 2 1 & \\
{\bf 4 5} 3 2 1 & A_n\\
4 \underline{5 3} 2 1 & \\
\underline{4 3} 5 2 1 & \\
 {\bf3 4 5} 2 1 & B_n \\
3 4 \underline{5 2} 1 & \\
3 \underline{4 2} 5 1 & \\
\underline{3 2} 4 5 1 & \\
{\bf 2 3 4 5} 1 & C_n \\
2 3 4 \underline{5 1} & \\
2 3 \underline{4 1} 5 & \\
2 \underline{3 1} 4 5 & \\
\underline{2 1} 3 4 5 & \\
{\bf 1 2 3 4 5} & D_n\\
{\bf 1 2 3 4 5} & o_n\\
\end{array}$$

In both of these sorts, the strict manner in which it operates oupon the data is rather obvious, however the storage requires a more imaginative view.
In insertion sort the data is stored in the array inorder starting with the outputs from the row of nodes in order, followed by the $$i$$'s in order.
In Selection the data is arranged with the $$o$$ that have been calculated first, followed by the outputs from the horizontal nodes starting with the smallest $$A,B...etc$$

---

At this point, I hope it is apparent that both selection and insertion sort come from the same abstraction. 
I did promise Bubble sort was also based on this machine.
This is slightly untrue.
In reality the Bubble sort is based on an even worse machine.

<center>
<img class="centered" style="background-color: gray; border-radius: 25px; padding: 20px" src="{{ "/assets/posts/sorts/bubble.svg" | prepend: site.baseurl }}" alt="The Worst Sorting Machine">
</center>

The Bubble sort is an abstraction of this stupid machine. 
If you look closely you can see the first machine within the trappings of this one.
It proceeds column-wise like Selection Sort except each step tends to consist of a single node rather than a column.
There are however some major differences.
The most obvious one is the dark nodes. These function in reverse to the white ones and represent the worthless comparisons at the tail of every loop through the machine.

$$\begin{array} {|r|r}
543\underline{21} & \\
54\underline{31}2 & \\
5\underline{41}32 & \\
\underline{51}432 & \\
{\bf 1}54\underline{32} & \\
{\bf 1}5\underline{42}3 & \\
{\bf 1}\underline{52}43 & \\
\underline{ {\bf 1 2} }543 & a_1\\
{\bf 12}5\underline{43} & \\
{\bf 12}\underline{53}4 & \\
{\bf 1}\underline{ {\bf 23} }54 & a_2\\
\underline{ {\bf 12} } {\bf 3} 54 & b_2\\
{\bf 123}\underline{54} & \\
{\bf 12\underline{34}5} & a_3\\
{\bf 1\underline{23}45} & b_3\\
{\bf \underline{12}345} & c_3\\
{\bf 12345} & \\
\end{array}$$

These dark nodes are simply avoided by the other sorts, and for good reason they don't do anything. They could very well just be eliminated or the connections just severed.

Leaving them in, however introduces an interesting property.
As you may recall waaaaay up at the top of this monothith I mentioned all of these sorts are $$O(n^2)$$ and with these diagrams we can see this intuitively.
With $$5=n$$ inputs, the number of nodes (representing serialized operations) is of size $$ (n-1)^2 $$ for Bubble or $$\frac{(n-1)^2}{2}$$ for Selection and Insertion.
Both of which increase in size more slowly than $$n^2$$ but faster than $$n$$ meaning its Big $$O$$ classification is sensible.


---
Notes:
* A good portion of the material from this article was inspired by this [video](https://www.youtube.com/watch?v=pcJHkWwjNl4). Though I thought the additional material merited a look.
* If you're reading this Akshay, "Nah nah nah nah nah nah, I was right"
