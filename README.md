# LAB1---cleanup.sh

## **Explicația detaliată a scriptului**

### 1. **Shebang și comentarii**
```bash
#!/bin/bash
```
- Spune sistemului să folosească interpretorul `bash` pentru a rula scriptul


### 2. **Verificarea primului argument**
```bash
if [ -z "$1" ]; then
```
- **`[ -z "$1" ]`**: Verifică dacă primul argument (`$1`) este gol sau lipsește
- **`-z`**: Înseamnă "zero length" - testează dacă stringul este gol
- **`"$1"`**: Primul argument dat scriptului (directorul de curățat)

### 3. **Afișarea mesajelor de eroare**
```bash
echo "Eroare: Trebuie sa specifici directorul de curatare!"
echo "Utilizare: $0 <director> [extensie1] [extensie2] ..."
exit 1
```
- **`echo`**: Afișează un mesaj pe ecran
- **`$0`**: Numele scriptului curent
- **`exit 1`**: Ieșire cu cod de eroare (1 = eroare, 0 = succes)

### 4. **Setarea variabilei pentru director**
```bash
director_tinta=$1
```
- Salvează primul argument într-o variabilă 

### 5. **Comanda shift**
```bash
shift
```
-  "Mută" argumentele cu o poziție la stânga
- **Exemplu**: Dacă am `script.sh /tmp log txt`, după `shift` rămân doar `log txt`


### 6. **Verificarea existenței directorului**
```bash
if [ ! -d "$director_tinta" ]; then
```
- **`! -d`**: Verifică dacă NU (`!`) este un director (`-d`),  dacă directorul nu există, afișează eroare

### 7. **Setarea extensiilor implicite**
```bash
if [ $# -eq 0 ]; then
  extensii=("tmp")
else
  extensii=("$@")
fi
```
- **`$#`**: Numărul de argumente rămase după `shift`
- **`-eq 0`**: Egal cu zero
- **`extensii=("tmp")`**: Creează un array cu o extensie implicită
- **`extensii=("$@")`**: Salvează toate argumentele rămase în array

### 8. **Inițializarea contorului**
```bash
fisiere_sterse=0
```
-  Setează contorul la zero pentru a număra fișierele șterse

### 9. **Bucla pentru procesarea extensiilor**
```bash
for extensie_curenta in "${extensii[@]}"; do
```
- **`for...in`**: Buclă care parcurge fiecare element
- **`"${extensii[@]}"`**: Toate elementele din array-ul `extensii`
- **`extensie_curenta`**: Variabila care va conține fiecare extensie pe rând

### 10. **Comanda principală de ștergere**
```bash
numar_fisiere=$(find "$director_tinta" -type f -name "*.$extensie_curenta" -print -delete | wc -l)
```

- **`find "$director_tinta"`**: Caută în directorul specificat
- **`-type f`**: Doar fișiere (nu directoare)
- **`-name "*.$extensie_curenta"`**: Numele să se termine cu extensia dorită
- **`-print`**: Afișează numele fișierului găsit
- **`-delete`**: Șterge fișierul
- **`| wc -l`**: Numără câte linii (= câte fișiere) au fost afișate
- **`$(...)`**: Salvează rezultatul comenzii în variabilă

### 11. **Adunarea la contor**
```bash
fisiere_sterse=$((fisiere_sterse + numar_fisiere))
```
- **`$((...))`**: Operații matematice în bash
-  Adună numărul de fișiere șterse la totalul general

### 12. **Afișarea rezultatului final**
```bash
echo "Curatare completa. Fisiere sterse: $fisiere_sterse"
```
-  Afișează numărul total de fișiere șterse

## 📝 **Exemple de utilizare:**

```bash
# Șterge doar fișierele .tmp din /tmp
./cleanup.sh /tmp

# Șterge fișierele .log și .bak din /var/log
./cleanup.sh /var/log log bak

# Șterge fișierele .tmp, .old și .backup din directorul curent
./cleanup.sh . tmp old backup
```
