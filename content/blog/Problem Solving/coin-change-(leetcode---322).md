---
title: Coin Change (LeetCode - 322)
date: 2022-04-25 12:04:11
category: Problem Solving
thumbnail: { thumbnailSrc }
draft: false
---

# [LeetCode] **Coin Change (322)**

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1

```

**Example 2:**

```
Input: coins = [2], amount = 3
Output: -1

```

**Example 3:**

```
Input: coins = [1], amount = 0
Output: 0

```

**Constraints:**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 2^31 - 1`
- `0 <= amount <= 104`

### 문제 접근

- 코인 배열(coins)과 목표 금액(amount)이 주어졌을 때, 코인을 사용해 목표 금액을 만드려고 한다. 이때 가장 최소한의 코인을 사용해 목표 금액을 만드려면 몇 개를 사용해야 할지를 구하는 문제다.
- 생각해 볼 수 있는 풀이법은 조합과 DP 두 가지로 나눌 수 있다.

### Solution 1 : DFS

```jsx
const coinChange = (coins, amount) => {
  let minCoin = -1

  const dfs = (i, sum, used) => {
    if (sum > amount) return

    if (sum === amount) {
      if (minCoin < 0) minCoin = used.length
      else minCoin = Math.min(minCoin, used.length)
      return
    }

    for (let j = i; j < coins.length; j += 1) {
      used.push(coins[j])
      dfs(j, sum + coins[j], used)
      used.pop()
    }
  }

  dfs(0, 0, [])

  return minCoin
}
```

- 최소 코인 사용수 갱신하기 위한 변수(minCoin)를 세우고 재귀 함수로 반복할 함수를 만든다. 함수의 인자는 coin의 index(i)와 코인의 합(sum), 사용한 코인을 담는 배열(used)을 전달한다.
  - 만약 코인의 합이 목표 금액보다 크면 재귀를 종료한다.(더 볼 필요가 없기 때문)
  - 만약 코인의 합이 목표 금액과 같다면
    - 최소 코인 사용 수는 초기에 -1이므로 처음 갱신되는 거면 used의 길이로 갱신한다.
    - 최초 갱신이 아니라면 둘 중에 낮은 값을 비교해 갱신한다.
  - 사용한 코인 배열에 코인을 추가하고 sum에 코인을 더해 재귀적으로 반복한다.
- 하지만 입력값이 2의 31승-1까지 들어오기 때문에 이 방식으로는 시간 초과가 발생한다.

### Solution 2 : DP

```jsx
const coinChange = (coins, amount) => {
  const dp = Array.from({ length: amount + 1 }, () => Infinity)

  dp[0] = 0

  coins.forEach(coin => {
    for (let i = coin; i <= amount; i += 1) {
      dp[i] = Math.min(dp[i], dp[i - coin] + 1)
    }
  })

  return dp[amount] === Infinity ? -1 : dp[amount]
}
```

- 먼저 dp를 하기 위한 배열을 선언한다. 길이는 목표 금액 + 1 만큼 설정한다. 초기값 = 0 부터 amount에 도달할 때까지 코인을 사용한 갯수를 갱신하며 담기 위함이다.
- 코인 배열을 반복하면서, 금액별로 필요한 최소 코인을 갱신한다.
- 만약 목표 금액을 주어진 코인들로 완성할 수 없으면 -1 을 반환하고 그렇지 않으면 dp 배열의 값을 반환한다.
