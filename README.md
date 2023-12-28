# task2XenonStack
Problem Statement And Solution:
Scenario There is a customer who came to you with a problem to have a custom linux command for his operations. Our task is to understand the problem and create a linux command via bash script as per the instructions.

Command name - internsctl
Command version - v0.1.0
Section A
I want a manual page of command so that I can see the full documentation of the command. For example if you execute the command man ls as output we get the doc and usage guidelines. Similarly if I execute man internsctl I want to see the manual of my command.

Each linux command has an option --help which helps the end user to understand the use cases via examples. Similarly if I execute internsctl --help it should provide me the necessary help

I want to see version of my command by executing internsctl --version

Solution:
step-1: #!/bin/bash

function display_help() { echo "Usage: internsctl [OPTIONS]" echo "Custom Linux Command for Operations" echo "" echo "Options:" echo " --help" echo " --version" }

function display_version() { echo "internsctl v0.1.0" }

case "$1" in --help) display_help ;; --version) display_version ;; *) echo "Invalid option. Use 'internsctl --help' for usage guidelines." exit 1 ;; esac

step-2: chmod +x internsctl.sh

step-3: ./internsctl.sh --help

step-4: ./internsctl.sh --version

Section B
I want to execute the following command for -

Part1 | Level Easy I want to get cpu information of my server through the following command:
$ internsctl cpu getinfo Expected Output - I want similar output as we get from lscpu command

I want to get memory information of my server through the following command: $ internsctl memory getinfo Expected Output I want similar output as we get from free command

Solution:
step-1: Just add bash script in above program :

function display_help() { echo " cpu getinfo" echo " memory getinfo " }

step-2: function get_cpu_info() { lscpu }

function get_memory_info() { free }

case "$1" in cpu) if [ "$2" == "getinfo" ]; then get_cpu_info else echo "Invalid subcommand for 'cpu'. Use 'internsctl cpu getinfo'." exit 1 fi ;; memory) if [ "$2" == "getinfo" ]; then get_memory_info else echo "Invalid subcommand for 'memory'. Use 'internsctl memory getinfo'." exit 1 fi ;; *) echo "Invalid option. Use 'internsctl --help' for usage guidelines." exit 1 ;; esac

step-3: chmod +x internsctl.sh

step-4: ./internsctl.sh cpu getinfo & ./internsctl.sh memory getinfo

Part2 | Level Intermediate I want to create a new user on my server through the following command: $ internsctl user create Note - above command should create user who can login to linux system and access his home directory
I want to list all the regular users present on my server through the following command: $ internsctl user list

If want to list all the users with sudo permissions on my server through the following command: $ internsctl user list --sudo-only

Solution:
step-1: function display_help() { echo " user create Create a new user" echo " user list" echo " user list --sudo-only" }

step-2: function create_user() { if [ -z "$2" ]; then echo "Error: Missing username. Usage: internsctl user create " exit 1 fi sudo useradd -m "$2" echo "User '$2' created successfully." }

function list_users() { cut -d: -f1 /etc/passwd }

function list_sudo_users() { getent group sudo | cut -d: -f4 | tr ',' '\n' }

step-3:
case "$1" in user) if [ "$2" == "create" ]; then create_user "$@" elif [ "$2" == "list" ]; then if [ "$3" == "--sudo-only" ]; then list_sudo_users else list_users fi else echo "Invalid subcommand for 'user'. Use 'internsctl user create ' or 'internsctl user list [--sudo-only]'." exit 1 fi ;; *) echo "Invalid option. Use 'internsctl --help' for usage guidelines." exit 1 ;; esac

step-4: chmod +x internsctl.sh

step-5:

./internsctl.sh user create testuser
./internsctl.sh user list
./internsctl.sh user list --sudo-only
Part3 | Advanced Level By executing below command I want to get some information about a file $ internsctl file getinfo Expected Output [make sure to have the output in following format only] xenonstack@xsd-034:~$ internsctl file getinfo hello.txt File: hellot.txt Access: -rw-r--r-- Size(B): 5448 Owner: xenonstack
Modify: 2020-10-07 20:34:44.616123431 +0530

In case I want only specific information then I must have a provision to use options $ internsctl file getinfo [options] --size, -s to print size --permissions, -p print file permissions --owner, o print file owner --last-modified, m
