---
layout: default
---
[Back to Home](../index.html)

# GoLang Unexpected Looping Behaviour

To set this up, we are going to be working with a list of objects. 
Each item in this simple case only have a name.
And the list is super large at four items long.
On each of the items in the list we will do some work.
Print the index number of the item in the list and it's name.
So yes, some really complex work.
In the real world scenario the list was ten to a hundred items long and the work took between five and ten minutes to complete.
But was well suited to asynchronous processing, which we will get to.

Let get started by creating the element type that we will be doing the work on in this example.
The simple struct type has only one field "Name".

```
type element struct {
  Name string
}

```

The worker function takes two parameters.
A number which will be the list index and a pointer to the list item.
Printing the values is the work that is done.

```
func worker(index int, item *element){
  fmt.Fprint("\nIndex: %v, Element.Name: %s\n", index, item.Name)
```

With the worker function done lets create the list to do some work on.

```
items := []element{
            {Name: "Zero"}, 
            {Name: One"}, 
            {Name: "Two"}, 
            {Name: "Three"}
            }
```

In the loop we pass in the index and the pointer to the current item in the list.

```
for index, item := range items {
  worker(index, &item)The
}
```

Now we run the super complex program and we see the print out.
Notice that the index matches the element names.

```
index: 0, element.Name: Zero

index: 1, element.Name: One

index: 2, element.Name: Two

index: 3, element.Name: Three
```

Currently we know that the example works and the worker function handles the information as required.
It's now time to run this in parallel using goroutines.
There are a few changes required to the code base to allow the goroutines to work.
Firstly we need to add the `sync.WaitGroup` to keep the program alive will the functions run in the background.
This means changes to our setup, for loop, worker function and the ending of the main function.

```
func main(){
  var wg sync.WaitGroup

  items := []element{
                {Name: "Zero"}, 
                {Name: One"}, 
                {Name: "Two"}, 
                {Name: "Three"}
                }

  for index, item := range items {
    wg.add(1)
    worker(index, &item, &wg)
  }
  wg.wait()
}

function worker(index int, item *element, wg *sync.Wait.Group) {
  defer wg.done()
  fmt.Fprint("\nIndex: %v, Element.Name: %s\n", index, item.Name)
}
```

We can now run the program again to ensure we didn't break something by mistake.
The result should still be the same as the first runs.

It's time to go parallel.
This is what we have been working towards.
Lets make the complex code change.

```
for index, item := range items {
  wg.add(1)
  go worker(index, &item, &wg)
}
```

Yes, that one word `go`, makes it run in parallel.
So lets run it.

```
index: 3, element.Name: Three

index: 0, element.Name: Three

index: 1, element.Name: Three

index: 2, element.Name: Three
```

Ooh, something is wrong.
Close look shows the printed output does not match the expected output.
The index and the item name do not match.
How can this be?

In our worker function we pass in a pointer to the item we wanted to do work on from the list.
But what is actual happening is the `item` variable is what we pass in as a pointer.
Which is updated every time we iterate over the list.
By the time we get to the end of the list all the goroutines are pointing to the same item.

Let's correct this issue, lets not pass the list item into the worker function as a pointer.

```
for index, item := range items {
  wg.add(1)
  go worker(index, item, &wg)
}
```

The worker function signature most also be updated.

```
func worker(index int, item element, wg *sync.WaitGroup) {
```

Now when we run the program we see the correct printed output.
We solved the problem.
Well for this use case we did.

```
index: 0, element.Name: Zero

index: 1, element.Name: One

index: 3, element.Name: Three

index: 2, element.Name: Two
```

What happens if the worker function updates the list item that later we iterate over the list again expecting the new updated value.
How do we solve that problem?
In order to solve this problem we do need the worker function to have a pointer to the list item. 
But as we have seen we can't rely on the for loop item to be the pointer we require.
We must get the pointer form the list using the index.
Below is the full example that will update the list items and later print the updated values.
The updates are do in parallel.

```
package main

import (
  "fmt"
  "sync"
)

type elem struct {
  Name string
}

func main() {
  var wg sync.WaitGroup
  items := []element{
              {Name: "Zero"},
              {Name: "One"},
              {Name: "Two"},
              {Name: "Three"}
              }

  for index := range items {
    wg.Add(1)
    go worker(index, &items[index], &wg)
  }

  wg.Wait()

  fmt.Println("\nPrinting updated items in the list")
  for index, item := range items {
    fmt.Print("\nindex: %v, element.Name: %s\n", index, item.Name)
  }
}

func worker(index int, item *elem, wg *sync.WaitGroup) {
  defer wg.Done()
  fmt.Printf("\nindex: %v, element.Name: %s\n", index, item.Name)
  item.Name = item.Name + " -> Updated"
}
```

The print out of this program shows that we have updated the items in the list.
These items were updated in parallel using goroutines with pointers.

```
index: 3, element.Name: Three

index: 2, element.Name: Two

index: 0, element.Name: Zero

index: 1, element.Name: One

Printing updated items in the list

index: 0, element.Name: Zero -> Updated

index: 1, element.Name: One -> Updated

index: 2, element.Name: Two -> Updated

index: 3, element.Name: Three -> Updated

```

As a take away doing asynchronous work with pointers can cause some very unexpected behaviors.
Be ready to sink some time into finding the root cause of the issue.
