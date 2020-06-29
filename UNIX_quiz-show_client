#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>

#define PORTNUM 15689
#define oops(msg) {perror(msg); exit(1);}

extern int connect_to_server(char *, int);
void talk_with_server(int);

#define LEN 256

int main(int ac, char *av[])
{
    int fd;
    fd = connect_to_server(av[1], PORTNUM);
    if (fd == -1)
        exit(1);
    talk_with_server(fd);
    close(fd);
}

void talk_with_server(int fd)
{
    //시작 메시지 받아오기
    char buf_startmsg[LEN]="\0";
    int n, i;
    n = read(fd, buf_startmsg, LEN);
    write(1, buf_startmsg, n);

    //id 보내기
    char buf_id[LEN]="\0";
    scanf("%s", buf_id);
    write(fd, buf_id, strlen(buf_id));

    //카테고리 받아오기
    char buf_1[BUFSIZ]="\0";
    char buf_2[BUFSIZ]="\0";
    char buf_3[BUFSIZ]="\0";
    int m, k, h;
    m = read(fd, buf_1, BUFSIZ);
    write(1, buf_1, m);
    sleep(1);

    k = read(fd, buf_2, BUFSIZ);
    write(1, buf_2, k);

    h = read(fd, buf_3, BUFSIZ);
    write(1, buf_3, h);

    //카테고리 선택하기
    char category[LEN]="/0";
    scanf("%s", category);
    if(!write(fd, category, strlen(category))) oops("write error");
    sleep(1);

    //문제7번 받고 풀기
    char buf_quiz[LEN]="\0";
    char cor[LEN]="\0";
    int j, o;

    //문제 받기
    for(i=0; i<7; i++){
        o = read(fd, buf_quiz, LEN);
        write(1, buf_quiz, o);
        sleep(2);

        //답 입력
        char answer[LEN] = "\0";
        scanf("%s", answer);
        write(fd, answer, sizeof(answer));
        sleep(2);

        //정답 확인
        j = read(fd, cor, LEN);
        write(1, cor, j);
        sleep(2);
    }

    //결과 출력
    char result[LEN]="\0";
    int r;
    r = read(fd, result, LEN);
    write(1, result, r);

    //등수 출력
    char rank[LEN]="\0";
    int a;
    a = read(fd, rank, LEN);
    write(1, rank,sizeof(rank));

}
