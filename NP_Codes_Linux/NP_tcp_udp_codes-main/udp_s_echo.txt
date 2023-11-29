#include <sys/types.h>
#include <sys/socket.h>
#include <strings.h>
#include <stdio.h>
#include <unistd.h>
#include <arpa/inet.h>
int main(){
    char str[100];
    int fd=socket(AF_INET,SOCK_DGRAM,0);
    struct sockaddr_in servaddr,client;
    socklen_t cli_len=sizeof(client);
    servaddr.sin_port=htons(22000);
    servaddr.sin_family=AF_INET;
    servaddr.sin_addr.s_addr=htonl(INADDR_ANY);
    bind(fd,(struct sockaddr*)&servaddr,sizeof(servaddr));
    recvfrom(fd,str,100,0,(struct sockaddr*)&client,&cli_len);
    printf("echo%s",str);
    sendto(fd,str,100,0,(struct sockaddr*)&client,cli_len);
    close(fd);
}