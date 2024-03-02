
# Table of Contents

1.  [LLM](#orgf4d771f)
    1.  [Questions / to-do <code>[0/4]</code>](#orga6a4e2c)
    2.  [LLM optimization techniques](#org9a63e14)
        1.  [Knowledge distillation](#orge2623c8)
        2.  [Post-Training Quantization (PTQ)](#orgba5db3e)
        3.  [Pruning](#org2305d54)
    3.  [LLM lifecycle](#orgb07b374)
    4.  [Notes](#org00e39de)
    5.  [Retrieval Augmented Generation (RAG)](#orgb053968)
    6.  [Chain of thought prompting](#orgd083636)
    7.  [Program Aided Language (PAL) models](#org5d80169)
    8.  [ReAct: Synergize reasoning and action](#org5dedf78)
2.  [Pre-commit: Pre-commit hooks to facilitates CI/CD.](#org6552f5a)
3.  [Poetry: Python packaging and dependency management](#orgbd4b436)
    1.  [Steps](#org7369dfd)
    2.  [Publish to PyPi](#orgc8913cf)
4.  [Docker](#orge059bb3)
    1.  [Docker commands](#orgeb50a1b)
    2.  [`docker-compose`](#org5888bbf)
5.  [Git and GitHub](#orgd7fe7a0)
    1.  [`diff` and `patch`](#org1d00755)
    2.  [Setup](#orgede7c95)
    3.  [Showing stuff](#org8ac3417)
    4.  [Staging and stating-related commands](#org6ab7115)
    5.  [Committing and undoing commits](#orgdf5f0e9)
    6.  [Branches](#orga24c766)
    7.  [Working with the remote](#org5ea79c4)
    8.  [Rebasing](#org75549e9)
    9.  [Collaboration](#org5224031)
    10. [Multiple GitHub accounts](#orgd7971da)
6.  [Pandas](#org9caa2eb)
7.  [TensorFlow](#org6a8b8c5)
    1.  [Loss functions](#org6b31fd3)
8.  [Google Cloud Platform](#org94e285e)
    1.  [Commands](#orgf06da5e)
    2.  [Steps <code>[8/11]</code>](#org351d7f3)
        1.  [](#org996e354)
        2.  [Deployment](#org30909f0)
        3.  [Old (with the creation of the endpoint first&#x2014;which makes more sense)](#orgfaccb43)
9.  [Image manipulation](#org6d1f608)
    1.  [Reduce image sizes](#orgaa712cf)
10. [GPG](#org0fcc5ac)
11. [Emacs](#orge29ede2)
    1.  [Generic Emacs commands](#orge209cfa)
    2.  [Outline](#orge27a282)
    3.  [Tables](#org7fe7833)
    4.  [Code](#orgd580c21)
    5.  [LaTeX integration](#orgffc4819)
    6.  [Literate programming](#orgd5c3139)
    7.  [Todo stuff](#org5fdbd39)
    8.  [do this. Use ShiftM Enter to create another one](#orgcb6a215)
    9.  [cycle through states CcCt](#org39b882b)
    10. [Now what?](#org2096c2d)
12. [Linux](#org8d2fa82)
    1.  [The shell](#org22dc29a)
    2.  [Shell scripting](#orgd5bc828)
    3.  [Misc commands](#org778b498)
    4.  [Networking](#org809ab5f)
    5.  [Graphics](#org19d6767)
    6.  [Configuration in Debian](#org79e3927)
    7.  [tmux](#org962960c)
13. [ImageMagick](#org9abfbb4)
14. [SQL](#org2376387)
    1.  [MySQL](#org1ad931c)
    2.  [PostgreSQL](#orgb712216)
15. [Vagrant](#orgca77dea)
16. [LaTeX](#orgf557bcc)
17. [PySpark](#org49e5593)



<a id="orgf4d771f"></a>

# LLM


<a id="orga6a4e2c"></a>

## Questions / to-do <code>[0/4]</code>

-   [ ] What is a tokenizer?
-   [ ] What is Encoder, Decoder, Encoder-Decoder?
    -   [ ] Look at examples of GenAI and specify whether there are E, D, or ED
-   [ ] Autoregression
-   [ ] Review all the Notebooks


<a id="org9a63e14"></a>

## LLM optimization techniques


<a id="orge2623c8"></a>

### Knowledge distillation

-   LLM teacher -> LLM student
-   Works best for encoder models (e.g., Bert)


<a id="orgba5db3e"></a>

### Post-Training Quantization (PTQ)

-   LLM -> 16-bit float or 8-bit integer quantized LLM
-   Applied to model weights and or activations


<a id="org2305d54"></a>

### Pruning

-   LLM -> Pruned LLM
-   Remove model weights that are close to zero
-   Requires model retraining


<a id="orgb07b374"></a>

## LLM lifecycle

1.  Pretraining: This is where the base model is built. This focuses on next-token prediction
2.  Prompt engineering: This aims at increasing the task performance. Only involves prompt customization.
3.  Prompt tuning and fine-tuning: This aims at increasing the task performance. Updates the LLM or adapter wieghts.
4.  Reinforcement learning from human feedback: Needs a separate reward model to align with human goals. Updates the LLM or adapter weights.
5.  Compression, optimization, and deployment: pruning, post-training quantization, or knowledge distillation.


<a id="org00e39de"></a>

## Notes

-   Prompt-completion
-   Context window
-   Foundation model (base LLM)
-   Prompt engineering vs. instruction fine-tuning
-   Quantization to reduce the memory footprint
-   In-context learning means you provide prompt completion examples during inference
-   BLEU focuses on precision in matching generated output to the reference text and is used for text translation. ROUGE focuses on recall.
-   Catastrophic forgetting
    -   Multitask fine-tuning
    -   Fix the parameters to be tuned and freeze the rest: Parameter Efficient Fine-Tuning (PEFT)
        -   **Select** a subset of initial LLM parameters to fine-tune
        -   **Reparametrize** the model weights using a low-rank representation: 
            -   LoRA: usually only applied on the attention weights, not the feed-forward. The rank r is a hyperparameter
        -   **Add** trainable layers or parameters to the model: Keep the original model frozen and add a new layer.
            -   Adapters
            -   Soft prompts (**Prompt tuning**). Manipulate the input. (? Is this some kind of AI-driven prompt engineering?)
                -   Prepend the prompt token: Input manipulation (as opposed to working with the LLM itself)
-   Fine-tuned LAnguage Net (FLAN)
-   Metrics (always need to be compared for the same task)
    -   ROUGE and BLEU
    -   Precision and recall of n-grams of LCS (Longest Common Subsequences).
    -   Clipping function (? to avoid "class imbalance"?)
-   Hallucination
-   Stemming
-   Reinforcement Learning from Human Feedback (RLHF): Align with human feedback
    -   Human labelers score a dataset of completions by the original model based on alignment criteria like helpfulness, harmlessness, and honesty. This dataset is used to train the reward model that scores the model completions during the RLHF process.
    -   The LLM generates several completions for the same prompt and then human labelers rank those completions. Those ranking score then drive the "reward" of the reinforcement learning algorithm.
    -   KL divergence is used to moderate the changes induced by the reinforcement learning such that the difference between the base model and and the one that is being optimized does not get too big. This avoids that the rewarding (against, e.g., toxic languages) does not get hacked.


<a id="orgb053968"></a>

## Retrieval Augmented Generation (RAG)

-   Instead of retraining the model to update it with new knowledge, RAG mitigates the knowledge cutoff.
-   LangChain can help chop off the retrieved, external documentation so that it fits in the context window.


<a id="orgd083636"></a>

## Chain of thought prompting

-   Prompt fine-tuning should include the chain of thought


<a id="org5d80169"></a>

## Program Aided Language (PAL) models

-   LLM + Code interpreter (Python). This basically translates the prompt to Python code.


<a id="org5dedf78"></a>

## ReAct: Synergize reasoning and action

-   Prompting strategby that combines chain-of-thought and action planning


<a id="org6552f5a"></a>

# [Pre-commit](https://pre-commit.com/): Pre-commit hooks to facilitates CI/CD.


<a id="orgbd4b436"></a>

# [Poetry](https://www.youtube.com/watch?v=0f3moPe_bhk): Python packaging and dependency management


<a id="org7369dfd"></a>

## Steps

1.  `poetry init`: Will generate the pyproject TOML file if it does not exist.
2.  `poetry config virtualenvs.in-project true`: Ensures the .venv remains in the current directory. (optional)
3.  `poetry install`: Install the environment based on the parameters of the TOML file.
4.  `poetry env info`: Give information as to where the environment is installed.
5.  `poetry shell`: Enter the shell with the environment. Note that this nested shell does not inherit any of the setups from the parent shell.
6.  `poetry {add, remove} <some pip package>`: Add or remove packages on the fly. The TOML is updated accordingly.
7.  `poetry env list`: List all the active environments.
8.  `deactivate`: Deactivate the environment. Equivalent to leaving the shell.


<a id="orgc8913cf"></a>

## Publish to PyPi

1.  `poetry config repositories.test-pypi https://test.pypi.org/legacy`
2.  `poetry config pypi-token.test.pypi <token name>`
3.  `poetry build`
4.  `poetry publish -r test-pypi`


<a id="orge059bb3"></a>

# Docker


<a id="orgeb50a1b"></a>

## Docker commands

-   Build and run a docker container
    -   `docker build -t hello-folks .`
    -   `docker run -p 80:80 -v /d/Documents/Docker/src/:/var/www/html/ hello-folks`
-   Run a container with mounted volumes
    -   `docker run -it -v <local folder>:<remote folder> <image name>`
    -   `docker run -it -v $(pwd):/home/domains/ housebunting/tfcpu`
-   Commit a container to an image: `docker commit <container ID> <image name>`
-   Stop and remove all containers
    -   `docker stop $(docker container ls -q -a)`
    -   `docker rm $(docker container ls -q -a)`
-   Remove all dangling images: `docker rmi $(docker images -f dangling=true -q)`
-   Push to Docker hub
    -   `docker tag <image name> housebunting/<image name>`
    -   `docker push housebunting/<image name>`
-   Build and tag an image: `docker build --tag=mlgpu .`
-   Network list: `docker network ls`
-   Inspect: `docker inspect <image name>`
-   Get into the bash shell of a running container: `docker exec -it $CONTAINER_ID /bin/bash`
-   List all the containers: `docker ps -a`
-   Detach from a container: `ctrl-P` then `ctrl-Q`
-   Attach to a container: `docker attach $CONTAINER_ID`


<a id="org5888bbf"></a>

## [`docker-compose`](https://docs.docker.com/compose/gettingstarted/)

It is a more structured, YAML-based alternative to `docker run` which can manage several networked containers all in one place.

-   `docker-compose ps`: List of containers launched by `docker-compose`
-   `docker compose run <service> env`: Environment variables available in the `<service>`


<a id="orgd7fe7a0"></a>

# Git and GitHub


<a id="org1d00755"></a>

## `diff` and `patch`

    diff original_file.py changed_file.py > file.diff
    patch original_file.py < file.diff


<a id="orgede7c95"></a>

## Setup

-   `git fetch origin`: get all the branches without pulling
-   `git remote add origin git@github.com:ala-csis/<REPO>.git`
-   `git remote set-url origin git@github.com:ala-csis/<REPO>.git`
-   `ssh -T git@github.com`: SSH identity
-   `git config --global core.editor emacs`: Change the default editor to emacs


<a id="org8ac3417"></a>

## Showing stuff

-   `git blame`: Show what revision and author last modified each line of a file.
-   `git diff <file>`: diff of the `<file>` compared to its previous commit. Works with unstaged changes by default. To view staged (but not commited) changes, add the flag `--staged`.
-   `git log -p`: Show all patches in the log.
-   `git log -<N>`: Number of most recent commits to show.
-   `git show <commit>`: Show the patches for a given commit ID.
-   `git log --stat`: Show the statistics for the changes at each commit.
-   `git show <commit>:<file>`: Show an older version of the file.


<a id="org6ab7115"></a>

## Staging and stating-related commands

-   `git checkout -- <file>`: Revert changes to modified files before they get staged. To checkout individual changes instead of the whole file, one can use the `-p` flag.
-   `git add -p`: Prompts for which hunks in a file to add.
-   `git rm <file>`:  If you just use `rm`, you will need to follow it up with `git add <file>`. `git rm` does this in one step.
-   `git mv <old file> <new file>`: Rename a file (and have it staged by the same occasion).
-   `git reset HEAD -- <file>`: Opposite of `git add <file>`. Undoes the staging of `<file>` so that it becomes untracked.
-   `git restore --staged <file to unstage>`: Unstage a file. This is the new version that replaces `git reset`


<a id="orgdf5f0e9"></a>

## Committing and [undoing](https://stackoverflow.com/questions/58003030/what-is-the-git-restore-command-and-what-is-the-difference-between-git-restor/66309040#66309040) commits

Cf. this [visualization](https://git-school.github.io/visualizing-git/) tool or [this one](https://learngitbranching.js.org/).

-   `git commit --amend`: Overwrites the previous commit with whatever is currently in the staging area. Useful if we forgot a file or want to change the commit message. WARNING: Do not do this if the changes have already been pushed. This is only good for local changes. If the change is minor and doesn't require changing the commit message, add the flag `--no-edit`.
-   `git revert HEAD`: Revert the latest commit.
-   `git revert <commit>`: Rever back to `<commit>`.
-   `git restore <file to restore>`: Undoes all modifications to the file since the last commit (or whatever process got it into the working tree).
-   `git restore --source <commit> --worktree -- <file>`: Bring back a `<file>` from some earlier `<commit>` and place it in the working tree (but not in the index/staging area).
-   `git checkout <commit> -- <file>`: Bring back `<file>` from the `<commit>`.
-   `git reset --{hard, mixed, soft} HEAD~<N>`: Remove `N` commits. `hard` resets the index and the working tree; `mixed` only resets the index; `soft` leaves both the index and the working tree unmodified (aka. "squashing").


<a id="orga24c766"></a>

## Branches

-   `git branch <new branch>`: List the branches or create a `<new branch>`.
-   `git checkout <other branch> --`: Switch to the `<other branch>`. Use the flag `-b` to create the branch if it doesn't already exist. Note the difference between `git checkout -- <some file>` (clobber the file)  and `git checkout <other branch> --` (get the branch). In fact, to avoid confusion between branch names and file names, it is better to use `git switch` to avoid inadvertently clobbering a file with the same name as a branch.
-   `git switch <other branch>`: Switch to the `<other branch>`.
-   `git branch -d <branch to be deleted>`: Delete a branch.
-   `git merge <branch>`: Merge the `<branch>` into the current branch.
-   `git push -u origin <local branch>`: Push a local `<local branch>`.
-   `git push --delete origin <remote branch>`: delete a remote branch.
-   `git branch -d <branch>`: delete a local branch


<a id="org5ea79c4"></a>

## Working with the remote

-   `git pull`: Get the latest remote changes.
-   `git remote show origin`: Get all the information about the remote repository.
-   `git branch -r`: List of the remote branches
-   Workflow to update the remote repository:
    1.  `git pull`
    2.  `git merge`
    3.  `git push`
-   Workflow to update the local repository in two steps using `git fetch`:
    1.  `git fetch`: Will pull all the metainfo but won't change the actual files
    2.  `git merge origin/<branch>`:
-   Workflow to update the local repository in one step using `git pull`:
    1.  `git pull origin <remote branch>`:


<a id="org75549e9"></a>

## Rebasing

-   Rebasing workflow of `<branch>` on top of `main` (recall that the advantage of rebasing is that it keeps the history linear, as opposed to the three-way merging):
    1.  `git checkout <branch> --`
    2.  `git rebase main`: Rebase, i.e., move the current branch on top of the `main`
    3.  If there are some conflicts to settle, then just fix them and commit them. If it makes sense manually, then all good. Otherwise, `git rebase --abort` and then run `git rebase --interactive <branch>` to pick the changes you want in the current branch.
    4.  `git rebase --continue` if satisfied with everything.

Before rebasing:

    * ba8a8f8 (HEAD -> main) mainemacs file.txt                                                                                                            [69/117]
    * dfc0bca main 1                                                               
    * 6ed2b74 main 0                       
    | * faf2c49 (FFR) Moved around branch line
    | * b4bfff8 branch 2   
    | * f33b384 branch 1                   
    | * e87aaa1 branch 0                                                           
    |/                                     
    * d9a583b (origin/main) Prepare the clean slate                   
    * e5c58b4 Clean slate back in main

After rebasing:

    * 52bad8f (HEAD -> main) Interactive rebasing // Had to do some picking here with the interactive rebasing.
    * faf2c49 (FFR) Moved around branch line
    * b4bfff8 branch 2
    * f33b384 branch 1
    * e87aaa1 branch 0
    * d9a583b (origin/main) Prepare the clean slate
    * e5c58b4 Clean slate back in main


<a id="org5224031"></a>

## Collaboration


<a id="orgd7971da"></a>

## Multiple GitHub accounts

    Host *
         IgnoreUnknown AddKeysToAgent,UseKeychain
         AddKeysToAgent yes
         UseKeychain yes
         IdentityFile ~/.ssh/id_rsa


<a id="org9caa2eb"></a>

# Pandas

-   `df.info()`: Prints information about the DataFrame
-   `df.head()`: Prints first (n) rows of the DataFrame
-   `df.describe()`: Generates state of the columns
-   `df.apply()`: Applies functions on columns
-   `df.groupby()`: Applies aggregation of a column
-   `df.short_values()`: Sorts tables
-   `df.sample()`: Sample data
-   `pd.read_filetype(filename)`: Imports from a filetype
-   `df.to_filetype(filename)`: Exports to a filetype
-   `df.plot()`: Generates plot
-   `pd.to_datetime`: Converts object to a datetime column
-   `df.filter()`: Filter on columns
-   `df.drop()`: Drops rows based on a condition
-   `df.rank()`: Generates ranks on a column
-   `df1.append(df2)`: Adds the rows in df1 to the df2
-   `pd.isnull()`: Checks the null values


<a id="org6a8b8c5"></a>

# TensorFlow

-   [ ] The necessity of prefetching
-   [ ] `map` vs. `flat_map`
-   [ ] All the documentation on whatever replaces `tf.feature_columns`
-   [ ] Use `tf.data` for the DGA classifier


<a id="org6b31fd3"></a>

## Loss functions

-   Loss functions
    -   Regression
        -   mean squared error (MSE): default if the target distribution is Gaussian
        -   mean squared logarithmic error (MSLE): if the target distribution is spread out. The logarithm will make sure the targets are more "gathered".
        -   mean absolute error (MAE): if the target distribution contains outliers. This won't punish them as much as the MSE.
    -   Classification
        -   Binary classification
            -   binary cross entropy
                -   sigmoid activation function
            -   hinge loss or squared hinge loss function
                -   hyperbolic tangent activation function
        -   Multiclass classification
            -   categorical cross entropy
                -   softmax activation function
            -   sparse categorical cross entropy: if there are too many categories to be one-hot encoded
                -   softmax activation function
            -   Kullback-Leibler divergence loss
                -   softmax activation function


<a id="org94e285e"></a>

# Google Cloud Platform

-   [ ] Update `get_env_vars()` in the utilities module.
-   [X] Machine learning <code>[1/1]</code>
    -   [X] AutoML
-   [-] Data analytics <code>[3/5]</code>
    -   [X] BigQuery (storage and analytics): Data warehouse. Integrated with Vertex AI.
    -   [X] Pub/Sub: Expose topics (i.e., antennas)
    -   [X] Dataflow (ETL): Batch and streaming data processing patterns. The pipelines are created in Apache Beam then run on Dataflow.
    -   [ ] Looker: Visualization and dashboarding of what is in BigQuery or many other databases.
    -   [ ] Data studio: Dashboarding and business intelligence. Unlike Looker, it is readily integrated with BigQuery.
    -   [ ] BigQuery ML

-   Cloud Storage for batch data or Pub/Sub for streaming data. Both lead to Dataflow (ETL). Then the loaded data goes into BigQuery.

Steps:

1.  Create a BigQuery dataset with the relevant tables.
2.  Create a Cloud Storage bucket for a Dataflow pipeline.
3.  Restart the connection to the Dataflow API.
4.  Create a new streaming pipeline from template by specifying the input topic from Pub/Sub and the output table in BigQuery.


<a id="orgf06da5e"></a>

## Commands

    gcloud functions deploy classify_flower --region=europe-west4 --source=function --runtime=python37 --memory=2048MB --trigger-http --allow-unauthenticated --set-env-vars=ENDPOINT_ID=${ENDPOINT_ID}

Tensorboard:

    pip3 install protobuf==3.20.*
    tb-gcp-uploader --tensorboard_resource_name projects/<project ID>/locations/europe-west4/tensorboards/8377223072490455040 --logdir=csis-data-warehouse/map/iris/lesson --experiment_name=iris --one_shot=True


<a id="org351d7f3"></a>

## Steps <code>[8/11]</code>


<a id="org996e354"></a>

### 


<a id="org30909f0"></a>

### Deployment

1.  [ ] Create the learner map.
2.  [ ] Push the Docker image `europe-west4-docker.pkg.dev/<project name>/map/$MAP_NAME` to the `Artifact registry`.
3.  [ ] Upload the data to a bucket (or to BigQuery).
4.  [ ] Create a new training pipeline with a custom container under `Training`.
5.  [ ] Import the model to the `Model registry` (use a pre-built container).
6.  [ ] Deploy the model in the Model Registry to an endpoint. (You may need to push a new image with the endpoint's ID in the `cloud_params.json`.)
7.  [ ] Serve <code>[0/2]</code>
    -   [ ] Python:
        1.  [ ] SSH into the GCP console.
        2.  [ ] Download the Docker image.
        3.  [ ] Get inside the container.
        4.  [ ] pip3 install google-cloud-aiplatform and gcsfs
        5.  [ ] Run predict.
    -   [ ] `gcloud ai endpoints predict $ENDPOINT_ID --region=$REGION --json-request=request.json`


<a id="orgfaccb43"></a>

### Old (with the creation of the endpoint first&#x2014;which makes more sense)

1.  [X] Create the learner map.
2.  [X] Upload the data to a bucket or to BigQuery.
3.  [X] Create an endpoint and specify its ID in `cloud_params.json`.
4.  [X] Push the Docker image `europe-west4-docker.pkg.dev/<project name>/map/$MAP_NAME` to the `Artifact registry`.
5.  [X] Create a new training pipeline under `Training`.
6.  [X] Import the model to the `Model registry` (use a pre-built container).
7.  [X] Deploy the model to the artifacts registry.
8.  [ ] Serve <code>[0/2]</code>
    -   [ ] Python:
        1.  [ ] SSH into the GCP console.
        2.  [ ] Download the Docker image.
        3.  [ ] Download the learner container and log into it
        4.  [ ] Install google-cloud-aiplatform and gcsfs
        5.  [ ] Run predict.
    -   [ ] gcloud ai endpoints predict $ENDPOINT<sub>ID</sub> &#x2013;region=$REGION &#x2013;json-request=request.json


<a id="org6d1f608"></a>

# Image manipulation


<a id="orgaa712cf"></a>

## Reduce image sizes

-   PDF to PNG: `pdftoppm pairgrid.pdf output.png -png &`
-   Reduce PDF size: `gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf pairgrid.pdf &`


<a id="org0fcc5ac"></a>

# GPG

-   Encrypt: `gpg --output DOC.gpg --encrypt --recipient aknary@yahoo.com DOC`
-   Decrypt: `gpg --output DOC --decrypt DOC.gpg`


<a id="orge29ede2"></a>

# Emacs


<a id="orge209cfa"></a>

## Generic Emacs commands

F10 to access the menu
CxCf open a file
Cxb switch to buffer (with autocompletion, includes **Messages**, **scratch**)
CxCb list open buffers

Mx<autocomplete>

Cx0 Close a window

Mx dired
Mx calendar

M< beginning of text
M> end of text
Ma beginning of sentence
Me end of sentence

Cg stop a command that is taking too long

Ch k <command> describe the command
Ch: help
Cx 1 only leave one window open and make all the other ones disappear

Cu 8 \* insert eight characters \*

Md delete the next word
Cd delete the next character

This is the first sentence. This is the second sentence.

Mk kill (and copy) to the end of the sentence
Ck kill (and copy) to the end of the line.
C<space> select Cw: Select a passage

here is the way that we did it.  

My after Cy cycles back in the set of kills

C/ undo
C\_ undo

Cg C\_ redo, then cycle back into undo

Cx s Get asked whether to save the unsaved buffers

Cz temporarily suspend emacs (useful when there is only one terminal window)

Mx replace-string

Mx recover-this-file: do this inside the file which wasn't saved before a crash. This will integrate the #filename# backup.

Mx text-mode/org-mode/

Ch m documentation about the current mode

Mx auto-fill-mode

Cx 20: set the width of a paragraph to 20 characters. This requires the auto-fill-mode
Mq: to apply the width setting to a paragraph

Cx o move the cursor to the other window
Cx 2 split horizontally the screen into two windows
Cx 3 split vertically the screen into two windows
CMv to scroll to the other window below

Cx 4 Cf: open a new window below and move the cursor there

Cx 5 2: Open a new frame
Cx 5 0: Remove the current frame

If Cg doesn't work because we're within a recursive level, use Esc Esc Esc.

Ch ? for generic help

Ch c <key seq>: short description of the key sequence including the name of the function (e.g., previous-line for <key seq> = Cp)
Ch k <key seq>: long descirption of the key sequence
Ch f previous-line: describe the function previous-line

Ch i: consult manuals

Ch r: consult the emacs manual

Example of a minor mode:
Mx hl-line-mode

Mx toggle-truncate-lines to break long lines

Cx Left/Right switch between buffers

Mx describe-bindings == Ch b


<a id="orge27a282"></a>

## Outline

Text text text!

-   more items
-   yet more

**Bold**, *italic*, `verbatim`, <del>strikethrough</del> 

-   bulleted
-   list
-   items

[CSIS Security Group](https://csis.com)


<a id="org7fe7833"></a>

## Tables

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Some</th>
<th scope="col" class="org-left">Data</th>
<th scope="col" class="org-left">&#xa0;</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">Python</td>
<td class="org-left">This is where it goes!</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left">12</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">now what?</td>
</tr>


<tr>
<td class="org-left">ⵜⴰⵎⴰⵣⵉⵖⵜ</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>

CcCe ways to export


<a id="orgd580c21"></a>

## Code

    
    def hello(name):
        print(name)
    
    myname = Amine
    
    hello(myname)

CcCc will evaluate the results


<a id="orgffc4819"></a>

## LaTeX integration

-   Characters: &alpha; &rarr; &beta;
-   \(O(n \log n)\)

\begin{align*}
a & = 3 \cdot 3 + 1\\
& = 1 
\end{align*}

Note that one can export to beamer as LaTeX, but that seems to require some install.


<a id="orgd5c3139"></a>

## Literate programming

Literate programming is mixing natural language and computer programming. It is only useful for reserach papers or didactic purposes.


<a id="org5fdbd39"></a>

## Todo stuff


<a id="orgcb6a215"></a>

## TODO do this. Use ShiftM Enter to create another one


<a id="org39b882b"></a>

## TODO cycle through states CcCt


<a id="org2096c2d"></a>

## TODO Now what?


<a id="org8d2fa82"></a>

# Linux


<a id="org22dc29a"></a>

## The shell

Environment variables

-   Username $USER
-   Home directory $HOME
-   Path variable $PATH

which echo

rwxrwxrwx
1 owner of the file, 2 group, 3 everybody else

For files: [r] read, [w] write, [x] execute
For directories: [r] list, [w] rename/create files in the directory, [x] search (entry rights into the directroy)

CTRL-L 

echo hello > hello.txt

cat notes.txt
cat < notes.txt

cat < notes.txt > notes2.txt
cp notes.txt notes2.txt

cat < notes.txt >> notes2.txt

ls -l | tail -n1
ls -l | tail -n1 > ls.txt
curl &#x2013;head &#x2013;silent adaptive.finance | grep -i content-length | cut &#x2013;delimiter=' ' -f2

cat /sys/class/backlight/intel<sub>backlight</sub>/brightness

cd /sys/class/backlight/intel<sub>backlight</sub>
echo 500 > brightness  # Permission error
sudo echo 500 > brightness # Permission error because brightness is the user (shell $) even if "sudo echo 500" is by the (root #).

sudo su

cat max<sub>brightness</sub> | sudo tee brightness

cd /sys/class/leds/input36::scrolllock
echo 1 | sudo tee brightness

xdg-open syllabus.pdf
open syllabus.pdf

sh semester

./semester | grep -i last-modified > last-modified.txt


<a id="orgd5bc828"></a>

## Shell scripting

source mcd.sh

mcd MyNewDirectory

echo $\_

sudo!!

echo $?

false || echo "Oops fail"

true && echo "will be printed"
false && echo "will not be printed"

echo "Azul" && echo "Hello"
echo "Azul" || echo "Hello"
echo "Azul" ; echo "Hello"

foo=$(pwd)
echo $foo

ls project\*  # will return all of them
ls project?  # will only return those that have a single character instead of ?, namely project1 and project2

ls\t\t

ls \t\t

touch foo{,1,2,3,5,100}
touch foo{1,2,3}bar{1,2,3}  # Cartesian products
touch foo{1..2}bar{a..j}
touch {foo,bar}.{a..j}

mkdir foo bar
touch {foo,bar}/{a..j}
touch foo/x bar/y
diff <(ls foo) <(ls bar)

sudo apt install shellcheck

find . -name script.py -type f
find . -name "\*.txt" -type f

find . -path '**\*/test/**.py' -type f 

find . -mtime -1 -type d

find ~/Python/learners/ -name "\*.py"

grep -R mytest .

grep -R -n "import learners" ~/GitHub/\*\*/\*.py

rg -C 3 "import learners" ~/GitHub/\*\*/\*.py

history

nnn


<a id="org778b498"></a>

## Misc commands

gnome-disks

shred

lsblk

xkill

smbpasswd -r vstgdc1.csis.local -U ala

apt update
sudo apt dist-upgrade

fg %<number>

dmesg

alias emacs='emacs -nw'

/etc/init.d/docker stop
sudo !!

sudo reboot

df

lp mytext.txt -o sides=two-sided-long-edge

rsync -r . ala@bmo.csis.local:/home/ala/sandbox # Copy from local to bmo

lshw

sudo apt clean
sudo apt autoremove &#x2013;purge

nmcli connection up csis &#x2013;ask

python -m json.tool my<sub>json.json</sub>

dig +short -x 64.34.199.12

netstat -alpnt

xxd

xclip -sel c < myfile.txt


<a id="org809ab5f"></a>

## Networking

dig nordea.dk -t ns


<a id="org19d6767"></a>

## Graphics

ps2pdf -dPDFSETTINGS=/ebook input.pdf output.pdf

sudo apt-get install librsvg2-bin
rsvg-convert -f pdf -o t.pdf t.svg


<a id="org79e3927"></a>

## Configuration in Debian

sudo apt install zsh
sudo apt install (some of these should be optional)
shellcheck, ripgrep (rg), updatedb, locate, fzf [fuzzy matching in searching the history], nnn
sudo apt install -y gnome-disk-utility  # For mounting/unmounting disks, formatting them, etc.
sudo apt install tmux


<a id="org962960c"></a>

## tmux

tmux new -s my<sub>session</sub>
Cb d

tmux ls

tmux attach -t my<sub>session</sub>

Cb PageUp to scroll. Use Esc to get back to normal mode.


<a id="org9abfbb4"></a>

# ImageMagick

mogrify -format jpg **.eps
convert -delay 20 bloch**.png bloch.gif

Resize:
mogrify -path fullpathto/temp2 -resize 60x60% -quality 60 -format jpg \*.png


<a id="org2376387"></a>

# SQL


<a id="org1ad931c"></a>

## MySQL

mysql -u root (-p) -e "create database MY<sub>DB</sub>;"
mysql -u root (-p) CAFFS < local<sub>dir</sub>/sql<sub>file.sql</sub>
use MY<sub>DB</sub>;
desc MY<sub>TABLE</sub>;
select \* from MY<sub>TABLE</sub> limit 10;


<a id="orgb712216"></a>

## PostgreSQL

psql -U secdns
psql -U postgres # (root account&#x2014;not recommended to use)
\du # Users and their privileges

psql secdns-master -U secdns
\i sql<sub>file.sql</sub> # Run a SQL file. (Will need to be connected to a specific database first I presume.)

&copy; (SELECT zone FROM whitelist) TO '~/Documents/SecureDNS/whitelist.csv' WITH CSV


<a id="orgca77dea"></a>

# Vagrant

vagrant up
vagrant destroy
vagrant reload
vagrant ssh
vagrant halt

Place the config file in the directory that I want to consider as local. Edit the configuration file to specify the default local and remote directories.


<a id="orgf557bcc"></a>

# LaTeX

sudo apt-get install texlive-full
sudo apt-get install texmaker
pdflatex document.tex


<a id="org49e5593"></a>

# PySpark

pip install pyspark

sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer

pyspark &#x2013;driver-class-path ~/Downloads/postgresql-42.2.5.jar &#x2013;master local

INSTALLATION OF THE ML TOOLS
`==========================`

chmod ug+x Anaconda3-2018.12-Linux-x86<sub>64.sh</sub>
./Anaconda3-2018.12-Linux-x86<sub>64.sh</sub> # May need to specify \`bash\` first
export PATH="<absolute path to>/anaconda3/bin:$PATH"

conda install python=3.6

pip install tensorflow

conda install Keras

