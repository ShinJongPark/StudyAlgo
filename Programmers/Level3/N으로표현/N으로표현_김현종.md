```java
package Level3;

import java.util.HashSet;

public class N으로포현 {
	public static void main(String[] args) {
		int N = 1;
		int number = 11; // 정답 : 4
		System.out.println(solution(N, number));
	}

	static int solution(int N, int number) {
		HashSet<Integer>[] list = new HashSet[9];
		for (int i = 1; i < 9; i++) {
			list[i] = new HashSet<>();
		}

		if (N == number) {
			return 1;
		}

		list[1].add(N);

		StringBuilder sb = new StringBuilder();
		sb.append(N);

		int cnt = 2;

		while (cnt <= 8) {
			for (int i = 1; i < cnt; i++) {
				HashSet<Integer> s1 = list[i];
				HashSet<Integer> s2 = list[cnt - i];

				for (int num1 : s1) {
					for (int num2 : s2) {
						int plus = num1 + num2;
						int minus = num1 - num2;

						if (plus == number || minus == number) {
							return cnt;
						}

						list[cnt].add(minus);
						list[cnt].add(plus);
						if (num1 == 0 || num2 == 0) {
							continue;
						}

						int mul = num1 * num2;
						int div = num1 / num2;

						if (mul == number || div == number) {
							return cnt;
						}

						list[cnt].add(mul);
						list[cnt].add(div);
					}
				}
			}

			sb.append(N);
			int num = Integer.parseInt(sb.toString());
			list[cnt].add(num);
			if (num == number) {
				return cnt;
			}

			cnt++;
		}

		return -1;
	}
}
```



N의 숫자로 BFS로 풀었다. 중복을 없애기 위해서 Set을 사용했다. 가로로 인해서 먼저 푸는 유무를 구현하기 위해서 모든 경우의 수를 모두 사칙연산을 해줬다.