
find file and open in code
```
find . -name "backend.tf.json" -exec code {} \;
```
 Wyjaśnienie:
- `find .`: Rozpoczyna wyszukiwanie w bieżącym katalogu (możesz zastąpić `.` inną ścieżką, jeśli chcesz szukać gdzie indziej).
- `-name "backend.tf.json"`: Szuka plików o nazwie `backend.tf.json`.
- `-exec code {} \;`: Dla każdego znalezionego pliku (`{}` to miejsce, w którym znajduje się ścieżka do pliku) wykonuje polecenie `code`, aby otworzyć ten plik w VSCode.

***

