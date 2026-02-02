# frontend-test-questioncard

## 1. Архитектура

QuestionCard  
├── Stem (TipTap + KaTeX)  
├── Answers  
├── Actions (Check)  
├── Explanation  
└── ErrorBoundary  

State (local):
- selectedAnswerId      // выбранный ответ
- isChecked             // ответ проверен
- isChecking            // идёт запрос

State (global):
- questionId            // текущий вопрос
- questionData          // данные вопроса
- userMode              // demo / paid

selectedAnswerId — local  
isChecked — local  

on questionId change:
- сброс локального состояния

fast clicks:
- кнопка Check disabled
- повторные клики игнорируются

## 2. Logic (pseudo)

if !selectedAnswerId → disable Check   // нельзя проверить без ответа

selectAnswer(id):
  if isChecked return                  // после проверки менять нельзя
  selectedAnswerId = id

check():
  if isChecking or isChecked return
  isChecking = true
  api.check()
    → isChecked = true
    → isChecking = false

showExplanation =
  isChecked &&
  explanationExists &&
  userMode === paid

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
