problem name: Modification
problem link: https://vjudge.net/contest/456652#problem/L
prerequisite: dp, partial sum, 2d partial sum

description: 
we have N x M size matrix. 
then we have 2 type of operations.
1. you have to add k to all the cells in the block represented by (r1,c1) as upper left corner and (r2,c2) as the lower right corner.
2. The answer of each query is the sum of values of all the cells in the block
 represented by (r1,c1) as upper left corner and (r2,c2) as the lower right corner.

idea: the naive approach of this problem is not efficient enough. 
so we have to think about partial sum on 2d matrix approach.
the easy way to update range is
 
let's say we have to update N x M matrix in range x1,y1 to x2,y2 with the value k.
we have 4x5 matrix.
now we have to update 2,2 to 3,4 wiht the value 4;

let's say, we have a matrix v, then we have to take another matrix dp. for update the range quere.
after all update we have to just sum up the martix. and then add matrix v and matrix dp.

|---|---|---|---|---|
|   |   |   |   |   |
|---|---|---|---|---|
|   |2,2|   |   |   |
|---|---|---|---|---|
|   |   |   |3,4|   |
|---|---|---|---|---|
|   |   |   |   |   |
|---|---|---|---|---|

dp[x1][y1] += k;
dp[x1][y2+1] -= k;
dp[x2+1][y1] -= k;
dp[x2+1][y2+1] += k;

|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
|---|---|---|---|---|
| 0 | 4 | 0 | 0 |-4 |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
|---|---|---|---|---|
| 0 |-4 | 0 | 0 | 4 |
|---|---|---|---|---|

after the update of the range we have to sum up the matrix. 
for every index i,j. we have to do this operation. 
dp[i][j] += dp[i-1][j]+dp[i][j-1]-dp[i-1][j-1];

|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
|---|---|---|---|---|
| 0 | 4 | 4 | 4 | 0 |
|---|---|---|---|---|
| 0 | 4 | 4 | 4 | 0 |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
|---|---|---|---|---|

then we have to do partial sum on 2d matrix. 
and add with the original matrix.
for(int i=1;i<=n;i++){
    for(int j=1;j<=m;j++){
        v[i][j] += dp[i][j];
    }
}
|---|---|---|---|---|
| 3 | 2 | 1 | 2 | 1 |
|---|---|---|---|---|
| 2 | 2 | 1 | 1 | 2 |
|---|---|---|---|---|
| 1 | 3 | 2 | 1 | 1 |
|---|---|---|---|---|
| 1 | 1 | 2 | 2 | 1 |
|---|---|---|---|---|
then we have to do partial sum on 2d matrix. 
for all i,j index we have to do. 2 operation.
firstly v[i][j] += v[i-1][j];
then   v[i][j] += v[i][j-1];

after doing this operation we found matrix. 
|---|---|---|---|---|
| 3 | 5 | 6 | 8 | 9 |
|---|---|---|---|---|
| 5 | 9 |11 |14 |17 |
|---|---|---|---|---|
| 6 |13 |17 |21 |25 |
|---|---|---|---|---|
| 7 |15 |21 |27 |32 |
|---|---|---|---|---|
now come to the final part of our solution.
|---|---|---|---|---|
| 3 | 5 | 6 | 8 | 9 |
|---|---|---|---|---|
| 5 | 9 |11 |14 |17 |
|---|---|---|---|---|
| 6 |13 |17 |21 |25 |
|---|---|---|---|---|
| 7 |15 |21 |27 |32 |
|---|---|---|---|---|
now we have to find the sum of the submatrix of the original matrix.
let's say we have to find the sum of 2,2 to 3,4;
the idea to find the sum of this submatrix is 
if index is x1,y1 to x2,y2;
sum = v[x2][y2]-v[x1-1][y2]-v[x2][y1-1]+v[x1-1][y1-1];

this is the way to find the submatrix sum. and the complexity is O(nxm);
