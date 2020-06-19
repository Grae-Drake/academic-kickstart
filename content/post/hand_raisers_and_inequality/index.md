---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "The Hand Raiser Problem and Inequality"
subtitle: ""
summary: ""
authors: [Grae]
tags: [Education, Ethics, Inequality]
categories: []
date: 2020-06-19T12:43:57-07:00
lastmod: 2020-06-19T12:43:57-07:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---


While I was at Thinkful our instructional design and features evolved _a lot_. At the beginning things were simple. The curriculum was plain text (a Google doc we shared) that curated 3rd-party resources and "explained" (in a way that will make instructional designers cringe) the remaining topics. It was a great MVP in that things really _were_ minimal and it (mostly) worked.

Over time we layered on more features. We added a robust student community. Loads of open office hours that anyone could attend. Chat-based technical coaching. Instructional design basics like learning objectives, formative and summative assessments, backwards design. Things unarguably got better. But not as much as I expected and, critically, _not for as many people_ as I expected.

A while into building new features I realized that the features we were building often didn't have the effect I expected them to have. Take office hours. All of a sudden students now had access to experts almost around the clock. And the highest-performing students were attending office hours. _And yet_ we didn't see a big change in student achievement rates. What gives?

It turns out the causation between office hours attendance and achievement was the reverse of what I expected: high performing students saw a chance for additional value and took it. They didn't _need_ office hours. They'd have been successful without them. But they were valuable and they saw that value and weren't afraid to take it. Students at the margin, the students who would most benefit from these extra resources weren't taking advantage of them.

Internally we called this the "hand raiser problem". Many seemingly great features would help the people who didn't need help, the people in a class who always raised their hand, and _wouldn't_ help those who were quietly struggling and most in need of support.

{{< figure library="true" lightbox="false" src="hand_raiser.gif" >}}


Not every feature implicated the hand raiser problem, but I can confidently say that 100% of us on the education, product, and engineering teams were, ourselves, handraisers. That's why we worked at Thinkful in the first place. That made all these seemingly wonderful features, these shiny features that were _obviously great_ because _we would love to have them_, super tempting. But those weren't the features we needed to build. The features that actually had an impact were the ones that _everyone_ valued (not just the hand raisers) or even the ones that hand raisers _disliked_ but that boosted students at the margins.

There is a story here about product strategy and feature selection process and understanding your users that is interesting but not what I want to talk about today.

What I want to talk about today is inequality.

We thought, when we added office hours, that we were _making the world_ (or at least our product) _a better place_. Before there wasn't a thing. Now there's a thing. People who interact with the thing benefit. The thing doesn't harm anyone; worst case it gets ignored. Multiply the number of people who use it by the utility each gets from using it and you have a measure of exactly _how much_ it made the world a better place.

{{< figure library="true" lightbox="false" src="make_the_world_a_better_place.gif" caption="This show has no business being so accurate">}}


But while the thing doesn't harm _people_ it had the potential to harm the _community_. In isolation, this feature amplifies the existing performance gap between students. Learning outcomes become more bimodal. Winners are clearer, better winners. Others lose ground by standing still.

This example is just a silly feature from one education company. Students weren't competing against one another in any meaningful way. These games were not zero sum; making well-off students better-off didn't hurt anyone at Thinkful.

But looking around at our society now I see hand-raiser problems _fucking everywhere_.

Look at all these amazing useful webapps we've got now! Oh, unless accessibility issues make them painful for you to use.

Thank goodness we have tax-advantaged savings plans to make it easier to save for retirement! Yet, somehow, [only 37% of Americans who can contribute to a 401(k), do](https://www.fool.com/retirement/2017/06/19/does-the-average-american-have-a-401k.aspx). 

Healthcare is expensive and complicated, good thing for health insurance! (From our employer of course!) Except for the [51% of us who aren't employed, don't receive employment benefits, or otherwise fall through the cracks](https://www.kff.org/other/state-indicator/total-population/?dataView=0&currentTimeframe=0&selectedDistributions=employer&sortModel=%7B%22colId%22:%22Location%22,%22sort%22:%22asc%22%7D).


## My Dilemma

Do employer-sponsored health plans make our world a better place? I don't think so. There seem to be much more efficient ways to organize and allocate healthcare resources. Other countries have figured this out and implemented better solutions. I'd love to go back in time and stop the people who made that a thing.

Does [Khan Academy](https://www.khanacademy.org/) make the world a better place? I have to believe it does. I'm sure hand raisers disproportionately benefit from KA, but I'm also sure that the sheer number of students touched by KA is net positive. Here, KA, have an internet fist bump from me. 👊

Did the 20,000 recorded lectures Berkeley posted to Youtube make the world a better place before [Berkeley removed them because of ADA complaints](https://www.insidehighered.com/news/2017/03/06/u-california-berkeley-delete-publicly-available-educational-content)? I... I don't know.

This is where it gets selfish. I want to massively scale 1-on-1 learning. I think that's possible today in a way it's never been before. But _should_ I? I believe many people could benefit enormously from 1-on-1 mentoring relationships. Assuming for now that I'm successful in facilitating those, will I just end up making life better for professionals who already have it "great" by any rational measure? Open up new professional opportunities... for those of us who least _need_ them? If my work creates a ton of value, is a _little bit_ of increased inequality an acceptable tradeoff? How much, and how do I measure that?

I don't have any answers to these questions yet. For now I'm going to keep building and hope it works out, but I'm keeping an eye on this. If _you_ have any helpful ways to look at these issues [drop me a line](https://twitter.com/Grae_Drake).