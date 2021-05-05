# Sort

다음은 버블정렬, 선택정렬, 삽입정렬, 쉘정렬을 구현하고, 각 배열별로 정렬된 상태(랜덤, 내림차순 정렬, 대부분 정렬)에 따른 수행시간의 차이를 알아본다.



### 전체 코드

```java
import java.util.*;

interface SortMethods{ //정렬 방법
    void bubbleSort(int[] x,int n);
    void selectionSort(int[] x,int n);
    void insertionSort(int[] x,int n);
    void shellSort(int[] x,int n);
}
interface SortDegree{ //배열의 정렬 정도
    void random(int[] x,int n); //랜덤
    void descendingSort(int[] x,int n); //내림차순 정렬
    void almostSort(int[] x,int n); //거의 정렬이 되어있는 상태
}
class Sort implements SortMethods,SortDegree {
    static void swap(int[] x, int a, int b) {
        int t = x[a];
        x[a] = x[b];
        x[b] = t;
    }

    @Override
    public void bubbleSort(int[] x, int n) {
        for (int i = 1; i < n; i++)
            for (int j = 0; j < n - i; j++)
                if (x[j] > x[j + 1])
                    swap(x, j, j + 1);
    }

    @Override
    public void selectionSort(int[] x, int n) {
        int min;
        for (int i = 0; i < n - 1; i++) {
            min = x[i];
            int k=i;
            for (int j = i + 1; j < n; j++)
                if (x[j] < min)
                  k=j;
            swap(x,i,k);
        }
    }

    @Override
    public void insertionSort(int[] x, int n) {
        int k;
        int j;
        for(int i=1;i<n;i++){
            k=x[i];
            j=i-1;
            while(j>=0&&x[j]>k){
                x[j+1]=x[j];
                j=j-1;
            }
            x[j+1]=k;
        }
    }

    @Override
    public void shellSort(int[] x, int n) {
        int k,j;
        for(int h=n/2;h>0;h/=2) {
            for (int i=h;i<n;i++){
                k=x[i];
                j=i;
                while(j>=h&&x[j-h]>k){
                    x[j]=x[j-h];
                    j=j-h;
                }
                x[j]=k;
            }
        }
    }
    public void time(int[] x,int n,String s1,String s2){
        double beforeTime = System.currentTimeMillis();
        bubbleSort(x,n);
        double afterTime = System.currentTimeMillis();
        double secDiffTime = (afterTime - beforeTime) / 1000;
        System.out.println(s1+"정렬에서의 "+s2+"시간:"+secDiffTime);
    }

    @Override
    public void random(int[] x,int n) {
        Random r=new Random();
        for(int i=0;i<x.length;i++)
            x[i]=r.nextInt(100)+1;
        time(x,n,"랜덤","bubbleSort");
        time(x,n,"랜덤","selectionSort");
        time(x,n,"랜덤","insertionSort");
        time(x,n,"랜덤","shellSort");

    }

    @Override
    public void descendingSort(int[] x,int n) {
        Arrays.sort(x);
        int[] k=x.clone();
        for(int i=0;i<x.length;i++)
            x[i]=k[n-i-1];
        time(x,n,"내림차순","bubbleSort");
        time(x,n,"내림차순","selectionSort");
        time(x,n,"내림차순","insertionSort");
        time(x,n,"내림차순","shellSort");
    }

    @Override
    public void almostSort(int[] x,int n) {
        Arrays.sort(x);
        for(int i=0;i<x.length/50;i++)
            swap(x,i+20,x.length-i-1);
        time(x,n,"대부분","bubbleSort");
        time(x,n,"대부분","selectionSort");
        time(x,n,"대부분","insertionSort");
        time(x,n,"대부분","shellSort");
    }
}
public class SortAlgorithm{
    public static void main(String[] args) {
        Scanner sc=new Scanner(System.in);

        Sort s=new Sort();
        int n=sc.nextInt();
        int[] x=new int[n];
        s.random(x,n);
        s.descendingSort(x,n);
        s.almostSort(x,n);
    }
}
```



### 코드 설명

```java
interface SortMethods{ //정렬 방법
    void bubbleSort(int[] x,int n);
    void selectionSort(int[] x,int n);
    void insertionSort(int[] x,int n);
    void shellSort(int[] x,int n);
}
interface SortDegree{ //배열의 정렬 정도
    void random(int[] x,int n); //랜덤
    void descendingSort(int[] x,int n); //내림차순 정렬
    void almostSort(int[] x,int n); //거의 정렬이 되어있는 상태
}
```

위와 같이 interface 두개를 선언해서 정렬 방법 4개와 정렬된 정도 3개를 각각 나타낸다.



```java
@Override
    public void bubbleSort(int[] x, int n) {
        for (int i = 1; i < n; i++)
            for (int j = 0; j < n - i; j++)
                if (x[j] > x[j + 1])
                    swap(x, j, j + 1);
    }

    @Override
    public void selectionSort(int[] x, int n) {
        int min;
        for (int i = 0; i < n - 1; i++) {
            min = x[i];
            int k=i;
            for (int j = i + 1; j < n; j++)
                if (x[j] < min)
                  k=j;
            swap(x,i,k);
        }
    }

    @Override
    public void insertionSort(int[] x, int n) {
        int k;
        int j;
        for(int i=1;i<n;i++){
            k=x[i];
            j=i-1;
            while(j>=0&&x[j]>k){
                x[j+1]=x[j];
                j=j-1;
            }
            x[j+1]=k;
        }
    }

    @Override
    public void shellSort(int[] x, int n) {
        int k,j;
        for(int h=n/2;h>0;h/=2) {
            for (int i=h;i<n;i++){
                k=x[i];
                j=i;
                while(j>=h&&x[j-h]>k){
                    x[j]=x[j-h];
                    j=j-h;
                }
                x[j]=k;
            }
        }
    }
```

위에 인터페이스에 선언한 메소드를 구현한다.



```java
@Override
    public void random(int[] x,int n) {
        Random r=new Random();
        for(int i=0;i<x.length;i++)
            x[i]=r.nextInt(100)+1;
        time(x,n,"랜덤","bubbleSort");
        time(x,n,"랜덤","selectionSort");
        time(x,n,"랜덤","insertionSort");
        time(x,n,"랜덤","shellSort");

    }

    @Override
    public void descendingSort(int[] x,int n) {
        Arrays.sort(x);
        int[] k=x.clone();
        for(int i=0;i<x.length;i++)
            x[i]=k[n-i-1];
        time(x,n,"내림차순","bubbleSort");
        time(x,n,"내림차순","selectionSort");
        time(x,n,"내림차순","insertionSort");
        time(x,n,"내림차순","shellSort");
    }

    @Override
    public void almostSort(int[] x,int n) {
        Arrays.sort(x);
        for(int i=0;i<x.length/50;i++)
            swap(x,i+20,x.length-i-1);
        time(x,n,"대부분","bubbleSort");
        time(x,n,"대부분","selectionSort");
        time(x,n,"대부분","insertionSort");
        time(x,n,"대부분","shellSort");
    }
}
```

역시 위에 인터페이스에 선언한 메소드를 구현한다.

random은 1~100사이의 임의의 수로 배열을 나타내는 메소드이고,

descendingSort는 내림차순으로 배열을 나타내는 메소드,

almostSort는  오름차순으로  정렬된 베열에서 조금만 바꿔주는 메소드이다.

```java
public void time(int[] x,int n,String s1,String s2){
        double beforeTime = System.currentTimeMillis();
        bubbleSort(x,n);
        double afterTime = System.currentTimeMillis();
        double secDiffTime = (afterTime - beforeTime) / 1000;
        System.out.println(s1+"정렬에서의 "+s2+"시간:"+secDiffTime);
    }
```

time메소드는 얼마나 정렬된 배열에서 어떤 정렬을 사용하는지에 따라 시간이 얼마나 걸리는지를

나타내주는 메소드이다. 



### 성능 평가

![bubble](https://user-images.githubusercontent.com/80511046/117133597-6c3a3b80-addf-11eb-8482-c127ddc1ed6d.png)


![img](data:image/png;base64,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)

![img](data:image/png;base64,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)



위와 같이 10~20000개의 배열로 성능 구현을 해보았는데, 네 정렬 모두 내림차순 배열과, 대부분 정렬된 상태에서의 배열에서는 비슷한 성능이 구현되었고, 버블정렬과 삽입정렬의 경우 랜덤 배열에서는 다른 배열에서보다 성능이 안 좋게 나타났다. 그 중에서도 버블정렬이 가장 성능이 안 좋게 나왔다.
