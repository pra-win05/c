# File Management 

## Write a C program to create a new text file and write "Hello, World!" to it?

```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
#include<string.h>
#define FILENAME "newfile.txt"
int main()
{
        int fd;
        const char *data = "Hello, World!\n";
        int bytes;
        fd=open(FILENAME,O_WRONLY | O_CREAT | O_TRUNC,0755);
        if(fd == -1)
        {
                perror("Error opening file");
                return 1;
        }

        if((bytes=write(fd,data,strlen(data))) == -1)
        {
                perror("Error writing data");
                close(fd);
                return 1;
        }
        printf("Hello, World! written successfully\n");
        close(fd);
        return 0;
}

```
## Develop a C program to open an existing text file and display its contents?

```c       
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
int main()
{
        int fd;
        const char *filename = "sample.txt";
        int bytes;
        char buffer[512];
        fd=open(filename,O_RDONLY);
        if(fd == -1)
        {
                perror("Error opening file");
                return 1;
        }
        if((bytes=read(fd,buffer,sizeof(buffer))) != 0)
        {
                buffer[bytes]='\0';
                printf("%s",buffer);
        }
        if(bytes < 0 )
        {
                perror("Error reading data");
                close(fd);
                return 1;
        }
        close(fd);
        return 0;
}
```
## Implement a C program to create a new directory named "Test" in the current directory?

```c
#include<stdio.h>
#include<dirent.h>
#include<sys/stat.h>

int main()
{
        const char *dirname = "Test";
        if(mkdir(dirname,0755) !=0)
        {
                perror("Error creating directory");
                return 1;
        }
        printf("Directory created successfully!\n");
        return 0;
}
```
## Write a C program to check if a file named "sample.txt" exists in the current directory?
```c
#include<stdio.h>
#include<unistd.h>

int main()
{
        const char *file = "sample.txt";

        if(access(file,F_OK) == 0)
        {
                printf("File '%s' exists\n",file);
        }
        else
        {
                perror("File check");
                return 1;
        }
        return 0;
}
```
## Develop a C program to rename a file from "oldname.txt" to "newname.txt"?
```c
#include<stdio.h>
#include<unistd.h>

int main()
{
        const char *oldname = "oldname.txt";
        const char *newname = "newname.txt";

        if(rename(oldname,newname) != 0)
        {
                perror("Error renaming file");
                return 1;
        }
        printf("File renamed successfully!\n");
        return 0;
}
```
## Implement a C program to delete a file named "delete_me.txt"?
```c
#include<stdio.h>
#include<unistd.h>

int main()
{
        const char *file = "delete.txt";
        if(unlink(file) != 0)
        {
                perror("Error deleting file");
                return 1;
        }

        printf("File deleted successfully!\n");
        return 0;
}
```
## Write a C program to copy the contents of one file to another?
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>

int main()
{
        int s_fd,d_fd;
        char data[1024];
        int bytes;
        s_fd = open("sample.txt", O_RDONLY);
        if(s_fd < 0 )
        {
                perror("Error opening file");
                return 1;
        }
        d_fd = open("copyfile.txt",O_WRONLY | O_TRUNC);
        if(d_fd == -1 )
        {
                perror("Error opening file");
                return 1;
        }

        if((bytes=read(s_fd,data,sizeof(data))) != 0)
        {
                write(d_fd,data,bytes);
        }

        if(bytes < 0)
        {
                perror("Error reading data");
                return 1;
        }
        printf("Successfully copied data from sample.txt to copyfile.txt\n");
        close(s_fd);
        close(d_fd);
        return 0;
}
```
## Develop a C program to move a file from one directory to another? 
```c
#include<stdio.h>
#include<unistd.h>

int main()
{
        const char *old = "direct1/file.txt";
        const char *new = "direct2/file.txt";

        if(rename(old,new) != 0 )
        {
                perror("Error moving file");
                return 1;
        }
        printf("File moved successfully from '%s' to '%s'\n",old,new);        return 0;
}
```
## Implement a C program to list all files in the current directory? 
```c
#include<stdio.h>
#include<dirent.h>
#include<sys/types.h>
int main()
{
        DIR *dir;
        struct dirent *start;

        dir=opendir(".");
        if(dir == NULL)
        {
                perror("opendir");
                return 1;
        }
        printf("Files in current diectory\n");
        while((start=readdir(dir)) != NULL)
        {
                if(start->d_name[0] == '.')
                {
                        continue;
                }
                printf("%s\n",start->d_name);
        }
        closedir(dir);
        return 0;
}
```
## Write a C program to get the size of a file named "file.txt"? 
```c
#include<stdio.h>
#include<sys/stat.h>

int main()
{
        const char *file = "sample.txt";
        struct stat st;
        if(stat(file,&st) !=0)
        {
                perror("stat");
                return 1;
        }

        printf("Size of '%s': %ld bytes\n",file,st.st_size);
        return 0;
}
```
## Develop a C program to check if a directory named "Test" exists in the current directory?
```c
#include<stdio.h>
#include<sys/stat.h>
#include<sys/types.h>

int main()
{
        const char *dirname = "Test";
        struct stat st;

        if(stat(dirname,&st) == 0)
        {
                if(S_ISDIR(st.st_mode))
                {
                        printf("Directory '%s' exists\n",dirname);
                }
                else
                {
                        printf("'%s' exists but is not a directory\n",dirname);
                }
        }
        else
        {
                perror("stat");
                printf("Directory does not exists\n");
        }
        return 0;
}
```
## Implement a C program to create a new directory named "Backup" in the parent directory? 
```c
#include<stdio.h>
#include<sys/stat.h>
#include<sys/types.h>

int main()
{
        const char *dir = "../Backup";

        if(mkdir(dir,0755)!=0)
        {
                perror("Error creating directory");
                return 1;
        }
        printf("Directory '%s' created successfully\n",dir);
        return 0;
}
```
## Develop a C program to delete all files in a directory named "Temp"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<dirent.h>
#include<unistd.h>
#include<sys/stat.h>
#include<string.h>

int main()
{
        const char *path = "Temp";
        DIR *dir;
        struct dirent *start;
        char file[1024];

        dir=opendir(path);
        if(dir == NULL)
        {
                perror("opendir");
                return 1;
        }

        while((start = readdir(dir)) != NULL)
        {
                if(strcmp(start->d_name,".") == 0 || strcmp(start->d_name,"..") == 0)
                {
                        continue;
                }
                snprintf(file,sizeof(file),"%s/%s",path,start->d_name);
                struct stat st;
                if(stat(file,&st) == 0 && S_ISREG(st.st_mode))
                {
                        if(unlink(file) != 0)
                        {
                                perror("unlink");
                                return 1;
                        }
                        else
                        {
                                printf("Deleted: %s\n",file);
                        }
                }
        }
        closedir(dir);
        return 0;
}
```
## Implement a C program to count the number of lines in a file named "data.txt"? 
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>

int main()
{
        int fd=open("01_newfile.c", O_RDONLY);
        if(fd < 0)
        {
                perror("open");
                return 1;
        }
        char data[1024];
        int bytes;
        int count = 0;
        while((bytes=read(fd,data,sizeof(data))) > 0)
        {
                for(int i=0;i<bytes;i++)
                {
                        if(data[i] == '\n')
                        {
                                count++;
                        }
                }
        }
        if(bytes < 0)
        {
                perror("read");
                return 1;
        }
        close(fd);
        printf("Total lines in : %d\n",count);
        return 0;
}
```
## Write a C program to append "Goodbye!" to the end of an existing file named "message.txt"? 
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>

int main()
{
        int fd;
        const char *data = "Goodbye!";
        int bytes;
        fd=open("message.txt",O_WRONLY | O_APPEND);
        bytes =write(fd,data,sizeof(data));
        if(bytes < 0)
        {
                perror("write");
                return 1;
        }
        printf("'%s' appended with message.txt\n",data);
        close(fd);
        return 0;
}
```
## Implement a C program to change the permissions of a file named "file.txt" to read only? 
```c
#include<stdio.h>
#include<sys/stat.h>

int main()
{
        const char *file = "sample.txt";


        if(chmod(file,0444) != 0 )
        {
                perror("chmod");
                return 1;
        }
        printf("Permissions for '%s' changed to read-only\n",file);
        return 0;
}
```
## Write a C program to change the ownership of a file named "file.txt" to the user "user1"? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pwd.h>
#include<sys/stat.h>
#include<sys/types.h>

int main()
{
        const char *file = "sample.txt";
        const char *username = "praveenkumar";
        struct passwd *pw= getpwnam(username);

        if(pw == NULL)
        {
                fprintf(stderr, "User '%s' not found\n",username);
                return 1;
        }

        uid_t uid= pw->pw_uid;
        gid_t gid= pw->pw_gid;

        if(chown(file,uid,gid) != 0)
        {
                perror("chown");
                return 1;
        }

        printf("Ownership of '%s' changed to use '%s' (UID: %d, GID : %d)\n",file,username,uid,gid);
        return 0;
}
```
## Develop a C program to get the last modified timestamp of a file named "file.txt"? 
```c
#include<stdio.h>
#include<sys/stat.h>
#include<time.h>

int main()
{
        const char *file = "file.txt";
        struct stat st;
        if(stat(file,&st) != 0)
        {
                perror("stat");
                return 1;
        }
        printf("Timestamp of '%s' is %s",file,ctime(&st.st_mtime));
        return 0;
}
```
## Implement a C program to create a temporary file and write some data to it? 
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
#include<stdlib.h>
#include<string.h>

int main()
{

        char template[] = "/tmp/mytempfileXXXXXX";
        int fd=mkstemp(template);
        if(fd == -1 )
        {
                perror("mkstemp");
                return 1;
        }
        printf("Temporary  file created: %s\n",template);
        const char *data = "This is some temporary data\n";
        if(write(fd,data,strlen(data)) == -1)
        {
                perror("write");
                close(fd);
                return 1;
        }
        printf("Data written to the temporary file\n");
        close(fd);
        return 0;
}
```
## Write a C program to check if a given path refers to a file or a directory? 
```c
#include <stdio.h>
#include <sys/stat.h>
#include <unistd.h>

int main(int argc, char *argv[]) {
    if (argc < 2) {
        fprintf(stderr, "Usage: %s /home/praveenkumar/mylinux\n", argv[0]);
        return 1;
    }

    struct stat path_stat;

    if (stat(argv[1], &path_stat) != 0) {
        perror("stat");
        return 1;
    }

    if (S_ISREG(path_stat.st_mode)) {
        printf("'%s' is a regular file.\n", argv[1]);
    } else if (S_ISDIR(path_stat.st_mode)) {
        printf("'%s' is a directory.\n", argv[1]);
    } else {
        printf("'%s' is neither a regular file nor a directory.\n", argv[1]);
    }

    return 0;
}
```
## Develop a C program to create a hard link named "hardlink.txt" to a file named "source.txt"?
```c
#include<stdio.h>
#include<unistd.h>

int main()
{
        const char *src = "shravya.txt";
        const char *linkname = "pk.txt";

        if(link(src,linkname) !=0)
        {
                perror("link");
                return 1;
        }

        printf("Hard link '%s' created for '%s'\n",linkname,src);
        return 0;
}
```
## Implement a C program to read and display the contents of a CSV file named "data.csv"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>

#define SIZE 1024
int main()
{
        int fd;
        ssize_t bytes;
        char buffer[SIZE];

        fd=open("data.csv",O_RDONLY);
        if(fd<0)
        {
                perror("open");
                return 1;
        }
        while((bytes=read(fd,buffer,sizeof(buffer))) > 0)
        {
                if(write(STDOUT_FILENO,buffer,bytes) != bytes)
                {
                        perror("write");
                        close(fd);
                        return 1;
                }
        }
        if(bytes < 0)
        {
                perror("read");
                close(fd);
                return 1;
        }

        close(fd);
        return 0;
}
```
## Write a C program to get the absolute path of the current working directory? 
```c
#include<unistd.h>
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

#define PATH_LEN 4096

int main(void)
{
        char cwd[PATH_LEN];

        if(getcwd(cwd,sizeof(cwd)) == NULL)
        {
                perror("getcwd");
                return 1;
        }

        write(STDOUT_FILENO, "Currentt working directory : ",27);
        write(STDOUT_FILENO, cwd,strlen(cwd));
        write(STDOUT_FILENO,"\n",1);
        return 0;
}
```
## Develop a C program to get the size of a directory named "Documents"? 
```c
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <string.h>
#include <sys/stat.h>
#include <unistd.h>

#define PATH_LEN 4096

int main(void)
{
    DIR *dir;
    struct dirent *entry;
    struct stat st;
    char path[PATH_LEN];
    long long total_size = 0;
    dir = opendir("direct1");
    if (!dir)
    {
        perror("opendir");
        return 1;
    }
    while ((entry = readdir(dir)) != NULL)
    {
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
        {
            continue;
        }
        snprintf(path, sizeof(path), "Documents/%s", entry->d_name);
        if (stat(path, &st) == -1)
        {
            perror("stat");
            continue;
        }
        if (S_ISREG(st.st_mode))
        {
            total_size += st.st_size;
        }
    }

    closedir(dir);
    char buffer[128];
    int len = snprintf(buffer, sizeof(buffer), "Total size: %lld bytes\n", total_size);
    write(STDOUT_FILENO, buffer, len);
    return 0;
}
```
## Write a C program to get the number of files in a directory named "Images"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<dirent.h>
#include<string.h>
#include<string.h>
#include<sys/stat.h>
#include<unistd.h>

#define PATH_LEN 4096
int main()
{
        DIR *dir;
        struct dirent *start;
        struct stat st;
        char path[PATH_LEN];
        int file=0;
        dir=opendir("Images");
        if(!dir)
        {
                perror("opendir");
                return 1;
        }
        while((start=readdir(dir)) !=NULL)
        {
                if(strcmp(start->d_name,".") == 0 || strcmp(start->d_name,"..") == 0)
                {
                        continue;
                }

                snprintf(path,sizeof(path),"Images/%s",start->d_name);

                if(stat(path,&st) == -1)
                {
                        perror("stat");
                        continue;
                }

                if(S_ISREG(st.st_mode))
                {
                        file++;
                }
        }
        closedir(dir);

        char buffer[128];
        int len=snprintf(buffer,sizeof(buffer),"Number of files: %d\n",file);
        write(STDOUT_FILENO,buffer,len);
        return 0;
}
```
## Develop a C program to create a FIFO (named pipe) named "myfifo" in the current directory? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<unistd.h>

int main(void)
{
        const char *fifo="myfifo";
        if(mkfifo(fifo,0666) == -1)
        {
                perror("mkfifo");
                return 1;
        }
        write(STDOUT_FILENO,"FIFO 'myfifo' created successfully\n",36);
        return 0;
}
```
## Implement a C program to read data from a FIFO named "myfifo"? 
```c
#include<fcntl.h>
#include<unistd.h>
#include<stdlib.h>
#include<stdio.h>

#define SIZE 1024

int main()
{
        int fd;
        ssize_t bytes;
        char buffer[SIZE];

        fd=open("myfifo",O_RDONLY);
        if(fd < 0)
        {
                perror("open");
                return 1;
        }

        while((bytes=read(fd,buffer,sizeof(buffer))) > 0)
        {
                if(write(STDOUT_FILENO,buffer,bytes) != bytes)
                {
                        perror("write");
                        close(fd);
                        return 1;
                }
        }
        if(bytes < 0)
        {
                perror("read");
                return 1;
        }
        close(fd);
        return 0;
}
```
## Write a C program to truncate a file named "file.txt" to a specified length?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>

int main(int argc,char *argv[])
{
        const char *file="praveen.txt";
        int length;

        if(argc!=2)
        {
                write(STDOUT_FILENO,"Usage: ./truncate_file\n",24);
                return 1;
        }
        length=atoll(argv[1]);
        if(length < 0)
        {
                write(STDOUT_FILENO, "Length must be non-negative\n",28);
                return 1;
        }

        if(truncate(file,length) == -1)
        {
                perror("truncate");
                return 1;
        }
        write(STDOUT_FILENO,"File truncated successfully!\n",28);
        return 0;
}
```
## Develop a C program to search for a specific string in a file named "data.txt" and display the line number(s) where it occurs? 
```c
#include <fcntl.h>
#include <unistd.h>
#include <string.h>
#include<stdio.h>

#define BUF_SIZE 1024

int main()
{
    int fd, n, i = 0, line_num = 1, found = 0;
    char buf[BUF_SIZE], line[BUF_SIZE], search[100];

    write(1, "Enter string to search: ", 25);
    read(0, search, sizeof(search));
    search[strcspn(search, "\n")] = '\0';

    fd = open("shravya.txt", O_RDONLY);
    if (fd < 0)
    {
        write(2, "Cannot open file\n", 17);
        return 1;
    }
    while ((n = read(fd, buf, BUF_SIZE)) > 0)
    {
        for (int j = 0; j < n; j++)
        {
            line[i++] = buf[j];
            if (buf[j] == '\n')
            {
                line[i] = '\0';
                if (strstr(line, search))
                {
                    char msg[128];
                    int len = snprintf(msg, sizeof(msg), "Found on line %d: %s", line_num, line);
                    write(1, msg, len);
                    found = 1;
                }
                i = 0;
                line_num++;
            }
        }
    }
    if (!found)
    {
        write(1, "Not found\n", 10);
    }

    close(fd);
    return 0;
}
```
## Implement a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path?
```c
#include <sys/stat.h>
#include <unistd.h>
#include <string.h>

int main() {
    char path[256];
    struct stat st;

    printf("Enter path: ");
    fgets(path, sizeof(path), stdin);
    path[strcspn(path, "\n")] = '\0';

    if (stat(path, &st) == -1)
    {
        perror("stat");
        return 1;
    }

    if (S_ISREG(st.st_mode))
    {
        printf("Type: Regular File\n");
    }
    else if (S_ISDIR(st.st_mode))
    {
        printf("Type: Directory\n");
    }
    else if (S_ISLNK(st.st_mode))
    {
        printf("Type: Symbolic Link\n");
    }
    else if (S_ISCHR(st.st_mode))
    {
        printf("Type: Character Device\n");
    }
    else if (S_ISBLK(st.st_mode))
    {
        printf("Type: Block Device\n");
    }
    else if (S_ISFIFO(st.st_mode))
    {
        printf("Type: FIFO (Named Pipe)\n");
    }
    else if (S_ISSOCK(st.st_mode))
    {
        printf("Type: Socket\n");
    }
    else
    {
        printf("Type: Unknown\n");
    }

    return 0;
}
```
## Write a C program to create a new empty file named "empty.txt"? 
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>

int main()
{
        int fd;
        fd=open("empty.txt",O_WRONLY | O_CREAT | O_TRUNC,0644);
        if(fd < 0)
        {
                perror("open");
                return 1;
        }
        printf("File 'empty.txt' created successfully!\n");

        close(fd);
        return 0;
}
```
## Develop a C program to get the permissions (mode) of a file named "file.txt"? 
```c
#include <stdio.h>
#include <sys/stat.h>

void get_modes(mode_t mode)
{
    char perms[10] = "---------";

    if (mode & S_IRUSR) perms[0] = 'r';
    if (mode & S_IWUSR) perms[1] = 'w';
    if (mode & S_IXUSR) perms[2] = 'x';

    if (mode & S_IRGRP) perms[3] = 'r';
    if (mode & S_IWGRP) perms[4] = 'w';
    if (mode & S_IXGRP) perms[5] = 'x';

    if (mode & S_IROTH) perms[6] = 'r';
    if (mode & S_IWOTH) perms[7] = 'w';
    if (mode & S_IXOTH) perms[8] = 'x';

    printf("Permissions: %s\n", perms);
}

int main()
{
    struct stat st;

    if (stat("pk.txt", &st) == -1)
    {
        perror("stat");
        return 1;
    }

    get_modes(st.st_mode);

    return 0;
}
```
## Implement a C program to recursively delete a directory named "Temp" and all its contents? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<dirent.h>
#include<sys/stat.h>
#include<unistd.h>
#include<errno.h>

void remove_dir(const char *path)
{
        struct dirent *entry;
        DIR *dir = opendir(path);
        if(!dir)
        {
                perror("opendir");
                return;
        }

        while((entry = readdir(dir)) != NULL)
        {
                char fullpath[4096];

                if(strcmp(entry->d_name,".") == 0 || strcmp(entry->d_name,"..") == 0)
                {
                        continue;
                }

                snprintf(fullpath,sizeof(fullpath),"%s/%s",path,entry->d_name);

                struct stat st;
                if(stat(fullpath,&st) == -1)
                {
                        perror("stat");
                        continue;
                }
                if(S_ISDIR(st.st_mode))
                {
                        remove_dir(fullpath);
                }
                else
                {
                        if(unlink(fullpath) == -1)
                        {
                                perror("unlink");
                        }
                }
        }
        if(rmdir(path) == -1)
        {
                perror("rmdir");
        }
}
int main()
{
        remove_dir("Temp");
        return 0;
}
```
## Write a C program to read and display the first 10 lines of a file named "log.txt"?
```c
#include<fcntl.h>
#include<unistd.h>
#include<string.h>

#define SIZE 1024

int main()
{
        int fd=open("log.txt",O_RDONLY);
        if(fd < 0)
        {
                const char *err ="Failed to open file\n";
                write(1,err,strlen(err));
                return 1;
        }
        char buf[SIZE];
        char line[SIZE];
        int bytes;
        int lines = 0;
        int lines_print = 0;

        while((bytes = read(fd,buf,SIZE)) >0 && lines_print < 10 )
        {
                for(int i=0; i< bytes && lines_print < 10;i++)
                {
                        line[lines++] = buf[i];
                        if(buf[i] == '\n')
                        {
                                write(1,line,lines);
                                lines = 0;
                                lines_print++;
                        }
                }
        }

        if(lines > 0 && lines_print < 10)
        {
                write(1,line,lines);
        }

        close(fd);
        return 0;
}
```
## Develop a C program to read data from a text file named "input.txt" and write it to another file named "output.txt" in reverse order? 
```c
#include<fcntl.h>
#include<unistd.h>
#include<stdio.h>
#include<sys/stat.h>

int main()
{
        int fd_in,fd_out;
        struct stat st;

        fd_in = open("log.txt",O_RDONLY);
        if(fd_in < 0)
        {
                perror("open log.txt");
                return 1;
        }

        if(fstat(fd_in,&st) == -1)
        {
                perror("fstat");
                close(fd_in);
                return 1;
        }
        int size = st.st_size;
        fd_out = open("logout.txt",O_WRONLY | O_CREAT | O_TRUNC,0644);
        if(fd_out < 0)
        {
                perror("open logout.txt");
                close(fd_in);
                return 1;
        }

        char ch;
        for(int i=size-1;i>= 0;i--)
        {
                if(lseek(fd_in,i,SEEK_SET) == -1)
                {
                        perror("lseek");
                        break;
                }
                if(read(fd_in,&ch,1) != 1)
                {
                        perror("read");
                        break;
                }
                if(write(fd_out,&ch,1) != 1)
                {
                        perror("write");
                        break;
                }
        }

        close(fd_in);
        close(fd_out);

        printf("Reversed content written to logout.txt\n");
        return 0;
}
```
## Implement a C program to create a new directory named with the current date in the format "YYYY-MM-DD"?
```c
#include<stdio.h>
#include<time.h>
#include<sys/stat.h>
#include<sys/types.h>

int main()
{
        char dir[100];
        time_t now = time(NULL);
        struct tm *t = localtime(&now);

        strftime(dir,sizeof(dir),"%Y-%m-%d",t);

        if(mkdir(dir,0755) == -1)
        {
                perror("mkdir");
                return 1;
        }
        printf("Directory '%s' created successfully\n",dir);
        return 0;
}
```
## Write a C program to read and display the contents of a binary file named "binary.bin"? 
```c
#include<fcntl.h>
#include<unistd.h>
#include<stdio.h>

#define SIZE 1024
int main()
{
        int fd= open("binary.bin",O_RDONLY);
        if(fd < 0)
        {
                perror("open");
                return 1;
        }

        unsigned char buffer[SIZE];
        int bytes;

        while((bytes = read(fd,buffer,SIZE)) > 0)
        {
                for(int i=0;i<bytes;i++)
                {
                        printf("%02x",buffer[i]);
                        if((i+1) % 16 == 0)
                        {
                                printf("\n");
                        }
                }
        }
        if(bytes < 0)
        {
                perror("read");
        }
        close(fd);
        return 0;
}
```
## Develop a C program to get the size of the largest file in a directory?
```c
#include<stdio.h>
#include<sys/stat.h>
#include<sys/types.h>
#include<dirent.h>
#include<string.h>

#define PATH_LEN 4096

int main()
{
        const char *dirname = ".";
        DIR *dir = opendir(dirname);
        struct dirent *entry;
        struct stat st;
        char filepath[PATH_LEN];

        if(!dir)
        {
                perror("opendir");
                return 1;
        }

        int max_size = 0;
        char large_file[PATH_LEN]="";
        while((entry=readdir(dir)) != NULL)
        {
                if(strcmp(entry->d_name,".") == 0 || strcmp(entry->d_name,"..") == 0 || strcmp(entry->d_name,"a.out") == 0 || strcmp(entry->d_name,"fifo_reader") == 0)
                {
                        continue;
                }

                snprintf(filepath,sizeof(filepath),"%s/%s",dirname,entry->d_name);

                if(stat(filepath,&st) == -1)
                {
                        continue;
                }

                if(S_ISREG(st.st_mode))
                {
                        if(st.st_size > max_size)
                        {
                                max_size = st.st_size;
                                strncpy(large_file,entry->d_name,sizeof(large_file)-1);
                                large_file[sizeof(large_file) -1] = '\0';
                        }
                }
        }

        closedir(dir);
        if(strlen(large_file) > 0)
        {
                printf("Largest file: %s (%ld bytes)\n",large_file,(long)max_size);
        }
        else
        {
                printf("No regular files found in directory\n");
        }
        return 0;
}
```
## Implement a C program to check if a file named "data.txt" is readable? 
```c
#include<stdio.h>
#include<unistd.h>


int main()
{
        const char *filename = "data.txt";
        if(access(filename,R_OK) == 0)
        {
                printf("File '%s' is readable\n",filename);
        }
        else
        {
                perror("File is not readable");
        }
        return 0;
}
```
## Write a C program to create a new directory named "Logs" and move all files with the ".log" extension into it? 
```c
#include<stdio.h>
#include<sys/stat.h>
#include<sys/types.h>
#include<dirent.h>
#include<string.h>
#include<unistd.h>

#define MAX_PATH 4096

int moving(const char *str,const char *suffix)
{
        if(!str || !suffix)
        {
                return 0;
        }

        int lenstr=strlen(str);
        int lensuf=strlen(suffix);
        if(lensuf > lenstr)
        {
                return 0;
        }

        return strcmp(str + lenstr - lensuf,suffix) == 0;
}
int main()
{
        const char *log_dir = "Logs";
        if(mkdir(log_dir,0755) == -1)
        {
                perror("mkdir");
        }

        DIR *dir =opendir(".");
        if(!dir)
        {
                perror("opendir");
                return 1;
        }
        struct dirent *entry;

        char oldpath[MAX_PATH];
        char newpath[MAX_PATH];

        while((entry=readdir(dir)) != NULL)
        {
                if(strcmp(entry->d_name,".") == 0 || strcmp(entry->d_name,"..") == 0)
                {
                        continue;
                }

                if(moving(entry->d_name,".log"))
                {
                        snprintf(oldpath,sizeof(oldpath),"./%s",entry->d_name);
                        snprintf(newpath,sizeof(newpath),"./%s/%s",log_dir,entry->d_name);

                        if(rename(oldpath,newpath) == -1)
                        {
                                perror("rename");
                        }
                        else
                        {
                                printf("Moved %s to %s/\n",entry->d_name,log_dir);
                        }
                }
        }
        closedir(dir);
        return 0;
}
```
## Develop a C program to check if a file named "config.ini" is writable?
```c
#include<stdio.h>
#include<unistd.h>

int main()
{
        const char *filename = "config.ini";
        if(access(filename,W_OK) == 0)
        {
                printf("File '%s' is writable\n",filename);
        }
        else
        {
                perror("File is not writable");
        }
        return 0;
}
```
## Implement a C program to read the contents of a text file named "instructions.txt" and execute the instructions as shell commands?
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

#define MAX_LINE 1024

int main()
{
        FILE *fp=fopen("instructions.txt","r");
        if(!fp)
        {
                perror("fopen");
                return 1;
        }

        char line[MAX_LINE];

        while(fgets(line,sizeof(line),fp))
        {
                int len=strlen(line);
                if(len > 0 && line[len -1] == '\n')
                {
                        line[len - 1] == '\0';
                }

                if(line[0] == '\0')
                {
                        continue;
                }
                printf("Executing: %s\n",line);
                int ret=system(line);
                if(ret == -1)
                {
                        perror("system");
                        break;
                }
        }

        fclose(fp);
        return 0;
}
```
## Write a C program to get the number of hard links to a file named "file.txt"? 
```c
#include <stdio.h>
#include <sys/stat.h>

int main()
{
    struct stat st;

    if (stat("pk.txt", &st) == -1)
    {
        perror("stat");
        return 1;
    }

    printf("Number of hard links to 'pk.txt': %lu\n", (unsigned long)st.st_nlink);
    return 0;
}
```
## Develop a C program to copy the contents of all text files in a directory into a single file named "combined.txt"? 
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<dirent.h>

#define MAX_PATH 4096
#define BUF_SIZE 8192

int textfile(const char *filename)
{
        int len=strlen(filename);
        return (len > 4) & (strcmp(filename+len -4,".txt") == 0);
}

int main()
{
        DIR *dir=opendir(".");

        if(!dir)
        {
                perror("opendir");
                return 1;
        }

        FILE *out=fopen("combined.txt","w");
        if(!out)
        {
                perror("fopen combined.txt");
                closedir(dir);
                return 1;
        }
        struct dirent *entry;
        char buf[BUF_SIZE];
        while((entry=readdir(dir)) != NULL)
        {
                if(entry->d_name[0] == '.')
                {
                        continue;
                }

                if(!textfile(entry->d_name))
                {
                        continue;
                }
                FILE *in=fopen(entry->d_name,"r");
                if(!in)
                {
                        perror("fopen input file");
                        continue;
                }
                fprintf(out,"\n----- Contents of %s -------\n",entry->d_name);

                int n;
                while((n = fread(buf,1,sizeof(buf),in)) > 0)
                {
                        if(fwrite(buf,1,n,out) !=n)
                        {
                                perror("fwrite");
                                fclose(in);
                                fclose(out);
                                closedir(dir);
                                return 1;
                        }
                }
                fclose(in);
        }
        fclose(out);
        closedir(dir);
        printf("All .txt files combined into combined.txt\n");
        return 0;
}
```
## Implement a C program to recursively calculate the total size of all files in a directory and its subdirectories?
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<dirent.h>
#include<string.h>
#include<unistd.h>

#define PATH_LEN 4000

int get_size(const char *path)
{
        struct stat st;
        int size = 0;

        if(stat(path,&st) == -1)
        {
                perror("stat");
                return 1;
        }

        if(S_ISREG(st.st_mode))
        {
                return st.st_size;
        }

        if(S_ISDIR(st.st_mode))
        {
                DIR *dir = opendir(path);
                if(!dir)
                {
                        perror("opendir");
                        return 0;
                }
                struct dirent *entry;
                char fullpath[PATH_LEN];

                while((entry = readdir(dir)) != NULL)
                {
                        if(strcmp(entry->d_name,".") == 0 || strcmp(entry->d_name,"..") ==0)
                        {
                                continue;
                        }

                        snprintf(fullpath,sizeof(fullpath),"%s/%s",path,entry->d_name);
                        size += get_size(fullpath);
                }
                closedir(dir);
        }
        return size;
}
int main(int argc,char *argv[])
{
        const char *dir = ".";
        if(argc > 1)
        {
                dir=argv[1];
        }
        int total = get_size(dir);
        printf("Total size of all file in '%s' (including subdirectories): %lld bytes\n",dir,(long long)total);
        return 0;
}
```
## Write a C program to get the number of bytes in a file named "data.bin"? 
```c
#include <stdio.h>
#include <sys/stat.h>

int main()
{
    struct stat st;

    if (stat("data.bin", &st) == -1)
    {
        perror("stat");
        return 1;
    }

    printf("Size of 'data.bin' is %lld bytes\n", (long long)st.st_size);
    return 0;
}
```
## Develop a C program to create a new directory named with the current timestamp in the format "YYYY-MM-DD-HH-MM-SS"? 
```c
#include <stdio.h>
#include <time.h>
#include <sys/stat.h>
#include <sys/types.h>

int main()
{
    char dirname[64];
    time_t now = time(NULL);
    struct tm *t = localtime(&now);

    if (!t)
    {
        perror("localtime");
        return 1;
    }
    if (strftime(dirname, sizeof(dirname), "%Y-%m-%d-%H-%M-%S", t) == 0)
    {
        fprintf(stderr, "strftime failed\n");
        return 1;
    }
    if (mkdir(dirname, 0755) == -1)
    {
        perror("mkdir");
        return 1;
    }
    printf("Directory created: %s\n", dirname);
    return 0;
}
```
## Write a C program to create a new directory named "Documents" in the current directory?
```c
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>

int main()
{
    const char *dirname = "Documents";
    if (mkdir(dirname, 0755) == -1)
    {
        perror("mkdir");
        return 1;
    }

    printf("Directory '%s' created successfully.\n", dirname);
    return 0;
}
