# Day 1 Notes

## What is DVCS?
**DVCS (Distributed Version Control System)** allows every developer to have a full copy of the repository, including history, on their local machine.

### Advantages over CVCS (Centralized Version Control System)
- Work offline without needing constant server access.
- Faster operations (commits, diffs, logs happen locally).
- Easier branching and merging.
- No single point of failure — every clone is a backup.
- Collaboration is more flexible.

------------------------------+------------------------------

## Git Workflow
**Flow:** Working Directory → Staging Area → Local Repo → Remote Repo

+------------------+       git add        +------------------+
|  Working         | ------------------>  |  Staging Area    |
|  Directory       |                      |                  |
+------------------+                      +------------------+
                                                |
                                                | git commit
                                                v
                                        +------------------+
                                        |  Local Repo      |
                                        +------------------+
                                                |
                                                | git push
                                                v
                                        +------------------+
                                        |  Remote Repo     |
                                        +------------------+


---------------------------------+------------------------------

Explanation and examples of the following commands:

1. git add

- Moves changes from Working Directory to Staging Area.
  
  *git add file1.txt
   git add .

       +------------------+
       |      git add     |
       +------------------+ 


2. git commit

- Saves staged changes into the Local Repo.

  *git commit -m "Added new feature"
        
       +------------------+
       |    git commit    |
       +------------------+


3. git restore

- Restores file(s) from last commit or staging.

  *git restore file1.txt
   git restore --staged file1.txt  # unstage a file
       
       +------------------+
       |   git restore    |
       +------------------+
 
       
4. git rm --cached

- Removes a file from staging/index but keeps it in working directory.

  *git rm --cached secret.txt

       +------------------+
       | git rm --cached  |
       +------------------+


5. git log

- Shows commit history with details.

  *git log

       +------------------+
       |     git log      |
       +------------------+


6. git log in oneline

- Shows commit history in compact form.
   
 *git log

      +------------------+
      | git log --oneline|
      +------------------+

---
##  Completion
With these notes, I now understand the basics of DVCS vs CVCS, the Git workflow, and the essential Git commands that move code from my working directory all the way to the remote repository.

