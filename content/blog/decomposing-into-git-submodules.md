+++
title = "Decomposing a Large Codebase Into Git Submodules"
date = 2025-08-28
authors = ["Hildeberto Mendonca",]
description = "Git Submodules is a nice way to add dependencies to a project in a source code level."
+++

I recognize good developers by their capacity of cleaning existing code and
adding a minimal amount of code for new features. Their pull requests are
compact, favoring reusability and elegance. But even with their effort, some
codebases are simply large because the implemented business is complex. This
is the case of [OSIS], an open source software designed to manage the core
business of the [Université catholique de Louvain][UCL].

OSIS is a monolithic [Django] project, composed of several applications.
These applications can have their own resources or share resources with other
applications. They can even be activated or deactivated at runtime, which means
the entire codebase is not necessarily running in production. This architecture
is so flexible that the IT department of the university decided to stimulate
other departments, in need of a business application, to develop apps for OSIS,
instead of other heterogenous choices. We kindly call them satellite apps in the
OSIS constellation. The strategy seems to work because three apps were already
developed in the last 18 months by three different teams.

<!-- more -->

However, those apps have tripled the OSIS' codebase, making it larger than what
was originally planned. It isn't so serious in the case of a [Python] project
because there are no such things as compilation, packaging or startup time. But
a large codebase is harder to maintain and, when developed by multiple teams,
may cause several workflow issues.

To address this problem, we picked one of the apps, externalized it in a
different repository and added it back to OSIS as a
[Git submodule][git-submodule]. Some of you may argue that
[Git subtree][git-subtree] would be a better option compared to Git submodule
because of its transparency to other developers, but it is so much more complex
to configure, push and pull with the remote repository than to perform a single
command like `git submodule update` whenever needed. Actually, we have been
working with submodule for a while and we had no major issues so far.

![GIT submodule](/images/posts/github-repo-submodule.png)

Others may also argue that it's time for micro services. Well, I don't see it as
an advantage just yet because we may reduce the codebase but we would, at the
same time, complexify the architecture with an additional web service layer,
additional security measures and more configurations. We don't even have the
excuse of a performance issue, so the added value is obviously not there yet.
But when the time comes, what we are doing now will certainly simplify the
transition to micro services.

## Moving the Internship App to a New Repository

I'm currently working on the Internship app, one of those satellite apps, and
here is a step by step guide to move the app to another repository and add it
back as a submodule of [OSIS]. To avoid messing up with my programming
environment, I've decided to do everything in a temporary folder:

    $ cd ~/python/projects/osis
    $ mkdir temp

Then I cloned the OSIS repository inside of the temporary folder:

    $ cd temp
    $ git clone https://github.com/uclouvain/osis.git

To preserve the history of changes in the Internship app, I decided to start the
new repository from a clone of OSIS:

    $ git clone osis ./osis-internship
    $ cd osis-internship

In my case, the default branch of OSIS is `dev`. After cloning it, the branch of
`osis-internship` is also `dev`, so I have to create a `master` branch from
`dev`, but you don't have to do that if you cloned the `master` branch of your
repository:

    $ git checkout -b master

At this point, the repository `osis-internship` is ready for the clean up. The
intention here is to remove all files that aren't part of Internship, put the
content of the folder `internship` in the root of the repository and remove the
folder `internship`:

    $ git rm -rf assessments/ backoffice/ assistant/ base/ dissertation/ \
                 reference/ attribution/ cms/ doc/ osis_common/ dev-settings.py \
                 Dockerfile __init__.py
    $ git mv internship/* .
    $ rm -rf internship

This is an important step, so I commit these changes. In this project, we put
the number of the issue we are working on in the commit message:

    $ git commit -m "INTERNSHIP-1 Removed unnecessary resources and moved the " \
                    "content of the folder 'internship' to the root."

The last steps for the repository `osis-internship` is to create a new
repository on GitHub, reset the remote origin to point to this new GitHub
repository and push the local master branch:

    $ git remote set-url origin https://github.com/uclouvain/osis-internship.git
    $ git push origin master

The repository [osis-internship] contains now only the artifacts of the
Internship app.

## Creating the Submodule in the OSIS Repository

Moving back to `osis`, I removed the folder `internship` and commit the change:

    $ cd ../osis
    $ git rm -rf internship/
    $ git commit -m "OSIS-195 Folder 'internship' removed."

Then I add the repository `uclouvain/osis-internship` as a submodule of `osis`:

    $ git submodule add https://github.com/uclouvain/osis-internship.git \
          ./internship
    $ git submodule init
    $ git submodule update

Finally, I commit and push the change to GitHub:

    $ git commit -m "OSIS-195 Submodule 'internship' added."
    $ git push origin dev

Voilà! The new submodule was added to the codebase of OSIS, which is now
smaller, since a submodule is just a reference to another repository.

![OSIS' submodules](/images/posts/github-repo-with-submodules.png)

## How to Develop in the New Submodule

At this point, the submodule is ready to be developed, but I did everything in a
temporary folder. So, I have to get back to the working environment and pull the
latest changes:

    $ cd ~/python/projects/osis/osis
    $ git pull origin dev

Then I setup the local submodule:

    $ git submodule init
    $ git submodule update

This setup is necessary every time the repository is cloned. After that, the only
command I need to remember is:

    $ git submodule update

which will be useful when somebody else updates the reference to the `internship`
repository, thus the latest modifications are not _in loco_ yet.

Using `submodule` we have a repository inside another one. So, when we are at
the `osis` repository and type `git status` we see the active branch is `dev`.
But when we enter in the submodule `internship` and type `git status`, we see
the active branch is `master`. These repositories have different remote origins
(check with `git remote -v`), thus when we commit and push changes they go to
their respective repositories. Therefore, developing `osis-internship` consists
of pulling, branching, committing, pushing and everything else in the submodule
`internship`.

I would add, in conclusion, that another important motivation to move an
application to another repository is to have an independent lifecycle from the
rest of the system. This is particulary important in applications developed by
different teams under distinct context and subordination. Imagine how difficult
it is to coordinate all parties to have a synchronized release under short
iteration cycles. Using submodules, the application can evolve according to the
pace of its team and be released without interfearing on other projects'
deadlines.

[Django]: https://www.djangoproject.org
[git-submodule]: https://git-scm.com/docs/git-submodule
[git-subtree]: https://git-scm.com/book/en/v1/Git-Tools-Subtree-Merging
[OSIS]: https://github.com/uclouvain/osis
[osis-internship]: https://github.com/uclouvain/osis-internship
[Python]: https://www.python.org
[UCL]: https://www.uclouvain.be
