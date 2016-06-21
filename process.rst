A Proposal for Proposals
========================

Background
----------

Recently Anthony Cowley presented a critique of his experiences contributing to
GHC. One of the major themes in Anthony's critique was that the current
mechanisms for proposing features and changes to GHC are less than approachable.
In particular our tools for managing the specification and surrounding
discussions make participation harder than necessary. We are considering
revising our process for proposal consideration, for which we would like to hear
your opinions and experiences.

Over the last few weeks I have been looking at how other open-source compiler
projects address these issues (focusing on LLVM, Python, and Rust). While all
struggle with many of the same challenges that GHC faces, it's clear that we can
do better than what we currently have.

To be clear, the process discussed below only affects proposal of the following
three classes,

* A syntactic change to GHC Haskell (e.g. a language extension introducing new
  syntax)

* A major change to the user-visible behaviour of the compiler (e.g. the recent
  change in super-class solving, and ``-Wall`` behavior)

* The addition of major features to the compiler (e.g. ``-XTypeInType``, GHCi
  commands)

* The removal of major features from the compiler

Note how this does not include changes to the Haskell Report (which are overseen
by the Haskell Prime committee) nor the core libraries (which is covered by the
Core Libraries Committee).

The Problem
-----------

Let's start by examining a few of the shortcomings of our current process,

1. Higher than necessary barrier-to-entry.

   Involving oneself in the proposal process requires following a Trac ticket,
   a Wiki page, one or more mailing list threads, and possibly Phabricator,
   each of which requires a separate account

2. The proposal itself and the discussion surrounding it are too decoupled.

   The proposal specification itself is kept on the Trac Wiki, design
   discussion occurs in largely independent Trac tickets, and further
   discussion is fragmented across several mailing lists, Reddit, and IRC. This
   leads to repetitive and often difficult-to-follow discussions.

3. The phases of the lifecycle of a proposal are not clearly defined.

   This leads to unfortunate cases like that of `ShortImports` where discussion
   proceeds for too long without a concrete specification. This drains effort
   from both the author of the proposal and those discussing it.

4. The process that exists isn't always consistently applied.

   While this arguably has improved in recent years, there are cases where
   features have been added without a formal specification or perhaps less
   discussion than would be desired.

One of the points that Anthony has rightly emphasized is that our process
suffers from having too many decoupled channels of communication. To make
matters worse, none of these channels particularly excel at their task:

* Mailing lists are difficult to reference and are too coarse-grained,
  requiring that users actively filter for the threads in which they have
  interest.
  
* Trac's Wiki has poor version control, no inline commenting functionality, and
  a mark-up syntax which is foreign to many potential users

* Trac tickets are nearly entirely decoupled from the specification itself,
  making it hard to follow both the spatial and chronological structure of a
  discussion.

Of course, Trac also has several nice properties,

* Cross-referencing tickets, wiki pages, and comments is quite easy once you
  learn the syntax

* It already exists and a significant fraction of GHC developers are already
  quite comfortable with it


Proposed change
---------------

I propose that we adopt a variant on the
`Rust RFC scheme <https://github.com/rust-lang/rfcs#what-the-process-is>`_,
building our proposal process on top of existing code review tools such as
Github. These tools enjoy a large user-base, and are fairly effective in
facilitating discussion.

GHC committee
-------------

The process involves forming a small committee, which is responsible for
deciding whether proposed changes should be accepted after discussion
within the community. 

The committee should be large enough to reflect the
diversity of GHC's contributor- and user-base but small enough to ensure a sense
of individual responsibility among its members.  I suggest that we start in the range of six to
eight members.

To simplify the committee selection process, I propose that the committee is chosen as folows.
We seek nominations (including self-nominations); and then Simon Peyton-Jones and Simon Marlow
select members from those nominations.  This is simple and clear.  It is also not very
democratic, but Simon & Simon did originate GHC, and it is far from clear that a voting
process would yield a better result.  (Also: who could vote? Who can decide who can vote? etc)

The GHC committee will have two principal roles,

* *Deciding proposal acceptance.* At the proposal author's invitation, the committee makes the decision over
  whether a proposal should be accepted, in light of a set of defined criteria
  (see below).

* *Judging whether the proposal process should be applied.* There will no doubt
  be proposal where it is unclear whether the burden of the proposal process is
  necessary. The responsibility for determining the scope of the process lies
  with the committee.

Proposal process
----------------
Each proposal goes through the following stages:

1. The process begins with the opening of a pull request to the GHC RFC's
   repository, which includes a document (derived from a provided template)
   specifying the proposed change. This document is the primary specification
   of the feature and takes the place of the current Trac Wiki document.
   
2. Community members (including members of the proposal committee) will discuss
   the proposal. The submitter is responsible for amending the specification to
   account for collected comments. It should be expected that the proposal will
   change during this process, which may last from days to months.

3. When the submitter feels that the proposal is complete, and not being improved by further discussion,
   they can submit the proposal to the proposal committee for a decision.

4. Within two (exceptionally three) weeks the committee will arrive at a consensus as to
   whether the proposal in its submitted form meets a set of criteria for
   inclusion (see below) while weighing the opinions expressed by the community.

   If the proposal is rejected then it will be sent back to the submitter along
   with a rationale (referencing the criteria defined below). This is by no
   means a final decision; the submitter may amend and re-submit the proposal to
   address the concerns of the committee

4. When the proposal is accepted the pull request will be merged and the
   document will be preserved in the RFCs repository as a permanent
   specification for the feature.
   
5. The proposer will create a Trac ticket linking to the proposal to
   provide a place track the progress of the implementation.

6. The submitter may choose to implement the proposal after acceptance, but is
   under no obligation to do so. (Of course, neither is anyone else.)

7. During implementation, it is very likely that the specification will be refined.
   The implementor should keep the specification up to date. 

Since the RFC wiki pages already existing on Trac represent a significant amount
of effort and knowledge, we'll make an effort to import these into the RFC
repository if this scheme is adopted.

Criteria for new features
--------------------------

The committee is  responsible for weighing a variety of factors when deciding whether to adopt a feature.  These are all judgement calls.

1. *The problem*. What exactly is the problem that the feature solves solves?  Is it an important problem, felt by many users, or is it very specialised?

2. *The right solution; generality*.  Elegance, economy, and generality are important.   Sometimes a problem has lots of solutions, none of whcih have that "aha" feeling of "this is the Right way to solve this". A classis example is that of records, for which we have had endless proposals (including many from the GHC authors themselves) none of which felt "right", and none of which made it into GHC.

3. *Fit with the language*.  If we just throw things into GHC willy-nilly, it will become a large ball of incoherent and inconsistent mud.  We strive to add features that are consistent with the rest of the language.

4. *Specification cost*.  Does the benefit of the feature justify the extra complexity in the language specification?  Does the new feature interact awkwardly with existing features, or does it enhance them?  How easy is it for users to understand the new feature?

5. *Implementation cost*.  How complex is the implementation likely to be?  Is it highly localised, or does it require pervasive changes?  As an author, it may look like you are giving GHC free work, but this viewpoint doesn't align with the reality of a large project with a timescale in decades. Writing code is cheap; maintaining it is expensive.

Alternatives
------------

Of course, group decision-making processes are difficult to manage and tools
will only bring you so far. While the Rust scheme does seem to function more
smoothly than our current scheme, it is not free of issues. These issues may
apply to the above proposal as well,

* Github discussions in particular don't scale terribly well; the lack of
  hierarchical threading means that long threads can become difficult to follow

* The ease of commenting may bring a slightly diminished signal-to-noise ratio
  in collected feedback, particularly on easily bike-shedded topics.

There are a few alternatives which are worth considering,

* we continue to build on Trac, but attempt to be more rigorous with our
  current scheme. Namely we attempt to better document and more consistently
  enforce

* we move to something closer to the Python PIP scheme. Here a committee is
  formed for each proposal; discussions typically occur on specially-created
  mailing lists.

* something else...


Moving closer to the Rust process
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Also, there are a few facets of the Rust process which the proposed process does
not carry over for a variety of reasons:

* *Shepherds*. In the Rust process each submitted proposal is assigned a
  shepherd. This is a trusted core developer who is charged with keeping the
  proposal moving through the process. At the moment GHC lacks the contributor
  count to guarantee this.

* *Final comment period*. The Rust process defines a portion of the proposal
  lifecycle known as the "final comment period". This is a (typically one-week)
  period directly before the responsible sub-team makes its decision which is
  widely announced to solicit final comments from the community. This period is
  omitted from the process described above; instead it is up to the proposal
  submitter to ensure that sufficient discussion is solicited.

Open Questions
--------------

There are still questions regarding the desired scope of the process. While we
want to process to ensure that changes which affect the user-facing aspects of
the compiler are well-considered, we certainly don't want the process to become
unduly burdensome. This is a careful balance which will require care to maintain
moving forward.

Acknowledgments
---------------

Thanks to the Rust contributors ``eddyb``, ``nmatsakis``, and ``steveklabnik``
for useful discussions sharing their experiences in the Rust community. Also,
thanks to Anthony Cowley for his persistence in raising his concerns and helpful
discussions over the course of this effort.
