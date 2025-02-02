# GitHub Project Standards

![Project Standards](/img/1.png)


- **Master (Main):** Do not modify files on the master branch, only push project features from **develop**. When everything is complete, push to *production*. However, if the **master** branch encounters an issue in the production environment, push to the **hotfixes** branch.
- **Develop:** The development branch for features, creates **feature branches**, and contains all code that will be released in the next version.
- **Feature branches:** Each branch corresponds to a feature. After completion, push to **develop**. The **develop** branch will then push to the **Release** branch.
- **Release branches:** These are pushed from **develop**, and after completing tasks and fixing bugs, merge them into the **master** branch. Then delete the **old release version** and continue creating features.
- **Hotfixes:** A branch pushed from **master** to fix issues in the production environment. Once fixed, push to **develop**, and the process continues from **develop → feature → release → master**. However, if the fix is quick, it may go directly to **master**.

# Git Flow Workflow Implementation

## 1. Create the develop branch and upstream the develop branch
```bash
git branch develop
git push -u origin develop
```

## 2. Create a feature label on GitHub

- Issues → Choose Label → Edit label
![Edit Label](/img/2.jpg)


- Create a task feature. Note # 1 (task 1).
![Task Creation](/img/3.png)

## 3. `Checkout -b` to a `feature` based on task 1 from the `develop` branch

```bash
git checkout -b feature/1-add-cart.model.file develop
```

## 4. Add the newly created `cart.model.js `file and commit it to the corresponding task (issue) `#1`

```bash
git add cart.model.js
git commit -m '#1 - phongthanhbuiit update file model cart.model.js'
git push
```

![Commit](/img/4.png)

## 5. The manager compares & creates a pull request

![Pull Request](/img/5.png)

**NOTE: REMEMBER TO SELECT THE DEVELOP BRANCH**
![Select Develop Branch](/img/6.png)

Make sure to mention `#1` again
![Mention Issus](/img/7.png)

`Assign` a person and `merge` the git
![Assign and Merge](/img/8.png)

## 6. Switch to develop, then git pull
```bash
git checkout develop
git pull
```
## 7. Create a release branch from the develop branch
```bash
git checkout -b release-1.0.0 develop
```
- Create a tag
```bash
git tag 'v1.0.0'
git push --tag
```
- Merge from the develop branch
```bash
git merge develop
```
- Git push
```bash
git push
```
- Merge request the release-1.0.0 branch with main
![Merge Request](/img/9.png)

## 8. Push to master and deploy to production
- Checkout and pull code from main
```bash
git checkout main
git pull
```
- Or merge the code from the main branch from the release-1.0.0 branch
```bash
git checkout main
git merge release-1.0.0
git push
```
- Create a Tag version v1.0.0 if it doesn’t exist
```bash
git tag 'v1.0.0'
git push --tags
```
## 9. Delete release-1.0.0 and feature branches

```bash
git branch -d release-1.0.0
git push origin -d release-1.0.0
git branch -d feature/1-add-cart.model.file
git push origin -d feature/1-add-cart.model.file
```
- Remember to checkout to develop after deleting to avoid forgetting

```bash
git checkout develop
```
# Another Example

- Create issues
![Merge Request](/img/9.png)

```bash
git checkout develop
git checkout -b feature/6-create-product-model
git add product.model.js
git commit -m '#6 - add file product'
git push
```

- The leader's task is to Compare & Pull request to the develop branch
![Leader Compare](/img/10.png)

```bash
git checkout develop
```

- Checkout to develop, pull, and delete
```bash
git checkout develop
git pull
git branch -d feature/6-create-product-model 
git push origin -d feature/6-create-product-model 
```

- Create release v1.1.0
```bash
git checkout -b release-v1.1.0 develop
git merge develop  
git push
```

- Merge release v1.1.0 into main
![Merge Release](/img/11.png)

- Pull from main and delete release and feature
```bash
git checkout main
git pull
git branch -d release-v1.1.0       
git push origin -d release-v1.1.0 
```

- Create Tag v1.1.0 and push the code
```bash
git tag 'v1.1.0'
git push --tags
```

- Checkout to develop
```bash
git checkout develop
```

# If Main Is Issues

- Create a hotfix branch
```bash
git checkout -b hotfixes
```

- Fix the file `product.model.js`
- Add and commit #6

![Hotfix](/img/12.png)
- You'll see `#6 - fix product`

- Leader will pull into main or develop as needed. Here, it will be pulled into main
![Leader Pull](/img/13.png)

- Git checkout main and pull
```bash
git checkout main
git pull
```

- Delete the hotfixes branch
```bash
git branch -d hotfixes
git push origin -d hotfixes
```

- Remember to Checkout to develop
```bash
git checkout develop
```

# Git Rebase vs Merge
![Rebase and Merge](/img/15.jpg)

*Rebase is better than Merge*
- Rebase provides a cleaner commit history on a personal level. Your commits are moved to the top, allowing you to continue your work without worrying about the messy commit history below.
- Merge, on the other hand, combines conflicts and arranges commits chronologically.

*When to use Rebase or Merge*
- It depends on the company:
    - In a large team or with a complex commit history → use Rebase.
    - In small projects → Merge may suffice.
- However, Rebase is generally preferred, though both should be used as needed.

# Question 1: How to bring Task 1 to Production when there are multiple Tasks (T2...Tn)

- Master will include T1 and T2.
![Rebase and Merge](/img/16.jpg)
- Slave will include T2 and T3.
![Rebase and Merge](/img/17.jpg)

## Setting up a Git environment for the question
- Creating data on the master branch
```bash
git branch master
git branch slave

git checkout master
touch task.md
git add .
git commit -m 'T1'

git branch master_backup
```
- Switching to the slave branch
```bash
git checkout slave

touch task.md
git commit -m 'T2'
git commit -m 'T3'

git branch slave_backup
```

## Handling bringing T1 and T2 to master using git cherry-pick
```bash
git checkout slave
```

- View the log on the slave branch and copy the commit ID of T2
```bash
git log --oneline

# copy d1a564d
```

- Return to the master branch and use `git cherry-pick`
```bash
git checkout master
git cherry-pick d1a564d
git add .
git commit -m 'merge t1 and t2'
git push
```
## Moving multiple commits using git cherry-pick
- Creating commits T4 → T8 on the slave branch
```bash
git checkout slave

git commit - m 'T4'
git add .
git commit - m 'T5'
git add .
git commit - m 'T6'
git add .
git commit - m 'T7'
git add .
git commit - m 'T8'
```

- Moving T3 and T4 to master
```bash
git log --oneline

# copy id commit T3 and T4 
# 8832094 and ec7562e

git checkout master
git cherry-pick 8832094 ec7562e
```

- Moving consecutive commits T5, T6, T7 without copying each ID with syntax: <id_T4>..<id_T7
```bash
# copy id commit code T4 and T7
# ec7562e and 3d66ebc

git checkout master
git cherry-pick ec7562e..3d66ebc
```

- To include T4, use syntax <id_T4^>..<id_T7
```bash
git cherry-pick ec7562e^..3d66ebc
```

# Question 2: Dev is working on Task A in the slave branch, but the leader asks for a bug fix on the master branch

- Use `git stash` to store the current work
```bash
git stash
```

- Switch to master (hotfixes) and work on the bug fix
```bash
git checkout master
```

- Retrieve the stashed work
```bash
git checkout slave
git stash apply
```

- If there are multiple stashes
```bash
git stash save 'fix 1'

git stash save 'fix 2'

git stash save 'fix 3'
```

- View the list of stashes
```bash
git stash list
```

- Apply the first stash
```bash
git stash apply stash@{0}
```

- Delete stash entries
```bash
git stash clear #clear all
git stash drop stash@{0} #clear first stash
```








