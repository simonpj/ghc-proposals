Background
==========

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
do better than we have done.

To be clear, in the following we mean "proposal" to include proposed any change
that involves,

 * A syntactic change to GHC Haskell (e.g. a language extension introducing new
   syntax)
 
 * A major change to the user-visible behaviour of the compiler (e.g. the recent
   change in super-class solving, and -Wall behavior)
 
 * The addition of major features to the compiler (e.g. -XTypeInType, GHCi
   commands)


The problem
===========

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
   a mark-up syntax which is foreign to many users
 
 * Trac tickets are nearly entirely decoupled from the specification itself,
   making it hard to follow both the spatial and chronological structure of a
   discussion.


What to do about it
===================

I propose that we adopt a variant on the :ref:`Rust RFC scheme
(https://github.com/rust-lang/rfcs#what-the-process-is)`, building our proposal
process on top of existing code review tools such as Github. These tools enjoy a
large user-base, and are fairly effective in facilitating discussion.

The process involves forming a small committee, which is openly responsible for
casting a final vote to accept or reject proposed changes after discussion
within the community. The committee includes major drivers of the GHC community
and elected voluntary members.

Each proposal goes through the following stages:

 1. The process begins with the opening of a pull request to the GHC RFC's
    repository, which includes a document (derived from a provided template)
    specifying the proposed change. This document is the primary specification
    of the feature and takes the place of the current Trac Wiki document.
    
 2. Community members (including members of the proposal committee) will discuss
    the proposal. The submitter is responsible for amending the specification to
    account for collected comments.

 3. When submitter feels that the proposal is reasonably concrete, they can
    submit the proposal to the proposal committee for a final decision.
    The committee will arrive at a consensus as to whether the proposal in its
    submitted form is well-specified and sufficiently useful to merit inclusion.
    It will do so accounting for the community comments provided on the ticket.

    If the proposal is rejected then it will be sent back to the submitter along
    with a rationale. The submitter may then amend and re-submit the proposal.

 4. When the proposal is accepted the pull request will be merged and the
    document will be preserved in the RFCs repository as a permanant
    specification for the feature.
    
 5. The GHC maintainers will create a Trac ticket linking to the proposal to
    provide a place to collect resources and discussion relevant to the
    implementation effort.

 6. The specification shall be kept up to date with the implementation as it
    changes. The submitter may choose to implement their proposal but is under
    no obligation to; any other interested party may also do so. The RFC can
    collect links to relevant

Since the RFC wiki pages already existing on Trac represent a significant amount
of effort and knowledge, we'll make an effort to import these into the RFC
repository if this scheme is adopted.


Alternatives
============

While the Rust scheme does seem to function more smoothly than our current
scheme, it is not free of issues,

 * Github discussions, in particular, don't scale terribly well; the lack of
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


Acknowledgments
===============

Thanks to the Rust contributors ``eddyb``, ``nmatsakis``, and ``steveklabnik``
for useful discussions sharing their experiences in the Rust community. Also,
thanks to Anthony Cowley for his persistence in raising his concerns and helpful
discussions.

