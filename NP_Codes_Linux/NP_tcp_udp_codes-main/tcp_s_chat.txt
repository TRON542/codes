
#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <string.h>
#include <netdb.h>
#include <unistd.h>
int main() {
    struct sockaddr_in servaddr;
    int fd = socket(AF_INET, SOCK_STREAM, 0);
    bzero(&servaddr, sizeof(servaddr));
    servaddr.sin_port = htons(22000);
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    int b = bind(fd, (struct sockaddr *)&servaddr, sizeof(servaddr));
    listen(fd, 10);
    char client[100];
    char server[100];
    int c_fd = accept(fd, NULL, NULL);
    while (1) {
        bzero(client, 100);
        recv(c_fd, client, sizeof(client), 0);
        printf("client: %s", client);
        if (strncmp(client, "exit", 4) == 0) {
            printf("client exit\n");
            break;
        }
        bzero(server, 100);
        printf("enter msg: ");
        fgets(server, sizeof(server), stdin);
        send(c_fd, server, strlen(server), 0);
    }
    close(c_fd);
    close(fd);
}