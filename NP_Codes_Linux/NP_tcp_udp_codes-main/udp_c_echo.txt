#include <sys/types.h>
#include <sys/socket.h>
#include <strings.h>
#include <stdio.h>
#include <unistd.h>
#include <arpa/inet.h>
int main(){
    char str[100];
    int fd=socket(AF_INET,SOCK_DGRAM,0);
    struct sockaddr_in servaddr;
    socklen_t cli_len=sizeof(servaddr);
    servaddr.sin_port=htons(22000);
    servaddr.sin_family=AF_INET;
    servaddr.sin_addr.s_addr=inet_addr("127.0.0.1");
    bind(fd,(struct sockaddr*)&servaddr,sizeof(servaddr));
    printf("enter msg ");
    fgets(str,100,stdin);
    sendto(fd,str,100,0,(struct sockaddr*)&servaddr,cli_len);
    bzero(str,100);
    recvfrom(fd,str,100,0,(struct sockaddr*)&servaddr,&cli_len);
    printf("from server %s",str);
    close(fd);
}
