
##
[](..... )

거의 3일 내내 풀었음에도 불구하고 (분명히 내 Solution이 맞다는 확신이 있어도 시간 초과로 )부분 점수도 못 받았다.. 하루 만에 쉽게 푼 사람들이 존경스럽게 느껴진다..

이번 대회를 통해..정말 뼈저리게 느낀 것은 “꼴찌를 해도 내 힘으로 끝까지 가봐야 만 배울 수 있는 부분이 있다는 것”이다. 
좌절했지만 너무나 많은 것을 배운 문제였다. 실패에서만이 배우는 것도 분명히 있다.. 성적에 연연하지 말자..

---

이번에 한 알고리듬 DFS(Depth First Search) 에 대해서 이렇게 오랜 시간을 고민해본 적이 처음인 듯 싶다.

처음에는 자바의 Collection 중 HashSet을 이용해서 문제를 풀었는데.. 하루 종일 풀어서 답이 맞을 것이라는 자신감으로 제출했다. 3 문제 중 처음 1번만 맞고 나머지는 시간 초과.. 

```
{1, 2} +  {1, 3} +  {2, 3} = {1,2,3}
{1, 2} + {2, 3} = {1,2,3}
```
위의 첫번째와 두 번째는 마지막 결과는 {1,2,3} 동일하다.
하지만 처음 것은 3번의 입력을 통해서 만들어졌고, 두 번째 것은 2 번의 입력을 통해 만들어진 Set 이다. 자바의 HashSet을 이용하면 빠르다. 문제는 데이타 양이 많아지면 전혀 속도를 내지 못한다는 것..

결국 DFS(Depth First Search) 방법을 사용하기로 하였었다. 이 방법을 사용해서 결국 DFS(Depth First Search) 방법을 사용하기로 하였었다. 이 방법을 사용해서 2차원 배열에 넣으면 다음과 같이 보여준다. 

1,2,3,4 와 같이 4개의 숫자 중 다음 3개가 연결되어 있을 때 다음 배열에 퍼진 관계를 보고 3개가 연결되어 있다는 것을 찾아야 하는 상황이었다. 이 과정에서 알고리듬이 가진 모든 장단점의 질감을 몸으로 체험한 듯 하다..

```
# Case : {1, 2} +  {1, 3} +  {2, 3} = {1,2,3}

0 1 1 0 
1 0 1 0 
1 1 0 0 
0 0 0 0 
```

```
# Case : {1, 2} + {2, 3} = {1,2,3}

0 1 0 0 
1 0 1 0 
0 1 0 0 
0 0 0 0 
```



### TestCase
```
1
5 3
1 2
3 4
4 5
>> 16		
		
1
5 3
1 5
3 5
2 5
>> 20

>>
1
5 2
1 5
3 5
>> 8


1
14 14
7 8
1 2
2 3
1 3
4 5
4 6
5 6
10 11
13 12
12 11
13 11
13 10
12 10
12 9
>> 352
```

### version -1

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Scanner;

public class G {

	static HashMap<HashSet<Integer>,List<Integer>> map = new HashMap<HashSet<Integer>,List<Integer>>();  
	static int totalCnt = 0;
	static int[][] ar = null;
	
	static int f(int n){
		return n * (n-1);
	}
	
	static int sumGroup(int n){
		if(n==1) return f(n);
		return sumGroup(n-1) + f(n);
	}
	
	static void printA(){
		for(int i=0;i<ar[0].length;i++){
			System.out.println(ar[0][i] + " " + ar[1][i]);
		}
	}
	
	static void calculate(){
		int acc = 0;
		for(int i=0;i<ar[0].length;i++){
			int member = ar[0][i];
			int count = ar[1][i];
			//System.out.println(ar[0][i] + " " + ar[1][i]);
			int med = sumGroup(member);
			int full = f(member);
			int remain = totalCnt - (member - 1);
			int med2 = full * remain;
			totalCnt -= (member - 1);
			acc += med + med2;
		}
		System.out.println(acc);
	}
	
	static int[][] sort(int[][] ar2)	{
		int len = ar2[0].length;
		int mid = len/2;
		
		if(ar2[0].length == 1){
			return ar2;
		}else{
			int[][] a = new int[2][mid];
			int[][] b = new int[2][len-mid];
			int a_cnt = 0, b_cnt = 0;
			for(int i=0;i<len;i++){
				if(i<mid) {
					a[0][a_cnt] = ar2[0][i];
					a[1][a_cnt] = ar2[1][i];
					a_cnt++;
				}
				else {
					b[0][b_cnt] = ar2[0][i];
					b[1][b_cnt] = ar2[1][i];
					b_cnt++;
				}
			}
			int[][] l = sort(a);
			int[][] r = sort(b);
			int[][] m = merge(l,r);
			return m;
		}
	}

	static int[][] merge(int[][]a, int[][]b){
		int len_a = a[0].length;
		int len_b = b[0].length;
		int[][] s = new int[2][len_a + len_b];
		int i=0, j=0, idx = 0;

		while(i<len_a && j<len_b){
			if(a[0][i] >= b[0][j]) {
				s[0][idx] = a[0][i];
				s[1][idx] = a[1][i];
				i++;
			}
			else{
				s[0][idx] = b[0][j];
				s[1][idx] = b[1][j];
				j++;
			}
			idx++;
		}
		
		if(i<len_a && j==len_b) {
			while(i<len_a){
				s[0][idx] = a[0][i];
				s[1][idx] = a[1][i];
				idx++;
				i++;
			}
		}
		if(j<len_b && i==len_a)  {
			while(j<len_b) {
				s[0][idx] = b[0][j];
				s[1][idx] = b[1][j];
				idx++;
				j++;
			}
		}
		
		return s;
	}


	static int valueGroup(){
		//size = ;
		ar = new int[2][map.size()];
		int i = 0;
		for(HashSet<Integer> key: map.keySet()) {
			List<Integer> lst = map.get(key);
			//System.out.println(key.size() + ":" + lst.get(1));
			totalCnt += lst.get(1);
			ar[0][i] = key.size();
			ar[1][i] = lst.get(1);
			i++;
			//int i2 = i++;
		}
		//System.out.println(totalCnt);
		return 0;
	}
	
	// 0: need to create new Group
	// 1/-1: join the group
	// 2: merge the group
	static int verfifyGroup(int a,int b){
	
		boolean isA = false;
		boolean isB = false;
		int status = 0;
		for(HashSet<Integer> key: map.keySet()) {
			if(key.contains(a) & !key.contains(b)) isA = true;
			else if(!key.contains(a) & key.contains(b)) isB = true;
			else if(key.contains(a) & key.contains(b)) isA = true;
		}
		if(!isA & !isB) status = 0;
		else if(isA & isB) status = 2;
		else {
			if(isA) status = 1;
			else status = -1;
		}
		
		return status;
	}
	
	static void buildGroup(int a, int b){
		int s = verfifyGroup(a,b);
		if(s==0) newGroup(a,b);
		else if(s==2) mergeGroup(a,b);
		else{
			if(s==1) addGroup(a,b);
			else addGroup(b,a);
		}
	}

	static void mergeGroup(int a,int b){ 
		HashSet<Integer> set = new HashSet<Integer>();
		List<Integer> lst = new ArrayList<Integer>();
		HashSet<Integer> remove1 = null;
		HashSet<Integer> remove2 = null;
		int weight = 0;
		int cnt = 0;
		for(HashSet<Integer> key: map.keySet()) {
			List<Integer> tLst = map.get(key);
			if(key.contains(a)){
				for(Integer k:key) set.add(k);
				int i = 0;
				for(Integer l:tLst){
					if(i==0) weight += l;
					if(i==1) cnt += l;
					i++;
				}
				remove1 = key;
			}
			if(key.contains(b)){
				for(Integer k:key) set.add(k);
				int i = 0;
				for(Integer l:tLst){
					if(i==0) weight += l;
					if(i==1) cnt += l;
					i++;
				}
				remove2 = key;
			}
			
		}
		map.remove(remove1);
		map.remove(remove2);
		lst.add(weight);
		lst.add(++cnt);
		map.put(set, lst);
	}
	
	// insert 'b' in the group having 'a'
	static void addGroup(int a,int b){ 
		HashSet<Integer> set = new HashSet<Integer>();
		List<Integer> lst = new ArrayList<Integer>();
		HashSet<Integer> remove1 = null;
		
		for(HashSet<Integer> key: map.keySet()) {
			//System.out.println(key);
			List<Integer> tLst = map.get(key);
			if(key.contains(a)){
				for(Integer k:key){
					set.add(k);
				}
				set.add(b);
				int i = 0;
				int n = f(set.size());
				for(Integer l:tLst){
					if(i==0) {
						lst.add(l+n);
					}
					if(i==1) lst.add(++l);
					i++;
				}
				remove1 = key;
				
			}
		}
		
		map.remove(remove1);
		map.put(set, lst);
	}

	
	static void newGroup(int a, int b){
		HashSet<Integer> set = new HashSet<Integer>();
		List<Integer> lst = new ArrayList<Integer>();
		
		Integer a1 = Integer.valueOf(a);
		Integer b1 = Integer.valueOf(b);
		set.add(a1);
		set.add(b1);
		int val = 2;
		int cnt = 1;
		lst.add(val);
		lst.add(cnt);
		map.put(set, lst);
	}

	static void printG(){ 
		System.out.println("/========Start=========");
		for(HashSet<Integer> key: map.keySet()) {
			for(Integer k:key){
				System.out.print(k + " ");
			}
			List<Integer> lst = map.get(key);
			for(Integer l:lst){
				System.out.print(" >> " + l + " ");
			}
			System.out.println();
		}
		System.out.println("========End=========/\n");
		
	}
		
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		
		int t = in.nextInt();
		for (int a0 = 0; a0 < t; a0++) {
			int n = in.nextInt();
			int m = in.nextInt();
			for (int a1 = 0; a1 < m; a1++) {
				int x = in.nextInt();
				int y = in.nextInt();
				buildGroup(x,y);
			}
		}

		valueGroup();
		ar = sort(ar);
		calculate();


	}
}

/***

		// TC-1 >> 32
		buildGroup(1,2);
		buildGroup(3,2);
		buildGroup(4,2);
		buildGroup(4,3);

		//TC-2 >> 92
		buildGroup(1,2);
		buildGroup(2,4);
		buildGroup(1,4);
		buildGroup(7,8);
		buildGroup(8,9);
		buildGroup(2,3);
		buildGroup(5,6);
		
		// TC-3 >> 16
		buildGroup(1,2);
		buildGroup(3,4);
		buildGroup(4,5);

		//TC-4 >> 352		
		buildGroup(7,  8);
		buildGroup(1,  2);
		buildGroup(2,  3);
		buildGroup(1,  3);
		buildGroup(4,  5);
		buildGroup(4,  6);
		buildGroup(5,  6);
		buildGroup(10, 11);
		buildGroup(13, 12);
		buildGroup(12, 11);
		buildGroup(13, 11);
		buildGroup(13, 10);
		buildGroup(12, 10);
		buildGroup(12, 9);

		//
		valueGroup();
		ar = sort(ar);
		printA();
		calculate();
**/


```

### version -2

```java
import java.math.BigInteger;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Scanner;


public class I {

	static int N = 0;
	static int total = 0;
	static int[][] a = null;
	static int[] visited = null;
	
	static boolean isValid (int x, int y) {
		if(x >=N || y >=N || a[x][y] ==0 || a[x][y] !=1) return false;
		else return true;
	}
	
	// dfs() get the total number of all members in the group()
	static long dfs2(int x,int y){
        if (!isValid(x, y)) return 0;
        
		long cnt = 0;
		if(a[x][y]==1){
			cnt += 1;
			a[x][y] = 2;
			for(int i=0;i<N;i++){
					cnt += dfs2(y,i);
					cnt += dfs2(x,i);
			}
		}
		
		return cnt;
	}
	
	
	static long dfs(int x,int y, int token){
//		System.out.println(" dfs >> a[" + x + "][" + y + "]=" + a[x][y]);
       if (!isValid(x, y)) return 0;
       if(token==0 && visited[x]==1) return 0;
       long cnt = 0;
		
		if(a[x][y]==1){
			if(visited[x]!=1){
				visited[x]=1;
				cnt++;
			}
			a[x][y] = 2;
			//printAr();
			for(int i=0;i<N;i++){
					cnt += dfs(y,i,0); // y-axis move
					cnt += dfs(x,i,1); // x-axis move
			}
		}
		return cnt;
	}
	
	
	static void printAr(){
		for(int i=0;i<N;i++){
			for(int j=0;j<N;j++){
				System.out.print(a[i][j] + " ");
			}
			System.out.println();
		}
		System.out.println("------------------");
	}
	
	static void printA(List<Integer> lst){
		for(Integer l:lst){
				System.out.print(l + " ");
		}
		System.out.println();
	}
	
	public static void main2(String[] args) {
		BigInteger acc = BigInteger.valueOf(0);
		System.out.println(acc);		
		BigInteger med = BigInteger.valueOf(2);
		System.out.println(med);		
		acc = acc.add(med);
		System.out.println(acc);		
	}
	
	
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		int t = in.nextInt();
		for (int a0 = 0; a0 < t; a0++) {
			N = in.nextInt(); 
			long m = in.nextLong();
			a = new int[N][N];
			visited = new int[N];
			
			for (int a1 = 0; a1 < m; a1++) {
				int x = in.nextInt();
				int y = in.nextInt();
				a[x-1][y-1] = 1;
				a[y-1][x-1] = 1;

			}
			
			//printAr();
			List<Long> lst = new ArrayList<Long>();
			int idx=0;
			for(int i=0;i<N;i++){
				for(int j=i+1;j<N;j++){
					if(a[i][j]==1) {
						long val = dfs(i,j,0);
						lst.add(val);
						//System.out.println("value= " + val + " \n");
						break;
					}
				}
			}
			Collections.sort(lst, Collections.reverseOrder());
			//printA(lst);
			doResult(lst,m);
		
		}
	}
	
	static void doResult(List<Long> lst, long t) {
		BigInteger acc = BigInteger.valueOf(0);
		for (Long ls : lst) {
			long med = sumGroup(ls);
			long full = f(ls);
			long remain = t - (ls - 1);
			
			BigInteger med2 = BigInteger.valueOf(full * remain);
			t -= (ls - 1);
			acc = acc.add(BigInteger.valueOf(med));
			acc = acc.add(med2);
		}
		System.out.println(acc);
	}
	
	static long f(long n){
		return n * (n-1);
	}
	
	static long sumGroup(long n){
		if(n==1) return f(n);
		return sumGroup(n-1) + f(n);
	}



}


```

### version -3

```cpp
#include <iostream>
#include <cstddef>
#include <cmath>
#include <vector>
#include <algorithm>
#include <time.h>


using namespace std;

#define FOR(i,n) for(unsigned int i = 0; i < (n); ++i)
#define FOR1(i,n) for(int i = 1; i < (n); ++i)

void printA(vector<vector<int>> g){
	cout << "printing .." << endl;
	FOR(i, g.size()){
		FOR(j,g[i].size()){
			cout << g[i][j] << " ";
		}
		cout << endl;
	}
	cout << "---------------" << endl;

}

void printG(int **g,int SIZE){
//	cout << "printing .." << endl;
    for(int i=0;i<SIZE;i++){
    	for(int j=0;j<SIZE;j++){
			cout << g[i][j] << " ";
    	}
    	cout << endl;
    }
	cout << "---------------" << endl;

}

bool isValid (int **g, int x, int y,int SIZE) {
	if(x >=SIZE || y >=SIZE || g[x][y] ==0 || g[x][y] !=1) return false;
	else return true;
}

long dfs(int **g,int *v,int SIZE, int x,int y, int token){

	if (!isValid(g, x, y, SIZE))	return 0;
	if (token == 0 && v[x] == 1) return 0;
	long cnt = 0;

	if (g[x][y] == 1) {
		if (v[x] != 1) {
			v[x] = 1;
			cnt++;
		}
		g[x][y] = 2;
		for (int i = 0; i < SIZE; i++) {
			cnt += dfs(g, v, SIZE, y, i, 0); // y-axis move
			cnt += dfs(g, v, SIZE, x, i, 1); // x-axis move
		}
	}
	return cnt;
}

long f(int n){
	return n * (n-1);
}

long sumGroup(int n){
	if(n==1) return f(n);
	return sumGroup(n-1) + f(n);
}


int main(){

	int t;
	cin >> t;

	time_t timer= clock();

	for (int a0 = 0; a0 < t; a0++) {
		int n;
		int m;
		cin >> n >> m;

		int SIZE = n;
		int **graph;
		graph = (int **) malloc(SIZE * sizeof(int*));
		for (int i = 0; i < SIZE; i++) {
			graph[i] = (int *) malloc(SIZE * sizeof(int*));
			for (int j = 0; j < SIZE; j++) graph[i][j] = 0;
		}

		//printG(graph, SIZE);
		for (int a1 = 0; a1 < m; a1++) {
			int x;
			int y;
			cin >> x >> y;
			//cout << "(x,y)" << x << " " << y << endl;
			int x1 = x-1;
			int y1 = y-1;
			graph[x1][y1] = 1;
			graph[y1][x1] = 1;
		}

		//printG(graph, SIZE);
		int *visited = (int *) malloc(SIZE * sizeof(int*));
		//vector<int> groupSize(100);
		vector<int> groupSize;

		for(int i=0;i<SIZE;i++){
			for(int j=i+1;j<SIZE;j++){
				if(graph[i][j]==1) {
					long val = dfs(graph,visited,SIZE,i,j,0);
					//cout << " " << val << endl;
					groupSize.push_back(val);
					//System.out.println("value= " + val + " \n");
					break;
				}
			}
		}

//		cout << " start ..." << endl;
		long long acc = 0L;
		sort(groupSize.begin(),groupSize.end(),greater<int>());
		for(unsigned int i=0;i < groupSize.size();i++){
			int s = groupSize[i];
			long med = sumGroup(s);
			long full = f(s);
			long remain = m - (s - 1);

			long long med2 = full * remain;
			m -= (s - 1);
			acc += med + med2;
		}
		cout << acc;

	}
	cout<< "timed out: " << (double)(clock()-timer)/CLOCKS_PER_SEC<<endl;
	return 0;
}


```
