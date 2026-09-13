# Przewidywanie Lądowania Pierwszego Stopnia Rakiety SpaceX Falcon 9

Ten projekt został zrealizowany na potrzeby ukończenia kursu **IBM Data Science Professional Certificate**. Celem jest przewidzenie, czy pierwszy stopień rakiety Falcon 9 wyląduje pomyślnie, co jest kluczowym czynnikiem w redukcji kosztów startów kosmicznych.

## Opis Projektu

Firma SpaceX oferuje starty rakiety Falcon 9 w niezwykle konkurencyjnych cenach, co zawdzięcza głównie możliwości ponownego wykorzystania pierwszego stopnia maszyny. Jeśli potrafimy dokładnie przewidzieć, czy ten stopień wyląduje na Ziemi, jesteśmy w stanie precyzyjnie oszacować koszt całego startu. Projekt ten wykorzystuje modele uczenia maszynowego, aby określić prawdopodobieństwo pomyślnego lądowania.

## Źródła Danych

Projekt opiera się na zbiorach danych udostępnionych w przestrzeni dyskowej chmury IBM:
* **Dane Część 1:** Ogólne informacje o lotach, masie ładunku, orbicie oraz miejscach startowych. [Link do pobrania pliku CSV](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_1.csv)
* **Dane Część 2 i 3:** Przeskalowane zmienne objaśniające (`X`) oraz zmienna docelowa (`Y` - Class). 
  * [Link do zmiennych objaśniających (X)](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_3.csv)
  * [Link do zmiennej docelowej (Y)](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/datasets/dataset_part_2.csv)

## Metodologia

Prace w notatniku zostały podzielone na następujące etapy:

1. **Gromadzenie i wstępne przetwarzanie danych:** Wczytanie danych do ramek, obsługa braków (np. uzupełnienie brakujących wartości `PayloadMass` wartością średnią) oraz utworzenie binarnej zmiennej docelowej `Class` (1 dla udanego lądowania, 0 w przypadku braku sukcesu).
2. **Eksploracyjna Analiza Danych (EDA):** Użycie bibliotek `matplotlib`, `seaborn` oraz `plotly` do wizualnej analizy takich relacji jak powiązanie masy ładunku z numerem lotu i wynikiem misji, czy wskaźnik sukcesu dla poszczególnych rodzajów orbit.
3. **Analiza z użyciem SQL:** Utworzenie bazy danych SQLite w pamięci komputera w celu wykonania zapytań wyciągających unikalne miejsca startu, sumę masy ładunków dostarczonych na stację ISS oraz wskaźniki sukcesu dla konkretnych platform.
4. **Analiza Przestrzenna:** Zastosowanie biblioteki `folium` do wygenerowania interaktywnej mapy z naniesionym znacznikiem stacji startowej `CCAFS LC-40`.
5. **Modelowanie Uczenia Maszynowego:** Stworzenie i ocena algorytmów klasyfikacyjnych.
   * Dane standaryzowano przy użyciu narzędzia `StandardScaler`.
   * Zbiór podzielono na dane treningowe i testowe w proporcji 80/20 (do testów przeznaczono 18 próbek).
   * Do znalezienia optymalnych parametrów użyto `GridSearchCV` z 10-krotną walidacją krzyżową.
   * Zbadano następujące modele: Regresję Logistyczną, Maszyny Wektorów Nośnych (SVM), Drzewo Decyzyjne oraz algorytm K-Najbliższych Sąsiadów (KNN).

## Podsumowanie Wyników

* **Platformy Startowe:** Zbiór danych uwzględnia operacje przeprowadzane z platform `CCAFS SLC 40`, `VAFB SLC 4E` oraz `KSC LC 39A`.
* **Ewaluacja Modeli:** Dokładność predykcji na zbiorze testowym kształtowała się następująco:
  * **Regresja Logistyczna (Logistic Regression):** ~83.3%
  * **Maszyny Wektorów Nośnych (SVM):** ~83.3%
  * **Drzewo Decyzyjne (Decision Tree):** ~66.7%
  * **K-Najbliższych Sąsiadów (KNN):** ~83.3%
* **Wniosek:** Ze względu na bardzo mały rozmiar próby w wyodrębnionym zbiorze testowym, większość zastosowanych modeli z reguły dała identyczną skuteczność na poziomie ok. 83.3%
