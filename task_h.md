# 🚀 Разбор задачи: Классический двоичный поиск по ответу

### 💡 Идея решения
Классический двоичный поиск по ответу, ничего более)

Мы ищем минимальное подходящее значение `x`. Для этого:
1. Задаем границы поиска: `l = 0` и `r = 1e13` (с запасом под ограничения задачи).
2. В функции `check` проверяем, укладываемся ли мы в лимит времени `T` при текущем значении `x`. Количество операций для каждого элемента безопасно округляем вверх: `(a[i] + x - 1) / x`.
3. Сдвигаем границы в зависимости от результата проверки, пока не сойдемся к точному ответу.

---

### 💻 Код решения (C++)

```cpp
#include <bits/stdc++.h>
/*
 author: The Pinnacle
*/
 
using namespace std;
using ll = long long;
using ld = long double; 

// Функция проверки выполнимости условия за время T
bool check(vector<ll>& a, ll x, ll T) {
    ll ans = 0;
    for (ll i = 0; i < a.size(); i++) {
        ans += max(1LL, (a[i] + x - 1) / x);
    }
    return ans <= T;
}

// Двоичный поиск по ответу
ll binSearch(vector<ll>& a, ll T) {
    ll l = 0;
    ll r = 1e13;
    
    while (l + 1 < r) {
        ll m = 1LL * l + (r - l) / 2;
        if (check(a, m, T)) {
            r = m;
        } else {
            l = m;
        }
    }
    
    return r;
}  

void solve() {
    ll n, T; cin >> n >> T;
    vector<ll> a(n);
    for (ll i = 0; i < n; i++) {
        cin >> a[i];
    }
    
    ll ans = binSearch(a, T);
    cout << ans;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    solve();
    
    return 0;
}
```
