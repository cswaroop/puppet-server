This document attempts to capture the details of our branching strategy
for Puppet Server.  This information is up-to-date as of 2015-03-06.

## THE MOST IMPORTANT THING TO KNOW

tl;dr: we are putting a freeze on merging up changes from `stable` to `master`
until Puppet Server 2.0 ships.

## Puppet Server Branches

Similar to most of the other projects at Puppet Labs, there should generally
only be two branches that are relevant at all in the puppetlabs github repo:
`master`, and `stable`.

For the period of time that exists before 2.0.0 is released there is
a third branch named `2.1.x`.  The reasoning and treatment of this branch is
described in the [Important Notes About Upcoming
Releases](#important-notes-about-upcoming-releases) section below.

In the case of Puppet Server, the `stable` branch maps to the 1.x series of
OSS Puppet Server releases.  These releases are compatible with Puppet 3.x.

The `master` branch maps to the 2.x series of OSS Puppet Server releases.  These
releases are compatible with Puppet 4.x.

## Important Notes About Upcoming Releases

At the time of this writing, we've just released OSS 1.0.8.  This was a bugfix
release and will be the OSS version of Puppet Server that is included in PE
3.8.

The next release of Puppet Server (in time, not sequential version number) will
be version 2.0.  It will be released in concert with the release of Puppet 4.0.
This release should be considered roughly equivalent to the Puppet Server 1.0.2
release in terms of functionality, and will largely only contain changes
related to Puppet 4.0 compatibility.

The changes for 1.1 have started to land in the `stable` branch now.  Some of
them are too risky to introduce into the `master` branch given our proximity to
the 2.1 release.  Therefore, it is critical that we do *not* do any merges from
`stable` to `master` until after 2.1 has shipped.

Work on new functionality intended for the 2.1 release has commenced prior to
the 2.0 release.  The `2.1.x` branch is where active development on 2.1
functionality needs to land during the period before the 2.0.0 release.  *This
is a deviation from the normal process of developing new functionality on top
of the master branch* because the master branch has been frozen until 2.0.0 is
released.

The 2.1.x branch will be established by branching from master and then
merging-up the 1.0.8 tag, but not the stable branch because stable contains
risky changes destined for 1.1.  The master branch does not contain this
merge-up because of the code freeze ahead of the 2.0.0 release.

There will be a quick turnaround for a 2.1 release following the 2.0 release.
This 2.1 release will be 2.0, plus the bugfixes in 1.0.8, plus a URL
compatibility layer that will allow Puppet 3.x agents to talk to Puppet Server
2.x (for more info see https://tickets.puppetlabs.com/browse/SERVER-526).

Immediately following the 2.0 release we'll kill the 2.1.x branch and return to
stable + master.  This will be accomplished by advancing (using a
non-fast-forward merge commit) the master branch to the HEAD of the 2.1.x
branch.  The 2.1.x branch will be deleted and the associated CI pipeline will
be disabled or removed.  At this point the master branch will return to the
normal state of being the head of active development for the most recent
feature release, 2.1.

NOTE: once 2.0 has been released *we'll still be under a merge freeze from
stable to `master` and `2.1.x` until 2.1 ships*.

Some time following the release of 2.1, we'll do a 1.1 and 2.2 release,
hopefully in close proximity to one another.  These will be the next major
feature releases (as opposed to 2.0 and 2.1, which are mostly just
compatibility releases), and will contain several new features, tuning
improvements, etc.

We'll update this document to reflect changes to that restriction as things
progress.

In summary:

 * `stable` is for 1.1.x and too risky to merge up into master or 2.1.x until
   after 2.1 ships.
 * `master` is for 2.0.x and frozen ahead of the 2.0.0 release.  It will
   advance to the HEAD of `2.1.x` when 2.0.0 ships.
 * `2.1.x` is for 2.1.x ahead of the 2.0.0 release.  It will be removed once
   2.0.0 ships and master has taken over as the target for 2.1.x work.

## "Normal" branching workflow

Under normal circumstances, the `stable` branch will be used for bugfix releases
off of the last stable "Y" release.  PRs should only be targeted at stable if
they are intended to go out in a z/bugfix release, and all code merged into the
stable branch should be merged up to the master branch at regular intervals.

Most PRs should normally be targeted at the `master` branch, which is where feature
work will be going on for the next X/Y release.

Ideally, as soon as a new x/major release is shipped (e.g. Puppet Server 2.0), that
would become the stable branch and there would no additional planned 1.x releases.
Because of the drastic nature of the changes between Puppet 3 and Puppet 4, it's
possible that we'll need to continue to do both 1.x and 2.x releases of Puppet
Server for some period of time.  If we find ourselves in a situation where we
need three branches (e.g. a stable branch for 1.x and another stable branch for 2.x),
we may introduce a third branch (e.g. we might have `1.x`, `stable`->2.0.x, and
`master`->2.1.x, or something along those lines).  We will update this document as
the plans solidify.  

