# 하노이 타워

```java
public class HanoiTower {

    int count = 0;  // 이동 횟수

    /**
     * @param first : 원반들이 있던 탑 (출발지)
     * @param center : 중간 위치 탑 (경유지)
     * @param last : 원반을 옮길 목적지 탑 (목적지)
     * @param n  : 원반 개수
     */
    public void move(String first, String center, String last, int n){

        if(n==1){
            ++count;
            System.out.println(n + "번 판 : " + first + " -> " + last);
        }else{
            move(first, last, center, n-1);
            ++count;

            System.out.println(n + "번 판 : " + first + " -> " + last);
            move(center, first, last, n-1);
        }
    }

    public void getCount(){
        System.out.println("총 이동 횟수 : " + this.count);
    }
}
```

```java
public static void main(String[] args){

    HanoiTower ht = new HanoiTower();

    ht.move("1", "2", "3", 2);
    ht.getCount();
}
```

출력 결과
```java
1번 판 : 1 -> 2
2번 판 : 1 -> 3
1번 판 : 2 -> 3
총 이동 횟수 : 3
```