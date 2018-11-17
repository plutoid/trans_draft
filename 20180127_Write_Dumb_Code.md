Write Dumb Code

写直白的代码

======

The best way you can contribute to an open source project is to remove lines of code from it。 We should endeavor to write code that a novice programmer can easily understand without explanation or that a maintainer can understand without significant time

为开源项目作贡献最好的方式是为它减少代码，我们应致力于写这样的代码， 它让新手程序员无需注释就能容易的理解，让维护者也无需花费太多精力就能着手维护。

As students we attempt increasingly challenging problems with increasingly sophisticated technologies。 We first learn loops， then functions， then classes， etc。。 We are praised as we ascend this hierarchy， writing longer programs with more advanced technology。 We learn that experienced programmers use monads while new programmers use for loops。

作为学生，我们会更多地用复杂巧妙的技术去挑战新遇到的难题。首先我们会学习循环，然后是函数啊，类啊，等等。 当我们到达一定高的程度，能用更高级的技术写更长的程序，我们会因此受到称赞。 此刻我们发现老司机们用 monads 而新手们用循环。

Then we graduate and find a job or open source project to work on with others。 We search for something that we can add， and implement a solution pridefully， using the all the tricks that we learned in school。

之后我们毕业找了工作，或者和他人合作开源项目。我们用在学校里学到的各种炫技寻求并骄傲地给出实现的解决方案。

Ah ha! I can extend this project to do X! And I can use inheritance here! Excellent!

哈哈， 我能扩展这个项目，并实现某牛X功能啦， 我这里能用继承啦， 我太聪明啦！


We implement this feature and feel accomplished， and with good reason。 Programming in real systems is no small accomplishment。 This was certainly my experience。 I was excited to write code and proud that I could show off all of the things that I knew how to do to the world。 As evidence of my historical love of programming technology， here is a [linear algebra language][1] built with a another meta-programming language。 Notice that no one has touched this code in several years。

我们以充分的理由实现了功能并并觉得自己做到了。  这当然是我个人的经验。 以前我很高兴写代码，并骄傲地向世界展示我所知道的事情。 有例为证，，作为对某种编程技术的偏爱，这段[一句话algebra代码] 嵌入了另一段元语言代码。请注意多年以来一直没人碰它。

However after maintaining code a bit more I now think somewhat differently。

然而在维护更多的代码后，我慢慢觉得并非如此。

  1。 We should not seek to build software。 Software is the currency that we pay to solve problems， which is our actual goal。 We should endeavor to build as little software as possible to solve our problems。
  2。 We should use technologies that are as simple as possible， so that as many people as possible can use and extend them without needing to understand our advanced techniques。 We should use advanced techniques only when we are not smart enough to figure out how to use more common techniques。

1。 我们不应去刻意探求如何构建软件。 软件是我们为解决问题所付出的代价， 那才是我们真实的目的。 我们应努力为了解决问题而构建较小的软件。

2。 我们应使用尽可能简单的技术，那么更多的人就越可能会使用，并且无需理解我们所知的高级技术就能扩展软件的功能。当然，在我们不知道如何使用简单技术去实现时，我们也可以使用高级技术。

Neither of these points are novel。 Most people I meet agree with them to some extent， but somehow we forget them when we go to contribute to a new project。 The instinct to contribute by building and to demonstrate sophistication often take over。

所有的这些例子都不只是听来的故事。我遇到的大部分人会认同某些部分，但不知为何，当我们向一个新项目贡献代码时又会忘掉这个初衷。直觉里，用复杂技术去构建的念头往往会占据上风。

### Software is a cost

软件是种投入

Every line that you write costs people time。 It costs you time to write it of course， but you are willing to make this personal sacrifice。 However this code also costs the reviewers their time to understand it。 It costs future maintainers and developers their time as they fix and modify your code。 They could be spending this time outside in the sunshine or with their family。

你写的每行代码都要花费人力。写代码当然是需要时间的，也许你会认为只是你个人在奉献，然而这些代码在被审阅的时候也需要花时间理解，对于未来维护和开发人员来说，他们在维护和修改代码时同样要花费时间。他们完全可以把这时间用来出去晒晒太阳，或者陪伴家人。

So when you add code to a project you should feel meek。 It should feel as though you are eating with your family and there isn't enough food on the table。 You should take only what you need and no more。 The people with you will respect you for your efforts to restrict yourself。 Solving problems with less code is a hard， but it is a burden that you take on yourself to lighten the burdens of others。

所以，当你向某个项目贡献代码时，请心怀谦恭。就像是，你正和你的家人进餐时，餐桌上却没有足够的食物，你索取你所需的部分，别人对你的自我约束将肃然起敬。以更少的代码去解决问题是很难的，你肩负重任的同时自然减轻了别人的重负。

### Complex technologies are harder to maintain

技术越复杂越难维护

As students， we demonstrate merit by using increasingly advanced technologies。 Our measure of worth depends on our ability to use functions， then classes， then higher order functions， then monads， etc。 in public projects。 We show off our solutions to our peers and feel pride or shame according to our sophistication。

作为学生，逐渐能够使用高端技术显示自己的价值。这体现在，首先我们有能力在开源项目中使用函数，接着是类，然后是高阶函数，monads 等等。我们向同行显示自己的解决方案，同时因为自己所用技术的难易而常觉自豪或卑微。

However when working with a team to solve problems in the world the situation is reversed。 Now we strive to solve problems with code that is as simple as possible。 When we solve a problem simply we enable junior programmers to extend our solution to solve other problems。 Simple code enables others and boosts our impact。 We demonstrate our value by solving hard problems with only basic techniques。

而在现实中，和团队去解决问题时，情况发生了反转。现在，我们致力于尽可能使用简单的代码去解决问题。我们用简单方式解决问题时，使新手程序员能够以此扩展并解决其他问题。简单的代码让别人容易上手，产生的效果立竿见影。我们藉以只用简单的技术去解决难题，从而展示自己的价值。

Look! I replaced this recursive function with a for loop and it still does everything that we need it to。 I know it's not as clever， but I noticed that the interns were having trouble with it and I thought that this change might help。

看， 我用循环替代了递归函数并且一样达到了我们的需求。 当然这并非多聪明的做法， 我注意到之前新手同事在这里会遇到麻烦，希望今后能有所改变。

If you are a good programmer then you don't need to demonstrate that you know cool tricks。 Instead， you can demonstrate your value by solving a problem in a simple way that enables everyone on yyyurouuryouroyouryyour team to contribute in the future。

如果你是个好的程序员， 你不需要证明你知道很多炫技。 相应的， 你可以通过用一个简单的方法解决一个问题来显示你的价值， 并激发你的团队在未来去完善它。

### But moderation， of course

但要保持节制

That being said， over-adherence to the "build things with simple tools" dogma can be counter productive。 Often a recursive solution can be much simpler than a for-loop solution and often times using a Class or a Monad is the right approach。 But we should be mindful when using these technologies that we are building for ourselves own system; a system with which others have had no experience。

话虽如此， 过于遵循 “用简单的工具去构建”的教条也会降低生产力。经常地，用递归会比用循环解决问题更简单，用类或 monad 才是正确的途径。但如果我们是构建一个别人毫无经验的系统时，或者可以另当别论。


--------------------------------------------------------------------------------

via: http://matthewrocklin.com/blog/work/2018/01/27/write-dumb-code

作者：[Matthew Rocklin][a]
译者：[plutoid](https://github.com/plutoid) 
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]:http://matthewrocklin.com
[1]:https://github.com/mrocklin/matrix-algebra

