---
title: "Sandesh and his girlfriend"
published: true
---
<p style="text-align:left; color:white">
<a href="/2021/11/19/Amfoss/index.html" >< Introspection</a>
</p>
<br/>

This blog will be exclusively about hackerrank questions i attempted for task-03. Also i have attached the code with each question.

## [Task-03 (Programming)](https://github.com/Akshaj000/amfoss-tasks/tree/master/task-03)

## The game i must lose

This was actually an easy question which i took more than 2-3 days to understand. I struggled with the language here. The core part of this question was there can be more than one round based on the choice they made.
The player wins if he/she gets an even number and the other player gets  odd. If he/she chooses odd there is no way of winning. If its even , then it depends on whose number become odd slower with each round. 
Here is an example in case player one and player two choose even numbers:\
Let player one choose 64 and player two choose 24.\
In the first round, they divide it by two , now the player-1 has 32 and player-2 has 12.\
In the third round, after dividing by two, player1 has 16 and the player2 has 6.\
In the fourth round, after dividing by two, player-1 has 8 and player-2 has 3. This time player-2 is having an odd number. Player-2 is terminated.

### Code:
```c++
#include <iostream>

int divide(int n){
    int count=0;
    while (n%2==0){
        count+=1;
        n/=2;
    }
    return count;
}

using namespace std;
int main(){
    int n , m;
    cin>>n>>m;
    int arr[m];
    for (int i=0 ; i<m ; i++){
        arr[i]=0;
    }

    int count=0;

    if (n%2 == 0){
        for (int i=1 ; i<=m ;  i++){
            if(i%2==1){
                arr[count]=i;
                count+=1;
            }
            else if(i%2==0 && i!=n){
                int countn = divide(n);
                int counti = divide(i);
                if (countn>counti){
                    arr[count]=i;
                    count+=1;
                }
            }
        }
    }
    
    cout<<count<<endl;

    for (int i=0 ; i<m ; i++){
        if(arr[i]!=0){
            cout<<arr[i]<<" ";
        }
    }
    return 0;
}
```

## Lisa and shelves

This was the most easiest question for me. I attempted the question with c++ first. I tried bruteforce and it showed timeout error so i thought of optimising it and came across taking modulo for numbers till N. Let i be numbers from 1 to N, if N%i==0 , I counted it. It showed error again. So I changed i from 1-N to from 1-A , where A is a number such that i*i = A and is greater that or equal to N. After doing all  these, i faced runtime error again. I tried the same question with the same method on python and it worked fine. All i did it wrong in c++ was i forgot to use long int instead of int.

### Code :
using python;
```py
num = int(input("")) 
count = 0;
i=1
while i*i <= num: 
    if num % i == 0: 
        if i*i ==num:
            count+=1
            break
        count+=2
    i+=1
print(count)
```
using c++,
```c++
#include <iostream>
#include<cmath>
using namespace std;

int main(){
    long long int n;
    cin>>n; 
    int count=0;
    long long int i=1;
    while(i*i<=n){
        if(n%i==0){
            if (i*i==n){
                count+=1;
                break;
            }
            count+=2;
        }
        i++;
    }
    cout<<count;
    return 0;
}
```

## Bumper cars

Bumper Cars is the most time consuming question i did. I wasn't confusing at all. I initiially made an array with the length of the bridge, I set the value 1 or -1 in place of cars according to the direction. I kept on figuring out a proper collision algorithm for a week. I wrote pages of zeros and ones in ma book. But whenever i try to code, all of it went in vain. After many tries i realised it has to do something with momentum, actually i didnt think it that way but i compared the outputs. I made all the cars pass through each other without colliding and took the count of the car at the end (left or right) till it go out of the list. I submited and it passed all the test. Then only I realised its all about momentum conservation and elastic collison.    

### Code:
using python,
```py
n,m=input().split()
n,m=int(n),int(m)
position=[]
direction=[]
bridge=[]
position=input().split()
direction=input().split()

for i in range(m+2):
    bridge.append(0);
for i in range(n):
    bridge[int(position[i])]=int(direction[i])

count=0
i=1
while(i<=m):
    if (bridge[i]==1 ):
        bridge[i],bridge[i+1]=bridge[i+1],bridge[i]
        count+=1
    elif(bridge[(m+1)-i]==-1):
        bridge[(m+1)-i],bridge[((m+1)-i)-1]=bridge[((m+1)-i)-1],bridge[(m+1)-i]
        count+=1
        
    i+=1
print(count)
```

## Fight's on
I couldnt solve it. I have posted my code [here](https://github.com/Akshaj000/amfoss-tasks/tree/master/task-03/wrongtrys).

## AverMax
The question was easy to understand. Though it took me some time to figure out the algorithm. I thought of many ways on finding all the subsets and changing signs accordingly. It was so long. Every question which was long was actually the trickiest one so i just compared the input and output. I figured out that the maximum average was actually in the input itself. So i took the maximum of the input as the maximum average. If im allowed to change the signs, then i changed the sign of the most minimum no, that is the least number with negative sign. I compared it with the maximum average i had and assigned this value as maximum average if its greater than the max average.
### Code :
using python,
```py
t=int(input())
ans=[]
for i in range(t):
    n,k=input().split()
    n,k=int(n),int(k)
    listt=[]
    listt=input().split()
    for j in range(n):
        listt[j]=float(listt[j])
    maxm=minm=0
    for j in listt:
        maxm=max(maxm,j)
        minm=min(minm,j)
    minmm=[]
    for j in listt:
        if j<0:
            minmm.append(j)
    if k==0:
        ans.append(maxm)
    else:
        for k in minmm:
            if -k > maxm:
                maxm = -k
        ans.append(maxm)
for i in ans:
    print(i)
```
## Problem solver vs developer
I guess this is the longest code i wrote out of all the other questions. And i find it unnessasary and confusing.I used bubble sorting here for some reason. So i basically made two lists with powers of problem solvers and developers respectively. Sorted both of them. And i eliminated the elements accordingly.  
### Code :
using c++,
```c++
#include <iostream>
using namespace std;


void sort(int arr[] , int n){
    int count=1;
    while (count < n){
        for(int j=0 ; j<n-count ; j++){
            if (arr[j]>arr[j+1]){
                int temp = arr[j+1];
                arr[j+1] = arr[j];
                arr[j] = temp;
            }
        }
        count++;
    }
}

int main(){
    int n , m;
    cin>>n>>m;
    int powdev[n];
    int powsol[m];

    for (int i=0 ; i<n ; i++){
        cin>>powdev[i];
    }
    for (int i=0 ; i<m ; i++){
        cin>>powsol[i];
    }

    sort(powdev , n);
    sort(powsol , m);

    for (int i=0 ; i<m ; i++){
        int cnt=0;
        for (int j=0 ; j<n ; j++){
            if (powdev[j]!=0){
                cnt=1;
                if(powdev[j]<powsol[i]){
                    powdev[j]=0;
                    break;
                }
                else{
                    powsol[i]=0;
                    break;
                }
            }
        }
        if (cnt==0){
            powsol[i]=0;
        }
    }

    int count=0;
    int sum = 0;
    for (int i=0 ; i<n ; i++){
        if (powdev[i]!=0){
            count=1;
        }
    }
    if (count == 0){
        for (int i=0 ; i<m ; i++){
            sum+=powsol[i];
        }
        cout<<"YES"<<endl;
        cout<<sum<<endl;
    }
    else{
        cout<<"NO"<<endl;
    }

    return 0;
}
```

## Sandesh and Strings
I tried it with a time consuming , diffrent approach first. I created two arrays , one with elements which repeats (if a repeats three times that array contains 3 a) and another array with nonrepeating letters.I sorted the non repeating arrays and removed all other elements other than the first element. From the repeating array, sorted it and added first odd digit element to other list if the other list is empty and substracted 1 from every other elements except the even ones. Created and added elements to both side of another array one by one and added the single element from other list. Yeah i realised i was complicating it. So i went for a different similar approach.Made an array with every element. I made a variable having value of one of the odd element. Removed one element from all the odd element.Added half of the even element to another list. Made another list reversing the previous one. Combined both list with oddelement in between. This time i didnt sort any element.  
### Code :
using python,
```py
n=int(input())
string=input()
oddletter=''
letters=[]
count=[]
for i in string:
    if i not in letters:
        letters.append(i)
        count.append(string.count(i))

j=0
for i in count:
    if len(letters)>1:
        if (i%2==1):
            oddletter+=letters[j]
            break
        j+=1

if len(letters)>1:
    for i in range(len(count)):
        if count[i]%2==1:
            count[i]-=1

string=''
if len(letters)>1:
    for i in range(len(count)):
        if count[i]>0:
            for j in range(count[i]//2):
                string+=letters[i]
    string0=string[::-1]
    string+=oddletter
    string+=string0

    print(len(string))
    print(string)
else:
    print(count[0])
    for i in range(count[0]):
        print(letters[0],end='')
```

## Journey to regionals
This task was easy but i faced with some error initially. I figured out adding "ios_base::sync_with_stdio(false);" to the code make it faster for some reason. Im not that good with c++. The algorithm is quiet direct . I sliced the list according to the key and added them.  
### Code :
using c++,
```c++
#include <iostream>
using namespace std;

int main(){
    ios_base::sync_with_stdio(false);
    int n;
    cin>>n;
    int arr[n];
    for (int i=0 ; i<n ; i++ ){
        cin>>arr[i];
    }
    int q;
    cin>>q; 
    int qarr[2*q];
    for (int i=0 ; i<2*q ; i++){
        cin>>qarr[i];
    }
    long int sum[q];
    for(long int i=0 ; i<q ; i++){
        sum[i]=0;
    }

    long int i=0;
    int k=0;
    while(i<2*q){
        for (long int j =qarr[i] ; j<=qarr[i+1] ; j++){
            sum[k]+=arr[j-1];
        }
        i+=2;
        k++;
        
    }
    for (int i=0 ; i<q ; i++){
        cout<<sum[i]<<endl;
    }
    return 0;
}
```

## Building Towers
Ig this is the most done question in this entire task. I made a list having weights as index. And counted the index according to the weight. The count gave me the maximum height and index gave me the weight.
### Code :
using c++,
```c++
#include <iostream>
using namespace std;

int main(){
    int n;
    cin>>n; 
    int rods[n];
    int maxm=0;
    for (int i=0 ; i<n ; i++){
        cin>>rods[i];
        maxm=max(maxm,rods[i]);
    }
    
    int towers[maxm+1];
    for (int i = 0 ; i<maxm+1 ; i++){
        towers[i]=0;
    }

    for(int i=0 ; i<n ; i++){
        towers[rods[i]]+=1;
    }

    int count =0;
    int tmax=0;
    for (int i=0 ; i<maxm+1 ; i++){
        if (towers[i]!=0){
            count+=1;
            tmax = max(tmax , towers[i]);
        }
    }
    cout<<tmax<<" "<<count<<endl;
    
    return 0;
}
```

## Ram and Gifts
I couldnt solve it. I have posted my code [here](https://github.com/Akshaj000/amfoss-tasks/tree/master/task-03/wrongtrys).

<br/>
<p style="text-align:left;">
<a href="/2021/09/25/hello-world/index.html" >< Previous</a>
<span style="float:right;"><a href="/2021/09/27/mars/index.html" >Next ></a>
</span>
</p>

