.. _contributor-guidelines-code-and-project-management-how-we-process-tickets:

######################
How We Process Tickets
######################

Tickets should be:
* given a status
* marked with needs
* marked with a kind
* marked with the components they apply to
* marked with miscellaneous other labels
* commented

A ticket’s status and needs are the most important of these. They tell us two key things:
* status: what stage the ticket is at
* needs: what next actions are required to move it forward

Needless to say, these labels need to be applied carefully, according to the rules of this system.

GitHub’s interface means that we have no alternative but to use colours to help identify our tickets. We’re sorry about this. We’ve tried to use colours that will cause the fewest issues for colour-blind people, so we don’t use green (since we use red) or yellow (since we use blue) labels, but we are aware it’s not ideal.

*****************************************
django CMS ticket processing system rules
*****************************************

* one and only one status must be applied to each ticket
* a healthy ticket (blue) cannot have any critical needs (red)
* when closed, tickets must have either a healthy (blue) or dead (black) status
* a ticket with critical needs must not have non-critical needs or miscellaneous other labels
* has patch and on hold labels imply a related pull request, which must be linked-to when these labels are applied
* component, non-critical need and miscellaneous other labels should be applied as seems appropriate



Status
======

The first thing we do is decide whether we accept the ticket, whether it’s a pull request or an issue. An accepted status means the ticket is healthy, and will have a blue label.
Basically, it’s good for open tickets to be healthy (blue), because that means they are going somewhere.


.. important::
    Accepting a ticket means marking it as healthy, with one of the blue labels

issues
    The bar for status: accepted is high. The status can be revoked at any time, and should be when appropriate. If the issue needs a design decision, expert opinion or more info, it can’t be accepted.

pull requests
    When a pull request is accepted, it should become work in progress or (more rarely) ready for review or even ready to be merged, in those rare cases where a perfectly-formed and unimprovable pull request lands in our laps. As for issues, if it needs a design decision, expert opinion or more info, it can’t be accepted.
    No issue or pull request can have both a blue (accepted) and a red, grey or black label at the same time.

Preferably, the ticket should either be accepted (blue), rejected (black) or marked as having critical needs (red) as soon as possible. It’s important that open tickets should have a clear status, not least for the sake of the person who submitted it so that they know it’s being assessed.

Tickets should not be allowed to linger indefinitely with critical (red) needs. If the opinions or information required to accept the ticket are not forthcoming, the ticket should be declared unhealthy (grey) with marked for rejection and rejected (black) at the next release.


Needs
======

Critical needs (red) affect status.
Non-critical needs labels (pink) can be added as appropriate (and of course, removed as work progresses) to pull requests.
It’s important that open tickets should have a clear needs labels, so that it’s apparent what needs to be done to make progress with it.


Kinds and components
====================

Of necessity, these are somewhat porous categories. For example, it’s not always absolutely clear whether a pull request represents an enhancement or a bug-fix, and tickets can apply to multiple parts of the CMS - so do the best you can with them.


Other labels
============
backport, blocker, has patch or easy pickings labels should be applied as appropriate, to healthy (blue) tickets only.

Comments
========
At any time, people can comment on the ticket, of course. Although only core maintainers can change labels, anyone can suggest changing a label.


