# Guide: Git and GitHub Pull Requests

1. Start at your main branch and get it up to date:

   ```
   git checkout main
   git pull origin main
   ```

2. Create a feature branch off of the main branch:

   ```
   git checkout -b feature-branch-name
   ```

3. Write some code
4. CHECK IF YOUR CODE WORKS
5. Add and commit your work.
   ```
   git add --all
   git commit -m 'your commit message here'
   ```
6. Repeat steps 3 and 4 and 5 for each chunk of code you complete.
7. When you’re ready to push your work up to Github, first merge the changes from the current main branch from Github with your current branch using this command:
   ```
   git pull origin main
   ```
   If there are any merge conflicts, resolve them and make a new commit.
8. Push your feature branch up to Github with:
   ```
   git push origin feature-branch-name
   ```
9. Make a pull request by clicking on the appropriate buttons on Github’s website.

   - If GitHub says it cannot merge the branch due to conflicts, go back to your terminal and run git pull origin main, fix the conflicts on your computer, then push the branch again with git push origin feature-branch-name

10. Ask another person on your team for a code review to merge your code on GitHub's main branch.

    - Explain what feature you were working on and show the main parts of the code that changed.
    - Share your screen and demo how the feature works. Make sure nothing is crashing and the feature is working as expected.
    - Once the other person approves of the code, they can click the "Merge" button on GitHub and delete the branch on GitHub

11. Delete the merged branch on your local computer:

    ```bash
    git checkout main
    git pull origin main
    git branch -D feature-branch-name
    ```

12. Go back to step 1 to begin a new feature. (You should use a new branch name each time for each feature.)

_Note: Sometimes your terminal will send you into Vim (a terminal based text editor). If you’re ever in Vim and need to save and exit, you can enter `:wq` (see more [here](http://stackoverflow.com/questions/11828270/how-to-exit-the-vim-editor)). To learn the basics of vim, enter `vimtutor` in your terminal and follow the instructions!_
