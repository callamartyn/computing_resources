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

Using the command line
======================
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
$ ssh qb3-dev1
```
#### Data transfer
To transfer data between any two locations use the following general format
```
$ scp /path/to/source/file user@server.edu:~/path/to/destination/
```
For wynton, you will want the destination to be the data transfer node wyndt
```
$ scp /path/to/source/file user@wyndt1.compbio.ucsf.edu:~/
```
You should always be transferring the data from your local drive, if you need to go the other way just reverse the order of the paths.

Setting up
----------
Ssh into the data transfer node from the login node
```
$ ssh wyndt1.compbio.ucsf.edu
```
#### Install anaconda
Download anaconda linux bash script from https://www.anaconda.com/distribution/  to your computer. Copy the script onto your linux account by running
```
$ scp /path/to/Anaconda3-2019.03-Linux-x86_64.sh <user>@wyndt1.compbio.ucsf.edu:~
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
These directions also helpful https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart
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

Programs I have found useful and how to install them
===================================================
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
IDSeq Command Line Interface
----------------------------
Install aws

Install idseq cli
```
$ pip install git+https://github.com/chanzuckerberg/idseq-cli.git --user --upgrade
```

BLAST
-----
#### Installing on wynton
Log into data transfer node (wyndt1.compbio.ucsf.edu)

Copy link to linux tar.gz file from [here](ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)
```
$ wget <copied link>
$ tar zxvpf ncbi-blast...tar.gz
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
Open the file you want to edit in atom with
```
$ rmate somefile.txt
```
The file should open in atom, when you save you should see the changes made on wynton
