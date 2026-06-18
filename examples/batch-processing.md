# Subagent Batch Processing

Делегирование задачи обработки файлов subagent'у.

## Задача

Обработать все `.txt` файлы в `/data`:
- Прочитать содержимое
- Найти ключевые слова
- Создать summary

## Код

```bash
# Main session
sessions_spawn \
  task="Process all .txt files in /data, find keywords, create summary report" \
  taskName=file-processor \
  context="isolated"

# Wait for completion
sessions_yield
```

## Что делает subagent

```python
# pseudo-code того, что делает subagent
import os
import glob

files = glob.glob("/data/*.txt")
results = []

for file in files:
    content = read(file)
    keywords = extract_keywords(content)
    results.append({
        "file": file,
        "keywords": keywords,
        "summary": summarize(content)
    })

write_report(results)
```

## Monitoring

```bash
# Check status
subagents list

# Send message if needed
sessions_send taskName=file-processor message="Status?"
```

## Output

```
✅ Processed 42 files
📊 Keywords found: 156 unique
📝 Report: /data/summary-report.md
```
