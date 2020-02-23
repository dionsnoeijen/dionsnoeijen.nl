---
layout: post
title:  "Hello switch"
date:   2020-02-23 14:14:14 +0200
categories: concept
comments: true
author: "Dion"
---

<div class="larger">
So, what is this switch all about. Let's start with an example about a bakery that wants to make cookies.
</div>

![Cookies](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-switch/cookie-factory.png "Cookies")

If you look at the cookie bakery **(1)**. It needs things to be able to bake a cookie and give it to the cookie monster. So it's being provided by all the providers that have the individual product. It also needs batter, which is produced by a machine. The machine needs ingredients as well.

The cookie bakery is happy and it can produce cookies without any problem. The cookie monster is happy. But the cookie bakery is getting smarter. They want to get the ingredients from a supplier that knows the bakeries needs. Luckily, there is one!

![Cookies](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-switch/cookie-factory-with-supplier.png "Cookies")

The supplier **(1)** has all kinds of products, in fact, it has everything the bakery needs. Also things the bakery does not need, but that doesn't matter. The bakery has a special gate for the large pallet of ingredients coming from the supplier **(2)**.

Also, the color coding is letting you know what to expect from the connections.

- **White** row. Normal output, directly what's in the row. It cannot connect to the switch.
- **Blue** row. Gives produced output (The batter machine takes all ingredients and outputs batter). Blue rows cannot connect to the switch.
- **Green** row. Successful output. Can connect to the switch, driving it's enabled state and passes on what the cookie bakery is needing.
- **Red** row. Unsuccessful output. Some ingredient was not present maybe.
- **Yellow** row. This row's value is driven by input from the outside.

Lets have a closer look at the supplier by selecting it.

![Cookies](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-switch/supplier-with-menu.png "Cookies")

On selection of the supplier **(1)**, we get a closer look at it's internals. It shows all that it can supply. It has a lot of ingredients in stock. You can also choose from different types of ingredients. **(2)** For example salt, it can be changed to salt from the himalayas too. Or oil, change it to sunflower oil or olive oil.

There are also produced outputs **(4)**. Based on the ingredients that are selected it may be able to generate a produced output. After all, the baking supplier has it's own batter machine. The checked circle in the menu behind the "batter" bar indicates that we are requiring the supplier to be able to provide batter. So once it can create batter with the ingredients, it continues with the green path.

Looking at the menu, you see the rows of ingredients it can have. The opened lock means you can change it's contents from the menu. **(3)** Closing the eye would make it disappear from the node, just to keep the node small.

It means we could just as well disable all the rows because we are only needing the green and the red row.

![Cookies](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-switch/clean-baking-supplies.png "Cookies")


### Flow

Lets have a look at how the flow is determined.

![Cookies](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-switch/looking-at-flow.png "Cookies")

The baking supplies node is enabled, you can see that because of the green switch. The baking supplier has everything to create it's batter as well. Since we required the node to be able to create batter, it will influence the way it will continue the flow. It means it will choose to continue the flow with the green path. The geen row is connected to the switch input of the bakery, and the bakery is happily supplied with everything it needs to bake it's cookies. Therefore it will continue the flow with the green path as well. The flow reaches the cookie monster, resulting in a happy cookie monster.

##### Let's make the cookie monster unhappy

It's simple. For example, we can just make the supplier devoid of a required ingredient for batter.

![Cookies](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-switch/unhappy-supplier-flow.png "Cookies")

Select the supplier node to bring out the menu again **(1)**. Use the `multiselect` to set it to have no value for flour.

Now the supplier **(2)** can't produce flour, but we told it that it should be able to produce flour so the flow will continue with the red row. It will never reach the bakery, **(4)** but it's no problem. The baking supplier is happy to report the sad news to the cookie monster **(3)**.

##### Can the bakery make the cookie monster unhappy?

Definitely. Let's change a setting.

![Cookies](https://dionsnoeijen.s3-eu-west-1.amazonaws.com/nodes/hello-switch/unhappy-bakery-flow.png "Cookies")

If we select the supplier again, to bring up the supplier node menu again and disable the checkbox behind the `batter` output and we leave the flour `multiselect` to have nothing selected. The output of the supplier will be through the green row. But now the bakery cannot bake a cookie, thus it will continue on the red row **(2)** leaving the bakery to have to inform the cookie monster **(3)**.

In a real situation the flow is dependant on the request, therefore the flow can only been made visible by mock request.

There is a lot more to explore about the switch button. It will be explored later on. Thanks for reading!

{% include concept-footnote.html %}
