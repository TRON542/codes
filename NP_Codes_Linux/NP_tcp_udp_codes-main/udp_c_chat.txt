#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <string.h>
#include <netdb.h>
#include <arpa/inet.h>
#include <unistd.h>
int main(){
    char str[100];
    struct sockaddr_in servaddr;
    socklen_t serv_len = sizeof(servaddr);
    int fd=socket(AF_INET,SOCK_DGRAM,0);
    servaddr.sin_port=htons(22000);
    servaddr.sin_family=AF_INET;
    servaddr.sin_addr.s_addr=htonl(INADDR_ANY);
    //bind(fd,(struct sockaddr*)&servaddr,sizeof(servaddr));
    while(1){
        bzero(str,100);
        printf("enter msg:");
        fgets(str,100,stdin);
        sendto(fd,str,100,0,(struct sockaddr*)&servaddr,sizeof(servaddr));
        if(strcmp(str,"exit")==0){
            printf("client exit");
            break;
        }
        bzero(str,100);
        recvfrom(fd,str,100,0,(struct sockaddr*)&servaddr,&serv_len);
        printf("server side:%s",str);
    }
    close(fd);
}