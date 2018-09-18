---
layout: post
title: "Thoughts on Scaling Agile"
date: 2017-11-01
category: blog
tags: leadership agile
---

I used to work on large (200-300 people) agile programs and back in those days how to scale agile projects was something I thought about almost daily. The 300 people were organized in some 40 different teams all trying to create value by developing one big product line.
In contrast, last few years I have worked in an organization with several smaller independent projects. These independent projects resulted in different end-products with different clients. These projects all had at most three Scrum teams each. So although there was a lot going on concurrently in the organization in my role as software manager and architect, scaling wasn't something I thought about every day.

There has recently been a lot of talk about the need for scaling agile and it seems like the Scaled Agile Framework (SAFe) movement is getting more popular each day. With this post, I wanted to address some of my views on scaling agile and comment on the two main approaches you can use when scaling agile: Scrum and SAFe.

# Keeping things independent makes scaling trivial

My first observation is from the comparison of the large program (300 people) with the situation with several independent projects I just mentioned. The takeaway is that if you have the ability to run your business as several separate project/programs/value streams that each can be handled with a Scrum team or two each, do it. Then your scaling problem really is just about hiring a new team when more work needs to get done. I'm the first to admit that hiring is not easy but it's at least a well understood activity. In this situation, the whole Scrum of Scrums, Product owner hierarchies or SAFe's Agile Release Trains are just not needed. You have a simpler problem and you can use Scrum or Kanban out-of-the-box to solve it.

# If you have to scale, should you work off a recipe or follow a few basic principles?

So given that you do have a situation where you can't accomplish all that needs to be accomplished within reasonable time for a single project/program/value stream with just a team or two and any which way you look at the problem there really aren't smaller completely independent pieces. The way I see it there are two ways to go: following a more detailed recipe or following core principles and adapt them to your organization. Or put in other words, use Scrum or use SAFe.

## Scrum: following a few basic principles

I've been living with Scrum as taught by Jeff Sutherland and Ken Schwaber for some 15 years and this is the project management methodology and I will always think of first for an organization. Scrum, as I see it, is best defined by the three books: [Book1](https://www.amazon.com/Agile-Software-Development-Scrum/dp/0130676349/ref=sr_1_1?ie=UTF8&qid=1536892072&sr=8-1&keywords=scrum+schwaber), [Book2](https://www.amazon.com/Agile-Project-Management-Developer-Practices/dp/073561993X/ref=sr_1_1?ie=UTF8&qid=1536890364&sr=8-1&keywords=schwaber&dpID=51G-Vkd-beL&preST=_SY291_BO1,204,203,200_QL40_&dpSrc=srch), and [Book3](https://www.amazon.com/gp/product/0735623376/ref=dbs_a_def_rwt_bibl_vppi_i3) .
Scrum is really slimmed down and has very few rules. My assumption is that any reader knows basic Scrum. To scale Scrum to several teams and ultimately the whole enterprise some additional processes are added to basic one-team scrum:

- Several teams can and should work off of single backlog. The items in backlog can be marked with attributes showing it needs skill set or falls into a domain usually implemented by a specific team. But there is one backlog.
- Product owner can work with more than one team. If you use more than one product owner there is a strict hierarchy so that the "single wringable neck" principle doesn't get violated.
- Scrum of Scrums meeting is held to coordinate the work and remove obstacles that can't be solved within a team.
- Use Scrum to run the project of introducing Scrum in the whole company. Use a backlog for the prioritizing the rollout.

But there really aren't that many rules. And that is the point. Scrum is adaptive by nature and so you should use it to adapt the basic framework to fit your organization. This means that not every "implementation" of large scale scrum or scaled agile implementation using scrum will be the same. So if you are going to scale agile using Scrum it boils down to knowing the agile principles, follow these principles and adapt.

## SAFe: working off a recipe

I haven't truly lived SAFe like I have lived Scrum but have a lot of people in my network that are experts. My thoughts here are my own but influenced by others. SAFe is defined at [SAFe site](https://www.scaledagileframework.com/) and resource linked from there. The heavy 800 page  [reference guide](https://www.amazon.com/SAFe-4-5-Reference-Guide-Enterprises/dp/0134892860) describes the framework in detail.

SAFe compared to Scrum has a lot more rules and roles, artefacts and processes. That is also by design. SAFe set out to fill the gap that Scrum left by not having enough process for scaling.
Ken Schwaber made the post [unSAFe at any speed](https://kenschwaber.wordpress.com/2013/08/06/unsafe-at-any-speed/) bashing the whole SAFe framework and likening it to RUP. I've never worked on full blown RUP project but [Krutchen's book](https://www.amazon.com/Rational-Unified-Process-Introduction-3rd/dp/0321197704) still brings back memories, and not fond ones. On a side note, I think there are things from the RUP-era that are very valuable like the 4+1 architecture view and UML in general. I know RUP/UP is not the same as UML but when RUP and agile replaced it, it also gave UML a bad rep. But I digress.
I don't share Ken Schwaber's analysis in full. Reading and learning about SAFe, it all makes good sense. But so did RUP back in the day too.

# Conclusion

If you can avoid scaling agile, do it! If you can't, you have two options. My suggestion is trying to stick to Scrum but be prepared to put in the work to make it work for your organization.
