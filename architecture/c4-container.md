
```markdown
# C4 Level 1 — Containers

Показывает, из каких исполняемых единиц состоит Subly и как они взаимодействуют.

```mermaid
C4Container
    title Container diagram — Subly

    Person(user, "Пользователь")
    Person(finance, "Финансовый отдел")

    System_Ext(paymentGateway, "Платёжный шлюз")

    Container_Boundary(subly, "Subly") {
        Container(spa, "Web SPA", "React (или аналогичный SPA-фреймворк) `[СВЕРИТЬ со стеком]`", "Интерфейс личного кабинета: активные подписки, смена тарифа, отключение, история платежей")
        Container(api, "Backend API", "Node.js / Express `[СВЕРИТЬ со стеком]`", "Бизнес-логика подписок, расчёт стоимости, интеграция с платёжным шлюзом, авторизация")
        ContainerDb(db, "База данных", "PostgreSQL `[СВЕРИТЬ со стеком]`", "Пользователи, подписки, тарифы, платежи, история операций")
        ContainerDb(cache, "Кэш / сессии", "Redis `[УТОЧНИТЬ: используется ли]`", "Сессии, кэш справочников тарифов")
    }

    Rel(user, spa, "Использует", "HTTPS")
    Rel(finance, spa, "Просматривает платежи", "HTTPS")
    Rel(spa, api, "Вызывает", "JSON / REST")
    Rel(api, db, "Читает/пишет", "SQL")
    Rel(api, cache, "Читает/пишет", "Redis protocol `[УТОЧНИТЬ]`")
    Rel(api, paymentGateway, "Инициирует платёж и принимает callback", "HTTPS/JSON")

    UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")
