# Специфікація: модель даних платформи підготовки до НМТ

## Сутності, атрибути, ключі

- **User**
  - `user_id` (`uuid`, PK) — ідентифікатор користувача
  - `email` (`string`) — адреса пошти
  - `name` (`string`) — ім'я
  - `registred_at` (`datetime`) — дата реєстрації

- **Subject**
  - `subject_id` (`uuid`, PK) — ідентифікатор предмета
  - `title` (`string`) — назва предмета
  - `kind` (`string`) — `mandatory` або `elective`

- **Topic**
  - `topic_id` (`uuid`, PK) — ідентифікатор теми
  - `subject_id` (`uuid`, FK) — предмет, до якого належить тема
  - `title` (`string`) — назва теми

- **Question**
  - `question_id` (`uuid`, PK) — ідентифікатор питання
  - `topic_id` (`uuid`, FK) — тема питання
  - `question_type` (`string`) — `single` або `multiple`
  - `body` (`string`) — текст питання
  - `image_url` (`string`) — посилання на зображення, необов'язкове
  - `explanation` (`string`) — пояснення до питання

- **Answer_option**
  - `answer_option_id` (`uuid`, PK) — ідентифікатор варіанта
  - `question_id` (`uuid`, FK) — питання, якому належить варіант
  - `body` (`string`) — текст варіанта
  - `isCorrect` (`boolean`) — чи є варіант правильним

- **Attempt**
  - `attempt_id` (`uuid`, PK) — ідентифікатор спроби
  - `question_id` (`uuid`, FK) — на яке питання
  - `user_id` (`uuid`, FK) — хто відповідав
  - `answered_at` (`datetime`) — час відповіді

- **Mistake_review**
  - `mistake_id` (`uuid`, PK) — ідентифікатор розбору
  - `attempt_id` (`uuid`, FK) — неправильна спроба, яку розбирають
  - `note` (`string`) — нотатка користувача
  - `status` (`string`) — `unreviewed` або `reviewed`

## Зв'язки

1. `User` — `Subject`, 1:N

Користувач обирає 0...N предметів, предмет належить 1 користувачу.

2. `Subject` — `Topic`, 1:N

Предмет містить 0...N тем, тема належить рівно одному предмету.

3. `Topic` — `Question`, 1:N

Тема групує 0...N питань, питання належить рівно одній темі.

4. `Question` — `Answer_option`, 1:N

Питання має 1...N варіантів, варіант належить рівно одному питанню.

5. `User` — `Attempt`, 1:N

Користувач робить 0...N спроб, спроба належить рівно одному користувачу.

6. `Attempt` — `Answer_option`, M:N

Спроба обирає 1...N варіантів, варіант може бути обраний у 0...N спроб.