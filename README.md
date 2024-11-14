# git-clean-practice
A simple repo for practicing git clean commands

## Exercises

### 01 Simple clean

git clean deletes untracked files, as you can't recover easily from this the default git settings make sure
you are consciously doing this by requiring one of the modifiers to be added. To see that in action follow the steps below

1. Open a terminal at the project root and run `./01-create-test-files.sh`
2. You should see an output similar to 
```
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        created-file-1.txt
        created-file-2.txt
```
3. These untracked files will stop us if we try to switch branches, as we don't need to have them under source control try to run `git clean` to remove them
4. If you have the default configuration you'll see this message `fatal: clean.requireForce defaults to true and neither -i, -n, nor -f given; refusing to clean`
3. As we could accidentally delete something useful we can use `git clean -n` to get a preview
4. Running that should give you this output
```
Would remove created-file-1.txt
Would remove created-file-2.txt
```
5. As we don't need those files this time add the -f flag to force the deletion i.e. run `git clean -f`
6. Now when you run git status those created files won't show up

### 02 Simple clean in interactive mode

Sometimes the dry run will show up files that we want
to keep. While we could use the information returned to 
add the files we want to retain to the index and then use 
the simple version of the git clean command as above, git clean's interactive mode
gives us more power. Let take it for a spin

1. Open a terminal at the project root and run `./02-create-test-files.sh` this generates another set of test files
2. Run `git status` that should give this output
```
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        created-file-1.txt
        created-file-10.png
        created-file-2.png
        created-file-3.txt
        created-file-4.png
        created-file-5.txt
        created-file-6.png
        created-file-7.txt
        created-file-8.png
        created-file-9.txt
```
3. Run `git clean -i` to start git clean in interactive mode
```
Would remove the following items:
  created-file-1.txt   created-file-2.png   created-file-4.png   created-file-6.png   created-file-8.png
  created-file-10.png  created-file-3.txt   created-file-5.txt   created-file-7.txt   created-file-9.txt
*** Commands ***
    1: clean                2: filter by pattern    3: select by numbers    4: ask each             5: quit                 6: help
What now> 
```

4. enter a at the What Now prompt, 
5. answer N to the first prompt and y to all the others
6. Now when you run `git status` created-file-1.txt should be the only file remaining
7. run `git clean -f` to get back to the initial state

#### Interactive using a filter

If there are lots of files we want to save then using a filter could be more efficient

1. Open a terminal at the project root and run `./02-create-test-files.sh` this generates another set of test files
2. Run `git clean -i` to start git clean in interactive mode
3. This time enter f at the prompt
4. enter '*.png' at the Input ignore patterns>> prompt
5. press enter the second time the Input ignore patterns>> prompt is displayed
6. When the What now prompt is displayed for the second time press c

The text below captures the whole interaction

```
Would remove the following items:
  created-file-1.txt   created-file-2.png   created-file-4.png   created-file-6.png   created-file-8.png
  created-file-10.png  created-file-3.txt   created-file-5.txt   created-file-7.txt   created-file-9.txt
*** Commands ***
    1: clean                2: filter by pattern    3: select by numbers    4: ask each             5: quit                 6: help
What now> f
  created-file-1.txt   created-file-2.png   created-file-4.png   created-file-6.png   created-file-8.png
  created-file-10.png  created-file-3.txt   created-file-5.txt   created-file-7.txt   created-file-9.txt
Input ignore patterns>> *.png
  created-file-1.txt  created-file-3.txt  created-file-5.txt  created-file-7.txt  created-file-9.txt
Input ignore patterns>> 
Would remove the following items:
  created-file-1.txt  created-file-3.txt  created-file-5.txt  created-file-7.txt  created-file-9.txt
*** Commands ***
    1: clean                2: filter by pattern    3: select by numbers    4: ask each             5: quit                 6: help
What now> c
Removing created-file-1.txt
Removing created-file-3.txt
Removing created-file-5.txt
Removing created-file-7.txt
Removing created-file-9.txt
```

#### Interactive mode selecting by numbers

1. Run `git clean -f` to get back to initial state
2. Run `./02-create-test-files.sh` to generate test files
3. Run `git clean -i` to enter interactive mode
4. Enter s at the What now prompt
5. At the select items to delete prompt enter some numbers for the files you want to remove followed by enter, You can do this more than once if you like
6. When you're finished selecting files press enter at the prompt
7. Press c at the What now prompt and the selected files will be removed

```
*** Commands ***
    1: clean                2: filter by pattern    3: select by numbers    4: ask each             5: quit                 6: help
What now> s
    1: created-file-1.txt     2: created-file-10.png    3: created-file-2.png     4: created-file-3.txt     5: created-file-4.png     6: created-file-5.txt
    7: created-file-6.png     8: created-file-7.txt     9: created-file-8.png    10: created-file-9.txt
Select items to delete>> 5 6 8 10
    1: created-file-1.txt     2: created-file-10.png    3: created-file-2.png     4: created-file-3.txt   * 5: created-file-4.png   * 6: created-file-5.txt
    7: created-file-6.png   * 8: created-file-7.txt     9: created-file-8.png   *10: created-file-9.txt
Select items to delete>> 1 2
  * 1: created-file-1.txt   * 2: created-file-10.png    3: created-file-2.png     4: created-file-3.txt   * 5: created-file-4.png   * 6: created-file-5.txt
    7: created-file-6.png   * 8: created-file-7.txt     9: created-file-8.png   *10: created-file-9.txt
Select items to delete>> 
Would remove the following items:
  created-file-1.txt   created-file-10.png  created-file-4.png   created-file-5.txt   created-file-7.txt   created-file-9.txt
*** Commands ***
    1: clean                2: filter by pattern    3: select by numbers    4: ask each             5: quit                 6: help
What now> c

```




NB selecting c before filtering or selecting will delete all the files

### 03 Directories

1. Run `git clean -f` to get back to initial state
2. Run `./03-create-test-files.sh` to generate test files
3. Run `git status` to see what we're dealing with, this time the images are in their own directory
```
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        created-file-1.txt
        created-file-2.txt
        images/
```
4. run `git clean -f` followed by `git status`
5. Are you surprised by what you see? 
```
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        images/
```
6. To remove untracked directories `git clean -fd` is needed

NB interactive and dryrun mode both ignore untracked directories unless the d flag is specified.
try a few experiments of your own to see this in action


### 04 Git Ignore

1. Run `git clean -fd` to get back to initial state
2. Run `./04-create-test-files.sh` to generate test files
3. Run `git status` and you should see this:
```
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        created-file-1.txt
        created-file-2.txt
```
4. Check in your ide and you should also see that an in-git-ignore.txt has been generated, as its in gitignore its not marked as untracked
5. Run `git clean -n`
6. Run `git clean -Xn`
6. Run `git clean -xn`
7. What do you notice?
```
... git-clean-practice % git clean -n
Would remove created-file-1.txt
Would remove created-file-2.txt
... git-clean-practice % git clean -Xn
Would remove in-git-ignore.txt
... git-clean-practice % git clean -xn
Would remove created-file-1.txt
Would remove created-file-2.txt
Would remove in-git-ignore.txt
```

Any files matching a gitignore pattern are excluded when a standard clean is run. adding X only removes the files that would be 
ignored and adding lower case x removes all the normal files and the gitignored files.


Tip - if you don't want to see the output add q to your command
try running `git clean -fxq`


## Challenge

1. Run `git clean -fd` to get back to initial state
2. Run `./02-create-test-files.sh` to generate test files
3. Come up with a git clean command that removes all untracked files apart from created-file-7.txt

https://git-scm.com/docs/git-clean
