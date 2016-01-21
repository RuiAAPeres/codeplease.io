>With MVVM, each cell will have its own ViewModel from where it will get the necessary data it needs.

That's pretty sweet, until you have to dequeue a cell:

<script src="https://gist.github.com/RuiAAPeres/d49ec71b53873d9b43b6.js"></script>

So, what's the identifier? 

> **The ViewModel shouldn't be aware of the View**.

And the reason why is: **if at any point the View is replaced, the ViewModel should be able to exist without any change whatsoever**. But on the other hand the View creation (`UITableViewCell`/`UICollectionViewCell`) is based on the ViewModel, so what now?  

Something we have been using is:

<script src="https://gist.github.com/RuiAAPeres/5ff2be0fdf7c90b2e696.js"></script>

Which will make the `table:cellForRowAtIndexPath:` pretty straightforward:

<script src="https://gist.github.com/RuiAAPeres/b9f65c24ad2ca2b3724d.js"></script>

So depending on the ViewModel you will have a corresponding Cell[^n]. 

This approaches separates the ViewModel's core from the View via an extension. I am still looking for other approaches for this problem, but right now it has been working quite well. 


[^n]: I might try [this approach](https://alisoftware.github.io/swift/generics/2016/01/06/generic-tableviewcells/) adapted to this use case.