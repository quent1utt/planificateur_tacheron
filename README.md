# planificateur_tacheron

## Introduction
This project aims to create a Linux command in Bash language called "tacheron". This command is responsible for executing scripts or commands defined and planned in advance. The scheduling of tasks is indeed very useful in computer systems. This command automates tasks that must be performed at fixed or periodic times.

## Install

### Requirement
- Linux System or Virtual Box or WSL

:exclamation: With WSL on Windows System, the program will only execute the tasks of __your__ session __and__ root (not other users)
This problem is due to deamon on WSL and command "who".

### Installation : 
You must initialize deamon service in your system.
The tree structure detailed above is created and the init_daemon file is called so that the demons_tacheron.sh script starts up as a service on your Linux system. You don't have to program anything. The program takes care of registering the service, putting it in the background, and activating it every time you start your system.

```bash
$ sudo ./tacheron --init
```
### Permissions : 
After this initialization phase, the root user can use the functionality of the tacheron command and allow other users of the machine to use them. To do this, the root must indicate in the __/etc/tacheron.allow__ file the logins of the users he authorizes. If they don't indicate them, they won't be able to use the task scheduler.

If the root does not want to authorize any more a particular user, it must remove it from the same file. From when a user is authorized to use the command and when they are not, they have been able to schedule tasks. The root can simply prevent the execution of future tasks of the user they have just deleted by banning them. To do this, he must enter his login in the __/etc/tacheron.deny_ file__.

## Syntax Command :
• To initialize, display the logs, delete the tree structure or display the help:
```bash
$ ./tacheron <-i | -l | --dell | help >
```
\-i (ou --init) : Initialize the command

\-l (ou --log) : Display logs and __commands outputs__

\--dell (ou -d): Stop the service, remove it from the system, and delete the tree. This is an __uninstall__ of the command.

\help : Display help

• To manually edit, view and delete the __/etc/tacherontab__ or __/etc/tacheron/tacherontab\<user\>__ files:
```bash
$ ./tacheron [ -u user] { -a | -r | -e }
```
\-u user (ou --user) : Mention a specific user

  \-a (ou --aff) : Display the tacherontab file of the specified user
  
  \-r (ou --rem) : Delete the tacherontab file of the specified user
  
  \-e (ou --edit) : Edit the tacherontab file of the specified user
  
• To edit from the Shell the __/etc/tacherontab__ or __/etc/tacheron/tacherontab\<user\>__ files:
```bash
$ ./tacheron <second> <minute> <hour> <day> <month> <day_week> [-u user] <command>
```
\<time field\> : Allows you to program the date and time of the order. These fields take into account several formats.
- x: A precise value
- *: All values of the field
- xx-yy: A valid interval
- xx-zz ~ yy: A valid interval with exclusion
- xx, yy, zz: A list of values
- */x: A periodicity depending on the field

:boom: The \<second\> field only takes into account the values 0, 1, 2, 3, * or a list of these values to determine the interval during which the command should be executed. An * in the second field means that the command will be performed every 15 seconds.

\<command\>: This is the command that will be executed in the task scheduler.
A user enters their schedule following the order. The command checks whether the set of arguments are correct before appending it to the corresponding tacherontab [user] file. If there is a problem, the command will notify it and the user will be prompted to start over.

## Exemples
On Shell :

```bash
$ ./tacheron root * * * * * * date
# Root schedule display of system date every 15 seconds.
```


```bash
$ ./tacheron root 0 30-45~31~37 * * 1,3,6 5 echo week-en is soon
# Root displays the text "week-end is soon" every Friday in January, March, and June, every hour between the 30th and 45th minutes except for the 31st and 37th minutes.
```

```bash
$ ./tacheron quentin 1,3 02 09 * * 1 cal
# Quentin displays the calendar every Monday at approximately 9:02 and 15 seconds and approximately 9:02 and 45 seconds.
```

