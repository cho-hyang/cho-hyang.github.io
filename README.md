
# 분할 정복(Divide-and-Conquer) 알고리즘
> 주어진 문제의 입력을 분할하여 문제를 해결(정복)하는 방식의 알고리즘

## 최근접 점의 쌍 찾기(Closest Pair of Points)
> 2차원 평면상의 n개의 점이 입력으로 주어질 때, 거리가 가장 가까운 한 쌍의 점을 찾는 문제이다.

> 이번 과제에서는 최근접 점 한 쌍의 두 점 사이의 최단거리를 구하였다.

### 최근접 점의 쌍 찾기 과정
	1. 간단한 방법. 모든 점에 대하여 각각의 두 점 사이의 거리를 계산하여 가장 가까운 점의 쌍을 찾는 것.
	=> 거리를 계산하여야 할 쌍 = nC2, 시간복잡도 = O(n²)

	2. 1번의 경우보다 효율적인 방법으로, '분할 정복'을 이용하는 것.
	n개의 점을 1/2로 분할하여 각각의 부분문제에서 최근접 점의 쌍을 찾고, 2개의 부분해 중에서 짧은 거리를 가진 점의 쌍을 찾는다.
	(※주의할 점 : 중간 영역 안에 있는 점들 중에 최근접 점의 쌍이 있는지 확인 후, 최종 최단 거리를 확정한다.)
  	=> 2개의 부분문제로 분할, 부분문제의 크기가 1/2로 감소하는 전형적인 분할 정복 형태의 알고리즘.
  	=> 입력 점들은 x-좌표를 기준으로 미리 정렬 가정.

### 최근접 점 쌍 찾기 알고리즘

	[ClosestPair(S)]

	입력 : x-좌표의 오름차순으로 정렬된 배열 S에는 i개의 점 (단, 각 점은 (x,y)로
	표현된다.)
	출력 : S에 있는 점들 중 최근접 점의 쌍의 거리

	1. if (i ≤ 3) return (2 또는 3개의 점들 사이의 최근접 쌍)
	2. 정렬된 S를 같은 크기의 SL과 SR로 분할한다. |S|가 홀수이면, |SL| = |SR|+1이 되도록 분할
	3. CPL = ClosestPair(SL) // CPL은 SL에서의 최근접 점의 쌍
	4. CPR = ClosestPair(SR) // CPR은 SR에서의 최근접 점의 쌍
	5. d = min{dist(CPL), dist(CPR)}일 때, 중간 영역에 속하는 점들 중에서
	최근접 점의 쌍을 찾아서 이를 CPC라고 하자. 단, dist()는 두 점 사이의 거리
	6. return (CPL, CPC, CPR 중에서 거리가 가장 짧은 쌍)

### 최근접 점의 쌍 찾기 알고리즘 시간복잡도
> ClosestPair 알고리즘의 분할과정은 합병 정렬의 분할 과정과 동일하다.

> 그러나, 합병 정렬에서는 합병 과정에서 O(n) 시간이 걸리고, ClosestPair 알고리즘에서는 O(n㏒n) 시간이 걸린다.

> 최종적으로, ClosestPair 알고리즘 시간복잡도 = O(n㏒²n)

> (참고사항 : 시간복잡도를 더 향상시킬 수 있는 방법 => y-좌표를 전처리 과정에서 미리 정렬하고 저장해두고 실행 할 때마다 참조한다.)

### 최근접 점의 쌍 찾기 알고리즘 응용분야
> 컴퓨터 그래픽스, 컴퓨터 비전(Vision), 지리 정보 시스템(GIS), 분자 모델링, 항공 트래픽 제어, 신규 가맹점 오픈 위치 선정 마케팅 등등




# Closest pair of points 코드


## 분할정복

<pre><code>{
    public int closestPair(ArrayList<Point> p, int start, int end){


        int i = end-start + 1;
    
        if(i <= 3) return bruteForce(p, start, end);
        int num = (start+end)/2;
    
        int CPL = closestPair(p, start, num);
        int CPR = closestPair(p, num+1, end);
        int CPC = (int)((p.get(num+1).x - p.get(num).x)*(p.get(num+1).x - p.get(num).x))
                + (int)((p.get(num+1).y - p.get(num).y)*(p.get(num+1).y - p.get(num).y));
        if(CPC< 0) CPC = -CPC;
        int d = Math.min(CPC, Math.min(CPL, CPR));
    
        return d;
    
    }</code></pre>
 
   
​	분할정복 부분에서는 배열의 중심 부분을 기준으로 절반씩 나눠가면서 분할을 진행했습니다.<br/>
	메소드에서 시작과 끝 인덱스를 받아서 배열의 크기를 계산한 다음,<br/>
	크기가 3이하이면 다른 메소드에서 점들 사이의 거리를 구해 가장 작은 값을 반환했습니다.<br/>
	여기서 주의할 점은 분할의 기준이되는 중간 값들의 거리도 비교해 주어야 한다는 것입니다.<br/>
	중간 인덱스를 기준으로 왼쪽과 오른쪽으로 분할하다보면 중간인덱스들 사이의 거리를 계산하지 않게 됩니다.<br/>
	하지만 중간값들 사이의 거리가 최소가 될 수 있기 때문에 반드시 두점 사이의 거리를 구해 왼쪽에서 반환된 값과 오른쪽에서 반환된 값을 비교해 주어야 합니다.<br/>


## BRUTEFORCE

<pre><code>{

```
public int bruteForce(ArrayList<Point> p, int start, int end) {
    int min = 214700000;

    for(int i = start; i < end; i++){
        for(int j =i + 1; j <= end; j++) {
            int cmp = (int)((p.get(j).x - p.get(i).x)*(p.get(j).x - p.get(i).x))
                    + (int)((p.get(j).y - p.get(i).y)*(p.get(j).y - p.get(i).y));
            if(cmp < 0) cmp = -cmp;
            if(cmp < min) min = cmp;
        }
    }
```

}</code></pre>


​	bruteforce 부분에서는 int의 최대값이 21억 4천7만 정도임을 이용해 최소값의 초기값을 설정하고,<br/>
	그 이후에 배열의 시작과 끝까지 각 점들의 거리를 구해 최소값과 비교해서 마지막에 가장 작은 값을 반환해 줍니다.<br/>
	여기서 배열의 크기는 항상 3이하이기 때문에 시각 복잡도는 항상 O(상수)가 됩니다.<br/>
	따라서 bruteforce의 시간복잡도는 O(1)입니다.<br/>


## main, Point class

<pre><code>
class Point implements Comparable<Point>{
    int x, y;
    Point(int x, int y){
        this.x =x;
        this.y = y;
    }
    public int compareTo(Point p){
        return x - p.x;
    }

}
</code></pre>


<pre><code>
    public static void main(String[] args) {
        long seed = System.currentTimeMillis();
        Random rand = new Random(seed);


        ArrayList<Point> p = new ArrayList<Point>();
    
        for(int i =0; i < 5; i++) {
            int x = rand.nextInt(10) + 1;
            int y = rand.nextInt(10) + 1;
    
            p.add(new Point(x, y));
        }
        Collections.sort(p);
        for(Point s : p) {
            System.out.printf("%d %d", s.x, s.y);
            System.out.println();
        }
        Closest point = new Closest();
    
        System.out.println(Math.sqrt(point.closestPair(p, 0, p.size()-1)));
    }
</code></pre>

​	main함수에서는 x와 y의 값을 입력받기 위해 Point class를 만들어 객체의 형태로 값을 저장할 수 있도록 하였고,<br/>
	그 이후 랜덤으로 x와 y값을 입력받아 ArrayList에 각각 저장하고 x값을 기준으로 정렬하였습니다.<br/>
	x값으로 정렬을 하면 좌표처럼 값을 비교할 수 있게 됩니다.<br/>
	그리고 앞에서 설명한 분할 정복 과정을 수행하기 위해 Closest객체를 만들어 closestPair메소드를 호출하였습니다.<br/>

## 결론, 느낀점

​	closest pair of points문제를 해결하면서 느낀점은 알고리즘에서 시간 복잡도의 중요성이었습니다.<br/>
	사실 closest pair of points 문제는 브르투포스 알고리즘 만으로 문제를 해결할 수 있습니다.<br/>
	하지만 모든 점을 한번씩 다 비교해야하기 때문에 O(n^2)의 시간 복잡도로 문제를 해결하게 됩니다.<br/>
	이 방식대로 라면, n의 크기가 커질수록 알고리즘은 비효율적입니다.<br/>
	하지만 합병정렬의 원리를 기반으로 분할정복하여 문제를 해결해보면, 분할하는 과정에서 logn, 합병하는 과정 n, 그리고 브루투포스 에서 0(1)의 시간 복잡도를 가져,<br/>
	총 시간 복잡도는O(nlogn)+O(1)됩니다.<br/>
	결론적으로 O(nlogn)의 시간복잡도를 가지기 때문에 O(n^2)보다 훨씬 효율적인 알고리즘으로 문제를 해결할 수 있습니다.<br/>

