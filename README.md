# Robust Subject-Independent EEG Classification

Исследовательский репозиторий магистерской работы:

**«Разработка прототипа модели устойчивой классификации сигналов неинвазивной электроэнцефалографии к шумам и артефактам»**

**Автор:** Дворяшина Ирина Александровна  
**Магистратура:** Науки о данных  
**Научный руководитель:** Егоров Константин Сергеевич

## Цель исследования

Разработка и экспериментальная проверка прототипа модели классификации моторного воображения по неинвазивной ЭЭГ, устойчивой к межсубъектной вариабельности, шумам и артефактам.

Основной сценарий — **subject-independent classification**: модель должна работать на новом субъекте без предварительного обучения на его размеченных данных.

## Подход

В качестве базовой архитектуры используется EEGNet.

Исследовательский pipeline включает:

- transfer learning между EEG-датасетами;
- supervised contrastive learning;
- subject-invariant representation learning;
- strict Leave-One-Subject-Out evaluation;
- task-aware denoising;
- анализ устойчивости к шумам и артефактам.

## Данные

### BCI Competition IV-2a
- 9 субъектов
- 22 EEG-канала
- Left Hand / Right Hand
- strict LOSO

### PhysioNet EEG Motor Movement/Imagery
- 109 субъектов
- 64 EEG-канала
- Left Hand / Right Hand
- strict LOSO

### Cho2017
Используется для предварительного обучения EEGNet и последующего transfer learning.

### EEGDenoiseNet
Используется в экспериментах по подавлению EOG/EMG-артефактов.

## Основные эксперименты

### 22-channel

Исследованы:

- EEGNet baseline;
- Transfer Learning;
- Supervised Contrastive Learning;
- Transfer + SupCon;
- GRL;
- MixStyle;
- subject-invariant ablations;
- task-aware denoising;
- clean и noisy EEG.

### PhysioNet 64-channel

Проведены:

- strict 109-subject LOSO;
- Cho2017 → PhysioNet transfer learning;
- learning-rate search;
- SupCon hyperparameter search;
- GRL и adaptive GRL;
- conflict-aware GRL;
- cross-subject SupCon;
- class-wise alignment;
- subject-held-out validation;
- fixed-epoch experiments;
- анализ hard subjects;
- сравнение с воспроизведённой архитектурой CCST.

## Структура репозитория

- `notebooks/` — основные исследовательские ноутбуки;
- `results/22ch/` — результаты 22-канальных экспериментов;
- `results/64ch/` — результаты PhysioNet 64ch;
- `docs/` — источники данных и описание исследования;
- `models/` — описание сохранённых checkpoints.

Сырые EEG-датасеты и веса моделей не включены в GitHub-репозиторий из-за объёма.

## Протокол оценки

Основной протокол — strict Leave-One-Subject-Out:

1. один субъект полностью исключается из обучения;
2. модель обучается на остальных субъектах;
3. исключённый субъект используется только для тестирования;
4. процедура повторяется для каждого субъекта.

## Статус работы

Завершены эксперименты по transfer learning, contrastive learning, subject invariance и task-aware denoising.

Следующий этап — полноценная оценка task-aware denoising на PhysioNet64. При недостаточном приросте качества планируется исследование альтернативного backbone при сохранении strict LOSO protocol.
