
#

###

https://www.hackerrank.com/contests/w26/challenges/taste-of-win

```cpp

```


```

	long s = pow(2,3)-1; // # of stones

	for(int i=1;i<s-1;i++){
		for(int j=i+1;j<s;j++){
			for(int k=j+1;k<=s;k++){
				int xr = i ^ j ^ k;
				cout << "(i,j,k)=(" << i << "," << j << "," << k << ")=" << xr << endl;
			}
		}
	}
  
(i,j,k)=(1,2,3)=0
(i,j,k)=(1,2,4)=7
(i,j,k)=(1,2,5)=6
(i,j,k)=(1,2,6)=5
(i,j,k)=(1,2,7)=4
(i,j,k)=(1,3,4)=6
(i,j,k)=(1,3,5)=7
(i,j,k)=(1,3,6)=4
(i,j,k)=(1,3,7)=5
(i,j,k)=(1,4,5)=0
(i,j,k)=(1,4,6)=3
(i,j,k)=(1,4,7)=2
(i,j,k)=(1,5,6)=2
(i,j,k)=(1,5,7)=3
(i,j,k)=(1,6,7)=0
(i,j,k)=(2,3,4)=5
(i,j,k)=(2,3,5)=4
(i,j,k)=(2,3,6)=7
(i,j,k)=(2,3,7)=6
(i,j,k)=(2,4,5)=3
(i,j,k)=(2,4,6)=0
(i,j,k)=(2,4,7)=1
(i,j,k)=(2,5,6)=1
(i,j,k)=(2,5,7)=0
(i,j,k)=(2,6,7)=3
(i,j,k)=(3,4,5)=2
(i,j,k)=(3,4,6)=1
(i,j,k)=(3,4,7)=0
(i,j,k)=(3,5,6)=0
(i,j,k)=(3,5,7)=1
(i,j,k)=(3,6,7)=2
(i,j,k)=(4,5,6)=7
(i,j,k)=(4,5,7)=6
(i,j,k)=(4,6,7)=5
(i,j,k)=(5,6,7)=4


```

Data
```
4 4 :  30240
4 5:  729120
3 5:   26040
3 6:  234360
1 10000000: 255718401
10 1000: 458463613
10 2000: 716246165
20 10000000: 700887404
20 1000000:  918612201
8 12 267680643
5 7398720: 321912827
2587330 1262385: 452042758
215 92: 526321136
46 7995884: 687744657
2457598 3672615: 38027518
3 2725332: 126297699
10 18: 328988621
8519675 2375451: 812636106
193 83: 251357712
19 9707813: 349565257
4298561 125: 138169107
3361711 725185:  802148441
20 27 :59271430
18 8352697: 769257327
7723618 6612730: 233386869
8519675 2375451; 812636106
19 9707813: 349565257
193 83: 251357712
10000000 10000000: 253223948

```
