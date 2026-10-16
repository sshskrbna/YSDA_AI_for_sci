## 2026-10-11 — ДЗ 1, сессия 1 из 2: независимая проверка (скрипт)

- Запрос: создать checks/check_hw1_fasta.py (единственный файл сессии) и запустить его; результата ещё нет.
- Что сделано: написан checks/check_hw1_fasta.py — независимая проверка на стандартной библиотеке Python 3. Сверяет SHA-256 source и original_source из homework/inputs.tsv, сам разбирает каждый FASTA (length_bp, a/c/g/t/ambiguous, gc_percent), читает results/hw1/fasta_stats.csv (или --result) с требуемым порядком столбцов и ровно тремя строками, сверяет целые точно и gc_percent с допуском 1e-6.
- Результат: запуск `python3 checks/check_hw1_fasta.py` вернул код 1 с сообщением «Нет файла результата: results/hw1/fasta_stats.csv» — ожидаемо, файла результата ещё нет.
- Точка проверки: код 1 при отсутствии файла результата.
- Итог проверки: фактический код 1, сообщение об отсутствии файла результата — соответствует ожиданию.

## 2026-10-11 — Статистика FASTA, сессия 2 из 2 (расчёт)
- Запрос: написать scripts/hw1_fasta_stats.py (только стандартная библиотека), прочитать homework/inputs.tsv, разобрать FASTA из source, записать results/hw1/fasta_stats.csv со столбцами sample_id,source,length_bp,a_count,c_count,g_count,t_count,ambiguous_count,gc_percent; gc_percent с 8 знаками после запятой. Запустить скрипт и проверку checks/check_hw1_fasta.py, сообщить фактический код.
- Что сделано: написан scripts/hw1_fasta_stats.py с docstring (Данные/Проверка/Используется в). Читает homework/inputs.tsv, для каждой строки разбирает FASTA из source (регистронезависимо, только ACGT), считает length_bp, a/c/g/t_count, ambiguous_count (символы вне ACGT) и gc_percent = (g+c)/length*100. Записывает results/hw1/fasta_stats.csv с требуемым порядком столбцов и gc_percent с 8 знаками.
- Результат: создан results/hw1/fasta_stats.csv с тремя строками (KU361241.1, KU736960.1, NC_032056.1). Длины 151324/151307/151282, ambiguous_count 0, gc_percent 37.68602469/37.69025888/37.67863989.
- Точка проверки: python3 checks/check_hw1_fasta.py.
- Итог проверки: фактический код 0, «OK: все проверки пройдены».

## 2026-10-11 — исправление формулы GC в hw1_fasta_stats.py
- Запрос: в scripts/hw1_fasta_stats.py gc_percent считался как (g+c)/length*100; по разделу 0 AGENTS.md знаменатель должен быть A+C+G+T (неоднозначные основания входят в длину, но не в знаменатель GC). Исправить только формулу, checks/ не трогать.
- Что исправлено: строка 39 — знаменатель заменён с length на (a+c+g+t): gc = (g+c)/(a+c+g+t)*100.0 if (a+c+g+t) else 0.0.
- Точка проверки: python3 scripts/hw1_fasta_stats.py; python3 checks/check_hw1_fasta.py.
- Итог проверки: фактический код 0, «OK: все проверки пройдены».

## 2026-10-11 — Точка проверки FASTA (вручную)
- Что проверено: checks/check_hw1_fasta.py на испорченной копии таблицы (GC KU361241.1 +1).
- Команда: python3 checks/check_hw1_fasta.py --result /tmp/bad.csv
- Итог проверки: код 1, найдено расхождение gc_percent у KU361241.1; на правильной таблице код 0. Принято.

## 2026-10-15 — Каскад статьи APG V: библиоссылка в L1 и сверка тезисов 2–4 в L2
- Запрос: завершить каскад статьи APG V, два места. 1) В memory/L1.md заменить строку-шаблон «Статья: <…>» на библиографическую ссылку (данные только из paper/angiosperm_2026/reference.bib и source.json). 2) В memory/L2.md заполнить таблицу «Сверка с оригиналом» для тезисов 2, 3, 4 по оригиналу (paper/angiosperm_2026/source.txt, страницы через \f), с дословной цитатой до 25 слов и номером страницы PDF; при расхождении исправить тезис в нумерованном списке. Дописать запись в diary.md.
- Что сделано: в L1.md строка-шаблон заменена на ссылку: The Angiosperm Phylogeny Group. «Large-scale nuclear and plastid phylogenomic analyses inform an updated Angiosperm Phylogeny Group classification: APG V». Journal of Systematics and Evolution 64(5): 845–874, 2026. DOI: 10.1111/jse.70096 (авторы, название, журнал, том(вып.): страницы, год, DOI — из reference.bib). В L2.md заполнена таблица для тезисов 2, 3, 4 по source.txt (страницы PDF 5, 5, 11): тезис 2 — цитата «APG V accepts 407 families.» (с. 5), подтверждено; тезис 3 — цитата «Gotty & Pillon (2025) found that since 2012 (when the APG III, 2009, classiﬁcation was implemented), family‐level changes have been reduced to zero.» (с. 5), расходится: в оригинале снижение до нуля с момента внедрения APG III (2009), а не «между APG IV и APG V»; тезис 3 исправлен в нумерованном списке; тезис 4 — цитата «fabids should be limited just to the nitrogen‐ﬁxing members, Cucurbitales, Rosales, Fagales, and Fabales.» (с. 11), подтверждено.
- Результат: L1.md и L2.md обновлены; тезис 3 скорректирован по оригиналу.
- Точка проверки: сверка цитат посимвольно с source.txt и страниц по source.pdf (pdf_search: «APG V accepts 407 families» — с. 5; «reduced to zero» — с. 5; «fabids» — с. 1 и 11).
- Итог проверки: цитаты и страницы совпадают с оригиналом; расхождение тезиса 3 исправлено.

## 2026-10-15 — Сверка тезисов L2 с PDF (вручную)
- Что проверено: три цитаты из таблицы «Сверка с оригиналом» в memory/L2.md (тезисы 2, 3, 4) найдены в paper/angiosperm_2026/source.pdf на с. 5 и 11.
- Итог проверки: все три цитаты совпадают с оригиналом; исправление тезиса 3 верно. Принято.
