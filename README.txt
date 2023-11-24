#+TITLE: Missing semester

* [[https://pre-commit.com/][Pre-commit]]
* [[https://www.youtube.com/watch?v=0f3moPe_bhk][Poetry]]
** Steps
1. =poetry init=: Will generate the pyproject TOML file if it does not exist.
2. =poetry config virtualenvs.in-project true=: Ensures the .venv remains in the current directory. (optional)
3. =poetry install=: Install the environment based on the parameters of the TOML file.
4. =poetry env info=: Give information as to where the environment is installed.
5. =poetry shell=: Enter the shell with the environment. Note that this nested shell does not inherit any of the setups from the parent shell.
6. =poetry {add, remove} <some pip package>=: Add or remove packages on the fly. The TOML is updated accordingly.
7. =poetry env list=: List all the active environments.
8. =deactivate=: Deactivate the environment. Equivalent to leaving the shell.
** Publish to PyPi
1. =poetry config repositories.test-pypi https://test.pypi.org/legacy=
2. =poetry config pypi-token.test.pypi <token name>=
3. =poetry build=
4. =poetry publish -r test-pypi=
* Docker
** Docker commands
- Build and run a docker container
  - =docker build -t hello-folks .=
  - =docker run -p 80:80 -v /d/Documents/Docker/src/:/var/www/html/ hello-folks=
- Run a container with mounted volumes
  - =docker run -it -v <local folder>:<remote folder> <image name>=
  - =docker run -it -v $(pwd):/home/domains/ housebunting/tfcpu=
- Commit a container to an image
  - =docker commit <container ID> <image name>=
- Stop and remove all containers
  - =docker stop $(docker container ls -q -a)=
  - =docker rm $(docker container ls -q -a)=
- Remove all dangling images
  - =docker rmi $(docker images -f dangling=true -q)=
- Push to Docker hub
  - =docker tag <image name> housebunting/<image name>=
  - =docker push housebunting/<image name>=
- Build and tag an image
  - =docker build --tag=mlgpu .=
- Network list
  - =docker network ls=
- Inspect
  - =docker inspect <image name>=
** [[https://docs.docker.com/compose/gettingstarted/][=docker-compose=]]
It is a more structured, YAML-based alternative to =docker run= which can manage several networked containers all in one place.
- =docker-compose ps=: List of containers launched by =docker-compose=
- =docker compose run <service> env=: Environment variables available in the =<service>=
* Git and GitHub
** =diff= and =patch=
#+BEGIN_SRC python
diff original_file.py changed_file.py > file.diff
patch original_file.py < file.diff
#+END_SRC

** Setup
- =git fetch origin=: get all the branches without pulling
- =git remote add origin git@github.com:ala-csis/<REPO>.git=
- =git remote set-url origin git@github.com:ala-csis/<REPO>.git=
- =ssh -T git@github.com=: SSH identity
- =git config --global core.editor emacs=: Change the default editor to emacs
** Showing stuff
- =git blame=: Show what revision and author last modified each line of a file.
- =git diff <file>=: diff of the =<file>= compared to its previous commit. Works with unstaged changes by default. To view staged (but not commited) changes, add the flag =--staged=.
- =git log -p=: Show all patches in the log.
- =git log -<N>=: Number of most recent commits to show.
- =git show <commit>=: Show the patches for a given commit ID.
- =git log --stat=: Show the statistics for the changes at each commit.
- =git show <commit>:<file>=: Show an older version of the file.

** Staging and stating-related commands

- =git checkout -- <file>=: Revert changes to modified files before they get staged. To checkout individual changes instead of the whole file, one can use the =-p= flag. 
- =git add -p=: Prompts for which hunks in a file to add.
- =git rm <file>=:  If you just use =rm=, you will need to follow it up with =git add <file>=. =git rm= does this in one step.
- =git mv <old file> <new file>=: Rename a file (and have it staged by the same occasion).
- =git reset HEAD -- <file>=: Opposite of =git add <file>=. Undoes the staging of =<file>= so that it becomes untracked.
- =git restore --staged <file to unstage>=: Unstage a file. This is the new version that replaces =git reset=

** Committing and [[https://stackoverflow.com/questions/58003030/what-is-the-git-restore-command-and-what-is-the-difference-between-git-restor/66309040#66309040][undoing]] commits
Cf. this [[https://git-school.github.io/visualizing-git/][visualization]] tool or [[https://learngitbranching.js.org/][this one]].
- =git commit --amend=: Overwrites the previous commit with whatever is currently in the staging area. Useful if we forgot a file or want to change the commit message. WARNING: Do not do this if the changes have already been pushed. This is only good for local changes. If the change is minor and doesn't require changing the commit message, add the flag =--no-edit=.
- =git revert HEAD=: Revert the latest commit.
- =git revert <commit>=: Rever back to =<commit>=.
- =git restore <file to restore>=: Undoes all modifications to the file since the last commit (or whatever process got it into the working tree).
- =git restore --source <commit> --worktree -- <file>=: Bring back a =<file>= from some earlier =<commit>= and place it in the working tree (but not in the index/staging area).
- =git checkout <commit> -- <file>=: Bring back =<file>= from the =<commit>=.
- =git reset --{hard, mixed, soft} HEAD~<N>=: Remove =N= commits. =hard= resets the index and the working tree; =mixed= only resets the index; =soft= leaves both the index and the working tree unmodified (aka. "squashing").

** Branches

- =git branch <new branch>=: List the branches or create a =<new branch>=. 
- =git checkout <other branch> --=: Switch to the =<other branch>=. Use the flag =-b= to create the branch if it doesn't already exist. Note the difference between =git checkout -- <some file>= (clobber the file)  and =git checkout <other branch> --= (get the branch). In fact, to avoid confusion between branch names and file names, it is better to use =git switch= to avoid inadvertently clobbering a file with the same name as a branch.
- =git switch <other branch>=: Switch to the =<other branch>=.
- =git branch -d <branch to be deleted>=: Delete a branch.
- =git merge <branch>=: Merge the =<branch>= into the current branch.
- =git push -u origin <local branch>=: Push a local =<local branch>=.
- =git push --delete origin <remote branch>=: delete a remote branch.
- =git branch -d <branch>=: delete a local branch

** Working with the remote

- =git pull=: Get the latest remote changes.
- =git remote show origin=: Get all the information about the remote repository.
- =git branch -r=: List of the remote branches 
- Workflow to update the remote repository:
  1. =git pull=
  2. =git merge=
  3. =git push=
- Workflow to update the local repository in two steps using =git fetch=:
  1. =git fetch=: Will pull all the metainfo but won't change the actual files
  2. =git merge origin/<branch>=:
- Workflow to update the local repository in one step using =git pull=:
  1. =git pull origin <remote branch>=: 

** Rebasing

- Rebasing workflow of =<branch>= on top of =main= (recall that the advantage of rebasing is that it keeps the history linear, as opposed to the three-way merging):
  1. =git checkout <branch> --=
  2. =git rebase main=: Rebase, i.e., move the current branch on top of the =main=
  3. If there are some conflicts to settle, then just fix them and commit them. If it makes sense manually, then all good. Otherwise, =git rebase --abort= and then run =git rebase --interactive <branch>= to pick the changes you want in the current branch.
  5. =git rebase --continue= if satisfied with everything.

Before rebasing:
#+BEGIN_SRC 
,* ba8a8f8 (HEAD -> main) mainemacs file.txt                                                                                                            [69/117]
,* dfc0bca main 1                                                               
,* 6ed2b74 main 0                       
| * faf2c49 (FFR) Moved around branch line
| * b4bfff8 branch 2   
| * f33b384 branch 1                   
| * e87aaa1 branch 0                                                           
|/                                     
,* d9a583b (origin/main) Prepare the clean slate                   
,* e5c58b4 Clean slate back in main
#+END_SRC


After rebasing:
#+BEGIN_SRC 
,* 52bad8f (HEAD -> main) Interactive rebasing // Had to do some picking here with the interactive rebasing.
,* faf2c49 (FFR) Moved around branch line
,* b4bfff8 branch 2
,* f33b384 branch 1
,* e87aaa1 branch 0
,* d9a583b (origin/main) Prepare the clean slate
,* e5c58b4 Clean slate back in main
#+END_SRC
** Collaboration

** Multiple GitHub accounts

#+BEGIN_SRC
Host *
     IgnoreUnknown AddKeysToAgent,UseKeychain
     AddKeysToAgent yes
     UseKeychain yes
     IdentityFile ~/.ssh/id_rsa
#+END_SRC

* TensorFlow
- [ ] The necessity of prefetching
- [ ] =map= vs. =flat_map=
- [ ] All the documentation on whatever replaces =tf.feature_columns=
- [ ] Use =tf.data= for the DGA classifier
** Loss functions
- Loss functions
  - Regression
    - mean squared error (MSE): default if the target distribution is Gaussian
    - mean squared logarithmic error (MSLE): if the target distribution is spread out. The logarithm will make sure the targets are more "gathered".
    - mean absolute error (MAE): if the target distribution contains outliers. This won't punish them as much as the MSE.
  - Classification
    - Binary classification
      - binary cross entropy
        - sigmoid activation function
      - hinge loss or squared hinge loss function
	- hyperbolic tangent activation function
    - Multiclass classification
      - categorical cross entropy
        - softmax activation function
      - sparse categorical cross entropy: if there are too many categories to be one-hot encoded
	- softmax activation function
      - Kullback-Leibler divergence loss
	- softmax activation function
* Google Cloud Platform
- [ ] Update =get_env_vars()= in the utilities module.
- [X] Machine learning [1/1]
  - [X] AutoML
- [-] Data analytics [3/5]
  - [X] BigQuery (storage and analytics): Data warehouse. Integrated with Vertex AI.
  - [X] Pub/Sub: Expose topics (i.e., antennas)
  - [X] Dataflow (ETL): Batch and streaming data processing patterns. The pipelines are created in Apache Beam then run on Dataflow.
  - [ ] Looker: Visualization and dashboarding of what is in BigQuery or many other databases.
  - [ ] Data studio: Dashboarding and business intelligence. Unlike Looker, it is readily integrated with BigQuery.
  - [ ] BigQuery ML

- Cloud Storage for batch data or Pub/Sub for streaming data. Both lead to Dataflow (ETL). Then the loaded data goes into BigQuery.

Steps:
1. Create a BigQuery dataset with the relevant tables.
2. Create a Cloud Storage bucket for a Dataflow pipeline.
3. Restart the connection to the Dataflow API.
4. Create a new streaming pipeline from template by specifying the input topic from Pub/Sub and the output table in BigQuery.

** Commands

#+BEGIN_SRC
gcloud functions deploy classify_flower --region=europe-west4 --source=function --runtime=python37 --memory=2048MB --trigger-http --allow-unauthenticated --set-env-vars=ENDPOINT_ID=${ENDPOINT_ID}
#+END_SRC

Tensorboard:
#+BEGIN_SRC
pip3 install protobuf==3.20.*
tb-gcp-uploader --tensorboard_resource_name projects/936888853746/locations/europe-west4/tensorboards/8377223072490455040 --logdir=csis-data-warehouse/map/iris/lesson --experiment_name=iris --one_shot=True
#+END_SRC

** Steps [8/11]
*** 
*** Deployment
1. [ ] Create the learner map.
2. [ ] Push the Docker image =europe-west4-docker.pkg.dev/machine-learning-beb494fb9629/map/$MAP_NAME= to the =Artifact registry=.
3. [ ] Upload the data to a bucket (or to BigQuery).
4. [ ] Create a new training pipeline with a custom container under =Training=.
5. [ ] Import the model to the =Model registry= (use a pre-built container).
6. [ ] Deploy the model in the Model Registry to an endpoint. (You may need to push a new image with the endpoint's ID in the =cloud_params.json=.)
7. [ ] Serve [0/2]
   - [ ] Python:
     1. [ ] SSH into the GCP console.
     2. [ ] Download the Docker image.
     3. [ ] Get inside the container.
     4. [ ] pip3 install google-cloud-aiplatform and gcsfs
     5. [ ] Run predict.
   - [ ] =gcloud ai endpoints predict $ENDPOINT_ID --region=$REGION --json-request=request.json=
*** Old (with the creation of the endpoint first---which makes more sense)
1. [X] Create the learner map.
2. [X] Upload the data to a bucket or to BigQuery.
3. [X] Create an endpoint and specify its ID in =cloud_params.json=.
4. [X] Push the Docker image =europe-west4-docker.pkg.dev/machine-learning-beb494fb9629/map/$MAP_NAME= to the =Artifact registry=.
5. [X] Create a new training pipeline under =Training=.
6. [X] Import the model to the =Model registry= (use a pre-built container).
7. [X] Deploy the model to the artifacts registry.
8. [ ] Serve [0/2]
   - [ ] Python:
     1. [ ] SSH into the GCP console.
     2. [ ] Download the Docker image.
     3. [ ] Download the learner container and log into it
     4. [ ] Install google-cloud-aiplatform and gcsfs
     5. [ ] Run predict.
   - [ ] gcloud ai endpoints predict $ENDPOINT_ID --region=$REGION --json-request=request.json
* Image manipulation
** Reduce image sizes
- PDF to PNG: =pdftoppm pairgrid.pdf output.png -png &=
- Reduce PDF size: =gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf pairgrid.pdf &=
* GPG
- Encrypt: =gpg --output DOC.gpg --encrypt --recipient aknary@yahoo.com DOC=
- Decrypt: =gpg --output DOC --decrypt DOC.gpg=
* Emacs
** Generic Emacs commands

F10 to access the menu
CxCf open a file
Cxb switch to buffer (with autocompletion, includes *Messages*, *scratch*)
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

Cu 8 * insert eight characters *

Md delete the next word
Cd delete the next character

This is the first sentence. This is the second sentence.

Mk kill (and copy) to the end of the sentence
Ck kill (and copy) to the end of the line.
C<space> select Cw: Select a passage

here is the way that we did it.  

My after Cy cycles back in the set of kills

C/ undo
C_ undo

Cg C_ redo, then cycle back into undo

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

** Outline

Text text text!
 * more items
 * yet more

*Bold*, /italic/, =verbatim=, +strikethrough+ 

- bulleted
- list
- items

[[https://csis.com][CSIS Security Group]]

** Tables


| Some     | Data                   |           |
|----------+------------------------+-----------|
| Python   | This is where it goes! |           |
| 12       |                        | now what? |
| ⵜⴰⵎⴰⵣⵉⵖⵜ |                        |           |

CcCe ways to export

** Code

#+BEGIN_SRC python

  def hello(name):
      print(name)

  myname = Amine

  hello(myname)

#+END_SRC

CcCc will evaluate the results

** LaTeX integration

- Characters: \alpha \rightarrow \beta
- $O(n \log n)$

\begin{align*}
a & = 3 \cdot 3 + 1\\
& = 1 
\end{align*}

Note that one can export to beamer as LaTeX, but that seems to require some install.

** Literate programming

Literate programming is mixing natural language and computer programming. It is only useful for reserach papers or didactic purposes.

** Todo stuff

** TODO do this. Use ShiftM Enter to create another one
** TODO cycle through states CcCt
** TODO Now what?

* Linux
** The shell

Environment variables
- Username $USER
- Home directory $HOME
- Path variable $PATH

# Find out where echo is stored.
which echo

rwxrwxrwx
1 owner of the file, 2 group, 3 everybody else

For files: [r] read, [w] write, [x] execute
For directories: [r] list, [w] rename/create files in the directory, [x] search (entry rights into the directroy)

# Alias for clear
CTRL-L 

# Streams
echo hello > hello.txt

# The following are equivalent
cat notes.txt
cat < notes.txt

# The following are equivalent
cat < notes.txt > notes2.txt
cp notes.txt notes2.txt

# To append, use the double-angle bracket
cat < notes.txt >> notes2.txt

# Pipe: the output to the program to the left is the input to the program to the right. For example
ls -l | tail -n1
ls -l | tail -n1 > ls.txt
curl --head --silent adaptive.finance | grep -i content-length | cut --delimiter=' ' -f2

# The root user has user ID 0

# The kernel is the core of the computer

# This has all kinds of parameters from the computer
cat /sys/class/backlight/intel_backlight/brightness

cd /sys/class/backlight/intel_backlight
echo 500 > brightness  # Permission error
sudo echo 500 > brightness # Permission error because brightness is the user (shell $) even if "sudo echo 500" is by the (root #).

# Run the following command as root and get into the superuser mode (the prompt will change from dollar to pound)
sudo su

# We don't need sudo su if we instead run the following, which is the preferred method.
cat max_brightness | sudo tee brightness

# Switch the scroll-lock LED on or off
cd /sys/class/leds/input36::scrolllock
echo 1 | sudo tee brightness

# Open a file using the preferred application.
xdg-open syllabus.pdf
open syllabus.pdf

# On Windows, use the Windows Subsystem for Linux

# Without "chmod u+x" a calling a script with "./semester" starting with "#!/bin/sh"  will not work, but
sh semester
# will work

# Extarct the line that specifies the last-modified date.
./semester | grep -i last-modified > last-modified.txt

** Shell scripting
# Use source to load the definition of the function. This should be done every time the function is modified.
source mcd.sh

# and then run
mcd MyNewDirectory

# Argument of the last command
echo $_

# Apply the previous command with sudo privileges
sudo!!

# Status of the last command (all fine 0; error 1)
echo $?

# Try the first before the ||. If false, try the second.
false || echo "Oops fail"

# Try the first. If false, don't try anything else
true && echo "will be printed"
false && echo "will not be printed"

# Concatenate commans with a semicolon. Compare the following:
echo "Azul" && echo "Hello"
echo "Azul" || echo "Hello"
echo "Azul" ; echo "Hello"

# Place commands in brackets
foo=$(pwd)
echo $foo

# Suppose you have three files project1 project2 and project42
ls project*  # will return all of them
ls project?  # will only return those that have a single character instead of ?, namely project1 and project2

# Return all the commands that start with ls
ls\t\t

# Same as ls\enter
ls \t\t

# Expansion within ZSH
touch foo{,1,2,3,5,100}
touch foo{1,2,3}bar{1,2,3}  # Cartesian products
touch foo{1..2}bar{a..j}
touch {foo,bar}.{a..j}

mkdir foo bar
touch {foo,bar}/{a..j}
touch foo/x bar/y
diff <(ls foo) <(ls bar)

# To run a python script directly with `./script.py`, make sure to include `#!/usr/local/bin/python` at the beginning.

# CMD1 <(CMD2) is called a process substitution whereby the command CMD1 is temporarily put into a file and fed into the command CMD2. 

sudo apt install shellcheck

# Find a file (type f) or a directory (type d)
find . -name script.py -type f
find . -name "*.txt" -type f

# Find .py files in any subfolder (at any level)
find . -path '**/test/*.py' -type f 

# Find all directories modified within the last day
find . -mtime -1 -type d

# Find recursively names with a certain wildcard. Note the use of the double quotes.
find ~/Python/learners/ -name "*.py"

# To grep recursively inside directories, including symboling links, use a capital R flag
grep -R mytest .

# Find all *.py in all subdirectories that include an import to `learners`. The flag `-n` returns the line number
grep -R -n "import learners" ~/GitHub/**/*.py

# A better interface than grep is offered by ripgrep (rg):
rg -C 3 "import learners" ~/GitHub/**/*.py

# rg can also find files that don't match a certain pattern (--files-without-match) etc. It can also return statistics (--stats)

# History command
history

# Seach a keyword in the history using: CTRL-r for a backward search

# Browser
nnn

** Misc commands

# Mount/unmount disks [sudo apt install -y gnome-disk-utility]
gnome-disks

# Shred files
shred

# List all drives
lsblk

# Kill an application with the mouse
xkill

# Update password with Samba (sudo apt install samba):
smbpasswd -r vstgdc1.csis.local -U ala

# Update everything to stable and bug-fixed
apt update
sudo apt dist-upgrade

# Resume a stopped job
fg %<number>

# List all the devices plugged in
dmesg

# To create a permanent alias in the CLI, to ``~/.bashrc`` or, better yet, install emacs-nox
alias emacs='emacs -nw'

# Kill a job
/etc/init.d/docker stop
sudo !!

# Reboot the machine
sudo reboot

# Disk usage
df

# Printing
lp mytext.txt -o sides=two-sided-long-edge

# FTP/Sync:
rsync -r . ala@bmo.csis.local:/home/ala/sandbox # Copy from local to bmo

# List all hardware
lshw

# Free disk space
sudo apt clean
sudo apt autoremove --purge

# CLI VPN
nmcli connection up csis --ask

# Pretty-print JSONs
python -m json.tool my_json.json

# Reverse IP lookup
dig +short -x 64.34.199.12

# Procsses
netstat -alpnt

# Read binaries
xxd

# Copy a text file to the clipboard
xclip -sel c < myfile.txt

** Networking

# Tells TTL and authoritative servers
dig nordea.dk -t ns

** Graphics

# Reduce PDF size
ps2pdf -dPDFSETTINGS=/ebook input.pdf output.pdf



# Converting SVG to PDF
sudo apt-get install librsvg2-bin
rsvg-convert -f pdf -o t.pdf t.svg

** Configuration in Debian

sudo apt install zsh
sudo apt install (some of these should be optional)
shellcheck, ripgrep (rg), updatedb, locate, fzf [fuzzy matching in searching the history], nnn
sudo apt install -y gnome-disk-utility  # For mounting/unmounting disks, formatting them, etc.
sudo apt install tmux

** tmux

tmux new -s my_session
Cb d

tmux ls

tmux attach -t my_session

Cb PageUp to scroll. Use Esc to get back to normal mode.

* ImageMagick

mogrify -format jpg *.eps
convert -delay 20 bloch*.png bloch.gif

Resize:
mogrify -path fullpathto/temp2 -resize 60x60% -quality 60 -format jpg *.png
* SQL
** MySQL

mysql -u root (-p) -e "create database MY_DB;"
mysql -u root (-p) CAFFS < local_dir/sql_file.sql
use MY_DB;
desc MY_TABLE;
select * from MY_TABLE limit 10;

** PostgreSQL

psql -U secdns
psql -U postgres # (root account---not recommended to use)
\du # Users and their privileges

psql secdns-master -U secdns
\i sql_file.sql # Run a SQL file. (Will need to be connected to a specific database first I presume.)

# Save a log to file
\copy (SELECT zone FROM whitelist) TO '~/Documents/SecureDNS/whitelist.csv' WITH CSV

* Vagrant

vagrant up
vagrant destroy
vagrant reload
vagrant ssh
vagrant halt

Place the config file in the directory that I want to consider as local. Edit the configuration file to specify the default local and remote directories.

* LaTeX

sudo apt-get install texlive-full
sudo apt-get install texmaker
pdflatex document.tex

* PySpark

# Download PySpark from the spark website

pip install pyspark

# Install java
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer

# Launch with the Postgres driver
pyspark --driver-class-path ~/Downloads/postgresql-42.2.5.jar --master local



INSTALLATION OF THE ML TOOLS
============================

# Install Anaconda
chmod ug+x Anaconda3-2018.12-Linux-x86_64.sh
./Anaconda3-2018.12-Linux-x86_64.sh # May need to specify `bash` first
export PATH="<absolute path to>/anaconda3/bin:$PATH"

# Anaconda comes with Python 3.7 but we need 3.6 for TensorFlow to run
conda install python=3.6

# Install TensorFlow, including TensorBoard
pip install tensorflow

# Install Keras
conda install Keras

