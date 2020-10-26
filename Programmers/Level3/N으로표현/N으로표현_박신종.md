### 풀이

>  DP인듯 BruteForce인듯.. 하여튼, 다음과같이 풀었다.
>
> 각 N의 개수에 대한 dp 리스트를 만들고(8까지만 구하면 되므로 dp는 8까지) 
>
> F(n) => F(n-1)@F(1), F(n-2)@F(2), ... F(1)@F(n-1)	( 점화식 세우고 )
>
> 와 같은 식을 만들었다. @는 사칙연산 네가지를 포함한다. ( @는 문제의 식에서 괄호쯤생각하면 될것이다. )
>
> 즉, 
>
> F(1) = N => `1가지`
>
> F(2) = F(1)@F(1) + (NN) => 1x1x4 + 1 =>`5가지`
>
> F(3) = F(2)@F(1), F(1)@F(2)
>
> ​		=> 5x1x4 + 1x5x4 + (NNN)
>
> ​		=> 총 40가지 + (NNN) => `41가지`
>
> F(4) = F(3)@F(1), F(2)@F(2), F(1)@F(3)
>
> ​		=> 41x1x4 + 5x5x4 + 1x41x4 + (NNNN)  => `428가지`
>
> dp[1] ~ dp[8] 까지 각 연속된 N을 추가해주고, @연산으로 연산된 값을 dp 리스트에저장해주었다.
>
> 이런식으로 모든 경우의 수를 구해주었다. (ㅎㅎ..비효율적이겠지..?)
>
> 결국, n을 1부터 8까지 답이 나올때 까지 구해주었는데, 8까지는 통과하겠지만,
>
> 만약 최솟값이 8이 넘어갈 경우, 경우의 수는 어마무시 할 것..

```java
package level3;

import java.util.*;

public class Solution_N으로표현 {
	public static void main(String[] args) {
		System.out.println(solution(4, 55));
	}

	static int calc(int o1, int o2, int c) {
		if (c == 0) {
			return o1 * o2;
		} else if (c == 1) {
			return o1 / o2;
		} else if (c == 2) {
			return o1 + o2;
		} else return o1 - o2;
	}

	static public int solution(int N, int number) {
		if (number == N) return 1;
		List<List<Integer>> dp = new ArrayList<>();
		int n = 0;
		for (int i = 0; i <= 8; i++) {
			dp.add(new ArrayList<>());
			if(i==0) continue;
			StringBuilder tmp = new StringBuilder("");
			for(int j=0; j<i; j++) {
				tmp.append(N);
			}
			n = Integer.parseInt(tmp.toString());
			if(n==number) return i;
			dp.get(i).add(n);
		}
		for (int i = 2; i <= 8; i++) {
			for (int j = i - 1; j >= 0; j--) {
				List<Integer> a1 = dp.get(i - j);
				List<Integer> a2 = dp.get(j);
				for (int a = 0; a < a1.size(); a++) {
					for (int b = 0; b < a2.size(); b++) {
						for (int c = 0; c < 5; c++) {
							if(c==1 && a2.get(b)==0) continue;
							n = calc(a1.get(a), a2.get(b), c);
							if (n == number) return i;
							dp.get(i).add(n);
						}
					}
				}
			}
		}

		return -1;
	}
}

```

