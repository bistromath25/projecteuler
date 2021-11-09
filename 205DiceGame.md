# Project Euler 205 - Dice Game

[Problem statement](https://projecteuler.net/problem=205)

This problem can be solved by determining all possible outcomes of Peter and Colin's dice rolls and finally comparing, for each possible total Peter rolls, how many of Colin's totals it beats. Store the number of ways to make a total of x in two arrays, one for Peter and another for Colin.

    #include <bits/stdc++.h>
    using namespace std;
    
    struct Timer {
        clock_t start;
        Timer() {
            start = clock();
        }
        ~Timer() {
            cout << "execution time: " << fixed << setprecision(3) << (float) (clock() - start) / CLOCKS_PER_SEC << " s" << endl;
        }
    };

    const int M = 36, T = 306110016;
    int peter[M + 1] = {0}, colin[M + 1] = {0};
    int dp = 9, np = 4, dc = 6, nc = 6; 
    
    void dice_sums(int d, int n, int cur, bool b) {
        // recursive function to fill peter[] and colin[]
        if (d == 0) {
            if (b) {
                peter[cur]++;
            }
            else {
                colin[cur]++;
            }
        }
        else {
            for (int i = 1; i <= n; ++i) {
                dice_sums(d - 1, n, cur + i, b);
            }
        }
    }
    
    int main() {
        Timer timer;
        
        dice_sums(dp, np, 0, 1);
        dice_sums(dc, nc, 0, 0);
        
        long long a = 0, b = 0;  // a = number of ways peter beats colin, b = total number of possibilities
        for (int i = 1; i <= M; ++i) {
            for (int j = 1; j < i; ++j) {
                a += peter[i] * colin[j];
            }
            for (int j = 1; j <= M; ++j) {
                b += peter[i] * colin[j];
            }
        }
        
        cout << fixed << setprecision(7) << 1.0 * a / b << endl;
        return 0;
    }

    
