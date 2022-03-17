Formatting Your Data
=============================
Before you start analyzing your data in a programming language (or handing your data off to someone else) there are few things you can do that will make your life easier. [This](./data_bad.xlsx) is a typical example of how you might format your data when you are just viewing it.

![](./data_bad.png?raw=true)

However, several aspects of this will be challenging if you want to work with the data in a programming language. Most analysis and plotting programs will require that your data be in "long form", meaning every variable is represented as a separate column and every observation (sample) is a row. To do this, merged cells and multi-layered headers should be converted to separate variables. Any information represented as formatting (cell colors, bold text, etc) should be converted to a variable. If a variable does not apply to a given observation, just leave the cell blank. [Here](./data_good.csv) is what the same data looks like when formatted as a long-form table.

![](./data_good.png?raw=true)

Before importing your data into a program or sending it out for analysis, use the following checklist.
1. all column names are unique
2. column names do not contain spaces
3. cell values do not contain commas or tabs
4. cells are not merged
5. file is .csv or .tsv

General programming resources
=============================

Atom
-----
[Atom](https://atom.io/) is a great text editor, I generally write scripts here and scp them to a server.
#### Useful add-on packages ####
* Hydrogen - to run individual lines at a time like a jupyter
notebook
* Remote-atom - to be able to edit files on a server

Python
------

[Miniconda](https://docs.conda.io/en/latest/miniconda.html) is a smaller version of the package manager anaconda, it will make it easy to install packages and manage versions.

Jupyter notebooks are a great way to work on python scripts interactively.

R
--
[R studio](https://www.rstudio.com/products/rstudio/download/) is a helpful interactive version of R.


Courses
-------
UCSF [data science initiative](https://www.library.ucsf.edu/data-science/) offer regular free one and two day classes in R and Python, as well as open programming hours with pizza to go get help

most of the classes are based on [software carpentry](https://software-carpentry.org/lessons/) material so if there are no classes coming up that's a good place to start

[Rosalind](http://rosalind.info/problems/locations/) is a great way to learn python while working on biological problems

[swirl](https://swirlstats.com/) is a nice way to learn R interactively

Using the command line
======================
Unix/Linux
-------
check out the data science initiative or software carpentry (above) for some intro lessons or the small [tutorial](https://github.com/callamartyn/linux_tutorial) I put together

Screens
-------
Screens are a super useful way to keep a file open in terminal while you run a program or to run a program in the background while you do something else

Open a new (named) screen session
```
$ screen -S session_name
```
Detach from a current screen session (will send you back to your normal terminal but the session will keep running along with any programs you are running)

<kbd>Ctrl</kbd>+<kbd>a</kbd>  <kbd>d</kbd>

Quit a current screen session

<kbd>Ctrl</kbd>+<kbd>a</kbd>   `:quit`

List all screens
```
$ screen -ls
```
Resume screen session
```
$ screen -r screenID
```
or
```
$ screen -rS session_name
```
Quit a screen by name
```
$ screen -XS session_name
```
Split terminal vertically
<kbd>Ctrl</kbd>+<kbd>a</kbd> <kbd>|</kbd>

Horizontally

<kbd>Ctrl</kbd>+<kbd>a</kbd> <kbd>S</kbd>

Toggle between split terminals

<kbd>Ctrl</kbd>+<kbd>a</kbd> <kbd>tab</kbd>

Unsplit terminals

<kbd>Ctrl</kbd>+<kbd>a</kbd> <kbd>Q</kbd>

Return to previous terminal

<kbd>Ctrl</kbd>+<kbd>a</kbd> <kbd>backspace</kbd>

Next terminal

<kbd>Ctrl</kbd>+<kbd>a</kbd> <kbd>space</kbd>

Wynton instructions
===================
Full instructions [here](https://ucsf-hpc.github.io/wynton/index.html)

Create account
--------------
Anyone affiliated with UCSF can get a free Wynton account, but they won’t have high priority unless associated with a contributing lab. Follow the instructions [here](https://ucsf-hpc.github.io/wynton/about/join.html) to create an account.

Wynton Support
--------------
Wynton admins and other wynton users are very responsive on the [slack](https://join.slack.com/t/ucsf-wynton/signup) channel, it's a great place to troubleshoot problems or double check if what you are doing makes sense

There is also a weekly office hour held by the wynton admins on Tuesdays from 11-noon, the zoom info is contained in the message-of-the-day when you log into wynton


Understanding the nodes
-----------------------
#### Login
The login node is where you can move files around and submit your jobs, don’t try to run any computation here, this is where you will land when you log in with
```
$ ssh <user>@log1.wynton.ucsf.edu
```
or
```
$ ssh <user>@log2.wynton.ucsf.edu
```
#### Optional : changing your config file
To make login faster you can add the server location and username to your config file. From your computer (not logged into wynton) type
```
$ nano ~/.ssh/config
```
Add the following lines
```bash
Host wynton
HostName log1.wynton.ucsf.edu
User <user>

Host wynton2
HostName log2.wynton.ucsf.edu
User <user>
```
<kbd>Ctrl</kbd>+<kbd>x</kbd> to exit, <kbd>Y</kbd> to save changes. You should now be able to log into wynton by typing
```
$ ssh wynton
```
or
```
$ ssh wynton2
```

#### Development
If you need to test out some pieces of your script or test the whole thing on a small sample you can do so from a development node (no job submission required). From wynton type
```
$ ssh dev1
```
#### Data transfer
To transfer data between any two locations use the following general format
```
$ scp /path/to/source/file user@server.edu:~/path/to/destination/
```
For wynton, you will want the destination to be the data transfer node wyndt
```
$ scp /path/to/source/file user@dt1.wynton.ucsf.edu:~/
```
You should always be transferring the data from your local drive, if you need to go the other way just reverse the order of the paths.

Setting up
----------
Ssh into the data transfer node from the login node
```
$ ssh dt1.wynton.ucsf.edu
```
#### Install anaconda
Download anaconda linux bash script from https://www.anaconda.com/distribution/  to your computer. Copy the script onto your linux account by running
```
$ scp /path/to/Anaconda3-2019.03-Linux-x86_64.sh <user>@dt1.wynton.ucsf.edu:~
```
Login to wynton, make the script executable (see below), then run script
```
$ chmod +x Anaconda3-2019.03-Linux-x86_64.sh
$ ./Anaconda3-2019.03-Linux-x86_64.sh
```
**Important!** When running the script it will choose a directory to install and ask you to approve it or change it, if it suggests something like
`/wynton/home/yourname/somedirectory`
Be sure to change it to
`$HOME/somedirectory`
[These directions](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart) are also helpful
If conda was not added to your path in the installation process run
```
$ /path/to/anaconda3/bin/conda init
```
#### Install pip3
Full installation instructions [here](https://pip.pypa.io/en/stable/installing/)
Download pip script
```
$ wget https://bootstrap.pypa.io/get-pip.py
```
Run script
```
$ python3 get-pip.py --user
```
Pip3 is installed but you probably need to run it from the following path
`~/.local/bin/pip3`
To add it to your path so you can start a command with pip3 you need to open your bash profile
```
$ nano ~/.bashrc
```
And add the following line
```bash
export PATH="~/.local/bin/:$PATH"
```
#### Install git
```
$ conda install git
```
Wrapper script and job submission
----------------------------------
Generally you will want to wrap your python/R scripts in a bash shell script in order to submit them through wynton. The format is generally as follows
```bash
#!/bin/env bash
python my_script.py
```
Save it as a file with a .sh extension and make it executable by running
```
$ chmod ugo+x myscript.sh
```
Submit your job
The command to submit your job to the wynton scheduler is as follows, the full details can be found [here](https://ucsf-hpc.github.io/wynton/scheduler/submit-jobs.html)
```
$ qsub -cwd myscript.sh
```
#### Bonus: Using conda environments in job submissions
In order to make your environmental variables (like conda environments) accessible you will need to add the -V flag to your submission. A sample script might look like this

```
#!/bin/bash
#$ -N jobname
#$ -cwd
#$ -pe smp 4
#$ -l mem_free=2G
#$ -l h_rt=01:00:00
#$ -V
source activate env_name
python script.py
```

-cwd runs the job from your current working directory

-l is the estimated run time

To run a job on multiple threads use -pe to specify number of cores and -l to specify RAM/core. In your script use $NSLOTS anywhere you would set the number of cores.

Managing Your Space
-----------------------
You are allocated 1000 GB of space in your home directory. To view how much space
you have remaining simply run

```
beegfs-ctl --getquota --storagepoolid=11 --uid <userid>
```

If, like me, you do not keep track of your usage and suddenly find you have hit
your quota you may want to find out what files to delete to free up some space.
Running

```du -sh ./*```

will show you how much space each file and directory in your current working directory
is taking up. You can add -r for recursive and change the path to view a different
directory

Downloading Data
================
To/From Box
-----------

First, set up a Box lftp password following [these instructions](https://ucsf.app.box.com/services/box_ftp_server)

from wynton, log into a data transfer node
```
ssh dt1
```

access your box account using lftp
```
lftp --user <your.name>@ucsf.edu ftps://ftp.box.com
Password:XXXXXXXXX <- UCSF Box lftp password NOT wynton password
```

to view the contents of your box directory
```
ls
```

to view the contents of your current wynton directory
```
!ls
```

to transfer individual files from box to wynton
```
get myfile.fasta .
```

you can also use wildcards
```
get *R1.fastq .
```

to transfer individual files from wynton to box
```
put ./myfile.fasta .
```

to copy an entire directory to wynton use mirror
```
mirror ./my_box_dir ./my_wynton_dir
```

to copy and directory from wynton to box
```
mirror --reverse ./my_wynton_dir ./my_box_dir
```

From AWS
---------
From wynton, log into a development node
```
ssh dev1
```

Then install the aws command line interface (you may wish to do this in a new conda environment)
```
conda install -c conda-forge awscli
```

One installed, run the aws shell script provided by the biohub or other collaborator. It will look something like

```
export AWS_ACCESS_KEY_ID=SOMEKEY
export AWS_SECRET_ACCESS_KEY=sOmEkEy761
export AWS_SESSION_TOKEN=s0meOtHeRletTTers0837
aws s3 sync s3://czb-seqbot/fastqs/190530_M05295_0279_000000000-G43YR ./my_wynton_dir &
```

be sure to use the & or type "exit" before closing your connecting to put the download in the background


SRA tools
----------
#### Install SRA tools
Download the sratoolkit tar.gz file from https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software

To run on Wynton select Ubuntu Linux 64bit architecture.
Unpack the tar.gz file by running
```
$ tar -xzf sratoolkit.current-centos_linux64.tar.gz
```
#### Selecting samples
To download sample you’ll need to direct the program to accession list
Select the runs you would like to download, click “send to” at the top of the page, send to a file with the format “Accession List”
#### Downloading and unpacking samples
To download each sample in your .txt file as an .sra file run the following command, be sure to specify the correct path to the txt file you created
```
$ /path/to/sratoolkit.2.9.6-ubuntu64/bin/prefetch --option-file SraAccList.txt
```
If you have many samples and would like to unpack them in a loop, create a shell script with some version of the following
```
SRA_files=$(find ~/ncbi/public/sra/ -maxdepth 1 -name "*.sra")
for run in $SRA_files
do
  ~/bin/sratoolkit.2.9.6-ubuntu64/bin/fastq-dump --split-files $run
done
```


Writing code and IDEs
=====================
Connecting Atom to remote server with atom-remote
-------------------------------------------------
Full instructions [here](https://atom.io/packages/remote-atom)
1. Open atom and go to "Settings > Install", search for remote-atom and install
2. Now you will need to install rmate on your wynton account. From the command line enter
```
$ pip install rmate
```
If you don’t have pip installed or want to use a different rmate (there is a version in other languages) you can see all the options at the link above.

3. From your local drive connect to the server with remote port forwarded
```
ssh -R 52698:localhost:52698 user@log1.wynton.ucsf.edu
```
You may want to add this to your config file to avoid typing it out
```
Host wynt-r
HostName log1.wynton.ucsf.edu
User <user>
RemoteForward 52698 localhost:52698
```

Open the file you want to edit in atom with
```
$ rmate somefile.txt
```
The file should open in atom, when you save you should see the changes made on wynton
