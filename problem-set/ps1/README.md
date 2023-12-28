[6.005 — Software Construction on MIT OpenCourseWare ](https://ocw.mit.edu/ans7870/6/6.005/s16/)| [OCW 6.005 Homepage](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-005-software-construction-spring-2016/)
6.005 — 软件构建在麻省理工学院开放课程 | OCW 6.005 主页

Spring 2016 2016 年春季

[Problem Set 1: Tweet Tweet](https://ocw.mit.edu/ans7870/6/6.005/s16/psets/ps1/#problem_set_1_tweet_tweet)[Get the code](https://ocw.mit.edu/ans7870/6/6.005/s16/psets/ps1/#get_the_code)[Overview](https://ocw.mit.edu/ans7870/6/6.005/s16/psets/ps1/#overview)[Problem 1: Extracting data from tweets](https://ocw.mit.edu/ans7870/6/6.005/s16/psets/ps1/#problem_1_extracting_data_from_tweets)[Problem 2: Filtering lists of tweets](https://ocw.mit.edu/ans7870/6/6.005/s16/psets/ps1/#problem_2_filtering_lists_of_tweets)[Problem 3: Inferring a social network](https://ocw.mit.edu/ans7870/6/6.005/s16/psets/ps1/#problem_3_inferring_a_social_network)[Problem 4: Get smarter](https://ocw.mit.edu/ans7870/6/6.005/s16/psets/ps1/#problem_4_get_smarter)

# Problem Set 1: Tweet Tweet

The purpose of this problem set is to give you practice with test-first programming. Given a set of specifications, you will write unit tests that check for compliance with the specifications, and then implement code that meets the specifications.
此问题集的目的是让您练习测试优先编程。根据一组规范，您将编写单元测试来检查是否符合规范，然后实现符合规范的代码。

## Get the code

To get started, [download the assignment code ](https://ocw.mit.edu/ans7870/6/6.005/s16/psets/ps1/ps1.zip)and initialize your repository. If you need a refresher on how to create your repository, see [Problem Set 0 ](https://ocw.mit.edu/ans7870/6/6.005/s16/psets/ps0/#clone).
要开始，请下载作业代码并初始化您的存储库。如果您需要复习如何创建存储库，请参阅问题集 0。

## Overview

The theme of this problem set is to build a toolbox of methods that can extract information from a set of tweets downloaded from Twitter.
此问题集的主题是构建一个方法工具箱，该工具箱可以从 Twitter 下载的一组推文中提取信息。

Since we are doing test-first programming, your workflow for each method should be ( **in this order** ).
由于我们正在进行测试优先编程，因此您对每个方法的工作流程应为（按此顺序）。

1. Study the specification of the method carefully.
   仔细研究方法的规范。
2. Write JUnit tests for the method according to the spec.
   根据规范为该方法编写 JUnit 测试。
3. Implement the method according to the spec.
   根据规范实现方法。
4. Revise your implementation and improve your test cases until your implementation passes all your tests.
   修改您的实现并改进您的测试用例，直到您的实现通过所有测试。

On PS0, we graded only your method implementations. On this problem set, we will also grade the tests you write. In particular:
在 PS0 中，我们只对您的方法实现进行评分。在此问题集中，我们还将对您编写的测试进行评分。特别是：

- **Your test cases should be chosen using the input/output-space partitioning approach** . This approach is explained in the [reading about testing ](https://ocw.mit.edu/ans7870/6/6.005/s16/classes/03-testing/).
  应使用输入/输出空间划分方法选择您的测试用例。此方法在有关测试的阅读材料中进行了说明。
- **Include a comment at the top of each test suite class describing your testing strategy** – how you partitioned the input/output space of each method, and then how you decided which test cases to choose for each partition.
  在每个测试套件类的顶部包含一条注释，描述您的测试策略——您如何划分每个方法的输入/输出空间，然后您如何决定为每个分区选择哪些测试用例。
- **Your test cases should be small and well-chosen.** Don’t use a large set of tweets from Twitter for each test. Instead, create your own artificial tweets, carefully chosen to test the partition you’re trying to test.
  您的测试用例应小巧且经过精心选择。不要对每个测试都使用来自 Twitter 的大量推文。相反，创建您自己的人工推文，仔细选择以测试您尝试测试的分区。
- **Your tests should find bugs.** We will grade your test cases in part by running them against buggy implementations and seeing if your tests catch the bugs. So consider ways an implementation might inadvertently fail to meet the spec, and choose tests that will expose those bugs.
  您的测试应找到错误。我们将通过在错误的实现上运行您的测试用例并查看您的测试是否捕获错误来部分对您的测试用例进行评分。因此，请考虑实现可能无意中无法满足规范的方式，并选择将暴露这些错误的测试。
- **Your tests must be legal clients of the spec.** We will also run your test cases against legal, variant implementations that still strictly satisfy the specs, and your test cases should not complain for these good implementations. That means that your test cases can’t make extra assumptions that are only true for your own implementation.
  您的测试必须是规范的合法客户端。我们还将针对仍然严格满足规范的合法变体实现运行您的测试用例，并且您的测试用例不应抱怨这些良好的实现。这意味着您的测试用例不能做出仅对您自己的实现为真的额外假设。
- **Put each test case in its own JUnit method.** This will be far more useful than a single large test method, since it pinpoints where the problem areas lie in the implementation.
  将每个测试用例放在其自己的 JUnit 方法中。这将比单个大型测试方法更有用，因为它可以精确定位实现中的问题区域。
- Again, keep your tests small. Don’t use unreasonable amounts of resources (such as `MAX_INT `size lists). We won’t expect your test suite to catch bugs related to running out of resources; *every* program fails when it runs out of resources.
  同样，保持测试的规模小。不要使用不合理数量的资源（例如 `MAX_INT `大小列表）。我们不会期望您的测试套件捕获与资源耗尽相关的错误；每个程序在资源耗尽时都会失败。

You should also keep in mind these facts from the readings and classes about specifications ( [part 1 ](https://ocw.mit.edu/ans7870/6/6.005/s16/classes/06-specifications/), [part 2 ](https://ocw.mit.edu/ans7870/6/6.005/s16/classes/07-designing-specs/)):
您还应该牢记有关规范的阅读和课程中的以下事实（第 1 部分，第 2 部分）：

- **Preconditions.** Some of the specs have preconditions, e.g. “this value must be positive” or “this list must be nonempty”. When preconditions are violated, the behavior of the method is *completely unspecified* . It may return a reasonable value, return an unreasonable value, throw an unchecked exception, display a picture of a cat, crash your computer, etc., etc., etc. In the tests you write, do not use inputs that don’t meet the method’s preconditions. In the implementations you write, you may do whatever you like if a precondition is violated. Note that if the specification indicates a particular exception should be thrown for some class of invalid inputs, that is a *postcondition* , not a precondition, and you *do* need to implement and test that behavior.
  前提条件。某些规范具有前提条件，例如“此值必须为正”或“此列表必须非空”。当违反前提条件时，方法的行为是完全不确定的。它可能会返回一个合理的值或是返回一个不合理的值又或者是抛出一个未检查异常甚至是显示一张猫的图片、使您的计算机崩溃等等。在您编写的测试中，请勿使用不满足方法前提条件的输入。在您编写的实现中，如果违反了前提条件，您可以做任何您想做的事情。请注意，如果规范指示应为某些类别的无效输入抛出特定异常，则这是一个后置条件，而不是一个前置条件，并且您确实需要实现和测试该行为。
- **Underdetermined postconditions.** Some of the specs have underdetermined postconditions, allowing a range of behavior. When you’re implementing such a method, the exact behavior of your method within that range is up to you to decide. When you’re writing a test case for the method, you must allow the implementation you’re testing to have the full range of variation, because otherwise your test case is not a legal client of the spec as required above.
  不确定的后置条件。某些规范具有不确定的后置条件，允许一系列行为。当您实现此类方法时，您需要决定该方法在该范围内的确切行为。当您为该方法编写测试用例时，您必须允许您正在测试的实现具有全部的变化范围，否则您的测试用例就不是规范的合法客户端。

Finally, in order for your overall program to meet the specification of this problem set, you are required to keep some things unchanged:
最后，为了使您的整体程序满足此问题集的规范，您需要保持某些内容不变：

- **Don’t change these classes at all:** the classes `Tweet `, `Timespan `, and `TweetReader `should not be modified *at all* .
  完全不要更改这些类：类 `Tweet `、 `Timespan `和 `TweetReader `绝对不能修改。
- **Don’t change these class names:** the classes `Extract `, `Filter `, `SocialNetwork `, `ExtractTest `, `FilterTest `, and `SocialNetworkTest `must use those names and remain in the `twitter `package.
  不要更改这些类名：类 `Extract `、 `Filter `、 `SocialNetwork `、 `ExtractTest `、 `FilterTest `和 `SocialNetworkTest `必须使用这些名称并保留在 `twitter `包中。
- **Don’t change the method signatures and specifications:** The public methods provided for you to implement in `Extract `, `Filter `, and `SocialNetwork `must use the method signatures and the specifications that we provided.
  不要更改方法签名和规范：在 `Extract `、 `Filter `和 `SocialNetwork `中为您提供的要实现的公共方法必须使用我们提供的方法签名和规范。
- **Don’t include illegal test cases:** The tests you implement in `ExtractTest `, `FilterTest `, and `SocialNetworkTest `must respect the specifications that we provided for the methods you are testing.
  不要包含非法测试用例：您在 `ExtractTest `、 `FilterTest `和 `SocialNetworkTest `中实现的测试必须遵守我们为所测试方法提供的规范。

Aside from these requirements, however, you are free to add new public and private methods and new public or private classes if you wish. In particular, if you wish to write test cases that test a stronger spec than we provide, you should put those tests in a separate JUnit test class, so that we don’t try to run them on staff implementations that only satisfy the weaker spec. We suggest naming those test classes `MyExtractTest `, `MyFilterTest `, `MySocialNetworkTest `, and we suggest putting them in the `twitter `package in the `test `folder alongside the other JUnit test classes.
但是，除了这些要求之外，如果您愿意，可以自由添加新的公共和私有方法以及新的公共或私有类。特别是，如果您希望编写测试用例来测试比我们提供的规范更强的规范，则应将这些测试放在一个单独的 JUnit 测试类中，以便我们不会尝试在仅满足较弱规范的实现上运行它们。我们建议将这些测试类命名为 `MyExtractTest `、 `MyFilterTest `、 `MySocialNetworkTest `，并建议将它们放在 `test `文件夹下的 `twitter `包中，与其他 JUnit 测试类并排。

------

## Problem 1: Extracting data from tweets 问题 1：从推文中提取数据

In this problem, you will test and implement the methods in `Extract.java `.
在此问题中，您将测试和实现 `Extract.java `中的方法。

You’ll find `Extract.java `in the `src `folder, and a JUnit test class `ExtractTest.java `in the `test `folder. Separating implementation code from test code is a common practice in development projects. It makes the implementation code easier to understand, uncluttered by tests, and easier to package up for release.
您可以在 `src` 文件夹中找到 `Extract.java`，以及 `test`文件夹中的 JUnit 测试类 `ExtractTest.java`。将实现代码与测试代码分开是开发项目中的常见做法。这使得实现代码更容易理解，不会被测试弄得杂乱无章，并且更容易打包以供发布。

1. Devise, document, and implement test cases for `getTimespan()`and `getMentionedUsers()`, and put them in `ExtractTest.java`.
2. 为 `getTimespan()`和 `getMentionedUsers()`设计、记录和实现测试用例，并将它们放入 `ExtractTest.java `中。

3. Implement `getTimespan()`and `getMentionedUsers()`, and make sure your tests pass.
4. 实现 `getTimespan()`和 `getMentionedUsers()`，并确保您的测试通过。

If you want to see your code work on a live sample of tweets, you can run `Main.java `. ( `Main.java `will not be used in grading, and you are free to edit it as you wish.)
如果你想在一个实时的推文样本上查看你的代码运行情况，你可以运行 `Main.java`。（ `Main.java`不会用于评分，你可以随意编辑它。）

Hints: 提示：

- Note that we use the class [`Instant `](https://docs.oracle.com/javase/8/docs/api/?java/time/Instant.html)to represent the date and time of tweets. You can check [this article on Java 8 dates and times ](https://java.dzone.com/articles/deeper-look-java-8-date-and)to learn how to use `Instant `.
- 请注意，我们使用 `Instant`类来表示推文的日期和时间。您可以查看这篇关于 Java 8 日期和时间的文章以了解如何使用 `Instant`类。

- You may wonder what to do about lowercase and uppercase in the return value of `getMentionedUsers() `. This spec has an underdetermined postcondition, so read the spec carefully and think about what that means for your implementation and your test cases.
- 您可能想知道如何处理 `getMentionedUsers()`的返回值中的小写和大写。此规范具有不确定的后置条件，因此请仔细阅读规范并考虑这对您的实现和测试用例意味着什么。

- `getTimespan() `*also* has an underdetermined postcondition in some circumstances, which gives the implementor (you) more freedom and the client (also you, when you’re writing tests) less certainty about what it will return.
  `getTimespan() `在某些情况下也具有不确定的后置条件，这为实现者（您）提供了更多的自由，而为客户端（也是您，当您编写测试时）提供了较少的关于它将返回什么的确定性。
- Read the spec for the `Timespan`class carefully, because it may answer many of the questions you have about `getTimespan() `.
- 仔细阅读 `Timespan`类的规范，因为它可能会回答您对 `getTimespan() `的许多疑问。

**Commit to Git.** Once you’re happy with your solution to this problem, commit to your repo! Committing frequently – whenever you’ve fixed a bug or added a working and tested feature – is a good way to use version control, and will be a good habit to have for your team projects.
提交到 Git。一旦您对解决此问题感到满意，请提交到您的存储库！频繁提交 - 无论何时修复错误或添加正在工作且经过测试的功能 - 都是使用版本控制的好方法，并且对您的团队项目来说是一个好习惯。

------

## Problem 2: Filtering lists of tweets 问题 2：过滤推文列表

In this problem, you will test and implement the methods in `Filter.java `.
在此问题中，您将测试并实现 `Filter.java `中的方法。

1. Devise, document, and implement test cases for `writtenBy() `, `inTimespan() `, and `containing() `, and put them in `FilterTest.java `.
   为 `writtenBy() `、 `inTimespan() `和 `containing() `设计、记录和实现测试用例，并将它们放入 `FilterTest.java `中。
2. Implement `writtenBy() `, `inTimespan() `, and `containing() `, and make sure your tests pass.
   实现 `writtenBy() `、 `inTimespan() `和 `containing() `，并确保您的测试通过。

Hints: 提示：

- For questions about lowercase/uppercase and how to interpret timespans, reread the hints in the previous question.
  有关小写/大写和如何解释时间跨度的疑问，请重新阅读前一个问题中的提示。
- For all problems on this problem set, you are free to rewrite or replace the provided example tests and their assertions.
  对于此问题集中的所有问题，您可以自由地重写或替换提供的示例测试及其断言。

**Commit to Git.** Once you’re happy with your solution to this problem, commit to your repo!
提交到 Git。一旦你对这个问题的解决方案感到满意，就提交到你的仓库！

------

## Problem 3: Inferring a social network 问题 3：推断社交网络

In this problem, you will test and implement the methods in `SocialNetwork.java `. The `guessFollowsGraph() `method creates a social network over the people who are mentioned in a list of tweets. The social network is an approximation to who is following whom on Twitter, based only on the evidence found in the tweets. The `influencers() `method returns a list of people sorted by their influence (total number of followers).
在这个问题中，你将测试并实现 `SocialNetwork.java `中的方法。 `guessFollowsGraph() `方法根据在推文列表中提及的人员创建一个社交网络。社交网络是对 Twitter 上谁关注谁的近似，仅基于在推文中找到的证据。 `influencers() `方法返回按影响力（关注者总数）排序的人员列表。

1. Devise, document, and implement test cases for `guessFollowsGraph() `and `influencers() `, and put them in `SocialNetworkTest.java `. Be careful that your test cases for `guessFollowsGraph() `respect its underdetermined postcondition.
   为 `guessFollowsGraph() `和 `influencers() `设计、记录和实现测试用例，并将它们放入 `SocialNetworkTest.java `中。注意，你的 `guessFollowsGraph() `测试用例要尊重其未确定的后置条件。
2. Implement `guessFollowsGraph() `and `influencers() `, and make sure your tests pass. For now, implement only the minimum required behavior for `guessFollowsGraph() `, which infers that Ernie follows Bert if Ernie @-mentions Bert.
   实现 `guessFollowsGraph() `和 `influencers() `，并确保你的测试通过。现在，仅实现 `guessFollowsGraph() `所需的最小行为，即如果 Ernie @ 提及 Bert，则推断 Ernie 关注 Bert。

If you want to see your code work on a live sample of tweets, run `Main.java `. It will print the top 10 most-followed people according to the social network you generated. You can search for them on Twitter to see if their actual number of followers has a similar order.
如果你想在推文的实时样本上看到你的代码运行，请运行 `Main.java `。它将根据你生成的社交网络打印出关注人数最多的前 10 个人。你可以在 Twitter 上搜索他们，看看他们实际的关注者数量是否具有相似的顺序。

------

## Problem 4: Get smarter 问题 4：变得更聪明

In this problem, you will implement one additional kind of evidence in `guessFollowsGraph() `. Note that we are taking a broad view of “influence” here, and even Twitter-following is not a ground truth for influence, only an approximation. It’s possible to read Twitter without explicitly following anybody. It’s also possible to be influenced by somebody through other media (email, chat, real life) while producing evidence of the influence on twitter.
在此问题中，您将在 `guessFollowsGraph() `中实现一种其他类型的证据。请注意，我们在这里对“影响”采取了广义的看法，即使是 Twitter 追随也不是影响力的真实依据，而只是一种近似。可以在不显式关注任何人的情况下阅读 Twitter。也可以通过其他媒体（电子邮件、聊天、现实生活）受到某人的影响，同时在 Twitter 上产生影响的证据。

Here are some ideas for evidence of following. Feel free to experiment with your own.
以下是一些关注证据的想法。随时尝试您自己的想法。

- **Common hashtags.** People who use the same hashtags in their tweets (e.g. `#mit `) may mutually influence each other. People who share a hashtag that isn’t otherwise popular in the dataset, or people who share multiple hashtags, may be even stronger evidence.
  常见的标签。在推文中使用相同标签的人（例如 `#mit `）可能会相互影响。共享在数据集中并不流行的标签的人，或共享多个标签的人，可能证据更充分。
- **[Triadic closure ](https://en.wikipedia.org/wiki/Triadic_closure).** In this context, triadic closure means that if a strong tie (mutual following relationship) exists between a pair A,B and a pair B,C, then some kind of tie probably exists between A and C – either A follows C, or C follows A, or both.
  三元闭包。在此上下文中，三元闭包意味着如果在对 A、B 和对 B、C 之间存在强关系（相互关注关系），那么 A 和 C 之间可能存在某种关系——A 关注 C，或 C 关注 A，或两者都关注。
- **Awareness** . If A follows B and B follows C, and B retweets a tweet made by C, then A sees the retweet and is influenced by C.
  意识。如果 A 关注 B，B 关注 C，并且 B 转发 C 发布的推文，那么 A 会看到转发并受到 C 的影响。

Keep in mind that whatever additional evidence you implement, your `guessFollowsGraph() `must still obey the spec. To test your specific implementation, make sure you put test cases in your own `MySocialNetworkTest `class rather than the `SocialNetworkTest `class that we will run against staff implementations. Your work on this problem will be judged by the clarity of the code you wrote to implement it and the test cases you wrote to test it.
请记住，无论您实施什么其他证据，您的 `guessFollowsGraph() `仍必须遵守规范。要测试您的具体实现，请确保将测试用例放在您自己的 `MySocialNetworkTest `类中，而不是我们将针对工作人员实现运行的 `SocialNetworkTest `类中。您对这个问题的工作将根据您为实现它而编写的代码的清晰度以及您为测试它而编写的测试用例来评判。

------

MIT EECS