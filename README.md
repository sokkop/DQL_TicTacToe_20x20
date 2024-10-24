# Игра "Крестики-нолики 5x в ряд" с агентом на основе Deep Q-learning

## Описание среды

- **Размер поля**: 20x20 клеток.
- **Игроки**: Два игрока — один управляется искусственным интеллектом (агент), другой может быть человеком или другим ИИ.
- **Цель игры**: Поставить в ряд пять своих символов (крестики или нолики) по горизонтали, вертикали или диагонали.

## Описание состояния

- **Состояние**: Представляется как текущее расположение всех крестиков и ноликов на поле 20x20.
- **Представление состояния**: Используется двумерный массив размера 20x20, где:
  - Пустая клетка — `0`,
  - Крестик — `1`,
  - Нолик — `-1`.
- **Пространство состояний**: В общей сложности существует 3^400 состояний, что делает использование Q-таблицы невозможным.

## Описание действий

- **Действие**: Выбор клетки для размещения своего символа.
- **Возможные действия**: От `0` до `399` (индекс клетки в матрице 20x20).
- **Ограничения**: Действие допустимо только если клетка свободна (содержит `0`).

## Описание системы наград

- **Победа агента**: +10
- **Поражение агента**: -5
- **Ничья**: -1
- **Неправильное действие** (попытка поставить символ в занятую клетку): -10

## Описание алгоритма обучения

Для аппроксимации Q-функции используется нейронная сеть.

- **Архитектура сети (DNetwork)**:
  - Входной слой принимает состояние размером 1200 (развернутый двумерный массив 20x20 в одномерный вектор).
  - Несколько скрытых слоев с активацией ReLU:
    - `input(1200) -> fc1(1024) -> ReLU -> fc2(512) -> ReLU -> fc3(256) -> ReLU -> output(400)`
    - Выходной слой имеет 400 нейронов для каждого возможного действия.
  
- **Обновление Q-функции**:
  - `Q(s, a) ← Q(s, a) + α[r + γ * max(Q(s', a')) - Q(s, a)]`

  Где:
  - `α` — скорость обучения,
  - `γ` — коэффициент дисконтирования,
  - `r` — полученная награда,
  - `s'` — новое состояние после выполнения действия `a`.

## Процесс симуляции игры

1. **Инициализация**: Создание пустого поля.
2. **Игровой цикл**:
 - Проверка текущего состояния игры (победа, ничья или продолжаем игру).
 - Выбор действия агентом на основе Q-значений, аппроксимированных нейронной сетью.
 - Выполнение действия и обновление состояния игры.
 - Противник выполняет свое действие.
 - Повторение до завершения игры (победа одного из игроков или ничья).
3. **Обновление нейронной сети** после каждого шага с использованием накопленного опыта.

## Ограничения проекта

- **Большое пространство состояний**: Из-за огромного количества возможных состояний использование Q-таблиц невозможно, и для аппроксимации Q-функции используется нейронная сеть.
- **Баланс обучения и вычислительной сложности**: Важно поддерживать баланс между скоростью обучения и качеством игры агента.
- **Эффективное хранение данных**: Необходимо эффективно хранить опыт (в виде буфера) для обновления нейронной сети.

## Используемые технологии

- Python
- TensorFlow / PyTorch для создания и обучения нейронной сети
- Среда для тренировки и симуляции игры
