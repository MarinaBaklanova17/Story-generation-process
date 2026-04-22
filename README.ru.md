# Story-generation-process

Пайплайн генерации: 19 больших языковых моделей пересказывают 16 классических сказок. Каждый ноутбук соответствует одной исходной сказке из Project Gutenberg.

Тексты, полученные здесь, передаются в [LLM_readability_score_analysis](https://github.com/MarinaBaklanova17/LLM_readability_score_analysis) для статистической оценки. Часть магистерской исследовательской работы в Whitireia New Zealand (2025).

## Назначение

Сформировать контролируемый корпус LLM-сгенерированных нарративов на фиксированных человеческих прототипах, чтобы оценка читаемости и точности могла проводиться при одинаковых промптах и одинаковых исходных текстах на всех моделях.

## Устройство

Каждый ноутбук:

1. Загружает одну классическую сказку как вход
2. Отправляет один и тот же промпт генерации 19 LLM от крупнейших провайдеров (Anthropic, OpenAI, Google, Meta, Mistral и другие)
3. Собирает выходы моделей в табличном виде
4. Считает `textstat`-метрики читаемости по каждому выходу

Межмодельная диспетчеризация изначально шла через сервис Unify.ai; см. историческую заметку ниже.

## Исходные сказки

16 ноутбуков, по одному на сказку:

| Ноутбук | Исходная сказка |
|---|---|
| `HOW_JACK_WENT_TO_SEEK_HIS_FORTUNE.ipynb` | How Jack Went to Seek His Fortune |
| `Henny_Penny.ipynb` | Henny Penny |
| `JACK_HANNAFORD.ipynb` | Jack Hannaford |
| `JOHNNY_CAKE.ipynb` | Johnny Cake |
| `Lazy_Jack.ipynb` | Lazy Jack |
| `MASTER_OF_ALL_MASTERS_readability_score.ipynb` | Master of All Masters |
| `MR_MIACCA.ipynb` | Mr Miacca |
| `MR_VINEGAR.ipynb` | Mr Vinegar |
| `TEENY_TINY.ipynb` | Teeny Tiny |
| `THE_CAT_AND_THE_MOUSE_fairy_tale.ipynb` | The Cat and the Mouse |
| `THE_MAGPIE'S_NEST.ipynb` | The Magpie's Nest |
| `THE_MASTER_AND_HIS_PUPIL.ipynb` | The Master and His Pupil |
| `THE_OLD_WOMAN_AND_HER_PIG.ipynb` | The Old Woman and Her Pig |
| `THE_ROSE_TREE.ipynb` | The Rose Tree |
| `THE_STORY_OF_THE_THREE_LITTLE_PIGS.ipynb` | The Story of the Three Little Pigs |
| `THE_THREE_SILLIES.ipynb` | The Three Sillies |

## Историческая заметка: Unify.ai

Изначальный пайплайн использовал **Unify.ai** — сервис-агрегатор, предоставлявший единый SDK для моделей от Anthropic, OpenAI, Google, Meta, Mistral, AWS Bedrock, Fireworks, Together, Anyscale, DeepInfra и других через единый интерфейс `from unify import Unify`.

**Unify.ai был закрыт в 2024 году.** Переменная `UNIFY_KEY` сохранена в ноутбуках как исторический артефакт, но больше не работает. Чтобы перезапустить пайплайн сегодня, шаг генерации необходимо переписать под нативные SDK каждого провайдера (`openai`, `anthropic`, `google-generativeai` и т. д.).

## Как воспроизвести (требуется портирование)

```bash
pip install textstat pandas openpyxl jupyter
# плюс нативные SDK провайдеров, к которым есть доступ
jupyter notebook
```

Замените вызовы `unify = Unify(model_name)` + `unify.generate(prompt, temperature)` в третьей ячейке каждого ноутбука эквивалентами из выбранного провайдерского SDK.

## Ограничения

- Закрытие Unify.ai означает, что строгая воспроизводимость исходных результатов требует доступа к тем же снапшотам моделей, что не гарантировано после обновления версий у провайдеров
- Одна генерация на пару (модель × сказка) — без усреднения по температуре или зернам генератора случайных чисел
- Промпт фиксирован; чувствительность читаемости к формулировке промпта в данной работе не изучается

## Связанные репозитории

- [LLM_readability_score_analysis](https://github.com/MarinaBaklanova17/LLM_readability_score_analysis) — статистический анализ текстов, сгенерированных здесь
- Магистерская работа: *The comparative analysis of human-written and large language model generated children's tales: Readability and accuracy assessments of LLM-generated texts*, Whitireia New Zealand (2025)

## Лицензия

Лицензия не указана; для коммерческого использования, пожалуйста, откройте issue.

---

*Язык:* [English](README.md) · **Русский**
