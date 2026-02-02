# frontend-test-questioncard

## 1. Архитектура

QuestionCard  
├── Stem (TipTap + KaTeX)  
├── Answers  
├── Actions (Check)  
├── Explanation  
└── ErrorBoundary 

State (local):
- selectedAnswerId
  выбранный ответ
- isChecked
  ответ проверен
- isChecking
  идёт запрос

State (global):
- questionId
  текущий вопрос
- questionData
  данные вопроса
- userMode
  demo / paid

selectedAnswerId — локальное  
isChecked — локальное  

on questionId change:
- сброс локального состояния

fast clicks:
- кнопка Check disabled
- повторные клики игнорируются

## 2. Logic (pseudo)

Если selectedAnswerId пустой
  disable Check

Функция selectAnswer(id):
  Если isChecked = true
    вернуть
  selectedAnswerId = id

Функция check():
  Если isChecking = true или isChecked = true
    вернуть
  isChecking = true
  api.check()
    после ответа:
      isChecked = true
      isChecking = false

showExplanation:
  показывать, если isChecked = true
  и explanation существует
  и userMode = paid

onQuestionChange():
  selectedAnswerId = null
  isChecked = false
  isChecking = false

## 3. Edge cases / UX

no explanation:
- блок не показываем

only formulas:
- центрируем контент

long text:
- скролл, actions закреплены

katex error:
- fallback текст

change answer after check:
- ответы заблокированы

demo mode:
- explanation заблюрен
- текст: "Доступно в платной версии"
- CTA на оплату
