#include <stdio.h>
#include <stdlib.h>
#include <omp.h>
#include <math.h>
int isPrime(int num)
{
    if (num <= 1)
        return 0;
    if (num == 2)
        return 1;
    if (num % 2 == 0)
        return 0;
    for (int i = 3; i < sqrt(num); i++)
    {
        if (num % i == 0)
            return 0;
    }
    return 1;
}
int main()
{
    int n;
    printf("Enter upper limit :");
    scanf("%d", &n);
    if (n < 2)
    {
        printf("There is no prime number less than 2");
        return 0;
    }
    printf("Finding prime numbers from 2 to %d", n);
    double start_time = omp_get_wtime();
    int sequentialPrimeCount = 0;
    for (int i = 1; i <= n; i++)
    {
        if (isPrime(i))
            sequentialPrimeCount++;
    }
    double timeSeq = omp_get_wtime() - start_time;
    printf("\nSequential: Found %d primes in %f seconds.\n", sequentialPrimeCount, timeSeq);
    start_time = omp_get_wtime();
    int parallelPrimeCount = 0;
#pragma omp parallel for reduction(+ : parallelPrimeCount) schedule(dynamic)
    for (int i = 1; i <= n; i++)
    {
        if (isPrime(i))
            parallelPrimeCount++;
    }
    double timePar = omp_get_wtime() - start_time;
    printf("\nSequential: Found %d primes in %f seconds.\n", parallelPrimeCount, timePar);
    if (timePar > 0 && timeSeq > 0)
    {
        printf("\n SpeedUp : %.2fx\n", timeSeq / timePar);
    }
    return 0;
}