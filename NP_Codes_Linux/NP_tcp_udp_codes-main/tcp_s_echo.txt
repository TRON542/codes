#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <string.h>
#include <netdb.h>
#include <unistd.h>
int main(){
    char str[100];
    struct sockaddr_in servaddr;
    int fd=socket(AF_INET,SOCK_STREAM,0);
    bzero(&servaddr,sizeof(servaddr));
    servaddr.sin_family=AF_INET;
    servaddr.sin_port=htons(22000);
    servaddr.sin_addr.s_addr=htonl(INADDR_ANY);
    bind(fd,(struct sockaddr *)&servaddr,sizeof(servaddr));
    listen(fd,10);
        int c_fd=accept(fd,(struct sockaddr*)NULL,NULL);
        bzero(str,100);
        recv(c_fd,str,100,0);
        printf("Echoing back %s",str);
        send(c_fd,str,strlen(str),0);
        close(c_fd);
}
