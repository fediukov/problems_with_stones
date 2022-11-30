# Problems with Candies

Интересные логические задачки про конфеты (решение на C++).

## Задача 1: Честный обмен конфетами

### Условие

У Алисы и Боба есть коробки с конфетами. В каждой коробке есть какое-то количество конфет. Общее количество конфет у Алисы и у Боба разное.

Поскольку Алиса и Боб друзья, они хотели бы обменять по одной коробке конфет, чтобы после обмена у них обоих было одинаковое количество конфет. Количество конфет, которое есть у Алисы (или у Боба), равно сумме количества конфет во всех коробках Алисы (или Боба, соответственно).

Даны два целочисленных массива `AliceCandies` и `BobCandies`, где `AliceCandies[i]` — количество конфет в `i`-й коробке конфет, которые есть у Алисы, а `BobCandies[j]` — количество конфет в `j`-й коробке конфет, которые есть у Боба. Необходимо вернуть целочисленный массив, где `answer[0]` — это количество конфет в коробке, которые Алиса должна обменять, а `answer[1]` — это количество конфет в коробке, которые должен обменять Боб. Если ответов несколько, можно вернуть любой из них. Гарантируется, что существует хотя бы один ответ.

### Ограничения

  + `1 <= AliceCandies.length, BobCandies.length <= 104`
  + `1 <= AliceCandies[i], BobCandies[j] <= 105`
  + У Алисы и Боба разное количество конфет.
  + Гарантируется, что будет как минимум один правильный ответ.

### Объяснение

Для начала необходимо определить количество конфет, которые есть у Алисы (`n_a`) и у Боба (`n_b`). Тогда цель после обмена коробками будет равна полусумме общего количества конфет (`(n_a + n_b)/2 = n`). Далее необходимо найти такую пару коробок для обмена (`box_a` и `box_b`), при котором `n == n_a - box_a + box_b` или `n == n_b - box_b + box_a`.

Таким образом, для решения задачи заведём два ассоциативных контейнера, первый из которых будет хранить количество конфет в каждой коробке Алисы, а второй - в каждой коробке Боба (я решил применить `unordered_map`. Далее записываем данные в эти контейнеры, пробегаясь по коробкам Алисы и Боба. Заодно считаем количество конфет у Алисы и у Боба. Далее пробегаемся по коробкам Алисы, и если среди набора коробок Боба находится та, которая содержит количество конфет, равное полусумме общего количества минус количество конфет у Алисы плюс количество конфет в конкретной коробке Алисы (`box_b == n - n_a + box_a`), то ответ найден. Ответом будет вектор `{box_a, box_b}`, где `box_a` - количество конфет (коробка), которое должна обменять Алиса, и `box_b` - количество конфет (коробка), которое должен обменять Боб.

### Решение

```
    class Candies {
    public:
        std::vector<int> Candies::FairCandySwap(std::vector<int>& AliceCandies, std::vector<int>& BobCandies)
        {
            std::unordered_map<int, int> a;
            std::unordered_map<int, int> b;
            int count_a = 0, count_b = 0;
            for (int i = 0; i < AliceCandies.size(); ++i)
            {
                ++a[AliceCandies[i]];
                count_a += AliceCandies[i];
            }
            for (int i = 0; i < BobCandies.size(); ++i)
            {
                ++b[BobCandies[i]];
                count_b += BobCandies[i];
            }
            int aim_count = (count_a + count_b) / 2;
            for (const auto [count_in_box, box_count] : a)
            {
                auto it = b.find(aim_count - count_a + count_in_box);
                if (it != b.end())
                {
                    return { count_in_box, it->first };
                }
            }
            return {};
        }
    };
    
    int main()
    {
        Candies c;
	
        std::vector<int> alice_candies = { 1,1 };
        std::vector<int> bob_candies = { 2,2 };
        std::vector<int> result = { 1,2 };
        
        assert(c.FairCandySwap(alice_candies, bob_candies) == result);
    }
```
