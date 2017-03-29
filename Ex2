/*
 ============================================================================
 Name        : Ex2.c
 Author      : Jonathan Geva
 Version     :
 Copyright   : Your copyright notice
 Description : Hello World in C, Ansi-style
 ============================================================================
 */

// Jonathan Geva //
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>
#include <pthread.h>

pthread_t tid[5];
pthread_mutex_t lock, intLock;
char queue[10000] = { "\0" };
int qLen = -1;
int internal_count = 0;
int flag = 0;

char getCh() {
	pthread_mutex_lock(&lock);
	char ch;
	char temp;
	if (qLen == -1) {
		pthread_mutex_unlock(&lock);
		return '\0';
	}
	ch = queue[0];
	int i;
	for (i = 0; i < qLen; i++) {
		queue[i] = queue[i + 1];
	}
	queue[qLen--] = '\0';
	pthread_mutex_unlock(&lock);
	return ch;
}

void putCh(char ch) {
	pthread_mutex_lock(&lock);
	queue[++qLen] = ch;
	pthread_mutex_unlock(&lock);
}

void* threadFunc(void *arg) {
	char temp;
	struct timespec tim;
	tim.tv_sec = 0;

	while (1) {
		temp = getCh();
		if (temp == '\0' && (flag)) {
			pthread_mutex_lock(&intLock);
			printf("%d\n", internal_count);
			pthread_mutex_unlock(&intLock);
			pthread_exit(NULL);
		}
		if (temp != '\0') {
			switch (temp) {
			case '1': {
				tim.tv_nsec = 100000000L;
				nanosleep(&tim, NULL);
				pthread_mutex_lock(&intLock);
				internal_count++;
				pthread_mutex_unlock(&intLock);
				break;
			}
			case '2': {
				tim.tv_nsec = 200000000L;
				nanosleep(&tim, NULL);
				pthread_mutex_lock(&intLock);
				internal_count += 2;
				pthread_mutex_unlock(&intLock);
				break;
			}
			case '3': {
				tim.tv_nsec = 300000000L;
				nanosleep(&tim, NULL);
				pthread_mutex_lock(&intLock);
				internal_count += 3;
				pthread_mutex_unlock(&intLock);
				break;
			}
			case '4': {
				tim.tv_nsec = 400000000L;
				nanosleep(&tim, NULL);
				pthread_mutex_lock(&intLock);
				internal_count += 4;
				pthread_mutex_unlock(&intLock);
				break;
			}
			case '5': {
				tim.tv_nsec = 500000000L;
				nanosleep(&tim, NULL);
				pthread_mutex_lock(&intLock);
				internal_count += 5;
				pthread_mutex_unlock(&intLock);
				break;
			}
			case '6': {
				pthread_mutex_lock(&intLock);
				printf("%d\n", internal_count);
				pthread_mutex_unlock(&intLock);
				break;
			}
			}
		}
	}
}

int main(int argv, char* argc[]) {
	int i = 0;
	int err;
	char input;
	char inputArr[128] = {"\0"};
	if (pthread_mutex_init(&lock, NULL) != 0) // initiate mutex
			{
		printf("\n mutex init failed\n");
		return 1;
	}
	if (pthread_mutex_init(&intLock, NULL) != 0) {
		printf("\n mutex init failed\n");
		return 1;
	}
	for (i = 0; i < 5; i++) {
		err = pthread_create(&(tid[i]), NULL, &threadFunc, NULL);
		if (err != 0) {
			printf("cant create thread : [%s]", strerror(err));
		}
	}
	while (1) {
		scanf("%s",&inputArr);
		input = inputArr[0];
		if (input == '7') {
			for (i = 0; i < 5; i++) {
				pthread_cancel(tid[i]);
			}
			printf("%d\n", internal_count);
			return 0;
		}
		if (input == '8') {
			flag = 1;
			for (i = 0; i < 5; i++) {
				pthread_join(tid[i], NULL);
			}
			printf("%d\n", internal_count);
			return 0;
		}
		putCh(input);
	}
}
