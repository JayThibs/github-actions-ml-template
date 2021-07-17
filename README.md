# GitHub Actions ML Template

This repo contains a short and simple ml project to test GitHub Actions for continuous integration for ml.

What you should know about this repository:

- Trains a random forest model on data. Graphs like feature importance and residuals are outputs of the training.
- GitHub Actions' Continuous Integration capabilities are put to action: a yaml file (`cml.yaml`) was created in `/.github/workflows/` so that GitHub retrains our model every time we push to the repository.

When a push is made to the repo, the following occurs:

- GitHub deploys a runner with the specified Docker environment [ubuntu-latest].
- The runner executes the specified workflow to train the ML model (`python training/train.py`).
- A visual CML report about the model performance will be return as a comment in the pull request.

When experimenting with the repository, make sure to create a new branch. You can simply click "Add new file" and then click and create a new branch once you are ready to commit the yaml file.

## How to Push Files to a Specific Subdirectory

I've figured out how to resolve this so I figured I should share here.

Since I am using CML (Continuous Machine Learning), I could simply include the following line in my GitHub Actions cml.yaml file:

    cml-pr training/outputs/*

This is because cml does not push outputs to my code automatically.

Besides using `cml-pr`, you can also use the following github push manually:

    git config --local user.email "action@github.com"
    git config --local user.name "GitHub Action"

    git add training/outputs/*
    git commit --message "Commit training outputs"
    git push

Please keep in mind that the latter solution won't handle merge conflicts gracefully if there is a race condition between several simultaneous runs.

**Important note**: Since the action will push files to your repository, it will trigger another GitHub Action if your action is triggered on push. To resolve this, simply add something like [skip ci] in your commit message and GitHub will prevent an action to be triggered. You can learn more about it here: https://github.blog/changelog/2021-02-08-github-actions-skip-pull-request-and-push-workflows-with-skip-ci/

@0x2b3bfa0 on GitHub helped me resolve this issue, you can find our conversation here: https://github.com/iterative/cml/issues/658

## Secrets and Environmental Variables

The only environmental variable set in `.github/workflows/cml.yaml` is `GITHUB_TOKEN`, which is configured by default in every GitHub repository. No secrets must be set by the user.

## References:

Code was borrowed from: https://github.com/elleobrien/wine

Great video covering this topic: https://www.youtube.com/watch?v=9BgIDqAzfuA
