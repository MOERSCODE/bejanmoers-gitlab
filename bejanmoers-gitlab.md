LAB 3
Create a Project
Goals
Learn how to create a git repository from scratch.
Create a “Hello, World” program
Starting in the empty working directory, create an empty directory named “hello”, then create a file named hello.rb with the contents below.

EXECUTE:
mkdir hello
cd hello
hello.rb
puts "Hello, World"
Create the Repository
You now have a directory with a single file. To create a git repository from that directory, run the git init command.

EXECUTE:
git init
OUTPUT:
$ git init
Initialized empty Git repository in /Users/jim/Downloads/git_tutorial/work/hello/.git/
Add the program to the repository
Now let’s add the “Hello, World” program to the repository.

EXECUTE:
git add hello.rb
git commit -m "First Commit"
You should see …

OUTPUT:
$ git add hello.rb
$ git commit -m "First Commit"
[master (root-commit) 4445720] First Commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.rb

 LAB 4
Checking Status
Goals
Learn how to check the status of the repository
Check the status of the repository
Use the git status command to check the current status of the repository.

EXECUTE:
git status
You should see

OUTPUT:
$ git status
On branch master
nothing to commit, working tree clean
The status command reports that there is nothing to commit. This means that the repository has all the current state of the working directory. There are no outstanding changes to record.

We will use the git status command to continue to monitor the state between the repository and the working directory.

LAB 5
Making Changes
Goals
Learn how to monitor the state of the working directory
Change the “Hello, World” program.
It’s time to change our hello program to take an argument from the command line. Change the file to be:

hello.rb
puts "Hello, #{ARGV.first}!"
Check the status
Now check the status of the working directory.

EXECUTE:
git status
You should see …

OUTPUT:
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   hello.rb

no changes added to commit (use "git add" and/or "git commit -a")
The first thing to notice is that git knows that the hello.rb file has been modified, but git has not yet been notified of these changes.

Also notice that the status message gives you hints about what you need to do next. If you want to add these changes to the repository, then use the git add command. Otherwise the git checkout command can be used to discard the changes.

Up Next
Let’s stage the change.

LAB 6
Staging Changes
Goals
Learn how to stage changes for later commits
Add Changes
Now tell git to stage the changes. Check the status

EXECUTE:
git add hello.rb
git status
You should see …

OUTPUT:
$ git add hello.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   hello.rb

The change to the hello.rb file has been staged. This means that git now knows about the change, but the change hasn’t been permanently recorded in the repository yet. The next commit operation will include the staged changes.

If you decide you don’t want to commit that change after all, the status command reminds you that the git reset command can be used to unstage that change.

LAB 7
Staging and Committing
A separate staging step in git is in line with the philosophy of getting out of the way until you need to deal with source control. You can continue to make changes to your working directory, and then at the point you want to interact with source control, git allows you to record your changes in small commits that record exactly what you did.

For example, suppose you edited three files (a.rb, b.rb, and c.rb). Now you want to commit all the changes, but you want the changes in a.rb and b.rb to be a single commit, while the changes to c.rb are not logically related to the first two files and should be a separate commit.

You could do the following:

git add a.rb
git add b.rb
git commit -m "Changes for a and b"
git add c.rb
git commit -m "Unrelated change to c"
By separating staging and committing, you have the ability to easily fine tune what goes into each commit.

LAB 8
Committing Changes
Goals
Learn how to commit changes to the repository
Commit the change
Ok, enough about staging. Let’s commit what we have staged to the repository.

When you used git commit previously to commit the initial version of the hello.rb file to the repository, you included the -m flag that gave a comment on the command line. The commit command will allow you to interactively edit a comment for the commit. Let’s try that now.

If you omit the -m flag from the command line, git will pop you into the editor of your choice. The editor is chosen from the following list (in priority order):

GIT_EDITOR environment variable
core.editor configuration setting
VISUAL environment variable
EDITOR environment variable
I have the EDITOR variable set to emacsclient.

So commit now and check the status.

EXECUTE:
git commit
You should see the following in your editor:

OUTPUT:
|
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#	modified:   hello.rb
#
On the first line, enter the comment: “Using ARGV”. Save the file and exit the editor. You should see …

OUTPUT:
git commit
Waiting for Emacs...
[master 569aa96] Using ARGV
 1 files changed, 1 insertions(+), 1 deletions(-)
The “Waiting for Emacs…” line comes from the emacsclient program which sends the file to a running emacs program and waits for the file to be closed. The rest of the output is the standard commit messages.

Check the status
Finally let’s check the status again.

EXECUTE:
git status
You should see …

OUTPUT:
$ git status
On branch master
nothing to commit, working tree clean
The working directory is clean and ready for you to continue.

Goals
Learn that git works with changes, not files.
Most source control systems work with files. You add a file to source control and the system will track changes to the file from that point on.

Git focuses on the changes to a file rather than the file itself. When you say git add file, you are not telling git to add the file to the repository. Rather you are saying that git should make note of the current state of that file to be committed later.

We will attempt to explore that difference in this lab.

First Change: Allow a default name
Change the “Hello, World” program to have a default value if a command line argument is not supplied.

hello.rb
name = ARGV.first || "World"

puts "Hello, #{name}!"
Add this Change
Now add this change to the git’s staging area.

EXECUTE:
git add hello.rb
Second change: Add a comment
Now add a comment to the “Hello, World” program.

hello.rb
# Default is "World"
name = ARGV.first || "World"

puts "Hello, #{name}!"
Check the current status
EXECUTE:
git status
You should see …

OUTPUT:
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   hello.rb

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   hello.rb

Notice how hello.rb is listed twice in the status. The first change (adding a default) is staged and is ready to be committed. The second change (adding a comment) is unstaged. If you were to commit right now, the comment would not be saved in the repository.

Let’s try that.

Committing
Commit the staged change (the default value), and then recheck the status.

EXECUTE:
git commit -m "Added a default value"
git status
You should see …

OUTPUT:
$ git commit -m "Added a default value"
[master c8b3af1] Added a default value
 1 file changed, 3 insertions(+), 1 deletion(-)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   hello.rb

no changes added to commit (use "git add" and/or "git commit -a")
The status command is telling you that hello.rb has unrecorded changes, but is no longer in the staging area.

Add the Second Change
Now add the second change to staging area, then run git status.

EXECUTE:
git add .
git status
Note: We used the current directory (‘.’) as the file to add. This is a really convenient shortcut for adding in all the changes to the files in the current directory and below. But since it adds everything, it is a really good idea to check the status before doing an add ., just to make sure you don’t add any file that is not intended.

I wanted you to see the “add .” trick, but we will continue to add explicit files in the rest of this tutorial just to be safe.

You should see …

OUTPUT:
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   hello.rb

Now the second change has been staged and is ready to commit.

Commit the Second Change
EXECUTE:
git commit -m "Added a comment"

LAB 9
Changes, not Files
Goals
Learn that git works with changes, not files.
Most source control systems work with files. You add a file to source control and the system will track changes to the file from that point on.

Git focuses on the changes to a file rather than the file itself. When you say git add file, you are not telling git to add the file to the repository. Rather you are saying that git should make note of the current state of that file to be committed later.

We will attempt to explore that difference in this lab.

First Change: Allow a default name
Change the “Hello, World” program to have a default value if a command line argument is not supplied.

hello.rb
name = ARGV.first || "World"

puts "Hello, #{name}!"
Add this Change
Now add this change to the git’s staging area.

EXECUTE:
git add hello.rb
Second change: Add a comment
Now add a comment to the “Hello, World” program.

hello.rb
# Default is "World"
name = ARGV.first || "World"

puts "Hello, #{name}!"
Check the current status
EXECUTE:
git status
You should see …

OUTPUT:
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   hello.rb

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   hello.rb

Notice how hello.rb is listed twice in the status. The first change (adding a default) is staged and is ready to be committed. The second change (adding a comment) is unstaged. If you were to commit right now, the comment would not be saved in the repository.

Let’s try that.

Committing
Commit the staged change (the default value), and then recheck the status.

EXECUTE:
git commit -m "Added a default value"
git status
You should see …

OUTPUT:
$ git commit -m "Added a default value"
[master c8b3af1] Added a default value
 1 file changed, 3 insertions(+), 1 deletion(-)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   hello.rb

no changes added to commit (use "git add" and/or "git commit -a")
The status command is telling you that hello.rb has unrecorded changes, but is no longer in the staging area.

Add the Second Change
Now add the second change to staging area, then run git status.

EXECUTE:
git add .
git status
Note: We used the current directory (‘.’) as the file to add. This is a really convenient shortcut for adding in all the changes to the files in the current directory and below. But since it adds everything, it is a really good idea to check the status before doing an add ., just to make sure you don’t add any file that is not intended.

I wanted you to see the “add .” trick, but we will continue to add explicit files in the rest of this tutorial just to be safe.

You should see …

OUTPUT:
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   hello.rb

Now the second change has been staged and is ready to commit.

Commit the Second Change
EXECUTE:
git commit -m "Added a comment"