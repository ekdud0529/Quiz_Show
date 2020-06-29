#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <string.h>
#include <time.h>

#define oops(msg) {perror(msg); exit(1);}
#define len 256

extern int make_server_socket(int);
void process_request(int);

int main(int ac, char*av[]){
    int sock, fd;
    sock = make_server_socket(atoi(av[1]));
    if(sock == -1) exit(1);

    while(1){
        fd = accept(sock, NULL, NULL);
        if(fd == -1) break;

        process_request(fd);
        close(fd);
    }
}

void process_request(int fd){
    char start[len] = "퀴즈를 시작합니다.\n문제는 총 7문제 입니다.\n사용하실 ID를 입력해주세요 : ";
    char ID[len]="\0";
    const char buf[len] = "님 카테고리를 선택해주세요.\n";
    int n;

    // 시작 메세지 출력
    if(!write(fd, start, strlen(start))) oops("start error");;
    sleep(1);

    // ID 읽어오기
    n = read(fd, ID, len);
    if(!write(fd, ID, n)) oops("write ID1");
    if(!write(fd, buf, strlen(buf))) oops("write ID2");
    sleep(1);

    // 카테고리 선택
    const char ctgr[len] = {"0. 넌센스\n1. 컴퓨터\n2. 수도\n3. 역사\n번호 선택 : "};
    if(!write(fd, ctgr, strlen(ctgr))) oops("write menu");
    sleep(1);

    char cate[len];
    if(!read(fd, cate, len)) oops("read category");
    sleep(2);

    //문제 파일 열기
    FILE *fp;
    char quiz[len][len];
    int i;
    switch(cate[0]){
        case '0':
            fp = fopen("sense1.txt", "r");
            if(fp == NULL) oops("fopen");
            i=0;
            while(fgets(quiz[i], len, fp) != 0){
                i++;
            }
            break;

        case '1':
            fp = fopen("computer.txt", "r");
            if(fp == NULL) oops("fopen");
            i=0;
            while(fgets(quiz[i], len, fp) != 0){
                i++;
            }
            break;

        case '2':
            fp = fopen("sense2.txt", "r");
            if(fp == NULL) oops("fopen");
            i=0;
            while(fgets(quiz[i], len, fp) != 0){
                i++;
            }
            break;

        case '3':
            fp = fopen("history.txt", "r");
            if(fp == NULL) oops("fopen");
            i=0;
            while(fgets(quiz[i], len, fp) != 0){
                i++;
            }
            break;

        default :
            oops("category error");
    }
    fclose(fp);
    sleep(1);

    // 문제 보내고 결과 출력하기
    int q[7];
    for(i=0; i<7; i++){
        srand(clock());
        q[i] = rand()%41;
        if(q[i]%2 != 0) q[i]++;
        int j;
        for(j=0; j<i; j++)
            if(q[i] == q[j]){
                i--;
                break;
            }
    }

    int cnt=0;
    const char answ[len] = "답을 입력해주세요(O / X) : ";
    const char fmsg[len] = "틀렸습니다! 까비~\n\n";
    const char tmsg[len] = "맞았습니다! 대단해요!!\n\n";
    for(i=0; i<7; i++){
        int qnum = q[i];
        strcat(quiz[qnum], answ);
        write(fd, quiz[qnum], strlen(quiz[qnum]));
        sleep(2);

        char ans[2] = "\0";
        read(fd, ans, len);
        sleep(5);

        char qu = quiz[qnum+1][0];
        if(ans[0] == qu){
            write(fd, tmsg, strlen(tmsg));
            cnt++;
        }
        else{
            write(fd, fmsg, strlen(fmsg));
        }
        sleep(2);
    }
    sleep(2);

    //총접 출력i
    char result[len] = "\n총 점 : ";
    char score[len] = "";
    sprintf(score, "%d", cnt);
    strcat(score, "\n");
    strcat(result, score);
    write(fd, result, strlen(result));
    sleep(2);

    // 등수 출력하기
    FILE *file;
    char rank[7][len] = {'\0', };
    int x;
    file = fopen("rank.txt", "r");
    if(!file){
        oops("rank open");
    }
    else{
        x=0;
        while(fgets(rank[x], len, file) != 0){
            x++;
        }
    }
    fclose(file);

    int rrr=0;
    int check=0, t;
    char at;
    for(x=1; x<6; x=x+2){
        at = rank[x][0];
        rrr = atoi(&at);
        if(rrr < cnt) {
            check = x;
            break;
        }
    }
    char buff[2];
    sleep(1);

    char ids[len] = "";
    char r[2] = "";
    char idf[len] = "명예의 전당 진입에 실패했습니다.\n";
    FILE *fp2;

    if(check != 0){
        int rnk;
        if(check == 1) rnk = 1;
        if(check == 3) rnk = 2;
        if(check == 5) rnk = 3;
        strcat(ids, ID);
        strcat(ids, "님은 ");
        sprintf(r, "%d", rnk);
        strcat(ids, r);
        strcat(ids, "등 입니다.\n");
        write(fd, ids, strlen(ids));

        sprintf(rank[check], "%d", cnt);
        sprintf(rank[check-1], "%s", ID);
        strcat(rank[check], "\n");
        strcat(rank[check-1], "\n");

        // rank file에 변한 결과 입력
        fp2 = fopen("rank.txt", "w");
        if(fp2){
            for(t=0; t<6; t++){
                fputs(rank[t], fp2);
            }
        }
        else oops("fopen errer");
    }
    else{
        write(fd, idf, strlen(idf));
    }
    fclose(fp2);
}
