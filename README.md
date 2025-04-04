# pipex_cmds
---

## ✅ CAS NORMAUX (fonctionnels)

Supposons que tu exécutes ton programme comme ceci :  
```bash
./pipex infile "cmd1" "cmd2" outfile
```

### 1. Commandes simples
```bash
echo "hello world" > infile
./pipex infile "cat" "wc -w" outfile
# Résultat attendu dans outfile : 2
```

### 2. Commande avec arguments
```bash
echo "hello world" > infile
./pipex infile "grep hello" "wc -l" outfile
# Résultat attendu dans outfile : 1
```

### 3. Utilisation de `tr`
```bash
echo "abc DEF" > infile
./pipex infile "tr a-z A-Z" "rev" outfile
# Résultat attendu dans outfile : "FED CBA"
```

### 4. Avec des chemins complets
```bash
./pipex infile "/bin/cat" "/usr/bin/wc -c" outfile
```

---

## 🟨 CAS SPÉCIFIQUES / LIMITES

### 5. Commandes avec espaces et guillemets
```bash
echo "one two three" > infile
./pipex infile "awk '{ print $2 }'" "wc -c" outfile
# Résultat attendu : nombre de caractères du mot "two"
```

### 6. Fichier d’entrée vide
```bash
touch infile
./pipex infile "cat" "wc -c" outfile
# Résultat : 0
```

### 7. `here_doc` (si tu l’as implémenté)
```bash
./pipex here_doc EOF "cat" "wc -l" outfile
# Tape plusieurs lignes, puis EOF pour terminer
```

### 8. Plusieurs pipes (bonus)
```bash
./pipex infile "cat" "grep e" "wc -c" outfile
```

---

## ❌ CAS D'ERREURS À GÉRER

### 9. Fichier d’entrée inexistant
```bash
./pipex nofile "cat" "wc" outfile
# Erreur attendue : "nofile: No such file or directory"
```

### 10. Commande introuvable
```bash
echo test > infile
./pipex infile "notacommand" "wc" outfile
# Erreur : "command not found"
```

### 11. Pas les bons arguments
```bash
./pipex infile "ls"
# Erreur : nombre d’arguments invalide
```

### 12. Fichier de sortie sans permission
```bash
touch outfile
chmod 000 outfile
./pipex infile "cat" "wc" outfile
# Erreur : "Permission denied"
```

### Leaks
```bash
valgrind --leak-check=full --trace-children=yes ./pipex file1 cmd1 cmd2 file2
```

