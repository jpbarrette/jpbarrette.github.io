Strategies
Different directories (git-p4 Default)

Taken from documentation: http://git-scm.com/docs/git-p4

Submitting changes from a Git repository back to the p4 repository requires a separate p4 client workspace. 

Currently, the way git-p4 commands are implemented, it requires the users to have two different directories, one which is git tracked, the other being a perforce client. Mostly, their strategy is to import perforce changelists with perforce client into git repository. Work in the git repository. Then, when we want to checkin into perforce, takes all the different changesets from git which aren't into perforce, creates patches for them and applies them onto the perforce directory. Then, from the perforce directory, the user can checkin into perforce.

In my case, I needed to have both repository at the same location. Mostly some of the build tools required a working perforce repository. So, I developped new commands for git-p4 that would allow the "same directory" strategy.

I've modified git-p4 to include the command "edit" (git-p4 edit), on which this strategy relies on. You can access the code here: GitHub repository. You can put this file in the "/usr/bin" repository for GitExtension shell if you want to.

A better way of mixing perforce and git usage, is to use the same directory. The idea here is to use git as the main VCS, and use perforce as a "slave". This means that when you only use git until you need to checkin into main branch. So, when you want to update to newer changelists, you just merge them into your branch (or you can re-base, which I don't recommend). Then, when you need to checkout some code you tell perforce client on which changelist you are "synced" to ("git-p4 edit"). Since git-p4 knows which git's changeset correspond to which perforce's changelist, it will do a "p4 sync -k ...@your_changelist_number_you_are_synced_to". Just after that it will add all files that you modified into the pending changes. So, in perforce you'll see all changed files.

Talk about how to create a commit which takes into account the git parent (not only the p4 parent)