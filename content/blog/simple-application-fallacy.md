+++
title = "The Simple Application Fallacy"
date = 2025-06-28
authors = ["Hildeberto Mendonca",]
description = "There is no such thing as a simple application."
+++

I recently joined a community
initiative to organize a flea market in our neighborhood. Our neighbor is a
lovely lady in her third age who knocks at our door whenever she has a technical
problem. In that lazy morning she came by, apparently in a hurry, asking me if I
knew a practical way to extract information from email messages to populate a
spreadsheet. She had to get ready for this because people would start
registering in three days by email and that was the agreed procedure with the
rest of the team. When she tried to demonstrate her intent in her laptop, it
became immediately clear she was not going anywhere with her limited computer
abilities.

<!-- more -->

## Usability or Nothing

To be honest, that question really tested my geekiness but this time I decided
to be pragmatic. Instead of proposing a [TensorFlow](https://www.tensorflow.org)
app that would mine her emails to look for patterns that would indicate the
message was actually a registration in the flea market, I simply suggested to
open her emails and the spreadsheet in separate windows, put them side by side
and copy data from a window to another. Looking at her expression of
frustration, that was clearly too much. She understood everything, but she was
not sure she would be able to reproduce all that afterwards.

Then I suggested a plan B: What if we create a form using a Google Spreadsheet
and ask people to fill it in? It would automatically populate the spreadsheet in
a structured way, without any artificial intelligence algorithm. So, we built
the form together, tested and it worked! Except that it wasn't what we were
looking for. The URL of the form was too complex to be advertised. The form was
too long despite handling just 7 fields, which is confusing for unexperienced
users. The title and the labels were in French, the main language of the target
users, but everything else were in English, such as error messages, buttons and
instructions. It was clear that usability was an essential requirement, thus I
had to think about a plan C.

## The Plan C

To precisely address the context, I counted on the help of
[my wife](https://keniasousa.github.io), a professional business analyst, to
propose a custom made app as a plan C. It had to be simple and straightforward,
so we would have time to finish it before the beginning of the registrations. To
make things more exciting, my sweat neighbor had to present the idea to her
peers and have their approval before we continue with plan C.

2 days later and one day left to the registrations, she gave us the green light.
The most productive thing I could think of was the
[Django Framework](https://www.djangoproject.com) because it would give us the
back-office for free and the front-office would be all we had to worry about. No
time for architecture and design, just default choices and realtime
transformation of requirements into code. The expectation was a usable online
form with 7 fields, fully visible without scrolling the page, everything in
French, and with a short URL for easy access. Since Django comes with a powerful
administration tool, almost no work had to be done to manage the registrations.
2 hours later everything was ready, 30 minutes more it was in production on
[Heroku](http://heroku.com).

![Flea market registration form](/images/posts/brocante-form.png)

As you can see, the application was simple. In the following days, the
registrations started flowing in. Good red wine and a table of French cheeses
served for celebration. Nothing else was planned. Suddenly, when the application
was already saving a lot of administrative time, it confronted the real world
with many issues popping out.

## From Simple to Not That Simple

A requirement that was really clear since the beginning was that each
registration has the right to reserve a maximum of 2 slots. The form validated
that. What we didn't expect was some clever people registering multiple times to
reserve more than 2 slots. We had to hurry with a solution to minimize the
problem by allowing a single registration per email address.

As the volume of registrations increased, some people complained about the time
it was taking to get a confirmation. They wanted some feedback, but all we've
shown as feedback was a screen confirming the submission of the form. With this
application, we've solved the problem of moving data from the emails to a
spreadsheet, but we didn't solve the other way around, when the organizers had
to send emails to confirm the registrations. It would force them to switch
between windows all the time. So, we had to add a feature to confirm the
registrations and automatically notify people by email, so organizers didn't
have to write any email at all.

Since we were dealing with volume, another question came out: what if there are
no more slots available, but people keep submitting their registrations? How can
we handle that? Our answer was managing a waiting list. It shows a message on
the form to alert users about the unavailability of slots. If they really wanted
to continue the registration they would be added to a waiting list. In the
back-office, for each cancellation of a confirmed reservation, the organizers
could pick a registration in the waiting list to fill-in the available slots.

Initially thought to be a very simple application, it turned out to be not that
simple. New requirements were appearing all the time. The simple ones we made
side by side with the organizers to get instant feedback for continuous
delivery. The others, I had to postpone to the next opportunity.

![Django Admin](/images/posts/django-admin.png)

But there was a latest demanding requirement that had to be done before
considering the project finished: printing. The organizers needed to print all
confirmed registrations to have some control during the event. They tried to use
the printing capability of the browser, but printing a Django Admin page simply
doesn't work well. To address the requirement as fast as possible, we developed
a custom html page suitable for browser printing, showing exactly the
information they needed during the event. Sorry, no PDF reports for the moment.

## Questioning Django Admin For The Long Term

It is quite clear that we wouldn't achieve so many results in such a short
period of time using something different from Django. It is incredibly
productive and Python is a "get things done" kind of language. Using this
framework for your volunteering projects helps you to get the most value
possible from your free time. However, it's not a silver bullet for every type
of project.

Sometimes, you have to design things considering users with almost no computer
experience (e.g. elderly people). Django Admin is a handful tool, but it was not
designed with usability in mind. It's useful for technical people who are
familiar with this kind of UI, otherwise people have to be trained.
Unfortunately, training may not be effective in some cases and we have to rely
on good design to implement both the front-office and the back-office in such a
way that no training is required.

The decision to replace Django Admin will eventually come to offer a better user
experience for organizers as well. It might imply on a lot of additional work to
do. Since it isn't a blocking issue, it can be done slowly, through several
iterations without disrupting the service.

![Flea market registration team](/images/posts/brocante-team.jpg)

We would like to thank our lovely neighbor and her colleague for this incredible
opportunity to contribute to our community. We also learned how important
usability is for business improvements, which will positively impact our
professional careers.
