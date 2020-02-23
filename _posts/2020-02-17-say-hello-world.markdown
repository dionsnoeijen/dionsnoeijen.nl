---
layout: post
title:  "Say hello world"
date:   2020-02-17 09:00:00 +0200
categories: concept
comments: true
author: "Dion"
---

<div class="larger">
Finally, I'm ready to talk about some of the real stuff. I really need to know how the ui is going to work. In detail! So I took my design application and think of things I would like to make. How would it work if it was a real application? I will write and think as if it's user documentation. As for all software, I will start with the hello world example.
</div>

First I need to explain some of the user interface basics. As for now, it's divided into a couple of regions.

![Base Layout](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/base-layout.png "base layout")

- A. A horizontal menu bar.
- B. A vertical menu bar, containing a tree view.
- C. Where the node interaction takes place.
- D. Clicking a node will present a menu there.
- E. A footer, containing contextual information.

### Hello World

Lets get started with hello world! With this example, there are already some useful things to be said. This is the flow that responds to a route. We have configured a route that responds to `/hello`, something like this: `https://somedomain.com/hello`.

![Hello World](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/hello-world.png "Hello World")

<span class="sub">So, as you may have guessed, this is displayed in  the C region.</span>

This will always leave you with at least two nodes. This is the request and response node **(1, 3)**.

We like to have a response that contains a json body with a `key` named `text` that contains the string `Hello world!` like this:

```json
{ "text": "Hello world!" }
```

This will require is to add a string variable node to the flow **(2)**.

<div class="row">
    <div class="left">
<img src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/text-var-node-step-1.png" />
    </div>
    <div class="right">
<img  src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/base-layout-c-region-highlight.png" />
    </div>
</div>

The nodes are added to the **C** region of the application.

Selecting the newly added node will have a form appear in the **D** region of the application.

<div class="row">
    <div class="left">
<img src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/text-var-node-selected.png" />
</div>
<div class="right">
<img src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/base-layout-d-region-highlight.png" />
</div>
</div>

This form. allows extra options for this node to be enabled. First of all, give it a text value: "Hello world!". Now enable the `key` checkbox and give the key a value of `text`.

<img style="display:block; margin: 0 auto;" src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/text-var-node-step-2.png" width="50%" />

This is number **2** in the hello world example.

As you can see, there is only one connection between the output of the text node and the body of the response node. **(3)**

This will produce the hello world response where the key name is determined by the nodes key variable and the text by the text output.

There are still more things that could be said about this example. Like what the switch buttons do or why there is no connection needed between the request and text variable node. But this will become apparent in the next examples.

### Hello you!

Let's make the example a little more interesting by adding a segment variable to the request url.

![Hello World](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/hello-you.png "Hello World")

#### 1. Change the route for the request node

By adding a segment variable to the request route we can exchange the word "world" by whatever that was added to this segment.

Selecting the request node **(1)** in the **C** region will open up a request node form in the **D** region of the application.

<img style="display:block; margin: 0 auto;" src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/base-layout-d-region-highlight.png" width="50%" />

Through this form you can configure the rout to accept a segment variable. This will look like this: `/hello/{name}`. This will add a row to the request node with an output connector on it.

#### 2. Change the text var node to accept a variable

We also need to select the text node again and make a change to the "Hello world!" text. Change it to: "Hello {name}!". This will add a row to the text node with an input connector. After making those changes the text node should look like this:

<img style="display:block; margin: 0 auto;" src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/text-var-node-step-3.png" width="50%" />

#### 3. Add connection

Now we can add a connection between the request node **(1)** and the text node **(2)**. Note that the connection is drawn between the two newly introduced rows that take care of the `{name}`.

<hr />

Now, when making a request like this: `https://somedomain.com/hello/Bert` will result in a json response like this one:

```json
{ "text": "Hello Bert!" }
```

You may have noticed that the connection from the text node **(2)** to the response node **(3)** has changed as the input is connected to the switch button of the response node. This changes the switch button position to the middle and changes the color to yellow, indicating it's driven by an outside factor which is the text node.

In this particular example, this change is unnecessary as it will produce the exact same output. It is worth noticing that the body row of the response node is still yellow, indicating that it's driven by an outside factor.

The same goes for the connection between the request node **(1)** and the text node **(2)**. If we were to make the connection between the green row on the request node **(1)** with the word `GET` and the switch button of the text node **(2)** the result of the output would be exactly the same.

For the next examples of the hello world document this is all that is relevant. If you want to dive more deeply into the subject of the switch button and it's connections between colored rows here: "SOME LINK".

### Hello you! In a random language

Let's make it more interesting by returning the word "Hello" in a random language.

![Hello World](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/random-language-hello.png "Hello World")

#### 1. Add a new `action node`.

This is a `random` node. It will output true for one of it's output connections.

#### 2. Add two more text nodes.

And make sure each one has the word "hello" in it's own language. Let's add one for French and another one for Spanish. So now we have a node for `Hello {name}!`, `Bonjour {name}!` and `Hola {name}!`. Also make sure each of those nodes have the `key` set to the value `text`.

#### 3. Connect the nodes

From the request node, connect the output of the row with the segment variable `{name}` to the input rows of the text nodes with the row for the variable `{name}` as we did before.

Now, connect the output connector of the `random` action node **(1)** to the switch buttons of the three text nodes **(2)**.

And last, connect the output of the text nodes to the body row of the response node.

Here also goes that the output row of the text nodes can also be connected to the switch of the response node. If you do so, make sure it's all three on the switch, or all three on the body row. For more information on the switch: "SOME LINK".

<hr />

Calling this setup with: `https://somedomain.com/hello/Bert` will result in one of the following json output!

```json
{ "text": "Hello Bert!" }
```

```json
{ "text": "Bonjour Bert!" }
```

```json
{ "text": "Hola Bert!" }
```


#### Enhancement by setting the key with a variable

As you can see, the keys of the text nodes are being set by hand for every node. In a real application this can easily lead to mistakes. It would be better to control the name of the key from a single point. This can be easily solved by using the input connector on the key row like this:

![Hello World](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/random-language-hello-var-key.png "Hello World")

Simply add a text node **(1)** and make sure it's value is `text`. Now connect the output of the text row to the key inputs **(2)** of the hello output text nodes.

Now the key name is driven by this text node.

### The power of groups

As you can see with the random language example, the amout of connections can build up quickly. This can easily lead to clutter, obscuring clarity.

To solve this issue, nodes can be grouped. This way you can introduce your own nodes that handle logic inside.

![Hello World](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/random-grouped.png "Hello World")

By selecting a set of nodes with the rectangular selection box and pressing the group button.  A rectangle will appear around the selected nodes.

The group button is located in the **A** region of the application and is only active whenever there is more then one node selected.

<img style="display:block; margin: 0 auto;" src="https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/base-layout-a-region-highlight.png" width="50%" />

You can give the group a custom name by changing the text in the top left corner of the group rectangle **(1)**. Let's name it "Random hello".

Whenever the group is in an opened state, every node added is automatically added to the group. We need to change the text node setup as displayed in the image.

As you can see, there is a node on the left **(2)** that is connected to the left outside of the group rectangle. It's output is connected to the three hello text nodes. This node is the node that passes on the `{name}` from the request node to one of the hello text nodes. The active hello text node. Which one is active is determined by the random node.

As you may notice, the key row is not present anymore in the hello text nodes. This is moved to the text node on the right side of the group **(3)**. This will be the node that contains one of the thee selected texts and it will act as an intermediate node so we can expose it's output to the outside of the group by dragging the node output to the right side edge of the group.

Collapsing the group, by pressing the arrow button in the right top corner of the group rectangle will result in the group being displayed as a node like this:

![Hello World](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-world/closed-group.png "Hello World")

As you can see, the collapsed group **(1)** has one row. Where the left side represents the connection made inside the group to the left side of the group rectangle. And the right side the connection made to the right side of the group rectangle.

Now we can connect the request node to the group and the group to the response node as displayed in the image. This will leave us with the same functionality as we had in the random hello response example but in a much less cluttered way. Grouping is a very important feature to help reduce the spaghetti problem.

### Conclusion

This document has shown the most basic functionality of the node system. But there's a lot more required to make this into the powerhouse it's supposed to be. Next week there will be more.

{% include concept-footnote.html %}
