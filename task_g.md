# 🚀 Разбор задачи: G. A + B = C

### 💡 Идея решения
Вместо сложного перебора самих чисел A и B с последующим разбором строк, гораздо эффективнее применить подход **«от букв к цифрам»**. Мы находим все уникальные буквы в загадке и распределяем между ними цифры от 0 до 9 с помощью встроенной функции `next_permutation`.

Основные правила и логика фильтрации:
* **Уникальность (Биекция)**: Поскольку `next_permutation` перемешивает массив изначально уникальных цифр, разным буквам гарантированно никогда не достанутся одинаковые цифры.
* **Защита от дубликатов**: Если уникальных букв меньше 10, неиспользованные цифры в хвосте массива продолжат переставляться, из-за чего одно и то же математическое решение встретится несколько раз. Для точного подсчета мы сохраняем финальные тройки чисел в `set<vector<int>>`, который автоматически отсекает повторы.
* **Ограничение на нули**: По условию числа не равны нулю и не имеют ведущих нулей. Это значит, что буквы, стоящие на первых позициях в строках `sa`, `sb` и `sc`, ни при каких условиях не могут получить цифру `0` (даже если длина строки равна 1).

---

### 💻 Код решения (C++)

```cpp
#include <bits/stdc++.h> 
/* 
@author: The Pinnacle 
*/
 
using namespace std; 
using ll = long long; 
using ld = long double; 

void solve() { 
    string sa, sb, sc; 
    cin >> sa >> sb >> sc; 
    set<vector<int>> cnt;
    vector<vector<int>> ans(1, vector<int>(3)); 
    
    // 1. Собираем все уникальные буквы загадки
    set<char> unique_chars;
    for(char c : sa) unique_chars.insert(c);
    for(char c : sb) unique_chars.insert(c);
    for(char c : sc) unique_chars.insert(c);

    // Если уникальных букв больше 10, решения гарантированно нет
    if(unique_chars.size() > 10) {
        cout << 0 << '\n';
        return;
    }

    // Переносим буквы в строку для удобного обращения по индексу
    string letters = "";
    for(char c : unique_chars) {
        letters += c;
    }

    // Задаем базовый массив цифр для генерации перестановок
    vector<int> digits = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    vector<int> char_to_digit(256, 0);

    // 2. Перебираем все возможные комбинации цифр для букв
    do {
        for(size_t i = 0; i < letters.size(); i++) {
            char_to_digit[letters[i]] = digits[i];
        }

        // Числа не могут начинаться с нуля и не могут быть равны нулю
        if(char_to_digit[sa[0]] == 0) continue;
        if(char_to_digit[sb[0]] == 0) continue;
        if(char_to_digit[sc[0]] == 0) continue;

        // 3. Собираем числа numa, numb и numc из строк по нашей карте разрядов
        int numa = 0;
        for(char c : sa) numa = numa * 10 + char_to_digit[c];

        int numb = 0;
        for(char c : sb) numb = numb * 10 + char_to_digit[c];

        int numc = 0;
        for(char c : sc) numc = numc * 10 + char_to_digit[c];

        // 4. Проверяем математическое равенство
        if(numa + numb == numc) {
            cnt.insert({numa, numb, numc});
            if(cnt.size()) { 
                ans[0][0] = numa;
                ans[0][1] = numb;
                ans[0][2] = numc;
            }
        }

    } while(next_permutation(digits.begin(), digits.end()));

    // 5. Выводим количество уникальных решений и любой подходящий вариант
    cout << cnt.size() << '\n'; 
    if(cnt.size() > 0) {
        cout << ans[0][0] << '\n' << ans[0][1] << '\n' << ans[0][2]; 
    }
} 

int main() { 
    ios::sync_with_stdio(0); 
    cin.tie(0); 
    ll t = 1; 
    //cin >> t; 
    while(t --> 0) { 
        solve(); 
    } 
    return 0; 
}
```
