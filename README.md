Analiza I Predykcja Kursów Walut
Projekt integrujący dane z API NBP, skrypt Python oraz raportowanie w Power BI.

# 1. Analiza Porównawcza
Centralny panel analityczny zestawiający kluczowe porównanie trendów czterech głównych walut w wybranym oknie czasowym.
<img width="1385" height="733" alt="Analiza porownawcza" src="https://github.com/user-attachments/assets/d36310d4-e0b0-4bb0-b7ff-e807acbafac6" />

# 2. Szczegółowa Analiza Rynku
Macierz przedstawiająca średnie kursy w ujęciu miesięcznym.
<img width="1337" height="731" alt="Kurs wybranej waluty" src="https://github.com/user-attachments/assets/ac4fe6bf-810e-439e-8233-0e46a3722aef" />

Wskaźnik porównujący obecny kurs do notowania z dnia poprzedniego.
<img width="1299" height="729" alt="Porownanie kursu" src="https://github.com/user-attachments/assets/9596efba-2539-4280-8637-0be1791c51e1" />

# 3. Zaawansowane funkcje
Integracja skryptu Python wewnątrz Power BI. Przedstawienie danych historycznych, wyliczenie trendu liniowego oraz predykcja na najbliższy okres.
<img width="1299" height="734" alt="Analiza predykcyjna" src="https://github.com/user-attachments/assets/7e5f4dda-ec48-4aac-8afe-22575570ab65" />

```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# 1. Przygotowanie danych
df = dataset.copy()
df['Data'] = pd.to_datetime(df['Data'])
df = df.sort_values('Data')

# 2. Pobranie nazwy waluty z filtra (logika dynamiczna Power BI)
if 'Wybór Waluty' in df.columns:
    nazwa_waluty = df['Wybór Waluty'].iloc[0]
else:
    nazwa_waluty = "Wybrana Waluta"

# 3. Obliczanie regresji liniowej (Trend)
df['Dni'] = (df['Data'] - df['Data'].min()).dt.days
z = np.polyfit(df['Dni'], df['Aktualny Kurs'], 1)
p = np.poly1d(z)

# 4. Predykcja na 30 dni w przód
dni_przyszlosc = 30
ostatni = df['Dni'].max()
x_future = np.arange(ostatni, ostatni + dni_przyszlosc)
y_future = p(x_future)
dates_future = [df['Data'].min() + pd.Timedelta(days=int(x)) for x in x_future]

# 5. Rysowanie wykresu (Matplotlib)
plt.figure(figsize=(10, 5))
plt.plot(df['Data'], df['Aktualny Kurs'], label='Historia', color='#118DFF')
plt.plot(df['Data'], p(df['Dni']), 'gray', linestyle='--', alpha=0.5, label='Trend')
plt.plot(dates_future, y_future, 'red', linewidth=3, label='Prognoza (+30 dni)')

plt.title(f'Prognoza dla: {nazwa_waluty}', fontsize=14, color='black')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```
Symulator Inwestycji
Możliwość sprawdzenia przez użytkownika, jaki jest koszt zakupu określonej ilości walut po średnim kursie.
<img width="1301" height="730" alt="Kwota inwestycji" src="https://github.com/user-attachments/assets/653a0e59-f341-4ea3-8938-794e5c0f0066" />

#4. Dane Źródłowe
Pełna Tabela Historyczna pobrana automatycznie z API Narodowego Banku Polskiego. (http://api.nbp.pl/api/exchangerates/tables/A/last/30/?format=json)
<img width="1338" height="729" alt="Pelne dane" src="https://github.com/user-attachments/assets/9314c772-c3e9-4cbc-bd5b-0e19e58c193b" />
