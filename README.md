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

## Secrets and environmental variables

The only environmental variable set in `.github/workflows/cml.yaml` is `GITHUB_TOKEN`, which is configured by default in every GitHub repository. No secrets must be set by the user.

References:

Code was borrowed from: https://github.com/elleobrien/wine
Great video covering this topic: https://www.youtube.com/watch?v=9BgIDqAzfuA
