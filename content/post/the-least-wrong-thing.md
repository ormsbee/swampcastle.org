+++
date        = "2018-02-18"
title       = "Gift Card Hacks and Least Wrong Thing"
tags        = []
+++

I recently had a peculiar experience with the Barnes and Noble website. There
was a promotional offer, where if you spent $100 or more on an order between
December 9th and 10th, you would get to take $25 off a purchase made after
December 26th. My wife and I had already planned on making such an order anyway,
so we smiled at our good fortune and bought our gifts.

That was when it got a bit weird. There was no feedback that we had actually
qualified for this offer — no followup email or messaging on the site. After
a few days, I actually contacted B&N support to ask what was happening. We got
a pre-canned reply that it would be delivered by email on the 26th.

On the 20th, we got a marketing email saying that that we would get our Reward
on the 26th.

On the 26th, we received the email with the actual Reward in it, in the form of
a nineteen digit reward number, a four digit PIN, and the following instructions
at the bottom:

> 1. Log in to My Account.
> 2. Select Manage Account from the My Account drop down.
> 3. Select Gift Cards from the sidebar & enter Reward Number & Pin.

I actually didn't see those instructions at first and proceeded to try to put
this code into the wrong place on the shopping cart. Even when I found the right
place, I mistyped it the first time.

I reflexively started criticizing this horrible user experience and sketching
out how it could have been made better. Like most developers, I've indulged in
that sort of behavior any number of times over the years. But then I had this
sudden, painful flash of senior developer empathy where I felt like I could
actually *see* the terrible arc of this project and the suffering of the people
behind it. It was not a story of people who were dumb, or couldn't see the Right
Thing. It was the story of hard working, sensible folks boxed into a situation
where the Right Thing was laughably far out of reach, and the best that could be
managed was the Least Wrong Thing.

To be clear, everything that follows is pure speculation. There are any number
of ways this could have played out, and I have no insider information. But in my
horrible vision, it starts with a November conversation like this:

---

**Business**: "Hey, we had this promotional idea to help boost sales *and* give
us a bump during the after-Christmas lull. We really need to pad out those
holiday sales numbers or our stock is going to take a beating."

**Engineering**: "Great! What's the idea?"

**Business**: "We'll give them a $25 off coupon if they spend more than $100 on
the 9th or 10th. But we won't send them that coupon until *after* Christmas!
They'll shop twice!"

*~(ominous silence)~*

**Engineering**: "The Coupon System we have doesn't support that."

**Business**: "Well, can we add it?"

**Engineering**: "The Coupon System is one of the oldest, craziest, hacked-over
pieces of code we have. You can't add anything without breaking something else,
and that's why we've been trying to get the resources to rewrite it for years.
It is way too risky to mess with during our peak sales season."

**Business**: "Look, this could be vital to the future of the company. We're
talking millions of dollars. We have to make this work."

**Engineering**: "Keeping the site up during the holidays is also pretty vital,
both to the company and to my continued employment here."

*~(head scratching)~*

**Engineering**: "Maybe we could do it with Gift Cards?"

**Business**: "What?"

**Engineering**: "Well, we already have a batch process where we send Gift Card
orders to our vendor and get back the card numbers that our commerce system will
recognize. So we run the promotion like you said. Then we do a manual database
query after the cutoff date to get all the users who qualified. We dump that
into a CSV file. Then we make the Gift Card batch, and merge the card
information into our CSV file. Then you can use that to send off the emails like
a normal marketing campaign. We don't have to make any changes to commerce or
coupons code, since it'll just look like the user got a Gift Card from a store."

*~(User Experience is walking by and overhears)~*

**User Experience**: "That's horrible! I actually threw up a little in my mouth
just now."

**Business**: "Oh, hey, didn't see you there. Um... but it'll do what we want,
right?"

**User Experience**: "It's going to confuse everyone. Do you realize how many
people are going to write it into the space for coupon codes like they've been
conditioned to do, instead of the space for gift cards? (Which, by the way, is
still hidden away on the second part of the checkout flow in tiny print at the
bottom. We have a whole wall of mockups for what it should look like instead.)
And speaking of entering the code... no link?

**Engineering**: "There's no way to generate a link to pre-fill a gift card on
that page. I mean, it's supposed to be a physical gift card."

**User Experience**: "How many people are going to screw that up? How many
people are going to call in because they didn't get any proper messaging about
this reward? How many support tickets are going to be filed for this?"

**Engineering**: "But it's quick and it's *safe*. The Gift Card integration is
already there. We can do all the other stuff without touching the commerce
system, the checkout flow code, or our crazy deployment process.

**Business**: "Even if we generate a lot of support tickets, the numbers
probably still work out. I guess we should tell the support team about this..."

**User Experience**: "I hate you all. If you want to find me, I'll be drowning
my sorrows in tequila along with the rest of the UX team."

**Business**: "I'm sorry. We'll clean it up after the holiday rush. Really."

**Engineering**: "I'm sorry too. Our team will meet you at the bar."

---

The definition of the "Right Thing" in software engineering is vague and will
differ from one person to another, but we have this general conceit that there
exists a proper, elegant solution to any given problem. One that is intuitive,
efficient, and easily extensible. When we engage in smug armchair criticism of
how something is built, we're often modeling the Right Thing in a vacuum,
entirely removed from historical baggage and business constraints. In doing so,
we're tackling only the simplest part of the problem.

It would have been great if the Coupon System supported their use case, or if it
could be easily extended to do so. But not only is it an operational risk to
change it right now, even a massive refactoring of this code to enable such
flexibility could be a huge risk in terms of opportunity cost. Business *thinks*
that this promotion will help things, but nobody really knows for certain. Maybe
this is the only new Coupon use case to come along in a while. Is it worth
burning months of time refactoring or rebuilding a big system to enable
functionality that might not even be effective at generating revenue?

In this hypothetical situation, the Gift Card Hack is clearly not the Right
Thing from an architectural or user experience point of view. Yet given the
real-world constraints these teams faced, it was probably the Least Wrong Thing
that they could do at the time. It wasn't pretty, but business opportunity was
realized quickly and with minimal risk.

The problem with the Least Wrong Thing is that it's a tactical mindset and not a
strategic one. It's easy to get into the habit of heroically jumping from one
crisis to the next in a chain of clever bandaid solutions that never add up to
a cohesive strategy for the architecture as a whole. If this promotion is a
success, then it absolutely should be more robustly implemented for next time.
Maybe the old Coupon System really does need to be rewritten to enable
innovative promotions next year.

For me, it was mostly a reminder that there are reasons behind these sorts of
things. Being critical of what exists today and wanting to build the Right Thing
are important. But it's just as important to understand how and why things got
to be where they are, because our solutions cannot exist in a vacuum. They have
to be grounded in the many constraints of the real world, including things like
business value, organizational structure, governance, interoperability, legacy
support, regulatory compliance, sales/purchasing methods, hardware constraints,
etc.

Sometimes the solution is two steps removed from the most obvious problem. We
see the glaring UX issues, but maybe what it really means is that the deployment
process needs to be fixed, so that changes aren't as scary. Maybe the root
causes are not technical at all, and you need to change team structures and
incentives.

As developers, most of us start off by turning narrowly defined tasks into code.
Later on, we try to dig deeper into requirements and capabilities, to build more
robust systems. But we also have to be able to step further back and understand
the needs of our organizations as a whole. Software development isn't about
creating the shiniest possible thing. It's an unending series of tradeoffs to
make, and only some of those tradeoffs are technical in nature.
