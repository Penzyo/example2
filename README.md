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





import openpyxl
import json

# Carica il file Excel
wb = openpyxl.load_workbook('esempio.xlsx')

# Seleziona il foglio di lavoro
ws = wb.active

# Inizializza il dizionario JSON
json_data = {}

# Itera sulle righe della tabella
for row in ws.iter_rows(values_only=True):
    # Inizializza il puntatore del dizionario JSON
    current_json_level = json_data

    # Itera sulle colonne della riga
    for col_idx, cell_value in enumerate(row):
        # Se la cella è vuota, salta la colonna
        if cell_value is None:
            continue

        # Crea la chiave del dizionario JSON in base alla colonna
        key = chr(ord('a') + col_idx)

        # Aggiungi il valore alla posizione corretta nel dizionario JSON
        if cell_value not in current_json_level:
            current_json_level[cell_value] = {}

        # Sposta il puntatore al livello corretto del dizionario JSON
        current_json_level = current_json_level[cell_value]

    # Resetta il puntatore del dizionario JSON per la prossima riga
    current_json_level = json_data

# Stampa il dizionario JSON
print(json.dumps(json_data, indent=4))
