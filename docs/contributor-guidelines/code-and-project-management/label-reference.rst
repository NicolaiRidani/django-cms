.. _contributor-guidelines-code-and-project-management-label-reference:

###############
Label Reference
###############

Components and kinds should be self-explanatory, but statuses, needs and miscellaneous other labels are clarified below.

********
Statuses
********

A ticket’s status is its position in the pipeline - its point in our workflow.
Every issue should have a status, and be given one as soon as possible. An issue should have only one status applied to it.
Many of these statuses apply equally well to both issues and pull requests, but some make sense only for one or the other:


accepted
    (issues only) The issue has been accepted as a genuine issue that needs to be addressed. Note that it doesn’t necessarily mean we will do what the issue suggests, if it makes a suggestion - simply that we agree that there is an issue to be resolved.

non-issue
    The issue or pull request are in some way mistaken - the ‘problem’ is in fact correct and expected behaviour, or the problems were caused by (for example) misconfiguration.
    When this label is applied, an explanation must be provided in a comment.

won’t fix
    The issue or pull request imply changes to django CMS’s design or behaviour that the core team consider incompatible with our chosen approach.
    When this label is applied, an explanation must be provided in a comment

marked for rejection
    We’ve been unable to reproduce the issue, and it has lain dormant for a long time. Or, it’s a pull request of low significance that requires more work, and looks like it might have been abandoned. These tickets will be closed when we make the next release.
    When this label is applied, an explanation must be provided in a comment.

work in progress
    (pull requests only) Work is on-going.
    The author of the pull request should include “(work in progress)” in its title, and remove this when they feel it’s ready for final review.

ready for review
    (pull requests only) The pull request needs to be reviewed. (Anyone can review and make comments recommending that it be merged (or indeed, any further action) but only a core maintainer can change the label.)

ready to be merged
    (pull requests only) The pull request has successfully passed review. Core maintainers should not mark their own code, except in the simplest of cases, as ready to be merged, nor should they mark any code as ready to be merged and then merge it themselves - there should be another person involved in the process.
    When the pull request is merged, the label should be removed.

*****
Needs
*****

If an issue or pull request lacks something that needs to be provided for it to progress further, this should be marked with a “needs” label. A “needs” label indicates an action that should be taken in order to advance the item’s status.

Critical needs
==============

Critical needs (red) mean that a ticket is ‘unhealthy’ and won’t be accepted (issues) or work in progress, ready for review or ready to be merged until those needs are addressed. In other words, no ticket can have both a blue and a red label.)

more info
    Not enough information has been provided to allow us to proceed, for example to reproduce a bug or to explain the purpose of a pull request.

expert opinion
    The issue or pull request presents a technical problem that needs to be looked at by a member of the core maintenance team who has a special insight into that particular aspect of the system.

design decision
    The issue or pull request has deeper implications for the CMS, that need to be considered carefully before we can proceed further.

Non-critical needs
==================
A healthy (blue) ticket can have non-critical needs:

patch
    (issues only) The issue has been given a status: accepted, but now someone needs to write the patch to address it.

tests

docs
    (pull requests only) Code without docs or tests?! In django CMS? No way!

Other
=====

has patch
    (issues only) A patch intended to address the issue exists. This doesn’t imply that the patch will be accepted, or even that it contains a viable solution.
    When this label is applied, a comment should cross-reference the pull request(s) containing the patch.

easy pickings
    An easy-to-fix issue, or an easy-to-review pull request - newcomers to django CMS development are encouraged to tackle easy pickings tickets.

blocker
    We can’t make the next release without resolving this issue

backport
    Any patch will should be backported to a previous release, either because it has security implications or it improves documentation.

on hold
    (pull requests only) The pull request has to wait for a higher-priority pull request to land first, to avoid complex merges or extra work later. Any on hold pull request is by definition work in progress.
    When this label is applied, a comment should cross-reference the other pull request(s).









