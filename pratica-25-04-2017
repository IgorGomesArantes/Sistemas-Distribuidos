#include <stdlib.h>
#include <pthread.h>
#include <iostream>
#include <time.h>
#include <unistd.h>

#define SECOND 1000000

pthread_mutex_t mutex;

char* GetRandomString(int stringSize);
void* ToUpper(void* void_string);

int main (int argc , char * argv []) 
{
	pthread_mutex_init(&mutex, NULL);
	int stringSize = strtol(argv [1] , NULL , 10);
	int threadCount = strtol(argv [2] , NULL , 10);
	char* string = GetRandomString(stringSize);

	std::cout << "Inicio: " << string << std::endl;

	pthread_t* threads = (pthread_t*)malloc(threadCount * sizeof(pthread_t));

	for(int i = 0; i < threadCount; i++)
	{
		pthread_create(&threads[i], NULL, ToUpper, (void*)string);
	}

	std::cout << "Todas threads criadas!" << std::endl;

	for(int i = 0; i < threadCount; i++)
	{
		pthread_join(threads[i], NULL);
	}

	std::cout << "Resultado: " << string << std::endl;

	pthread_mutex_destroy(&mutex);
	free (threads);
	free (string);

	return 0;
}

char* GetRandomString(int stringSize)
{
	srand(time(NULL));

	char* string = (char*)malloc(sizeof(char) * (stringSize + 1));
	
	int i;
	for(i = 0; i < stringSize; i++)
	{
		string[i] = (rand() % ('z' - 'a')) + 'a';
	}
	string[i] = '\0';
	
	return string;	
}

void* ToUpper(void* void_string)
{
	char* string = (char*)void_string;
	bool flag = false;
	
	int distance = 'a' - 'A';

	for (int i = 0; string[i] != '\0'; i++)
	{
		pthread_mutex_lock(&mutex);
		if(string[i] >= 'a' && string[i] <= 'z')
		{		
			
			string[i] -= distance;
			
			std::cout << string << std::endl;

			flag = true;
		}
		pthread_mutex_unlock(&mutex);

		if(flag)
		{
			usleep(SECOND);
			flag = false;
		}
	}
}

//g++ threads.cpp -o prog -pthread
//./prog 80 30
