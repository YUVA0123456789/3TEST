1. Write a C/C++ program to implement the cat command using general file API’s.
###############################################################################
#include<sys/types.h> 
#include<sys/stat.h> 
#include<stdio.h> 
#include<fcntl.h> 
main( int argc,char *argv[3] ) 
{ 
int fd,i; 
char buf[2]; 
fd=open(argv[1],O_RDONLY,0777); 
if(fd==-1) 
{ 
printf("file open error"); 
} 
else 
{ 
while((i=read(fd,buf,1))>0) 
{ 
printf("%c",buf[0]); 
} 
close(fd); 
} 
} 
Output: [UNIXLAB@localhost ~]$ gcc -0 p.out prgm1.c 
UNIXLAB@localhost ~]$cat >h.txt /*creating a text file*/ 
hello 
UNIXLAB@localhost ~]$./p.out h.txt /*appending text file to display contents on the 
terminal without using cat command*/ 
hello



3. Write a C/C++ program to implement the cp (copy) command using general file API’s.

########################################################################################

#include <stdio.h> 
#include <stdlib.h 
#include <fcntl.h> 
#include <errno.h> 
#define BUFF_SIZE 1024 
int main(int argc, char* argv[]) 
{ 
 int srcFD,destFD,nbread,nbwrite; 
 char *buff[BUFF_SIZE]; 
  
 /*Check if both src & dest files are received*/ 
 if(argc != 3) 
 { 
  printf("\nUsage: cpcmd source_file destination_file\n"); 
  exit(EXIT_FAILURE); 
 } 
  
 /*Open source file*/ 
 srcFD = open(argv[1],O_RDONLY); 
  
 if(srcFD == -1) 
 { 
  printf("\nError opening file %s errno = %d\n",argv[1],errno); 
  exit(EXIT_FAILURE);  
 } 
  
 /*Open destination file with respective flags & modes 
   O_CREAT & O_TRUNC is to truncate existing file or create a new file 
   S_IXXXX are file permissions for the user,groups & others*/ 
 destFD = open(argv[2],O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR |    
S_IWUSR | S_IRGRP | S_IWGRP | S_IROTH | S_IWOTH); 
  
 if(destFD == -1) 
 { 
  printf("\nError opening file %s errno = %d\n",argv[2],errno); 
  exit(EXIT_FAILURE); 
 } 
  
 /*Start data transfer from src file to dest file till it reaches EOF*/ 
 while((nbread = read(srcFD,buff,BUFF_SIZE)) > 0) 
 { 
  if(write(destFD,buff,nbread) != nbread) 
   printf("\nError in writing data to %s\n",argv[2]); 
 } 
  
 if(nbread == -1) 
  printf("\nError in reading data from %s\n",argv[1]); 
  
 if(close(srcFD) == -1) 
  printf("\nError in closing file %s\n",argv[1]); 
  
 if(close(destFD) == -1) 
  printf("\nError in closing file %s\n",argv[2]); 
  
 exit(EXIT_SUCCESS); 
} 
Output: Output: gcc prgm2.c 
[UNIXLAB@localhost ~]$ ./a.out q.txt j.txt 
UNIXLAB@localhost ~]$cat q.txt 
hello 
UNIXLAB@localhost ~]$cat j.txt 
hello


3. Write a C/C++ program to implement the ln/rename command using general file API’s.

######################################################################################

#include <stdio.h> 
#include<unistd.h> 
int main(int argc, char *argv[]) 
{ 
if(argc!=3) 
{ 
printf(“usage: %s <src_file><dest_file>\n”,argv[0]); 
Return 0; 
} 
if(link(argv[1],argv[2])==-1) 
{ 
printf(“link error\n”); 
return 1; 
} 
printf(“files linked\n”); 
printf(“Inode number of linked files\n”); 
char str[100]; 
sprintf(str,”ls –i %s %s \n”,argv[1],argv[2]); 
system(str); 
return 0; 
} 
Output: gcc prgm1.c 
[UNIXLAB@localhost ~]$ ./a.out j.txt q.txt 
Files linked 
Inode number of linked files 
416529 j.txt 416529 q.txt 



4. Write a C/C++ program to create a file called file1 in blocking read-write mode and show 
how you can use fcntl api to modify its access control flags to non-blocking read-write mode.

 ###################################################################################
 
#include<stdio.h> 
#include<sys/types.h> 
#include<fcntl.h> 
#include<stdlib.h> 
int main(int argc, char *argv[]) 
{ 
int accmode,val; 
if(argc!=2) 
{ 
fprintf(stderr,"usage:%s <description>",argv[0]); 
exit(1); 
} 
val=fcntl(atoi(argv[1]),F_GETFL,0); /* F_GETFL (void) 
Return (as the function result) the file access mode and the 
file status flags;*/ 
if(val<0) 
{ 
perror("fcntl error for fd"); 
exit(1); 
} 
accmode=val & O_ACCMODE; /*This macro stands for a mask that can be bitwise-ANDed 
with the file status flag value to produce a value representing the file access mode. The mode 
will be O_RDONLY, O_WRONLY, or O_RDWR.*/ 
if(accmode==O_RDONLY) 
printf("read only"); 
else if(accmode==O_WRONLY)  
printf("Write only"); 
else if(accmode==O_RDWR) 
printf("read write"); 
else 
{ 
fprintf(stderr,"unknown access mode"); 
exit(1); 
} 
if(val & O_APPEND) 
printf(",append"); 
if(val & O_NONBLOCK) 
printf(",nonblocking"); 
if(val & O_SYNC) 
printf(",synchronous write"); /*wait for writes to complete (data and attributes)*/ 
putchar('\n'); 
exit(0); 
} 
Output: 
gcc p22.c 
[UNIXLAB@localhost ~]$ ./a.out p22.c 
Read-Write 


5.Write a C/C++ program to duplicate the file descriptor of a file Foo to standard input file 
descriptor. 

#############################################################################


#include<stdlib.h> 
#include<unistd.h> 
#include<sys/types.h> 
#include<sys/stat.h> 
#include<fcntl.h> 
#include<stdio.h> 
int main(int argc,char **argv) 
{ 
int fd,nfd; 
if(argc<2){ 
printf("usage:%s pathname\n",argv); 
exit(1); 
} 
if((fd=open(argv[1],O_WRONLY))<0) 
{ 
perror("Problem in opening the file"); 
exit(1); 
} 
if((nfd=fcntl(fd,F_DUPFD,0))==-1) 
{ 
perror("Problem in duplicating fd"); 
exit(1); 
} 
printf("Fd %d duplicated with %d\n",fd,nfd); 
close(fd); 
close(nfd); 
} 
Output: gcc prgm.c 
[UNIXLAB@localhost ~]$ ./a.out  ll.txt 
Fd 3 duplicated with 4 
 
 
gcc prgm.c 
[UNIXLAB@localhost ~]$ ./a.out  ll.txt 
Fd 3 duplicated with 4


6.Write a C/C++ program to query and display the different attributes associated with a file. 

######################################################################################


#include<stdio.h> 
#include<sys/types.h> 
#include<sys/stat.h> 
#include<time.h> 
#include<stdlib.h> 
 
int main(int argc, char *argv[]) 
{ 
struct stat sb; 
if(argc!=2) 
{ 
fprintf(stderr,"usage: %s <pathname>\n", argv[0]); 
exit(EXIT_FAILURE); 
} 
if(stat(argv[1],&sb)==-1) 
{ 
perror("stat"); 
exit(EXIT_FAILURE); 
} 
 
printf("file type:          "); 
switch(sb.st_mode & S_IFMT) 
{ 
case S_IFBLK: printf("block device file\n");   
       break; 
case S_IFCHR: printf("character device file\n");   
       break; 
case S_IFDIR: printf("directory\n");   
       break; 
case S_IFIFO: printf("FIFO/pipe\n");   
       break; 
case S_IFLNK: printf("symlink\n");   
       break; 
case S_IFREG: printf("regular file\n");   
       break; 
case S_IFSOCK: printf("socket\n");   
        break; 
default:       printf("regular file\n");   
        break; 
} 
printf("Inode number:   %ld\n", (long) sb.st_ino); 
printf("Mode:   %lo(octal)\n", (unsigned long) sb.st_mode);  
printf("Blocks allocated:   %lld\n", (long long) sb.st_blocks); 
exit(EXIT_SUCCESS); 
} 
Output: gcc prgm.c 
[UNIXLAB@localhost ~]$ ./a.out  prgm.c 
File type: regular 
Inode number: 1067168 
Mode: 100664(octal) 
Blocks allocated: 8

7.Write C/C++ program to read and display the last 10 character’s of the input file. 

#####################################################################

#include<stdio.h> 
#include<stdlib.h> 
int main() 
{ 
FILE *fp; 
char ch; 
int num; 
long length; 
printf("Enter the value of num:"); 
scanf("%d",&num); 
fp=fopen("file.txt","r"); 
if(fp==NULL) 
{ 
puts("Cannot open this file"); 
exit(1); 
} 
fseek(fp,-1,SEEK_END); 
length=ftell(fp); 
fseek(fp,(length-num),SEEK_SET); 
do 
{ 
ch=fgetc(fp); 
putchar(ch); 
}while(ch!=EOF); 
fclose(fp); 
return(0); 
} 
Output 
cat > l.txt 
hello world hai beautiful girl 
gcc p55.c 
[UNIXLAB@localhost ~]$ ./a.out  
Enter the value of num:4 
Girl 


8.Write a C/C++ program to demonstrate masking of read/write/execute permission of a 
specified input file for user group and others category. 

################################################################################

#include<sys/types.h> 
#include<sys/stat.h> 
#include<stdio.h> 
int main() 
{ 
mode_t oldMask,newMask; 
oldMask=umask((mode_t)0); 
printf("\n Old mask = %o",(int)oldMask); 
if(oldMask & S_IRGRP){ 
printf("\nChanging group read permission from Masked to unmasked.n"); 
oldMask=(oldMask ^ S_IRGRP);/* ^ Operator is binar XOR  operator, copies the bit if it is 
set in one operand but not in both. Exclusive or is a logical operator that outputs true only 
when inputs differ*/ 
} 
newMask=(oldMask|S_IWGRP|S_IXGRP); 
umask(newMask); 
printf("\nNew MAsk = %o",(int)newMask); 
printf("\nThe file mode creation mask now specifies:"); 
printf("\n  Group read permission  UNMASKED"); 
printf("\n  Group write permission  MASKED"); 
printf("\n  Group execute permission  MASKED"); 
oldMask=umask((mode_t)0); 
printf("\n Old mask = %o",(int)oldMask); 
if(oldMask & S_IRUSR){ 
printf("\nChanging user read permission from Masked to unmasked.n"); 
oldMask=(oldMask ^ S_IRUSR); 
} 
newMask=(oldMask|S_IWUSR|S_IXUSR); 
umask(newMask); 
printf("\nNew MAsk = %o",(int)newMask); 
printf("\nThe file mode creation mask now specifies:"); 
printf("\n  User read permission  UNMASKEDn"); 
printf("\n  User write permission  MASKEDn"); 
printf("\n  User execute permission  MASKEDn"); 
oldMask=umask((mode_t)0); 
printf("\n Old mask = %o",(int)oldMask); 
if(oldMask & S_IROTH){ 
printf("\nChanging Other read permission from Masked to unmasked.n"); 
oldMask=(oldMask ^ S_IROTH); 
} 
newMask=(oldMask|S_IWOTH|S_IXOTH); 
umask(newMask); 
printf("\nNew MAsk = %o",(int)newMask); 
printf("\nThe file mode creation mask now specifies:"); 
printf("\n  Other read permission  UNMASKED"); 
printf("\n  Other write permission  MASKED"); 
printf("\n  Other execute permission  MASKED"); 
} 
Output: 
gcc p77.c 
[UNIXLAB@localhost ~]$ ./a.out p77.c 
Old mask = 2 
New MAsk = 32 
The file mode creation mask now specifies:nn 
Group read permission  UNMASKEDn 
Group write permission  MASKEDn 
Group execute permission  MASKEDn 
Old mask = 32 
New MAsk = 332 
The file mode creation mask now specifies: 
User read permission  UNMASKEDn 
User write permission  MASKEDn 
User execute permission  MASKEDn 
Old mask = 332 
New MAsk = 333 
The file mode creation mask now specifies:nn 
Other read permission  UNMASKEDn 
Other write permission  MASKEDn 
Other execute permission sMASKEDnss    

#######################  PART'A' ########################################

1. Execution of various file/directory handling commands.
Change directory (cd):
[Student@localhost /]$ cd ..     /* is current working Directory*/
Student@localhost
 [Student@localhost /]$ cd /   /* change directory to root directory*/

 [LabExam@ISELAB1 local]$ cd ~ /*~(Tilde) takes you back to the home directory*/


[LabExam@ISELAB1 ~]$ cd -  /*switching between present and previous directories or (-)hyphen will take you to the last directory which you have worked on
*/
/home/LabExam

[Student@localhost /]$ cd /usr/local /*Using Absolute Path, We give entire path details here.*/



[Student@localhost lib]$ cd /usr/local /* Change Directory Again*/

[Student@localhost local]$ cd lib /*Using Relative Path, We give only the sub directory name to which we need to change*/

Present working directory(pwd):
[Student@localhost lib]$ pwd  /*Present Working Directory, Prints the full path to the current Working Directory*/

 Output :/usr/local/lib

Ls command: listing files and directories
[Student@localhost local]$ ls  /*Lists the files and Directories under the current working directory

Output:
bin  etc  games  include  lib  libexec  sbin  share  src

[Student@localhost local]$ ls -t    /*List in the order of last modification time*/

Output:
share  bin  etc  games  include  lib  libexec  sbin  src

[Student@localhost local]$ ls -l  /* List all the files, Directories and their modes, number of links,owner of the file, file size, modified date and time and file name*/

total 36


Output:
drwxr-xr-x. 2 root root 4096 Oct  1  2009 bin
drwxr-xr-x. 2 root root 4096 Oct  1  2009 etc
drwxr-xr-x. 2 root root 4096 Oct  1  2009 games

[Student@localhost local]$ ls -a  /*Lists all entries including hidden files*/
.  ..  bin  etc  games  include  lib  libexec  sbin  share  src

Single dot represents hidden directory, double dot represents the parent of the hidden directory

[Student@localhost local]$ ls -d  /*Lists the directory files instead of the contents, here there are no contents.

Output:
.

[Student@localhost local]$ ls -p  /*Puts slash at the end of each directory*/
bin/  etc/  games/  include/  lib/  libexec/  sbin/  share/  src/


[Student@localhost local]$ ls -u  /* Lists in the order of last access time*/
lib  src  games  include  libexec  sbin  share  etc  bin


[Student@localhost local]$ ls -i  /*display inode information*/
2752577 bin  2752579 games    2752581 lib      2752583 sbin   2752607 src
2752578 etc  2752580 include  2752582 libexec  2752584 share


Each file has an inode and is identified by an inode number (i-number) in the file system where it resides. inodes provide important information on files such as user and group ownership, access mode (read, write, execute permissions) and type.

Combining ls options:
[LabExam@ISELAB1 local]$ ls -la
total 44
drwxr-xr-x. 11 root root 4096 Jul  7  2015 .
drwxr-xr-x. 14 root root 4096 Aug  4  2015 ..
drwxr-xr-x.  2 root root 4096 Oct  1  2009 bin
drwxr-xr-x.  2 root root 4096 Oct  1  2009 etc
drwxr-xr-x.  2 root root 4096 Oct  1  2009 games
drwxr-xr-x.  2 root root 4096 Oct  1  2009 include


[LabExam@ISELAB1 local]$ ls -l -a
total 44
drwxr-xr-x. 11 root root 4096 Jul  7  2015 .
drwxr-xr-x. 14 root root 4096 Aug  4  2015 ..
drwxr-xr-x.  2 root root 4096 Oct  1  2009 bin
drwxr-xr-x.  2 root root 4096 Oct  1  2009 etc
drwxr-xr-x.  2 root root 4096 Oct  1  2009 games
drwxr-xr-x.  5 root root 4096 Jul  7  2015 share
drwxr-xr-x.  2 root root 4096 Oct  1  2009 src



Echo command:

[Student@localhost local]$ echo haii   /*Echo is for printing*/

Vi editor:
Output:
Haii
[LabExam@ISELAB1 ~]$ vi dg.sh  /*creating new file using vi editor*/

Cat command:
1. Concatenation:
[LabExam@ISELAB1 ~]$ cat dg.sh td.sh  /* If there are two files, we can concatenate the contents of those two files by using cat command. */
ffhghngh
hjhjjj
fgfgff
hghgh

[LabExam@ISELAB1 ~]$ cat dg.sh
ffhghngh
hjhjjj

[LabExam@ISELAB1 ~]$ cat td.sh
fgfgff
hghgh

2. Creation of new file:
[LabExam@ISELAB1 ~]$ cat > l.txt  /* creating new file on the terminal page*/
this is a beutiful world

3. Displaying contents of file:
[LabExam@ISELAB1 ~]$ cat l.txt  /*displays contents of a file*/
this is a beutiful world

Options with cat command:
[LabExam@ISELAB1 ~]$ cat -n song.txt  /*numbering lines */

     1	i am a student
     2	
     3	

[UNIXLAB@localhost~]$ cat -b t.txt
     1	i am a student


	
[LabExam@ISELAB1 ~]$ cat -e test.sh  /*puts a $ at the end of each line*/

hello everyone, how do you do?$
Hey, am fine.$
How's your training going on?$

Make Directory:(Mkdir)
[LabExam@ISELAB1 local]$ mkdir p
mkdir: cannot create directory `p': Permission denied
[LabExam@ISELAB1 local]$ cd ~
[LabExam@ISELAB1 ~]$ mkdir usp
[LabExam@ISELAB1 ~]$  cd usp  /*We were able to create new directory under home directory*/
[LabExam@ISELAB1 usp]$  mkdir unix /*making subdirectories under parent directory*/

[LabExam@ISELAB1 unix]$       

Move command:(mv)
[LabExam@ISELAB1 ~]$ mv c.txt l.txt  /* mv stands for move. mv is used to move one or more files or directories from one place to another in file system like UNIX. */
 [LabExam@ISELAB1 ~]$ cat c.txt
bash: cd: usp: No such file or directory
[LabExam@ISELAB1 ~]$ cat l.txt
aaaaa
/*moving contents from source file to destination file,source file gets removed permanently after moving*/

Option with mv command :-i
[UNIXLAB@localhost~]$ mv -i ll3.sh l.sh  /* interactive - Attempt to move contents from source to destination file, but prompt before moving to confirm*/
mv: overwrite `l.sh'? yes
[UNIXLAB@localhost~]$ cat ll3.sh
cat: ll3.sh: No such file or directory
[UNIXLAB@localhost~]$ cat l.sh
HHH
    
Moving group of files to a directory:
[LabExam@ISELAB1 ~]$mv t.txt n.txt y.txt usp /*moving group of files to a directory*/
[LabExam@ISELAB1 ~]$cd usp
[LabExam@ISELAB1 usp]$ls
t.txt n.txt y.txt


copy command:(cp)
 [LabExam@ISELAB1 ~]$ cp b.txt c.txt  /* cp stands for copy. This command is used to copy files or group of files or directory. It creates an exact image of a file on a disk with different file name. cp command require at least two filenames in its arguments.*/
 [LabExam@ISELAB1 ~]$ cat b.txt
hello

[LabExam@ISELAB1 ~]$ cat c.txt
hello

Option with copy command:-i
[UNIXLAB@localhost sha]$ cp -i t1.txt t2.txt /*/* interactive - Attempt to copy contents from source to destination file, but prompt before copying to confirm*/
cp: overwrite `t2.txt'? yes
[UNIXLAB@localhost sha]$ cat t1.txt
uuuuu
[UNIXLAB@localhost sha]$ cat t2.txt
uuuuu
 
Remove files : rm command
[LabExam@ISELAB1 ~]$ ls
add.sh  mul.sh  odd.sh  sub.sh b.sh
[LabExam@ISELAB1 ~]$ rm b.txt   /* If you want to delete/remove any file, then using rm command*/
[LabExam@ISELAB1 ~]$ cat b.txt
cat: b.txt: No such file or directory
[LabExam@ISELAB1 ~]$ ls
add.sh  mul.sh  odd.sh  sub.sh 


[UNIXLAB@localhost sha]$ ls
add.sh  mul.sh  odd.sh  sub.sh
[UNIXLAB@localhost sha]$ rm *    /*removing all files from current working directory*/
[UNIXLAB@localhost sha]$ ls
[UNIXLAB@localhost ~]$ ls
21745.sh   a.txt      gokul     new file    p77.cv     p.txt
aa.sh      boi.py     goutham   oddeven.sh  p.cpp      Public
a.awk      b.sh       harshit1  p1.sh       Pictures   qq.sh
aayush     b.sh~      hell      p22.c       pogram.sh  sandesh

[UNIXLAB@localhost ~]$ rm -r * /*removing all files and directories from current working directory*/
[UNIXLAB@localhost ~]$ ls


[LabExam@ISELAB1 ~]$ rm a.txt l.txt  /* Deleting/Removing multiple files using rm command*/
[LabExam@ISELAB1 ~]$ cat a.txt
cat: a.txt: No such file or directory
[LabExam@ISELAB1 ~]$ cat l.txt
cat: l.txt: No such file or directory

rm command with option:
[LabExam@ISELAB1 ~]$rm  -i t.txt /*interactive - Attempt to remove file in the working directory, but  prompt before each file to confirm*/
rm: remove `t2.txt'? yes
[LabExam@ISELAB1 ~]$cat t.txt
cat: t.txt: No such file or directory
 [LabExam@ISELAB1 ~]$ mkdir usp  /*create a directory*/

Remove directory:(rmdir)
[LabExam@ISELAB1 ~]$ rmdir usp  /*Remove a directory*/
[LabExam@ISELAB1 ~]$ cd usp
bash: cd: usp: No such file or directory

Date command:(date)
[UNIXLAB@localhost~]$  date  /*display date and time*/
Thu Nov 22 15:11:53 IST 2018

Process status:(ps)
[UNIXLAB@localhost~]$ ps   /*display process details*/
  PID TTY          TIME CMD
 2354 pts/0    00:00:00 bash
 2637 pts/0    00:00:00 cat
 2640 pts/0    00:00:00 cat
10033 pts/0    00:00:00 cat
10034 pts/0    00:00:00 cat
10040 pts/0    00:00:00 ps


########################################################################################

2. Simple shell script for basic arithmetic and logical calculations. 
#!/bin/sh
a=2
b=4
c=3
val=`expr $a + $b`
echo "Sum = $val"

val=`expr $a - $b`
echo "Difference= $val"
val=`expr $a \* $b`
echo "Product = $val"

val=`expr $b / $a`
echo "quotient = $val"

val=`expr $a % $b`
echo "Modulus = $val"
if [ $a -eq $b ]
then
echo “a is equal to b”
else
echo “a is not equal to b”
fi
if [ $a != $b ]
then
echo “a is not equal to b”
else
echo “a is equal to b”
fi
if [ $a -lt 10 -o $b -gt 20 ]   
then
echo “true”
else
echo “false”
fi

if [ $a -lt 10 -a $b -gt 20 ]   
then
echo “True”
else
echo “False”
fi

[LabExam@ISELAB1 ~]$chmod a+x pr2.sh  /*To execute your shell program, use chmod  a+x program name(For changing mode), a+x  means giving execute  permission to all three category of people(user,group,others).*/

[LabExam@ISELAB1 ~]$sh pr2.sh  /*To run the program use any of the methods: sh program name or ./ program name*/

Sum = 8
Difference = -2
Product =8
Quotient =2
Modulus =2
a is not equal to b
a is not equal to b
True
False

###################################################################################

3. Shell scripts to check various attributes of files and directories. 

#!/bin/sh
file="filename.sh" 
if [ -r $file ] 
then 
echo "File has read permission"
else
echo "File does not have read permission"
fi
if [ -w $file ] 
then 
echo "File has write permission"
else
echo "File does not have write permission" 
fi
 if [ -x $file ]
 then
echo "File has execute permission" 
else 
echo "File does not have execute permission" 
fi
if [ -f $file ]
then 
echo "File is an ordinary file"
else echo "This is a special file"
fi
 if [ -d $file ]
then 
echo "File is a directory" 
else
 echo "File is not a directory"
 fi 
if [ -s $file ]
 then
 echo "File size is zero"
 else 
echo "File size is greater than  zero"
 fi
 if [ -e $file ]
 then 
echo "File exists"
 else 
echo "File does not exist" 
fi 
if [ -b $file ]
 then 
echo "File is a block file "
 else 
echo "File is not a block file" 
fi 
if [ -c $file ]
 then 
echo "File is a character file "
 else 
echo "File is not a character file" 
fi 
output: [LabExam@ISELAB1 ~]$ vi pr3.sh

[LabExam@ISELAB1 ~]$chmod a+x pr3.sh
[LabExam@ISELAB1 ~]$sh pr3.sh

File has read permission
File has write permission
File has execute permission
File is an ordinary file
File size is greater than zero
File exists
File is not a character file is not a block file


############################################################################

4. Shell scripts to check and list attributes of processes. 

#!/bin/sh
ps
echo "ps : process status"
ps -f
echo "ps -f : full listing"
ps -l
echo "ps -l : along listing showing memory related information"
ps -u
echo "ps -u : process of user only"
ps -e
echo "ps -e : all process including user and system process"
ps -a
echo "ps -a : process of all user including processes not listed with terminal"
ps -t
echo "ps -t : processes running on terminal -l along listing showing memory related information"


output:
 [LabExam@ISELAB1 ~]$ sh pr4.sh

[LabExam@ISELAB1 ~]$chmod a+x pr4.sh
[LabExam@ISELAB1 ~]$sh pr4.sh

  PID TTY          TIME CMD
 2053 pts/0    00:00:00 bash
 2422 pts/0    00:00:00 listing.sh
 2423 pts/0    00:00:00 ps
ps = process status

UID        PID  PPID  C STIME TTY          TIME CMD
admin     2053  2043  0 10:46 pts/0    00:00:00 bash
admin     2422  2053  0 11:03 pts/0    00:00:00 /bin/sh ./listing.sh
admin     2424  2422  0 11:03 pts/0    00:00:00 ps -f
ps -f=full listing

F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
0 S  1000  2053  2043  0  80   0 -  2306 wait   pts/0    00:00:00 bash
0 S  1000  2422  2053  0  80   0 -  2045 wait   pts/0    00:00:00 listing.sh
0 R  1000  2425  2422  0  80   0 -  2868 -      pts/0    00:00:00 ps
ps -l=along listing show memory related information

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
admin     1403  0.0  0.2  54036  9432 tty2     Ssl+ 10:46   0:00 /usr/libexec/gd
admin     1408  0.0  0.3  84184 12864 tty2     Sl+  10:46   0:00 /usr/libexec/gn
admin     1510  3.3  3.8 1117800 135452 tty2   Sl+  10:46   0:34 /usr/bin/gnome-
admin     1538  0.0  1.2 155672 44816 tty2     Sl+  10:46   0:00 /usr/bin/Xwayla
admin     1571  0.3  0.2  52168  7748 tty2     Sl   10:46   0:03 ibus-daemon --x
ps -u=process of user only
  
PID TTY          TIME CMD
    1 ?        00:00:01 systemd
    2 ?        00:00:00 kthreadd
    3 ?        00:00:00 rcu_gp
    4 ?        00:00:00 rcu_par_gp
    6 ?        00:00:00 kworker/0:0H-kb
  ps -e=all process including user and system process
  
PID TTY          TIME CMD
  868 tty1     00:00:00 gnome-session-b
  907 tty1     00:00:03 gnome-shell
 1136 tty1     00:00:00 Xwayland
 1163 tty1     00:00:00 ibus-daemon
 1166 tty1     00:00:00 ibus-dconf
 ps -a=process of all user including processes not listed with terminal
 
 PID TTY      STAT   TIME COMMAND
 2053 pts/0    Ss     0:00 bash
 2422 pts/0    S+     0:00 /bin/sh ./listing.sh
 2429 pts/0    R+     0:00 ps -t
ps -t=processes running on terminal -l along listing showing memoryb related information


###################################################################################

5. Write awk script that uses all of its features.
#!/bin/awk  -f
BEGIN {print “START”}
{print $1, “\t”, $3}
END {print “DONE”}
output:
 [LabExam@ISELAB1 ~]$ awk -f pr5.awk a.txt
And am
You can
Me you

LabExam@ISELAB1 ~]$ cat a.txt
And is am I was
You they can take
Me as you print


#############################################################################


6. Write a shell script to display list of users currently logged in.

#!/bin/sh

echo “User logged in:”
users
echo “Current logged in date and time :” 
date
echo “Currently logged in users:” 
who
echo “Currently logged in username:” 
whoami

output:
 [LabExam@ISELAB1 ~]$ sh pr6.sh

[LabExam@ISELAB1 ~]$chmod a+x pr6.sh
[LabExam@ISELAB1 ~]$sh pr6.sh


User logged in:
UNIX LAB UNIX LAB
Current logged in date and time:
Sat sep 29 12:25:30 IST 2018-11-22
Currently logged in users:
UNIXLAB TTY1 2018-09-29 11.08 (:0)
UNIXLAB pts/0 2018-09-29 12.03 (:0.0)
UNIXLAB pts/1 2018-09-29 12.28 (:0.0)
Currently logged in username:
UNIXLAB




















