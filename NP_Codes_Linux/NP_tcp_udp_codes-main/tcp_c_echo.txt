#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <string.h>
#include <netdb.h>
#include <arpa/inet.h>
int main(){
    char sendline[100];
    char recvline[100];
    struct sockaddr_in servaddr;
    int fd=socket(AF_INET,SOCK_STREAM,0);
    bzero(&servaddr,sizeof(servaddr));
    servaddr.sin_family=AF_INET;
    servaddr.sin_port=htons(22000);
    servaddr.sin_addr.s_addr=htonl(INADDR_ANY);
    connect(fd,(struct sockaddr*)&servaddr,sizeof(servaddr));
        bzero(sendline,100);
        bzero(recvline,100);
        printf("enter the string\n");
        fgets(sendline,100,stdin); //input line 
        send(fd,sendline,100,0);
        recv(fd,recvline,100,0);
        printf("server sent %s",recvline);
}
