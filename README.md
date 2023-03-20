import json
import pandas as pd

# Lettura del file Excel
df = pd.read_excel('nome_del_file.xlsx')

# Creazione del dizionario vuoto per il JSON
json_dict = {}

# Ciclo sulle righe del DataFrame
for row_index, row in df.iterrows():
    # Inizializza un nuovo oggetto JSON vuoto
    current_obj = json_dict

    # Ciclo sulle colonne del DataFrame
    for col_index, col in enumerate(df.columns):
        # Se la cella è vuota, salta questa colonna
        if pd.isnull(row[col_index]):
            continue

        # Crea una nuova chiave per il livello corrente del JSON
        key = col

        # Se la chiave non esiste ancora nell'oggetto JSON, creala come un nuovo oggetto vuoto
        if key not in current_obj:
            current_obj[key] = {}

        # Se questa è l'ultima colonna, aggiungi il valore come chiave nell'oggetto JSON corrente
        if col_index == len(df.columns) - 1:
            current_obj[key][row[0]] = row[col_index]
        else:
            # Sposta l'oggetto JSON corrente al livello successivo
            current_obj = current_obj[key]

# Converti il dizionario JSON in una stringa JSON
json_str = json.dumps(json_dict)

# Stampa la stringa JSON
print(json_str)
