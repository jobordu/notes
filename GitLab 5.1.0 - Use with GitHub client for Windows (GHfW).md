# Using GitHub Client for Windows with GitLab

GHfW is a an elegant way to manage your GitLab hosted repositories from Windows, here's how.

1. Create a new repository in the GitLab web interface.
2. Clone the repository using the GhfW command line

    `C:\Git> git clone http://ec2-50-10-170-40.compute-1.amazonaws.com/acme/widgets.git`

3. Drag and drop the newly cloned folder from the Windows UI over, on to the GitHub for Windows client
4. Violla. 

    Put some skeleton files (`README.md`, `.gitignore`, `.gitattributes`) into the project and use GHfW to make your first commit.

5. At this point, you're done. 
    However if you're about to migrate and commit a large project be sure to enable a large `postBuffer`. See [this post on StackOverflow](http://stackoverflow.com/questions/2702731/git-fails-when-pushing-commit-to-github) for more information

    `C:\Git\widgets>git config http.postBuffer 524288000`

