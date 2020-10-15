# Git. Adding files to stage

If we review [the oficial documentation](https://git-scm.com/docs/git-add), the command used to add files to the git stage is `git add`.

Lets see some particularities.

## git add [files]

It is the most basic way to add files, providing all file names to process in stage as command parameters. Example: `git add "README.md" LICENSE ...`

## __git add *__ vs. __git add .__

It should seem at a glance that both do the same (record all untracked files, adding new files, changes and deletions to stage), but this fact is wrong.

- `git add *` don't take into account hidden files like `.gitignore` neither files marked to deletion using terminal comands like `rm`. If the documentation is read in deep, this way lets you select files by regex.

- `git add .` allows you do that but not at all. Let's go deeper...

## How to ensure all/certain changes are tracked into the stage

Here a table for quick understanding:

**Git Version 1.x:**

|                |      New files     |   Mod files        |    Deleted files   | Entire working tree |                                                           |
|---------------:|:------------------:|:------------------:|:------------------:|:-------------------:|-----------------------------------------------------------|
| **git add -A** | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  | Stage All (new, modified, deleted) files (entire tree)    |
| **git add .**  | :heavy_check_mark: | :heavy_check_mark: |         :x:        |         :x:         | Stage New and Modified files only (current dir/subs)      |
| **git add -u** |         :x:        | :heavy_check_mark: | :heavy_check_mark: |         :x:         | Stage Modified and Deleted files only (current dir/subs)  |

**Git Version 2.x:**

Normalizes the use of **.** in all modifiers.

|                                |      New files     |   Mod files        |    Deleted files   | Entire working tree |                                                        |
|-------------------------------:|:------------------:|:------------------:|:------------------:|:-------------------:|--------------------------------------------------------|
| **git add -A**                 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  | Stage All (new, modified, deleted) files (entire tree) |
| **git add .**                  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |         :x:         | Stage New and Modified files only (current dir/subs)   |
| **git add --ignore-removal .** | :heavy_check_mark: | :heavy_check_mark: |         :x:        |         :x:         | Stage New and Modified files only (current dir/subs)   |
| **git add -u**                 |         :x:        | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:  | Stage Modified and Deleted files only (entire tree)    |

**Long-form flags:**

- `git add -A` is equivalent to `git add --all`
- `git add -u` is equivalent to `git add --update`


## Let's play...

```
git init
echo Change me > change-me
echo Delete me > delete-me
git add change-me delete-me
git commit -m initial

echo OK >> change-me
rm delete-me
echo Add me > add-me

git status
# Changed but not updated:
#   modified:   change-me
#   deleted:    delete-me
# Untracked files:
#   add-me

git add .
git status

# Changes to be committed:
#   new file:   add-me
#   modified:   change-me
# Changed but not updated:
#   deleted:    delete-me

git reset

git add -u
git status

# Changes to be committed:
#   modified:   change-me
#   deleted:    delete-me
# Untracked files:
#   add-me

git reset

git add -A
git status

# Changes to be committed:
#   new file:   add-me
#   modified:   change-me
#   deleted:    delete-me
```

## Conclusions

If you are in a other folder different of root and you use any form of **`git add .`**, only the files under this and their subdirectories are tracked.

So use **`git add -A`** if your purpouse is tracking all changes in entire working tree independently where that command is executed.
