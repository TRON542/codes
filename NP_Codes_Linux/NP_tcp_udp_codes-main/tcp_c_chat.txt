
#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <string.h>
#include <netdb.h>
#include <arpa/inet.h>
#include <unistd.h>
int main() {
    struct sockaddr_in servaddr;
    int fd = socket(AF_INET, SOCK_STREAM, 0);
    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_port = htons(22000);
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = inet_addr("127.0.0.1");
    int b = connect(fd, (struct sockaddr *)&servaddr, sizeof(servaddr));
    char server[100];
    char client[100];
    while (1) {
        bzero(client, sizeof(client));
        printf("enter msg: ");
        fgets(client, sizeof(client), stdin);
        send(fd, client, strlen(client), 0);
        if (strncmp(client, "exit", 4) == 0) {
            printf("client exit\n");
            break;
        }
        bzero(server, sizeof(server));
        recv(fd, server, sizeof(server), 0);
        printf("server: %s", server);
    }
    close(fd);
}
