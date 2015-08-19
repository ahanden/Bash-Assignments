# Bash-Assignments
An assignment submission system for Linux machines that relies on bash scripts. The particular value of this setup is that it does not require administrative privileges!

## Setup

### Files and directory structure
First, create a folder for your assignments. Under that folder, create an additional folder for each assignment you plan to give out. Under each assignment, create a text file `file_list.txt` containing a list of required files for submission, and a `starting_files` directory that contains all the files students need to begin the assignment. Example:
    Assignments
    |--Assignment1
    |  |--file_list.txt
    |  `--starting_files
    |     `--template_code.py
    |--Assignment2
    |  |--file_list.txt
    |  `--starting_files
    |      |--module1.py
    |      `--template_code.py
    `--Assignment3
      |--file_list.txt
      `--required_files
        |--data.txt
        `--template_code.py

Your `file_list.txt` file should contian one filename per line. When a student makes a submission, these files will be copied into the assignment folder in a new directory named after the user.

The `required_files` directory will be copied to students' home directories when requested.

*Be certain that these files and folders all have full read access to all users*

### turnin.config
The `turnin.config` file must be altered to point to the directory structure you just built. Alter the `assignments_home` variable to reference where the above files are stored. You should also edit the other fields in this file as necessary, especially for the log and pid files.

### PATH
In order for students to make use of the system, you will need to alter the `$PATH` environmental variable of your students. You will need to append the location of the Bash-Assignments folder to everyone's `$PATH` who requires access. If you have sys-admin priveleges, you can do this by modifying either `/etc/profile` or `/etc/environment`, depending on your OS. If you do not have sys-admin priveleges, you can have your students run this command (replace the path appropriately):

`export PATH=$PATH:/path/to/Bash-Assignments`

Students should only have to run this command once to permanently have access to the necessary commands.

### turnin_daemon
The `turnin_daemon` gathers and logs submissions from users. To start the daemon, run `turning_daemon start`. The system will automatically copy submissions to the assignments folders and keep track of when submissions were made in the log file. *Note: the turnin_daemon only needs to run for students to make submissions, not to grab necessary assignment files*

## Grab and Submit
These are the scripts used by students to interact with the system.

### Grab
The `grab` command will copy the designated `starting_files` directory from the assignments folder to the students home folder (or to wherever they run the command from). For example, `grab Assignment1` would create a folder named `Assignment1` in the students home directory containing the file `template_code.py`.

### Submit
The `submit` command allows students to turn in their work on the server. It takes two arguments: the assignment name, and the directory containing their work. The script will check that the students are submitting a valid assignment with all the required files (as defined in the appropriate `file_list.txt`). Once submission is complete, students will get a confirmation message with a time-stamp of when the system recognized their submission. All of the files listed in `file_list.txt` will be copied from the students' submission directory into a new folder under the appropriate assignment. The folder will automatically hve permissions set so a student cannot see what others have submitted.

*The turnin_daemon must be running in order for students to submit assignments. Otherwise the command will throw an error.*
